<template>
    <el-form :model="article" :rules="rules" :ref="article">
        <el-form-item prop="category">
            <el-select v-model="article.category" value-key="id" placeholder="请选择或搜索分类" filterable>
                <el-option v-for="category in categories" :key="category.id" :label="category.name" :value="category"/>
            </el-select>
        </el-form-item>
        <el-form-item prop="title">
            <el-input type="text" v-model="article.title" placeholder="标题" maxlength="60" show-word-limit/>
        </el-form-item>
        <el-form-item prop="synopsis">
            <el-input type="textarea" v-model="article.synopsis" :autosize="{minRows: 3, maxRows: 6}"
                      placeholder="摘要" minlength="10" maxlength="300" show-word-limit/>
        </el-form-item>
        <el-form-item prop="content">
            <Editor v-model="article.content"/>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" size="small" @click="onSubmit(article)" :loading="load">提交</el-button>
        </el-form-item>
    </el-form>
</template>

<script>
import Editor from '~/components/Editor';

export default {
    name: 'article-new',
    components: {
        Editor
    },
    layout: 'admin',
    head() {
        return {
            title: '发布文章'
        }
    },
    data() {
        return {
            article: {
                title: '',
                synopsis: '',
                content: '',
                category: '',
            },
            rules: {
                category: [
                    {
                        required: true,
                        message: '请选择分类'
                    }
                ],
                title: [
                    {
                        required: true,
                        message: '请输入标题',
                        trigger: 'blur'
                    }
                ],
                synopsis: [
                    {
                        required: true,
                        message: '请输入摘要',
                        trigger: 'blur'
                    }
                ],
                content: [
                    {
                        required: true,
                        message: '请输入内容',
                        trigger: 'blur'
                    }
                ]
            },
            categories: [],
            load: false
        }
    },
    created() {
        this.getCategories()
    },
    methods: {
        getCategories() {
            this.$axios.get('categories').then(response => {
                if (response.status === 'success') {
                    this.categories = response.data
                }
            })
        },
        onSubmit(article) {
            this.$refs[article].validate((valid) => {
                if (valid) {
                    this.load = true
                    this.$axios.post('article', article).then(response => {
                        if (response.status === 'success') {
                            this.$refs[article].resetFields()
                            this.$message.success(response.message)
                        }
                        this.load = false
                    });
                }
            });
        }
    }
}
</script>