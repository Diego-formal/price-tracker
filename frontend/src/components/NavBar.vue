<template>
    <nav class="navbar">
        <div class="title"> <RouterLink to="/overview">價格追蹤小幫手</RouterLink></div>

        <button
            class="hamburger"
            type="button"
            aria-label="選單"
            aria-controls="navbar-menu"
            :aria-expanded="isMenuOpen ? 'true' : 'false'"
            :class="{ active: isMenuOpen }"
            @click="toggleMenu"
        >
            <span class="bar"></span>
            <span class="bar"></span>
            <span class="bar"></span>
        </button>

        <ul id="navbar-menu" class="options" :class="{ open: isMenuOpen }">
            <li><RouterLink to="/overview">物價概覽</RouterLink></li>
            <li><RouterLink to="/trending">物價趨勢</RouterLink></li>
            <li><RouterLink to="/news">相關新聞</RouterLink></li>
            <li v-if="!isLoggedIn"><RouterLink to="/login">登入</RouterLink></li>
            <li v-else @click="logout">Hi, {{getUserName}}! 登出</li>
            <li class="author">{{ authorEmail }}</li>
        </ul>
    </nav>
</template>

<script>
import { useAuthStore } from '@/stores/auth';

export default {
    name: 'NavBar',
    data() {
        return {
            authorEmail: 'aa0963666696@gmail.com',
            isMenuOpen: false
        };
    },
    computed: {
        isLoggedIn(){
            const userStore = useAuthStore();
            return userStore.isLoggedIn;
        },
        getUserName(){
            const userStore = useAuthStore();
            return userStore.getUserName;
        }
    },
    watch: {
        // 換頁後把手機版選單收起來
        $route(){
            this.isMenuOpen = false;
        }
    },
    methods: {
        toggleMenu(){
            this.isMenuOpen = !this.isMenuOpen;
        },
        logout(){
            const userStore = useAuthStore();
            userStore.logout();
            this.isMenuOpen = false;
        }
    }
};
</script>

<style scoped>
.navbar {
    position: relative;
    display: flex;
    justify-content: space-between;
    background-color: #f3f3f3;
    padding: 1.5em;
    height: 4.5em;
    width: 100%;
    box-sizing: border-box;
    align-items: center;
    box-shadow: 0 0 5px #000000;
}

.navbar ul {
    list-style: none;
    display: flex;
    justify-content: space-around;
    margin: 0;
    padding: 0;
}

.title > a{
    font-size: 1.4em;
    font-weight: bold;
    color: #2c3e50 !important;
}

.navbar li {
    color: #575B5D;
    margin: 0 .5em;
    font-size: 1.2em;
}

.navbar li.author {
    color: #8a8f92;
    font-size: .95em;
}

.navbar li.author:hover{
    font-weight: normal;
    cursor: default;
}

.navbar li:hover{
    cursor: pointer;
    font-weight: bold;
}

.navbar a {
    text-decoration: none;
    color: #575B5D;
}

/* 漢堡按鈕:桌面版藏起來 */
.hamburger {
    display: none;
    flex-direction: column;
    justify-content: space-between;
    width: 1.9em;
    height: 1.4em;
    padding: 0;
    border: none;
    background: none;
    cursor: pointer;
}

.hamburger .bar {
    display: block;
    width: 100%;
    height: 3px;
    border-radius: 2px;
    background-color: #575B5D;
    transition: transform .25s ease, opacity .25s ease;
}

/* 打開時三條變 X */
.hamburger.active .bar:nth-child(1) {
    transform: translateY(calc(0.7em - 1.5px)) rotate(45deg);
}

.hamburger.active .bar:nth-child(2) {
    opacity: 0;
}

.hamburger.active .bar:nth-child(3) {
    transform: translateY(calc(-0.7em + 1.5px)) rotate(-45deg);
}

/* ---- 手機版:768px 為分界點 ---- */
@media (max-width: 768px) {
    .hamburger {
        display: flex;
    }

    /* 用 .navbar .options 才蓋得過上面的 .navbar ul */
    .navbar .options {
        display: none;
        position: absolute;
        top: 100%;
        left: 0;
        right: 0;
        flex-direction: column;
        align-items: flex-start;
        background-color: #f3f3f3;
        padding: .5em 0;
        box-shadow: 0 5px 5px -3px #000000;
    }

    .navbar .options.open {
        display: flex;
    }

    .navbar li {
        width: 100%;
        margin: 0;
        padding: .7em 1.5em;
        box-sizing: border-box;
        text-align: left;
    }

    .navbar li a {
        display: block;
    }

    .title > a {
        font-size: 1.2em;
    }
}
</style>
