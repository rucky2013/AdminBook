<template>
    <div class="bookupdate">
        <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
            <Form-item label="书名" prop="bookname">
                <Input v-model="formValidate.bookname" placeholder="请输入书名"></Input>
            </Form-item>
            <Form-item label="价格" prop="bookprice">
                <Input v-model="formValidate.bookprice" placeholder="请输入价格"></Input>
            </Form-item>
            <Form-item label="出版社" prop="bookpublish">
                <Input v-model="formValidate.bookpublish" placeholder="请输入出版社"></Input>
            </Form-item>
            <Form-item>
                <Button type="primary" @click="handleSubmit('formValidate')">提交</Button>
                <Button type="ghost" @click="back" style="margin-left: 8px">返回</Button>
            </Form-item>
        </Form>
    </div>
</template>
<script>
    import Store from '@/utils/store'
    import Url from '@/utils/url'
    export default {
        data () {
            return {
                books:[],
                formValidate: {
                    bookname: '',
                    bookprice: '',
                    bookpublish: ''
                },
                ruleValidate: {
                    bookname: [
                        { required: true, message: '书名不能为空', trigger: 'blur' }
                    ],
                    bookprice: [
                        { required: true, message: '价格不能为空', trigger: 'blur' }
                    ],
                    bookpublish: [
                        { required: true, message: '出版社不能为空', trigger: 'blur' }
                    ]
                }
            }
        },
        created () {
            this.formValidate = this.$route.params.book;
        },
        methods: {
            handleSubmit (name) {
                var self = this;
                this.$refs[name].validate((valid) => {
                    if (valid) {
                        self.$http.post('http://localhost:9000/book/bookupdate',self.formValidate).then(response => {
                            var res = response.body;
                            if(res.code==1){
                                self.$Message.success('修改成功!');
                                self.$router.go(-1);
                            }else{
                                self.$Message.error(res.message);
                            }
                        }, response => {
                            self.$Message.error(response.body.message);
                        });
                    } 
                })
            },
            back () {
                this.$router.go(-1);
            }
        }
    }
</script>
<style scoped>
    .bookupdate{
        padding: 20px;
    }
</style>
