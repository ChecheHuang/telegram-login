<template>
  <div>
    <p>小飛機登入</p>
    <vue-telegram-login
      mode="callback"
      telegram-login="sporttok_bot"
      @callback="callback"
    />
  </div>
</template>
<script setup>
import vueTelegramLogin from "./vue-telegram-login.vue";
import CryptoJS from "crypto-js";
import axios from "axios";

const callback = (user) => {
  console.log(user);
  // alert(JSON.stringify(user));
  const setHeader = () => {
    function urlEncode(param, key, encode) {
      if (param == null) return "";
      let paramStr = "";
      const t = typeof param;
      if (t == "string" || t == "number" || t == "boolean") {
        paramStr += `&${key}=${
          encode == null || encode ? encodeURIComponent(param) : param
        }`;
      } else {
        for (const i in param) {
          const k =
            key == null
              ? i
              : key + (param instanceof Array ? `[${i}]` : `.${i}`);
          paramStr += urlEncode(param[i], k, encode);
        }
      }
      return paramStr;
    }
    let headers = {};
    const obj = {
      "device-id": "PC-jgl4cV3v0rtBEEFUUVLHV8ZDgzVuU9lf",
      "os-type": "0",
      timestamp: Date.parse(new Date()).toString(),
      version: "1.0",
    };
    for (const key in obj) {
      if (obj.hasOwnProperty(key)) {
        headers[key] = obj[key];
      }
    }
    const sign = CryptoJS.MD5(urlEncode(obj).substr(1) + "global").toString();
    headers["sign"] = sign;

    return headers;
  };

  axios
    .post(
      "https://www.sporttok-test.com/api/forehead/user/third/loginOrRegister",
      {
        loginType: 6,
        validateContent: {
          loginHash: user.hash,
        },
      },
      {
        headers: setHeader(),
      }
    )
    .then((response) => console.log(response.data))
    .catch((error) => console.error(error));
};
</script>
<!-- sportokTestVersion1bot -->
<!-- sporttok_bot -->
<!-- D8EBAC1F9C8E472BA9A62AD861F06090 -->
