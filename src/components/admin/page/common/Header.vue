<template>
    <div class="header">
        <div class="logo"><img src="/static/img/logo.png" style="width: 170px; margin: 10px"></div>
        <div class="user-info">
            <el-dropdown trigger="click" @command="handleCommand">
                <span class="el-dropdown-link">
                    <img class="user-logo" :src="img">
                    {{username}}
                </span>
                <el-dropdown-menu slot="dropdown">
                    <el-dropdown-item command="loginout">退出</el-dropdown-item>
                </el-dropdown-menu>
            </el-dropdown>
        </div>
    </div>
</template>
<script>
    import { getInfo } from '@/api/user/admin.js'
    export default {
        data() {
            return {
                name: '未命名',
                username: localStorage.getItem('admin_username'),
                img: '',
                loadingParam: {
                    token: localStorage.getItem('token')
                }
            }
        },
        created(){
            getInfo(this.loadingParam).then(response => {
                this.img = response.data.userInfo.img;
                if(response.data.userInfo.nickname){
                    this.username = response.data.userInfo.nickname;
                }
            });
        },
        methods:{
            handleCommand(command) {
                if(command == 'loginout'){
                    localStorage.removeItem('admin_username')
                    localStorage.removeItem('token')
                    this.$router.push('/login');
                }
            }
        }
    }
</script>
<style scoped>
    .header {
        position: relative;
        box-sizing: border-box;
        width: 100%;
        height: 70px;
        font-size: 22px;
        line-height: 70px;
        color: #fff;
        background: #16a085;
    }
    .header .logo{
        float: left;
        width:220px;
        text-align: center;
    }
    .user-info {
        float: right;
        padding-right: 50px;
        font-size: 16px;
        color: #fff;
    }
    .user-info .el-dropdown-link{
        position: relative;
        display: inline-block;
        padding-left: 50px;
        color: #fff;
        cursor: pointer;
        vertical-align: middle;
    }
    .user-info .user-logo{
        position: absolute;
        left:0;
        top:15px;
        width:40px;
        height:40px;
        border-radius: 50%;
    }
    .el-dropdown-menu__item{
        text-align: center;
    }
</style>
