<template>
  <transition name="slide">
    <layout class="layout">
        <Header class="header">
          <span class="user">用户界面</span>
          <span class="back" @click="backLogin">返回</span>
        </Header>
        <div class="main-search">
          <Input v-model="searchContent" placeholder="请输入搜索内容"></Input>
          <Button type="ghost" icon="ios-search" @click="search"></Button>
        </div>
        <div class="main-table">
          <Table border :columns="col" :data="itemsComputed" ref="table"></Table>
          <Button class="table-additem" type="default" size="large" @click="addItem">添加项目</Button>
        </div>
        <modal :open="open" 
        :mode="mode" 
        @cancel="emitCancel" 
        @commitItem="commitItem"
        @editItem="editItem"
        :title="title"
        :currentItem="currentItem"
        @isSearch="updateItems"></modal>
    </layout>
  </transition>
</template>

<script>
import Modal from 'components/modal'

export default {
  data () {
    return {
      col: [
        {
          title: '项目名称',
          key: 'name'
        },
        {
          title: '项目类型',
          key: 'type'
        },
        {
          title: '已进行天数',
          key: 'day'
        },
        {
          title: '操作',
          key: 'operating',
          align: 'canter',
          width: 200,
          render: (h, params) => {     //使用render函数来自定义渲染组件
            return h('div', [
                h('Button', {
                  props: {
                    type: 'primary'
                  },
                  style: {
                    marginRight: '5px'
                  },
                  on: {
                    click: () => {
                      this.edit(params.index)
                    }
                  }
                }, '编辑'),
                h('Button', {
                  props: {
                    type: 'error'
                  },
                  style: {
                    marginRight: '5px'
                  },
                  on: {
                    click: () => {
                      this.remove(params.index)
                    }
                  }
                }, '删除')
            ])
          }
        }
      ],
      open: false, // 是否打开
      mode: 0, // 模式来确定是编辑还是添加，0为添加，1为编辑
      items: [],
      editIndex: 0,
      title: '',
      currentItem: {}, //用来维护编辑状态的已有键值对
      searchContent: '',
      searchItems: [],
      isSearch: false // 是否点击了搜索
    }
  },
  methods: {
    addItem() {
      this.open = true
      this.mode = 0
      this.title = '新建项目'
    },
    emitCancel() {
      this.open = false
    },
    backLogin() {
      this.$router.back()
    },
    updateItems() {               //解决点击搜索导致添加项目无法显示的问题
      this.isSearch = false
    },
    search() {                // 搜索
      let listSearch = []
      let flag = false
      for (let i = 0; i < this.items.length; i++) {
        for (let key in this.items[i]) {
          if (this.items[i][key].indexOf(this.searchContent) > -1) {
            flag = true
            break
          }
        }
        if (flag) {
          listSearch.push(this.items[i])
          flag = false
        }
      }
      this.searchItems = listSearch
    },
    edit(index) {
      this.open = true
      this.mode = 1
      this.editIndex = index
      this.title = '编辑项目'
      const item = this.items[index]
      if(!this.isSearch) {
        this.currentItem.name = item.name
        this.currentItem.type = item.type
        this.currentItem.day = item.day
      }
    },
    remove(index) {
      if (!this.isSearch) {
        this.items.splice(index,1)
      }
      if (this.isSearch) {
          for (let i = 0; i < this.items.length; i++) {
            if (this.items[i].name === this.searchItems[index].name) {
              this.items.splice(i,1)
            }
          }
          this.searchItems.splice(index,1)
      }
    },
    commitItem(commitItem) {
      this.items.push({
        name: commitItem.name,
        day: commitItem.day,
        type: commitItem.type
      })
      if (this.isSearch) {
        this.search()
      }
    },
    editItem(editItem) {
      const edit = this.items[this.editIndex]
      edit.name = editItem.name
      edit.type = editItem.type
      edit.day = editItem.day
      for (let i = 0; i < this.searchItems.length; i++) {
        if (this.searchItems[i].name === edit.name) {
          this.searchItems[i].name = edit.name
          this.searchItems[i].type = edit.type
          this.searchItems[i].day = edit.day
        }
      }
    }
  },
  computed: {
    itemsComputed() {
      if (this.isSearch) {
        return this.searchItems
      } else {
        return this.items
      }
    }
  },
  watch: {
    searchItems() {
      this.isSearch = true
    }
  },
  components: {
    Modal
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .layout
    position: fixed
    top:0
    left:0
    right:0
    bottom:0
    background-color: #fff
    z-index:50
    .header
        background-color : #2894FF
        .user
            display: inline-block
            font-family: "PingFang SC"
            font-size: 30px
            font-weight: 100
            color: #fff
        .back
            display: inline-block
            font-family: "PingFang SC"
            width:80px
            font-size: 30px
            font-weight: 100
            color: #fff
            float: right
          .back:hover
            text-shadow: 0 0 5px
    .main-search
      width: 50%
      height: 70px
      margin:0 auto
      margin-top:3%
      padding-left:5%
      font-size:0
      .ivu-input-wrapper
        height:30px
        width:80%
      .ivu-btn
        height:30px
        width:10%
        padding:3px
    .main-table
      margin:0 auto 
      width:70%
      .table-additem
        float:right
.slide-enter-active,.slide-leave-active
  transition: all 0.8s
.slide-enter,.slide-leave-to
  opacity: 0
</style>
