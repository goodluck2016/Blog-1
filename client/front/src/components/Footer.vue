<template>
    <footer id="footer" class="inner">
         &copy; 2016&nbsp;-&nbsp; {{ siteInfo.title.value }} &nbsp;-&nbsp;
         <a target=_blank href="https://github.com/smallpath/blog">博客源码</a>
         <br> Powered by&nbsp;
         <a target=_blank href="https://github.com/vuejs/vue">Vue2</a>
         &nbsp;&amp;&nbsp;
         <a target=_blank href="https://github.com/koajs/koa/tree/v2.x">Koa2</a>        
    </footer>
</template>

<script>
/* eslint-disable */
import store from '../store/api';

export default {
  data () {
    const isInitialRender = !this.$root._isMounted
    return {
        siteInfo:  this.$store.getters.siteInfo,
    }
  },

}
</script>