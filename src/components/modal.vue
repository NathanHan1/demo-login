<template>
    <Modal
        v-model="isOpen"
        :title="computedTitle"
        class-name="modal"
        @on-ok="ok"
        @on-cancel="cancel"
        >
        <Form ref="item" :model="item" :rules="ruleInline" :label-width="80">
            <FormItem prop="name" label="项目名称">
                <Input type="text" v-model="item.name" placeholder="请输入项目名称" >{{item.name}}</Input>
            </FormItem>
            <FormItem label="项目类型">
                <Select v-model="item.type">
                    <Option value="A类">A类</Option>
                    <Option value="B类">B类</Option>
                    <Option value="C类">C类</Option>
                </Select>
            </FormItem>
            <FormItem prop="day" label="已进行天数">
                <Input type="text" v-model="item.day" placeholder="请输入项目进行的天数" >{{item.day}}</Input>
            </FormItem>
        </Form>
    </Modal>
</template>

<script>
export default {
    props: {
        open: {
            type: Boolean
        },
        mode: {
            type: Number
        },
        title: {
            type: String
        },
        currentItem: {
            type: Object
        }
    },
    data () {
        return {
            item: {
                name: '',
                type: '',
                day: ''
            },
            commitItem: {},
            ruleInline: {
                name: [
                    { required: true, message: '请输入账号', trigger: 'blur' }
                ],
                day: [
                    {required: true, message: '请输入进行的天数', trigger: 'blur'}
                ]
            }
        }
    },
    computed: {
        isOpen: {
            get:function () {
                return this.open
            },
            set: function () {
            }
        },
        computedTitle: {
            get:function () {
                return this.title
            },
            set: function () {
            }
        }
    },
    methods: {
        cancel() {
            this.cancel()
        },
        ok() {
            this.commitItem = {
                name: this.item.name,
                type: this.item.type,
                day: this.item.day
            }
            if (this.mode === 0) {
                if (this.item.name && this.item.day && this.item.type) { //输入了数据才会在表单显示
                    this.$emit('commitItem', this.commitItem)
                    this.$emit('isSearch')  // 派发搜索信息
                } else {
                    this.$Message.error('请输入对应信息！')
                }
            }
            if (this.mode === 1) {
                this.$emit('editItem', this.commitItem)
            }
            this.cancel()
        },
        cancel() {
            this.$emit('cancel')
        }
    },
    watch: {
        open() {
            if (this.mode === 0) {
                this.item.name = ''
                this.item.type = ''
                this.item.day = ''
            }
            // 若是编辑状态，需要获得对应的键值对
            if (this.mode === 1) {
                this.item.name = this.currentItem.name
                this.item.type = this.currentItem.type
                this.item.day = this.currentItem.day
            }
        }
    }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
    .modal
        display: flex
        align-items: center
        justify-content: center 
        .ivu-modal
            top:0
        .ivu-form-item-label::before
            display: none 
</style>