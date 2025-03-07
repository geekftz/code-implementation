<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue'

// HASH ROUTER
// 定义 Router  
// class Router {  
//     constructor () {  
//         this.routes = {}; // 存放路由path及callback  
//         this.currentUrl = '';  
          
//         // 监听路由change调用相对应的路由回调  
//         window.addEventListener('load', this.refresh, false);  
//         window.addEventListener('hashchange', this.refresh, false);  
//     }  
      
//     route(path, callback){  
//         this.routes[path] = callback;  
//     }  
      
//     push(path) {  
//       // 更新 URL 的 hash，会自动触发 hashchange 事件
//       location.hash = path;
//       this.routes[path] && this.routes[path]() 
//     }  
// }  
  
// // 使用 router  
// window.miniRouter = new Router();  
// miniRouter.route('/', () => console.log('page1'))  
// miniRouter.route('/page2', () => console.log('page2'))  
// miniRouter.push('/page2') // page2  







// HISTORY ROUTER
// History API 路由实现
class Router {  
    // 存储路由路径和对应的回调函数
    constructor () {  
        this.routes = {};  // 路由映射表：path -> callback
        this.listerPopState()  // 监听浏览器前进/后退事件
    }  
      
    // 初始化路由状态，用于首次访问
    init(path) {  
        history.replaceState({path: path}, null, path);  // 替换当前历史记录
        this.routes[path] && this.routes[path]();  // 执行路由回调
    }  
      
    // 注册路由和对应的回调函数
    route(path, callback){  
        this.routes[path] = callback;
    }  
      
    // 路由跳转方法
    push(path) {  
        history.pushState({path: path}, null, path);  // 向历史记录添加新状态
        this.routes[path] && this.routes[path]();  // 执行路由回调
    }  
      
    // 监听浏览器前进/后退事件
    listerPopState () {  
        window.addEventListener('popstate' , e => {  
            const path = e.state && e.state.path;  // 获取历史记录中的路径
            this.routes[path] && this.routes[path]()  // 执行对应的路由回调
        })  
    }  
}  

// 使用示例
window.miniRouter = new Router();  // 创建路由实例
miniRouter.route('/', ()=> console.log('page1'))  // 注册首页路由
miniRouter.route('/page2', ()=> console.log('page2'))  // 注册页面2路由

  
// 跳转  
// miniRouter.push('/page2')  // page2  

</script>

<template>
  <div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
