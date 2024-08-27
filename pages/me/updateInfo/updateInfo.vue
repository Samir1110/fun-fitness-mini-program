<template>
    <view class="container">
        <view class="form-group" v-if="user">
            <text>账号: {{ user.memberAccount }}</text>
        </view>
        <view class="form-group">
            <text>姓名: {{ user.memberName }}</text>
        </view>
        <view class="form-group">
            <text>性别: {{ user.memberGender }}</text>
        </view>
        <view class="form-group">
            <text>办卡时间: {{ user.cardTime }}</text>
        </view>
        <view class="form-group">
            <text>购买课时: {{ user.cardClass }}</text>
        </view>
        <view class="form-group">
            <text>剩余课时: {{ user.cardNextClass }}</text>
        </view>
		<view class="form-group">
		    <text>积分: {{ user.memberCredit }}</text>
		</view>

        <view class="form-group">
            <text>年龄:</text>
            <input v-model="updatedAge" placeholder="请输入年龄" />
        </view>
        <view class="form-group">
            <text>身高:</text>
            <input v-model="updatedHeight" placeholder="请输入身高" />
        </view>
        <view class="form-group">
            <text>体重:</text>
            <input v-model="updatedWeight" placeholder="请输入体重" />
        </view>
        <view class="form-group">
            <text>电话:</text>
            <input v-model="updatedPhone" placeholder="请输入电话" />
        </view>
        <button @click="updateUserInfo">提交修改</button>
    </view>
</template>

<script>
export default {
    data() {
        return {
            memberAccount: uni.getStorageSync('memberAccount'),
            user: {},
            updatedAge: '',
            updatedHeight: '',
            updatedWeight: '',
            updatedPhone: ''
        };
    },
    onLoad() {
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
                        this.user = res.data.members[0];
                        this.updatedAge = this.user.memberAge;
                        this.updatedHeight = this.user.memberHeight;
                        this.updatedWeight = this.user.memberWeight;
                        this.updatedPhone = this.user.memberPhone;
                    } else {
                        uni.showToast({ title: res.data.message, icon: 'none' });
                    }
                },
                fail: () => {
                    uni.showToast({ title: '请求用户信息失败，请稍后再试', icon: 'none' });
                }
            });
        },
        updateUserInfo() {
            const data = {
                memberAccount: this.memberAccount,
                memberAge: this.updatedAge,
                memberHeight: this.updatedHeight,
                memberWeight: this.updatedWeight,
                memberPhone: this.updatedPhone
            };

            uni.request({
                url: `http://localhost:8080/user/updateInfo`,
                method: 'POST',
                data: data,
                header: {
                    'Content-Type': 'application/json'
                },
                success: (res) => {
                    if (res.data.success) {
                        uni.showToast({ title: '信息更新成功', icon: 'none' });
                        setTimeout(() => {
                            const eventChannel = this.getOpenerEventChannel();
                            eventChannel.emit('refreshData');
                            uni.navigateBack();
                        }, 1000);
                    } else {
                        uni.showToast({ title: res.data.message, icon: 'none' });
                    }
                },
                fail: () => {
                    uni.showToast({ title: '更新信息失败，请稍后再试', icon: 'none' });
                }
            });
        }
    }
};
</script>

<style scoped>
.container {
    padding: 20rpx;
}

.form-group {
    margin: 20rpx 0;
}

.form-group text {
    display: block;
    margin-bottom: 10rpx;
}

.form-group input {
    width: 100%;
    padding: 10rpx;
    border: 1px solid #ccc;
    border-radius: 5rpx;
}

button {
    width: 100%;
    padding: 10rpx;
    background-color: #007BFF;
    color: #fff;
    border: none;
    border-radius: 5rpx;
    text-align: center;
}
</style>
