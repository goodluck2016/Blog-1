<template>
    <div class="fk-side ps-container" id="fk-side">
      <div class="mod">
        <div class="mod-logo">
          <h1><a href="/">{{title}}</a></h1>
        </div>
      </div>
      <ul class="mod-bar" style="marginTop: 10">
        <input type="hidden" id="hide_values" val="0" />
          <li v-for="route in routes">
            <a v-if="route.children" v-link="{ path: route.children[0].url }" @click="go(route.children[0].url)" class="{{getClassName(route.icon, route.url)}}">
              <span>{{route.title}}</span>
            </a>
            <a v-else v-link="{ path: route.url }" @click="go(route.url)" class="{{getClassName(route.icon, route.url)}}">
              <span>{{route.title}}</span>
            </a>
            <ul v-if="route.children" :style="{ 'height': getHeight(route)  }">
                <li v-for="child in route.children">
                  <a v-link="{ path: child.url }" @click="go(child.url)"  class="{{getSubLinkClassName(child.url)}}">
                    <span>{{child.title}}</span>
                  </a>
                </li>
            </ul>
          </li>
      </ul>
    </div>
</template>

<script>
/* eslint-disable */
import classnames from 'classnames';
import store from '../store/index';

export default {
  props:{
    shouldTipShow: Boolean,
    tipType: String,
    tipInfo: String,
    currentRoute: String,
  },
  data () {
    return {
      title: '',
      routes: [
        {url: '/dashboard', icon: 'home', title:'概述'},
        {url: '/post', icon: 'topic', title: '文章管理', children: [
          {url: '/post/list', title: '文章列表'},
          {url: '/post/create', title: '添加文章'}
        ]},
        {url: '/page', icon: 'reply', title: '页面管理', type: 1, children: [
          {url: '/page/list', title: '页面列表'},
          {url: '/page/create', title: '添加页面'}
        ]},
        {url: '/cate', icon: 'report', title: '分类管理', type: 1, children: [
          {url: '/cate/list', title: '分类列表'},
          {url: '/cate/create', title: '添加分类'}
        ]},
        {url: '/tag', icon: 'report', title: '标签管理', type: 1, children: [
          {url: '/tag/list', title: '标签列表'},
          {url: '/tag/create', title: '添加标签'}
        ]},
        {url: '/update', icon: 'report', title: '更新管理', type: 1, children: [
          {url: '/update/list', title: '版本列表'},
          {url: '/update/create', title: '添加更新'}
        ]},
        {url: '/user', icon: 'user', title: '用户管理', type: 1, children: [
          {url: '/user/list', title: '用户设置'},
        ]},
        {url: '/option', icon: 'setting', title: '系统设置', type: 1, children: [
          {url: '/option/general', title: '基本设置'},
          {url: '/option/comment', title: '评论设置'},
          {url: '/option/analytic', title: '统计代码'},
        ]}
      ],

    }
  },
  ready(){
    if (this.currentRoute != this.$route.path)
      this.currentRoute = this.$route.path;


    store.fetchOptionByJSON(this, { key: 'title' }).then(result=>{
      this.title = result[0].value || '';
    })

  },
  route: {
    data({ to }){
      return {

      };
    }
  },
  methods: {
    isActive(routeUrl){
      return this.currentRoute.indexOf(routeUrl) > -1;
    },
    getHeight(route, routeUrl){
      let height = 49*( this.isActive(route.url) ? route.children.length : 0);
      return height+'px';
    },
    getClassName(icon, routeUrl){
      let active = this.isActive(routeUrl);
      return classnames({
        icon: true,
        [`icon-${icon}`]: true,
        active: active
      })
    },
    getSubLinkClassName(routeUrl){
      return classnames({
        active: this.isActive(routeUrl)
      })
    },
    go(routeUrl){
      // this.currentRoute = routeUrl;
    }
  }

}
</script>

<!-- Add "scoped" attribute to limi t CSS to this component only -->
<style scoped>
h1 {
  color: #42b983;
}
</style>
