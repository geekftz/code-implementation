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
  
// // miniRouter.push('/') // page1  
// miniRouter.push('/page2') // page2  


// HISTORY ROUTER
// 定义 Router  
class Router {  
    constructor () {  
        this.routes = {};  
        this.listerPopState()  
    }  
      
    init(path) {  
        history.replaceState({path: path}, null, path);  
        this.routes[path] && this.routes[path]();  
    }  
      
    route(path, callback){  
        this.routes[path] = callback;  
    }  
      
    push(path) {  
        history.pushState({path: path}, null, path);  
        this.routes[path] && this.routes[path]();  
    }  
      
    listerPopState () {  
        window.addEventListener('popstate' , e => {  
            const path = e.state && e.state.path;  
            this.routers[path] && this.routers[path]()  
        })  
    }  
}  
  
// 使用 Router  
  
window.miniRouter = new Router();  
miniRouter.route('/', ()=> console.log('page1'))  
miniRouter.route('/page2', ()=> console.log('page2'))  
  
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
