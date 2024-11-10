<script setup lang="ts">
const loginTelegram = () => {
  console.log("loginTelegram");
  window?.Telegram.Login.auth(
    { bot_id: "7946138503", request_access: "write", embed: 1 },
    (data) => {
      console.log(data, "这是回调数据"); //这里的data和之前返回的user数据和格式无差异
      if (!data) {
        //失败时你需要做的逻辑
      }
      //电报登录成功你需要做的逻辑（这里我是直接写了一个函数去调用登录成功后的业务逻辑）
    }
  );
};
</script>

<template>
  <div>vue-telegram-login2</div>
  <button @click="loginTelegram">电报登录</button>
</template>
