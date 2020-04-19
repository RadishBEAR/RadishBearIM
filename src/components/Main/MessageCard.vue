<template>
    <div style="width: 1060px;height: 100px;">
        <div v-if="this.left">
            <img :src="head" style="width: 70px;
        height: 70px;
        margin-top: 20px;
        float: left;
        margin-left: 20px;"/>
            <p style="color: #a5b5c1;
            position: relative;
            margin-top: 35px;
        float: left;
        margin-left: 10px;
        width:830px;
        text-align: left;
        font-size: 14px;
        margin-bottom: 0px;"
            >{{this.name}}</p>
            <p style="float: left;
            position: relative;
        margin-left: 10px;
        margin-top: 5px;
        color: #000000;
        width:850px;
        font-weight: 500;
        text-align: left;"
            >{{this.text}}</p>
        </div>
        <div v-if="this.right">
            <img :src="head" style="width: 70px;
        height: 70px;
        margin-top: 20px;
        float: right;
        margin-right: 20px;"/>
            <p style="color: #a5b5c1;
        float: right;
        margin-right: 10px;
        width:830px;
        text-align: right;
        margin-top: 35px;
        font-size: 14px;
        margin-bottom: 0px;"
            >{{this.name}}</p>
            <p style="float: right;
        margin-right: 10px;
        margin-top: 5px;
        color: #000000;
        width:850px;
        font-weight: 500;
        text-align: right;"
            >{{this.text}}</p>
        </div>
    </div>
</template>

<script>
    import HeadPortrait from '../../../src/assets/image/HeadPortrait0.png'
    export default {
        name: "MessageCard",
        props:{
          left:Boolean,
          right:Boolean,
            name:String,
            text:String
        },
        data(){
            return{
                head:HeadPortrait
            }
        }
    }
</script>

<style scoped>

</style>
