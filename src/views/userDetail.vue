<template>
    <div class="user-detail-container">
        <div class="user-detail-header">
            <p>账号</p>
        </div>
        <div class="user-detail-box">
            <img class="user-detail-avatar" :src="profile.avatarUrl" alt="">
            <div class="user-detail-wrap">
                <p class="user-detail-nickname">{{profile.nickname}}</p>
                <p class="user-detail-signature">{{profile.signature}}</p>
            </div>
        </div>
        <ul class="user-detail-feature-box">
            <li class="user-detail-feature-item">
                <p class="user-detail-feature-text">动态</p>
                <p class="user-detail-feature-num">{{profile.eventCount}}</p>
            </li>
            <li class="user-detail-feature-item" @click="jump('follow')">
                <p class="user-detail-feature-text">关注</p>
                <p class="user-detail-feature-num">{{profile.followeds}}</p>
            </li>
            <li class="user-detail-feature-item" @click="jump('followed')">
                <p class="user-detail-feature-text">粉丝</p>
                <p class="user-detail-feature-num">{{profile.follows}}</p>
            </li>
            <li class="user-detail-feature-item">
                <p class="user-detail-feature-text">我的资料</p>
            </li>
        </ul>
        <ul class="user-detail-feature-list">
            <li class="user-detail-feature-item logout" @click="logout">
                退出登录
            </li>
        </ul>
    </div>
</template>
<script>
import { mapGetters } from 'vuex';
import { requestUserDetail, logout } from '../api';

export default {
    computed: {
        ...mapGetters(['userId']),
    },
    watch: {
        userId(nval) {
            if (nval) {
                this.requestUserDetail(nval);
            }
        },
    },
    data() {
        return {
            profile: {},
        };
    },
    methods: {
        requestUserDetail(userId) {
            requestUserDetail(userId).then((data) => {
                if (+data.code === 200) {
                    this.profile = data.profile;
                }
            });
        },
        logout() {
            logout().then((data) => {
                if (data.code === 200) {
                    this.$router.push('/login');
                }
            });
        },
        jump(router) {
            if (router) {
                this.$router.push(router);
            }
        },
    },
    mounted() {
        if (this.userId) {
            this.requestUserDetail(this.userId);
        }
    },
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import "../style/common.scss";

$borderColor: #eee;

.user-detail-container {
    background-color: #f7f5f5;
}

.user-detail-header {
    height: 40px;
    line-height: 40px;
    background-color: #fff;
    border-bottom: 1px solid $borderColor;
    p {
        text-align: center;
        color: #000;
    }
}

.user-detail-box {
    display: flex;
    padding: 10px;
    align-items: center;
    border-bottom: 1px solid $borderColor;
    background-color: #fff;
    .user-detail-avatar {
        display: block;
        width: 60px;
        height: 60px;
        border: 1px solid $borderColor;
        border-radius: 50%;
        margin-right: 10px;
    }
    .user-detail-nickname {
        margin-bottom: 5px;
        font-size: 18px;
        font-weight: bold;
    }
}

.user-detail-feature-box {
    display: flex;
    margin-bottom: 10px;
    background-color: #fff;
    .user-detail-feature-item {
        position: relative;
        width: 25%;
        height: 50px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        &::after {
            content: "";
            position: absolute;
            right: 0;
            top: 10px;
            width: 1px;
            height: 30px;
            background-color: $borderColor;
        }
        .user-detail-feature-text {
            text-align: center;
            font-size: 14px;
            color: #999;
            margin-bottom: 5px;
        }
        .user-detail-feature-num {
            text-align: center;
            font-size: 12px;
            color: #000;
            font-weight: bold;
        }
    }
}

.user-detail-feature-list {
    background-color: #fff;
    .user-detail-feature-item {
        text-align: center;
        height: 40px;
        line-height: 40px;
    }
    .logout {
        color: $main_color;
        border-top: 1px solid $borderColor;
        border-bottom: 1px solid $borderColor;
    }
}
</style>
