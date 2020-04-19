<template>
    <div @click="c">
        <img :src="head" style="width: 60px;height: 60px;margin-top: 20px;float: left;margin-left: 20px;"/>
        <p style="color: #ffffff;float: left;margin-left: 10px;width:130px;text-align: left;margin-top: 25px;font-weight: 600;margin-bottom: 0px;">{{this.title}}</p>
        <div v-if=this.redDot style="float: left;width: 10px;margin-top:25px;height: 10px;background: #ff6969;border-radius: 50%;"></div>
        <p style="float: left;margin-left: 10px;margin-top: 5px;color: #a5b5c1;width:150px;text-align: left;">{{this.subTitle}}</p>
    </div>
</template>

<script>
    import HeadPortrait from '../../../src/assets/image/HeadPortrait1.png'
    // eslint-disable-next-line no-unused-vars
    import Vue from 'vue'
    import {EventBus} from "../../tools/EventBus";
    export default {
        name: "BusinessCard",
        data(){
            return{
                head:HeadPortrait
            }
        },
        methods:{
            c:function () {
                var msg={
                    title:this.title,
                    id:this.id
                };
                EventBus.$emit('SelectGroup',msg);
                this.redDot=false
                console.log(Vue.prototype.messageList);
            }
        },
        props:{
            title:String,
            subTitle:String,
            id:String,
            redDot:{
                type:Boolean
            }
        }
    }
</script>

<style scoped>

</style>
