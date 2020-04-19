<template>
    <div class="login-wrapper">
        <img :src="logo" width="100" height="100" style="border-radius: 5px;margin-bottom:12px;margin-top: 30px;"/>
<!--        <el-select v-model="userID" style="width:80%;">-->
<!--            <el-option v-for="user in users" :key="user" :label="user" :value="user"></el-option>-->
<!--        </el-select>-->
        <el-input v-model="userID" style="width:80%;"></el-input>
        <br>
        <el-button type="primary" @click="login" style="width:80%;margin-bottom:30px;">登录</el-button>
    </div>
</template>

<script>
    import { Select, Option } from 'element-ui'
    import logo from '../../assets/image/logo.png'
    import Vue from 'vue'
    import LibGenerateTestUserSig from '../../tools/lib-generate-test-usersig.min'
    import { EventBus } from '../../tools/EventBus'
    export default {
        name: 'Login',
        components: {
            // eslint-disable-next-line vue/no-unused-components
            ElSelect: Select,
            // eslint-disable-next-line vue/no-unused-components
            ElOption: Option
        },
        data() {
            return {
                userID: '',
                logo: logo,
                users: ['爱吃萝卜的熊','提拉米猪','伏特加猫','大白鹅']
            }
        },
        methods: {
            login() {
                // this.$store.dispatch('login', this.userID)
                var userSig = this.genTestUserSig(this.userID).userSig;
                console.log(userSig);
                let promise = Vue.prototype.tim.login({
                    userID: this.userID,
                    userSig: userSig
                });
                var userID=this.userID;
                Vue.prototype.ID=this.userID;
                // eslint-disable-next-line no-unused-vars
                promise.then(function (imResponse) {
                    console.log('登录成功！');
                    EventBus.$emit('login',userID);
                }).catch(function (imError) {
                    console.warn('login error:', imError); // 登录失败的相关信息
                    console.log('登录失败！');
                });
            },
            genTestUserSig (userID) {
                /**
                 * 腾讯云 SDKAppId，需要替换为您自己账号下的 SDKAppId。
                 *
                 * 进入腾讯云实时音视频[控制台](https://console.cloud.tencent.com/rav ) 创建应用，即可看到 SDKAppId，
                 * 它是腾讯云用于区分客户的唯一标识。
                 */
                const SDKAPPID = 1400353749;
                /**
                 * 签名过期时间，建议不要设置的过短
                 * <p>
                 * 时间单位：秒
                 * 默认时间：7 x 24 x 60 x 60 = 604800 = 7 天
                 */
                const EXPIRETIME = 604800;
                /**
                 * 计算签名用的加密密钥，获取步骤如下：
                 *
                 * step1. 进入腾讯云实时音视频[控制台](https://console.cloud.tencent.com/rav )，如果还没有应用就创建一个，
                 * step2. 单击“应用配置”进入基础配置页面，并进一步找到“帐号体系集成”部分。
                 * step3. 点击“查看密钥”按钮，就可以看到计算 UserSig 使用的加密的密钥了，请将其拷贝并复制到如下的变量中
                 *
                 * 注意：该方案仅适用于调试Demo，正式上线前请将 UserSig 计算代码和密钥迁移到您的后台服务器上，以避免加密密钥泄露导致的流量盗用。
                 * 文档：https://cloud.tencent.com/document/product/647/17275#Server
                 */
                const SECRETKEY = "cd14eaa381e5708e4d70bb91e0189486d8b8378d54111644827d06742e9e5d4d";

                // a soft reminder to guide developer to configure sdkAppId/secretKey
                if (SDKAPPID === "" || SECRETKEY === "") {
                    alert(
                        "请先配置好您的账号信息： SDKAPPID 及 SECRETKEY " +
                        "\r\n\r\nPlease configure your SDKAPPID/SECRETKEY in js/debug/GenerateTestUserSig.js"
                    );
                }
                const generator = new LibGenerateTestUserSig(SDKAPPID, SECRETKEY, EXPIRETIME);
                const userSig = generator.genTestUserSig(userID);
                return {
                    sdkAppId: SDKAPPID,
                    userSig: userSig
                };
            }
        }
    }
</script>

<style lang="stylus" scoped>
    .login-wrapper {
        display: flex;
        align-items: center;
        flex-direction: column;
        padding: 24px;
        background: #ffffff;
        color: $black;
        border-radius: 5px;
        box-shadow: 0 11px 20px 0 rgba(0, 0, 0, .3);
    }
</style>
