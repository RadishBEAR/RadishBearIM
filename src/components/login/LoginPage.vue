<template>
    <div class="container">
        <div id="wrapper" v-if="!isLogin" >
            <Login></Login>
        </div>
    </div>
</template>

<script>
    import Login from './login'
    export default {
        name: "LoginPage",
        components: {
            Login
        }
    }
</script>

<style scoped>
    .container{
        position:relative;
    }
    #wrapper {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        padding-top: 100px;
    }
</style>
