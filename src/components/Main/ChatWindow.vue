<template>
    <div id="ChatWindowMain">
        <div id="Title">
            <p
                    style="font-weight: 600;
                    float: left;
                    margin-left: 30px;
                    font-size: 20px;
                    margin-top: 10px;
                    color: #442438;
                    margin-bottom: 10px"
            >
                {{this.title}}&ensp;[{{this.id}}]
            </p>
        </div>
        <div style="width: 1060px;height: 2px;background-color: #e7e7e7"></div>
        <div style="width: 1060px;height: 486px;background-color: #ffffff;">
            <MessagePart></MessagePart>
        </div>
        <el-input
                type="textarea"
                :rows="2"
                placeholder="请输入内容"
                autosize
                style="position: relative;z-index: 0"
                v-model="textarea">
        </el-input>
        <el-button type="primary"
                   style="float: right;
                   z-index: 10;
                   position:relative;
                   margin-top: -50px;
                   margin-right: 15px;"
                   @click="sendMessage"
        >
            发送消息
        </el-button>
    </div>
</template>

<script>
    import Vue from 'vue'
    import TIM from 'tim-js-sdk';
    import MessagePart from '../Main/MessagePart'
    import {EventBus} from "../../tools/EventBus";
    export default {
        name: "Chat window",
        props:{
            title:String,
            id:String
        },
        components:{
            MessagePart
        },
        data(){
            return{
                textarea:''
            }
        },
        methods:{
            sendMessage:function () {
                var that=this;
                console.log(that.id);
                let message = Vue.prototype.tim.createTextMessage({
                    to: that.id,
                    conversationType: TIM.TYPES.CONV_GROUP,
                    payload: {
                        text:that.textarea
                    }
                });
                // 2. 发送消息
                let promise = Vue.prototype.tim.sendMessage(message);
                promise.then(function(imResponse) {
                    // 发送成功
                    console.log(imResponse);
                    var msg={
                        from:Vue.prototype.ID,
                        conversationID:that.id,
                        time:'',
                        text:that.textarea,
                        left:false,
                        right:true
                    };
                    if(Vue.prototype.messageList['GROUP'+that.id]==null){
                        Vue.prototype.messageList['GROUP'+that.id]=[]
                    }
                    Vue.prototype.messageList['GROUP'+that.id].push(msg);
                    that.textarea='';
                    EventBus.$emit('send');
                }).catch(function(imError) {
                    // 发送失败
                    console.warn('sendMessage error:', imError);
                });
            }
        }
    }
</script>

<style>
#ChatWindowMain{
    width: 1060px;
    height: 700px;
    background-color: #ffffff;
}
    #Title{
        width: 1060px;
        height: 50px;
        background-color: #ffffff;
    }
.el-textarea__inner{
    height: 162px!important;
}
</style>
