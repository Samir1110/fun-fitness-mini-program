<template>
	<view class = "coach">
		<button class= b1 @click="navigateToApplyClass">报名团课</button>
		<button class= b2 @click="navigateToMyClass">我的团课</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			navigateToApplyClass() {
			    uni.navigateTo({
			        url: '/pages/coach/applyClass/applyClass' // 修改为实际的路径
			    });
			},
			navigateToMyClass() {
			    uni.navigateTo({
			        url: '/pages/coach/myClass/myClass' // 修改为实际的路径
			    });
			}
		}
	}
</script>

<style>
	.coach{
		margin-top: 30px;
	}
	
	.b1{
		width: 100%;
		padding: 10rpx;
		background-color: #007BFF;
		color: #fff;
		border: none;
		border-radius: 5rpx;
		text-align: center;
		margin-top: 20rpx;
	}
	.b2{
		width: 100%;
		padding: 10rpx;
		background-color: #5500ff;
		color: #fff;
		border: none;
		border-radius: 5rpx;
		text-align: center;
		margin-top: 20rpx;
	}

</style>
