<script setup lang="ts">
/**
 * 兼容性处理：获取浏览器原生的 requestAnimationFrame 方法
 * 若浏览器不支持则降级使用 setTimeout 模拟（固定60FPS）
 */
 var rAF = (function () {
  return (
    window.requestAnimationFrame || // 标准方法
    window.webkitRequestAnimationFrame || // 兼容webkit内核浏览器
    function (callback) { // 兼容旧版本浏览器的降级方案
      window.setTimeout(callback, 1000 / 60); // 按60FPS（16.67ms/帧）的频率执行
    }
  );
})();

// 状态变量定义
var frame = 0;          // 用于统计最近1秒内的帧数
var allFrameCount = 0;  // 总帧数统计（可用于计算总平均FPS）
var lastTime = Date.now();      // 用于记录1秒统计周期的起始时间戳
var lastFameTime = Date.now();  // 用于记录上一帧的时间戳

/**
 * 主循环函数 - 通过递归调用持续计算帧率
 */
var loop = function () {
  var now = Date.now(); // 获取当前时间戳

  // 计算瞬时帧率（基于相邻两帧时间差）
  var fs = (now - lastFameTime); // 计算相邻帧时间差（单位：ms）
  var fps = Math.round(1000 / fs); // 根据时间差计算瞬时FPS（四舍五入）

  // 更新时间记录
  lastFameTime = now;    // 更新"上一帧"时间戳为当前帧
  allFrameCount++;       // 总帧数+1（此变量未被使用，可用于扩展统计）
  frame++;               // 当前统计周期内帧数+1

  /**
   * 每秒统计逻辑：当距离上次统计时间超过1秒时
   * 注意：由于JS定时器精度问题，实际间隔可能略大于1000ms
   */
  if (now > 1000 + lastTime) {
    // 计算过去1秒内的平均帧率（更准确的统计方式）
    var fps = Math.round((frame * 1000) / (now - lastTime)); // 计算精确的平均FPS

    // 输出统计结果（每秒触发一次）
    console.log(`${new Date()} 1S内 FPS：`, fps);

    // 重置统计状态
    frame = 0;          // 重置帧数计数器
    lastTime = now;     // 更新统计周期起始时间
  }

  // 递归调用：通过requestAnimationFrame保持循环运行
  rAF(loop);
}

// 启动帧率检测循环
loop();

/* 
 * 注意事项：
 * 1. 瞬时FPS（fps变量）与平均FPS（控制台输出的FPS）的区别：
 *    - 瞬时FPS反映相邻两帧之间的理论帧率
 *    - 平均FPS反映过去1秒内的实际平均帧率（更准确）
 * 2. allFrameCount变量当前未被使用，可用于统计总平均FPS（需配合总时长计算）
 * 3. 当页面处于后台时，部分浏览器会限制requestAnimationFrame的执行频率
 */
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
