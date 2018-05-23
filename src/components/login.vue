<template>
    <Layout>
        <Header class="header">
            <span class="string">登录</span>   
        </Header>
        <Content class="content">
            <Form ref="formInline" :model="formInline" :rules="ruleInline" class="longin-wrapper">
                <FormItem prop="user" class="user">
                    <Input type="text" v-model="formInline.user" placeholder="Username" >
                        <Icon type="ios-person-outline" slot="prepend"></Icon>
                    </Input>
                </FormItem>
                <FormItem prop="password" class="password">
                    <Input type="password" v-model="formInline.password" placeholder="Password">
                        <Icon type="ios-locked-outline" slot="prepend"></Icon>
                    </Input>
                </FormItem>
                <FormItem class="login">
                    <Button type="ghost" @click="handleSubmit('formInline')">登录</Button>
                </FormItem>
            </Form>
        </Content>
    </Layout>
</template>
<script>
    export default {
        data() {
            return {
                formInline: {
                    user: '',
                    password: ''
                },
                userMsg: {
                    user: 'abc',
                    password: '8888'
                },
                ruleInline: {
                    user: [
                        { required: true, message: '请输入账号', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'blur' },
                        { type: 'string', min: 4,message: '至少4位' }
                    ]
                }
            }
        },
        methods: {
            handleSubmit(name) {
                const valid = this.formInline.user === this.userMsg.user && this.formInline.password === this.userMsg.password
                this.$refs[name].validate((a) => {
                        if (a) {
                            if (valid) {
                                this.$Message.success('登录成功')
                                setTimeout(() => {
                                    this.$router.push({
                                    path: '/main'
                                    })
                                },1000)
                            } else {
                                this.$Message.error('请输入正确的账号密码')
                            }
                        }
                })
            }
        }
    }
</script>

<style lang="stylus">
    .header
        background-color : #2894FF
        .string
            display: inline-block
            font-family: "PingFang SC"
            font-size: 30px
            font-weight: 100
            color: #fff
    .content
        position :relative
        height:100%
        .longin-wrapper
            width: 33%
            margin: 0 auto
            margin-top: 10%
            font-size:1.5rem
            .ivu-input-group-prepend
                font-size: 1.6rem;
                width:10%;
                min-width: 20px
                font-weight: 200
                .ivu-icon
                    font-weight:500
            .ivu-input
                font-size 1.2rem
                padding:20px
            .ivu-btn
                font-size:1.2rem
                width:100%
                
</style>
