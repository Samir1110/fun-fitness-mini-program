<template>
    <view>
        <!-- 个人信息展示部分 -->
        <view v-if="users.length" class="table">
            <!-- 表头 -->
            <view class="table-header">
                <view class="table-cell-header">信息目录</view>
                <view class="table-cell-header">个人信息</view>
            </view>
            <!-- 数据行 -->
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">账号</view>
                <view class="table-cell">{{ user.memberAccount }}</view>
            </view>
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">姓名</view>
                <view class="table-cell">{{ user.memberName }}</view>
            </view>
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">性别</view>
                <view class="table-cell">{{ user.memberGender }}</view>
            </view>
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">年龄</view>
                <view class="table-cell">{{ user.memberAge }}</view>
            </view>
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">身高(cm)</view>
                <view class="table-cell">{{ user.memberHeight }}</view>
            </view>
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">体重(kg)</view>
                <view class="table-cell">{{ user.memberWeight }}</view>
            </view>
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">电话</view>
                <view class="table-cell">{{ user.memberPhone }}</view>
            </view>
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">办卡时间</view>
                <view class="table-cell">{{ user.cardTime }}</view>
            </view>
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">购买课时</view>
                <view class="table-cell">{{ user.cardClass }}</view>
            </view>
            <view v-for="(user, index) in users" :key="index" class="table-row">
                <view class="table-cell">剩余课时</view>
                <view class="table-cell">{{ user.cardNextClass }}</view>
            </view>
			<view v-for="(user, index) in users" :key="index" class="table-row">
			    <view class="table-cell">积分</view>
			    <view class="table-cell">{{ user.memberCredit }}</view>
			</view>
        </view>
        <view v-else>
            <text>没有找到用户信息</text>
        </view>

        <!-- 跳转到修改信息页面按钮 -->
        <button @click="navigateToUpdateInfo">修改个人信息</button>
    </view>
</template>

<script>
export default {
    data() {
        return {
            users: [],
            memberAccount: uni.getStorageSync('memberAccount') // 从存储中获取账号信息
        };
    },
    onLoad() {
        this.loadUserInfo();
    },
	onShow() {
	    this.loadUserInfo();
	},
    methods: {
        loadUserInfo() {
            if (!this.memberAccount) {
                uni.showToast({ title: '请先登录', icon: 'none' });
                return;
            }
            uni.request({
                url: `http://localhost:8080/user/toUserInfo?memberAccount=${this.memberAccount}`,
                method: 'GET',
                header: {
                    'Content-Type': 'application/json'
                },
                success: (res) => {
                    if (res.data.success) {
                        this.users = res.data.members; // 假设返回多个用户信息的数组
                    } else {
                        uni.showToast({ title: res.data.message, icon: 'none' });
                    }
                },
                fail: () => {
                    uni.showToast({ title: '请求用户信息失败，请稍后再试', icon: 'none' });
                }
            });
        },
        navigateToUpdateInfo() {
            uni.navigateTo({
                url: '/pages/me/updateInfo/updateInfo' // 修改为实际的路径
            });
        }
    }
};
</script>

<style scoped>
.table {
    width: 100%;
    margin-top: 20rpx;
}

.table-header {
    display: flex;
    background-color: #f0f0f0;
    font-weight: bold;
    border-bottom: 1px solid #ccc;
}

.table-cell-header {
    flex: 1;
    padding: 10rpx;
    text-align: center;
}

.table-row {
    display: flex;
    border-bottom: 1px solid #ccc;
}

.table-cell {
    flex: 1;
    padding: 10rpx;
    text-align: center;
}

button {
    width: 100%;
    padding: 10rpx;
    background-color: #007BFF;
    color: #fff;
    border: none;
    border-radius: 5rpx;
    text-align: center;
    margin-top: 20rpx;
}
</style>
