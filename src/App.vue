<template>
    <div id="app-box" class="container" :data-theme="currentTheme">
        <header class="header" data-tauri-drag-region>
            <div class="header-left">
                <div class="logo">
                    <img src="../public/yingge.ico" alt="莺歌" v-if="currentTheme === 'light'">
                    <img class="logo-dark" src="../public/yingge.ico" alt="莺歌" v-else>
                </div>

            </div>
            <div class="header-right">
                <button class="app-button setting-but" @click="toggleTheme()">
                    <i class="icon">
                        <img src="./assets/icons/svg/app_sunny.svg" alt="亮色模式" v-if="currentTheme === 'dark'">
                        <img src="./assets/icons/svg/app_dark.svg" alt="暗色模式" v-else>
                    </i>
                </button>
                <button class="app-button setting-but">
                    <i class="icon">
                        <img src="./assets/icons/svg/user.svg" alt="应用设置" v-if="currentTheme === 'dark'">
                        <img src="./assets/icons/svg/user_dark.svg" alt="应用设置" v-else>
                    </i>
                </button>
                <button class="app-button setting-but">
                    <i class="icon">
                        <img src="./assets/icons/svg/setting.svg" alt="应用设置" v-if="currentTheme === 'dark'">
                        <img src="./assets/icons/svg/setting_dark.svg" alt="应用设置" v-else>
                    </i>
                </button>
                <div class="separated">|</div>
                <button class="app-button window-but" @click="minimized()">
                    <i class="icon"><img src="./assets/icons/svg/window_min.svg" alt="最小化窗口"></i>
                </button>
                <button class="app-button window-but" @click="toggleMaximize">
                    <i class="icon" v-if="true"><img src="./assets/icons/svg/window_max.svg" alt="最大化窗口"></i>
                    <i class="icon" v-else><img src="./assets/icons/svg/window_formalization.svg" alt="窗口化窗口"></i>
                </button>
                <button class="app-button window-but" @click="closeWindow">
                    <i class="icon"><img src="./assets/icons/svg/window_close.svg" alt="关闭窗口"></i>
                </button>
            </div>
        </header>
        <main></main>
    </div>
</template>

<script setup lang="ts">
import {getCurrentWindow} from '@tauri-apps/api/window';
import {onMounted, ref} from "vue";

// 主题控制
const currentTheme = ref("light")

const toggleTheme = () => {
    currentTheme.value = currentTheme.value === "dark" ? "light" : "dark";
    localStorage.setItem("theme", currentTheme.value);
}
// window 控制
const appWindow = getCurrentWindow();

const minimized = async () => await appWindow.minimize();
const toggleMaximize = async () => await appWindow.toggleMaximize();

const closeWindow = async () => await appWindow.close();

//
onMounted(() => {
    // 主题控制 获取本地偏好
    let savedTheme = localStorage.getItem("theme")
    if (savedTheme) {
        currentTheme.value = savedTheme;
    }
})
</script>

<style scoped lang="scss">
@use "sass:color";

/*基础样式*/
@use "./assets/style/_base.scss" as *;
@use "./assets/style/_mixins.scss" as *;
@use "./assets/style/_variables.scss" as *;
/**/
@use "./assets/style/main" as *;

.logo {
  img.logo-dark {
    filter: invert(1);
  }
}

.app-button.window-but {
  background-color: transparent;
  margin-left: 8px;
  @include icon(16px);
}

.app-button.setting-but {
  width: 32px;
  height: 32px;
  background-color: transparent;
  display: flex;
  justify-content: center;
  align-items: center;
  @include icon(20px);
  transition: all 0.3s ease-in-out;

  &:hover {
    background-color: var(--accent);
  }
}

.separated {
  display: flex;
  align-items: center;
  margin: 0 16px;
  font-size: 16px;
  color: var(--secondary);
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  height: 48px;
  user-select: none;
  padding: 0 8px;
  
  .header-left {
    display: flex;
  }
  
  .header-right {
    display: flex;
  }
}

</style>

<!--默认样式去除-->
<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-size: 14px;
}

a {
    text-decoration: none;
    color: inherit;
}

a:hover, a:focus, a:active, a:hover {
    color: #333;
}

ul, ol {
    list-style: none;
}

img {
    vertical-align: top;
    border: none;
}

button {
    border: 0;
    background: none;
    outline: none;
    appearance: none;
    -webkit-appearance: none;
}

section {
    border: none;
    appearance: none;
    -moz-appearance: none;
    -webkit-appearance: none;
}

h1, h2, h3, h4, h5, h6 {
    font-weight: normal;
}

input, textarea, select {
    outline: none;
    border: none;
    background: none;
}

textarea {
    resize: none;
    overflow: auto;
}

table {
    border-collapse: collapse;
    border-spacing: 0;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    appearance: none;
    margin: 0;
}

input {
    appearance: textfield;
    -webkit-appearance: textfield;
    -moz-appearance: textfield;
}

</style>