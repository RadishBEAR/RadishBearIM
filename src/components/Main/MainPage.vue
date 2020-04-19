<template>
    <div>
        <div id="container">
            <Side></Side>
            <ChatWindow
                    style="float: right;margin-top: -700px"
                    :title="groupTitle"
                    :id="groupID"
            ></ChatWindow>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'
    import Side from '../Main/Side'
    import ChatWindow from '../Main/ChatWindow'
    import TIM from 'tim-js-sdk';
    import {EventBus} from "../../tools/EventBus";
    export default {
        name: "MainPage",
        components:{
            Side,
            ChatWindow
        },
        data(){
            return{
                message:'这是主界面',
                groupTitle:'',
                groupID:'请选择聊天室'
            }
        },
        methods:{
            send(){
                var msg=this.message;
                let message = Vue.prototype.tim.createTextMessage({
                    to: '123',
                    conversationType: TIM.TYPES.CONV_C2C,
                    payload: {
                        text: msg
                    }
                });
                // 2. 发送消息
                let promise = Vue.prototype.tim.sendMessage(message);
                promise.then(function(imResponse) {
                    // 发送成功
                    console.log('发送成功'+msg);
                    console.log(imResponse);
                }).catch(function(imError) {
                    // 发送失败
                    console.warn('sendMessage error:', imError);
                });
            }
        },
        mounted() {
            var that=this;
            let onMessageReceived = function(event) {
                // event.data - 存储 Message 对象的数组 - [Message]
                that.message=event.data[0]['payload'].text;
                var msg={
                    from:event.data[0]['from'],
                    conversationID:event.data[0]['conversationID'],
                    time:event.data[0]['time'],
                    text:event.data[0]['payload'].text,
                    left:true,
                    right:false
                };
                if (Vue.prototype.messageList[event.data[0]['conversationID']]==null){
                    Vue.prototype.messageList[event.data[0]['conversationID']]=[]
                }
                Vue.prototype.messageList[event.data[0]['conversationID']].push(msg);
                console.log(Vue.prototype.messageList[event.data[0]['conversationID']]);
                EventBus.$emit('getMessage',msg);
            };
            Vue.prototype.tim.on(TIM.EVENT.MESSAGE_RECEIVED, onMessageReceived);
            EventBus.$on('SelectGroup',(msg)=>{
                this.groupTitle=msg.title;
                this.groupID=msg.id.replace('GROUP','');
            });
            EventBus.$on('createGroup',(msg)=>{
                this.groupTitle=msg.name;
                this.groupID=msg.id.replace('GROUP','');
            })
        }
    }

</script>

<style scoped>
#container{
    top:50%;
    left:50%;
    position:absolute;
    margin-left:-650px;
    margin-top:-350px;
    width: 1300px;
    height: 700px;
    background: #ffffff;
    box-shadow: 7px 7px 7px rgba(136, 136, 136, 0.7);
}
</style>
