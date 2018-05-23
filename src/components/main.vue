<template>
  <transition name="slide">
    <layout class="layout">
        <Header class="header">
          <span class="user">用户界面</span>
          <span class="back" @click="backLogin">返回</span>
        </Header>
        <div class="main-search">
          <Input placeholder="请输入搜索内容"></Input>
          <Button type="ghost" icon="ios-search"></Button>
        </div>
        <div class="main-table">
          <Table :columns="col" :data="data"></Table>
          <Button class="table-additem" type="default" size="large" @click="addItem">添加项目</Button>
        </div>
        <modal :mode="mode" @reset="emitCancel"></modal>
    </layout>
  </transition>
</template>

<script>
import Modal from 'components/modal'

export default {
  name: 'HelloWorld',
  data () {
    return {
      search: [],
      col: [
        {
          title: '项目名称',
          key: ''
        },
        {
          title: '项目类型',
          key: ''
        },
        {
          title: '已进行天数',
          key: ''
        },
        {
          title: '操作',
          key: ''
        }
      ],
      data: [],
      mode: false
    }
  },
  methods: {
    addItem() {
      this.mode = true
    },
    emitCancel() {
      this.mode = false
    },
    backLogin() {
      this.$router.back()
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
            color: #E0E0E0
    .main-search
      width: 50%
      height: 100px
      margin:0 auto
      margin-top:5%
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
