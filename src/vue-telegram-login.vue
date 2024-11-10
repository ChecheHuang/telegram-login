<template>
  <div ref="telegram" class="test"></div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "vue-telegram-login",
  props: {
    mode: {
      type: String,
      required: true,
      validator(value) {
        return ["callback", "redirect"].includes(value);
      },
    },
    telegramLogin: {
      type: String,
      required: true,
      validator(value) {
        return value.endsWith("bot") || value.endsWith("Bot");
      },
    },
    redirectUrl: {
      type: String,
      default: "",
    },
    requestAccess: {
      type: String,
      default: "write",
      validator(value) {
        return ["read", "write"].includes(value);
      },
    },
    size: {
      type: String,
      default: "large",
      validator(value) {
        return ["small", "medium", "large"].includes(value);
      },
    },
    userpic: {
      type: Boolean,
      default: true,
    },
    radius: {
      type: String,
    },
  },
  setup(props, { emit }) {
    const telegram = ref(null);

    const onTelegramAuth = (user) => {
      emit("callback", user);
    };

    onMounted(() => {
      const script = document.createElement("script");
      script.async = true;
      script.src = "https://telegram.org/js/telegram-widget.js?14";
      script.setAttribute("data-size", props.size);
      script.setAttribute("data-userpic", props.userpic);
      script.setAttribute("data-telegram-login", props.telegramLogin);
      script.setAttribute("data-request-access", props.requestAccess);
      if (props.radius) {
        script.setAttribute("data-radius", props.radius);
      }
      if (props.mode === "callback") {
        window.onTelegramAuth = onTelegramAuth;
        script.setAttribute("data-onauth", "window.onTelegramAuth(user)");
      } else {
        script.setAttribute("data-auth-url", props.redirectUrl);
      }
      telegram.value.appendChild(script);
    });

    return {
      telegram,
    };
  },
};
</script>

<style lang="less">
.test {
  //这里可以自己调整外框样子
  height: 40px;
  width: 100px;
  overflow: hidden;
  color: greenyellow;
  border: 10px solid red;
}
</style>
<style>
/* //这里可以在控制台找到Telegram的dom结构和样式，自行修改Telegram呈现的样式 */

#telegram-login-sporttok_bot {
  /* opacity: 0; */
  /* pointer-events: auto; */
}
</style>
