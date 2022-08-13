<template>
    <div class="login_wrap">
        <el-form class="login_form" label-position="right" label-width="80px" :model="userinfo" :rules="loginRules"
            ref="loginForm">
            <!-- <h1>用户登录</h1> -->
            <el-form-item label="用户名" prop="username">
                <el-input v-model="userinfo.username"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="password">
                <el-input v-model="userinfo.password" type="password"></el-input>
            </el-form-item>
            <!-- <el-button type="primary" class="login_btn" @click.prevent="handleLogin">登录</el-button> -->
            <el-button class="login_btn" @click.prevent="handleLogin">登录</el-button>
        </el-form>
    </div>
</template>

<script>
//引入登录接口
import { loginRequest } from "@/api/user";
import {
    getToken,
    setToken,
    removeToken
} from '@/utils/auth'
export default {
    name: "login",
    data () {
        return {
            userinfo: {
                username: "admin",
                password: "123456"
            },
            loginRules: {
                username: [
                    { required: true, message: "请输入用户名", trigger: "blur" },
                    { min: 5, max: 20, message: "长度在 5 到 20个字符", trigger: "blur" }
                ],
                password: [
                    { required: true, message: "请输入密码", trigger: "blur" },
                    { min: 5, max: 60, message: "长度在 5 到 20 个字符", trigger: "blur" }
                ]
            }
        };
    },
    mounted () {
    },
    methods: {
        // 第一种
        // async handleLogin () {
        //     /**
        //      * 调用登录接口
        //      * 成功：
        //      *     保存token
        //      *     跳转到首页，并给出成功的提示
        //      * 失败：给出错误提示，让用户重新登录
        //      */
        //     const res = await loginRequest(this.userinfo);
        //     // console.log(res.data.data, 'ressss')
        //     const {
        //         meta: { msg, status }
        //     } = res.data;
        //     // const status = res.data.meta.status;
        //     // const msg = res.data.meta.msg;
        //     if (status === 200) {
        //         // const { token } = res.data.data.token;
        //         const token = res.data.data.token;
        //         // console.log(token, 'tokennnn');
        //         localStorage.setItem("token", token);
        //         //记住上次没有token要访问的页面地址，如果登录成功，再返回到上次要访问到页面
        //         const { redirect } = this.$route.query;
        //         console.log(this.$route, 'toute');
        //         // console.log(redirect, 'redirect跳转首页路由地址');
        //         //如果直接登录，没有redirect,成功后直接跳转到home 本路由首页打印this.$route.query;是这个Dashboard
        //         if (!redirect) {
        //             // this.$router.push({ name: "Home" });
        //             this.$router.push({ path: '/dashboard ' });
        //             // console.log(redirect, '上面这个写跳转首页路由地址');
        //         } else {
        //             // this.$router.push({ path: redirect });
        //             this.$router.push({ path: this.redirect || '/' })
        //         }
        //         this.$message({
        //             message: msg,
        //             type: "success"
        //         });
        //     } else {
        //         this.$message({
        //             message: msg,
        //             type: "error"
        //         });
        //     }
        // }
        // 第二种
        // (111)用这个
        // handleLogin () {
        //     this.$refs.loginForm.validate(valid => {
        //         if (valid) {
        //             this.loading = true;
        //             this.$store.dispatch('user/login', this.userinfo)
        //                 .then((res) => {
        //                     const { msg, status } = res.meta
        //                     if (status === 200) {
        //                         this.loading = false
        //                         this.$router.push({ path: this.redirect || '/' })
        //                         const token = res.data.token
        //                         this.$store.commit('user/SET_TOKEN', token)
        //                         // console.log(this.$store.getters.token, 'toiken');
        //                         setToken(token)
        //                         this.$message({
        //                             message: msg,
        //                             type: "success"
        //                         });
        //                     } else {
        //                         this.$message({
        //                             message: msg,
        //                             type: "error"
        //                         });
        //                     }
        //                 }).catch(() => {
        //                     this.loading = false;
        //                 })
        //         } else {
        //             console.log(333);
        //             return false
        //         }
        //     })
        // },
        // (222)用这个 // 最简练的方法 登录错误提示还有问题上面那个完整
        handleLogin () {
            this.$refs.loginForm.validate(valid => {
                if (valid) {
                    this.loading = true;
                    this.$store.dispatch('user/login', this.userinfo)
                        .then((res) => {
                            // console.log(res, 'chenggongle ');
                            //登录成功之后重定向到首页
                            this.loading = false
                            this.$router.push({ path: this.redirect || '/' })
                        }).catch((err) => {
                            console.log(err, 'login ----- err');
                            this.loading = false
                            // this.$message({
                            //     message: err,
                            //     type: "error"
                            // });
                        })
                } else {
                    // console.log(333);
                    return false
                }
            })
        },
    }
};
</script>

<style lang="scss" scoped>
.login_wrap {
    width: 100%;
    height: 100%;
    // background: #072765;
}

.login_form {
    width: 40%;
    height: 260px;
    padding: 30px;
    background: #fff;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: auto;

    h1 {
        text-align: center;
        margin: 10px 0;
    }
}

.login_btn {
    width: 100%;
}
</style>

