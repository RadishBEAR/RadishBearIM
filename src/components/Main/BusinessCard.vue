<template>
    <div>
        <img :src="head" style="width: 60px;height: 60px;margin-top: 20px;float: left;margin-left: 20px;"/>
        <p style="width:150px;text-align: left;color: #ffffff;float: left;margin-left: 10px;margin-top: 25px;font-weight: 600;margin-bottom: 0px;">{{this.title}}</p>
        <p style="width:150px;text-align: left;float: left;margin-left: 10px;margin-top: 5px;color: #a5b5c1">{{this.subTitle}}</p>
    </div>
</template>

<script>
    import HeadPortrait from '../../../src/assets/image/HeadPortrait0.png'
    export default {
        name: "BusinessCard",
        data(){
            return{
                head:HeadPortrait
            }
        },
        props:{
            title:String,
            subTitle:String
        }
    }
</script>

<style scoped>

</style>
