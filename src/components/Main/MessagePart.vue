<template>
    <div style="width: 1060px;height: 486px;">
        <happy-scroll color="rgba(0,0,0,0.5)" size="10" :scroll-top=height>
            <div>
                <MessageCard
                        v-for="item in this.group"
                        :key="item"
                        :left=item.left
                        :right=item.right
                        :text=item.text
                        :name=item.from
                />
                <div style="width: 1060px;height: 150px;"></div>
            </div>
        </happy-scroll>
    </div>
</template>

<script>
    // eslint-disable-next-line no-unused-vars
    import Vue from 'vue'
    import {EventBus} from "../../tools/EventBus";
    import MessageCard from '../Main/MessageCard'
    export default {
        name: "MessagePart",
        components:{
            MessageCard
        },
        data(){
            return{
                id:'',
                group:[],
                height:5400
            }
        },
        mounted() {
            EventBus.$on('SelectGroup',(msg)=>{
                this.id=msg.id;
                this.group=Vue.prototype.messageList[msg.id]
            });
            EventBus.$on('createGroup',(msg)=>{
                this.id=msg.id;
                Vue.prototype.messageList[msg.id]=[]
                this.group=Vue.prototype.messageList[msg.id]
            });
            EventBus.$on('send',()=>{
                this.height+=300;
            });
            EventBus.$on('getMessage',()=>{
                this.height+=300;
            });
        }
    }
</script>

<style scoped>

</style>
