<!--
 * @Author: your name
 * @Date: 2022-03-16 13:18:32
 * @LastEditTime: 2022-03-18 21:03:22
 * @LastEditors: Please set LastEditors
 * @Description: 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
 * @FilePath: \LHZ\vue\src\views\dashboard\cart\detail\index.vue
-->
<template>
  <div>
      <div class="cart-header">
         <span>{{title}}</span>
           <svg t="1647408739949" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1596" width="16" height="16"><path d="M512 65.984C266.048 65.984 65.984 266.048 65.984 512S266.048 958.016 512 958.016 958.016 757.952 958.016 512 757.952 65.984 512 65.984zM544 736c0 17.696-14.304 32-32 32s-32-14.304-32-32l0-288c0-17.696 14.304-32 32-32s32 14.304 32 32L544 736zM512 352c-26.496 0-48-21.536-48-48C464 277.472 485.504 256 512 256s48 21.472 48 48C560 330.464 538.496 352 512 352z" p-id="1597"></path></svg>
      </div>
       <div class="cart-content">
           {{count}}
      </div>
       <div class="cart-charts">
          <slot name='charts'></slot>
      </div>
       <div class="cart-footer">
           <slot name='footer'></slot>

      </div>
  </div>
</template>

<script>
export default {
name:'detail',
props:['title','count'],
}
</script>

<style scoped>
.cart-header{
      display: flex;
      justify-content: space-between;
      color: #3d3333;
      font-size: 12px;
}
.cart-content{
      font-size: 26px;
      padding: 10px 0px;
}
.cart-charts{
      height: 50px;
}
.cart-footer{
    border-top: 1px solid #eee;
    padding-top:5px;
}
</style>