<template>
    <div id="containerOfSide">
        <BusinessCard
                :title=this.userId
                subTitle="[您的账号]"
        ></BusinessCard>
        <GroudCard
                v-for="item in Grouds"
                :key="item"
                :title="item.title"
                :subTitle="item.subTitle"
                :id="item.conversationID"
                :redDot="item.redDot"
        ></GroudCard>
        <AddAndJoin></AddAndJoin>
    </div>
</template>

<script>
    import BusinessCard from '../Main/BusinessCard'
    import GroudCard from '../Main/GroudCard'
    import AddAndJoin from '../Main/AddAndJoin'
    import Vue from 'vue'
    // eslint-disable-next-line no-unused-vars
    import TIM from 'tim-js-sdk';
    import {EventBus} from "../../tools/EventBus";
    export default {
        name: "Side",
        components: {
            BusinessCard,
            GroudCard,
            AddAndJoin
        },
        data(){
            return{
                Grouds:[],
                refresh:1,
                userId:'',
                lastMessage:[],
                redDot:[]
            }
        },
        watch:{
          refresh(){
              console.log('状态改变');
              this.getGroups();
            }
        },
        mounted() {
            this.userId=Vue.prototype.ID;
            this.getConversationList();
            EventBus.$on('getMessage',(msg)=>{
                this.lastMessage[msg.conversationID]=msg.text;
                this.redDot[msg.conversationID]=true;
                for(var i=0;i<this.Grouds.length;i++){
                    console.log(this.Grouds[i].conversationID);
                    if(this.Grouds[i].conversationID===msg.conversationID){
                        this.Grouds[i].subTitle=msg.text;
                        this.Grouds[i].redDot=true;
                    }
                }
            });
            EventBus.$on('joinGroup',()=>{
                this.refresh*=-1;
            });
            EventBus.$on('createGroup',()=>{
                this.refresh*=-1;
            })
        },
        methods:{
            getGroups:function () {
                var that=this;
                    let promise = Vue.prototype.tim.getGroupList(TIM.TYPES.GRP_PROFILE_MEMBER_NUM);
                    promise.then(function(imResponse) {
                        console.log('获得组群成功！');
                        that.setGroups(imResponse.data.groupList);
                    }).catch(function(imError) {
                        console.warn('getGroupList error:', imError); // 获取群组列表失败的相关信息
                    });
            },
            setGroups:function (groups) {
                this.Grouds=[];
                let i = 0;
                for (;i< groups.length;i++){
                    // eslint-disable-next-line no-unused-vars
                    var sT='[暂无新消息]'
                    if(this.lastMessage['GROUP'+groups[i]['groupID']]!=null){
                        sT=this.lastMessage['GROUP'+groups[i]['groupID']]
                    }
                    var groud={
                        title:groups[i]['name'],
                        subTitle:sT,
                        conversationID:'GROUP'+groups[i]['groupID'],
                        redDot:false
                    };
                    this.Grouds.push(groud);
                }
            },
            getConversationList:function () {
                var that=this;
                let promiseFather = function() {
                    let promise = Vue.prototype.tim.getConversationList();
                    promise.then(function (imResponse) {
                        // eslint-disable-next-line no-unused-vars
                        const conversationList = imResponse.data.conversationList; // 会话列表，用该列表覆盖原有的会话列表
                        console.log("获取会话列表成功！");
                        that.refresh*=-1;
                        console.log(imResponse.data.conversationList)
                        var i=0;
                        for (;i<imResponse.data.conversationList.length;i++){
                            var k=imResponse.data.conversationList[i].groupProfile.groupID;
                            if(k!==null){
                                that.lastMessage['GROUP'+k]=imResponse.data.conversationList[i].lastMessage.messageForShow
                            }
                        }
                        console.log("你的列表成功！");
                    }).catch(function (imError) {
                        console.warn('getConversationList error:', imError); // 获取会话列表失败的相关信息
                    });
                };
                Vue.prototype.tim.on(TIM.EVENT.SDK_READY, promiseFather);
            },
            clickGroud:function (id) {
                alert(id);
            }
        }
    }
</script>

<style scoped>
#containerOfSide {
    height: 700px;
    width: 240px;
    background: #363e47;
}
</style>
