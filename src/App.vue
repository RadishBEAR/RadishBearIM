<template>
  <div id="app">
      <component :is="content"/>
      <div class="bg"></div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import LoginPage from './components/login/LoginPage'
import MainPage from './components/Main/MainPage'
import { EventBus } from '../src/tools/EventBus'

export default {
  name: 'App',
    data(){
      return{
          content:'LoginPage',
      }
    },
  components: {
    // eslint-disable-next-line vue/no-unused-components
    HelloWorld,
      // eslint-disable-next-line vue/no-unused-components
    LoginPage,
      MainPage
  },
    mounted() {
        // eslint-disable-next-line no-unused-vars
        EventBus.$on('login',(msg)=>{
            this.$message({
                message: '登录成功！',
                type: 'success'
            });
            this.content='MainPage'
        })
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.bg {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    z-index: -1;
    background-image: linear-gradient( 135deg, #5EFCE8 10%, #736EFE 100%);
    background-size: cover;
}

</style>
