<template>
    <div class="register">
        <el-card>
            <div slot="header">
                <div class="card-title">注册</div>
            </div>
            <el-form :model="user" :rules="rules" :ref="user">
                <el-form-item prop="email">
                    <el-input type="email" v-model="user.email" placeholder="邮箱"/>
                </el-form-item>
                <el-form-item prop="username">
                    <el-input type="text" v-model="user.username" placeholder="用户名"/>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input type="password" v-model="user.password" placeholder="密码"
                              @keyup.enter.native="onSubmit(user)"/>
                </el-form-item>
                <el-form-item>
                    <el-button class="button-long" type="primary"
                               @click="onSubmit(user)" :loading="load" round>
                        注册
                    </el-button>
                </el-form-item>
            </el-form>
        </el-card>
    </div>
</template>

<script>
export default {
    name: 'register',
    layout: 'blog',
    head() {
        return {
            title: '注册'
        }
    },
    data() {
        return {
            user: {
                email: '',
                username: '',
                password: ''
            },
            rules: {
                email: [
                    {
                        type: 'email',
                        required: true,
                        message: '请输入正确的邮箱',
                        trigger: 'blur'
                    }
                ],
                username: [
                    {
                        required: true,
                        message: '请输入用户名',
                        trigger: 'blur'
                    },
                    {
                        min: 2,
                        max: 10,
                        message: '长度在2到10个字符',
                        trigger: 'blur'
                    }
                ],
                password: [
                    {
                        required: true,
                        message: '请输入密码',
                        trigger: 'blur'
                    },
                    {
                        min: 6,
                        max: 20,
                        message: '长度在6到20个字符',
                        trigger: 'blur'
                    }
                ]
            },
            load: false
        }
    },
    methods: {
        onSubmit(user) {
            this.$refs[user].validate((valid) => {
                if (valid) {
                    this.load = true
                    this.$axios.post('register', user).then(response => {
                        if (response.status === 'success') {
                            this.$router.push({name: 'index'})
                            this.$message.success(response.message)
                        }
                        this.load = false
                    })
                }
            });
        }
    }
}
</script>