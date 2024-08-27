<template>
  <view class="container">
    <block v-for="product in productList" :key="product.product_id">
      <view class="product-card">
        <view class="product-info">
          <text class="product-name">{{ product.product_name }}<br></text>
          <text class="product-price">价格：{{ product.product_price }}<br></text>
          <text class="product-quantity">库存：{{ product.product_quantity }}</text>
        </view>
        <button class="buy-btn" @click="buyProduct(product.product_id)">购买</button>
      </view>
    </block>
  </view>
</template>

<script>
export default {
  data() {
    return {
      productList: []
    };
  },
  onLoad() {
    this.fetchProductList();
  },
  methods: {
    fetchProductList() {
      uni.request({
        url: 'http://localhost:8080/shop/catalog', // 替换为你的后端接口地址
        method: 'GET',
        success: (res) => {
          if (res.statusCode === 200) {
            // 对数据按价格排序
            this.productList = res.data.sort((a, b) => a.product_price - b.product_price);
          } else {
            uni.showToast({
              title: '获取商品信息失败',
              icon: 'none'
            });
          }
        },
        fail: () => {
          uni.showToast({
            title: '请求失败',
            icon: 'none'
          });
        }
      });
    },
    buyProduct(productId) {
      const memberAccount = uni.getStorageSync('memberAccount'); // 获取存储的用户账号
      if (!memberAccount) {
        uni.showToast({
          title: '用户未登录',
          icon: 'none'
        });
        return;
      }
      console.log(`Sending request with productId: ${productId} and memberAccount: ${memberAccount}`);
      uni.request({
        url: 'http://localhost:8080/shop/buyProduct', // 替换为你的后端接口地址
        method: 'POST',
        header: {
          'content-type': 'application/x-www-form-urlencoded' // 设置请求头为表单格式
        },
        data: {
          productId: productId,
          memberAccount: memberAccount
        },
        success: (res) => {
          if (res.data.success) {
            uni.showToast({
              title: res.data.message,
              icon: 'success'
            });
          } else {
            uni.showToast({
              title: res.data.message,
              icon: 'none'
            });
          }
        },
        fail: () => {
          uni.showToast({
            title: '请求失败',
            icon: 'none'
          });
        }
      });
    }
  }
};
</script>

<style>
.container {
  padding: 20rpx;
}

.product-card {
  background-color: #fff;
  padding: 20rpx;
  margin-bottom: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 4rpx rgba(0, 0, 0, 0.1);
}

.product-info {
  margin-bottom: 20rpx;
}

.product-name {
  font-size: 32rpx;
  font-weight: bold;
}

.product-price,
.product-quantity {
  font-size: 28rpx;
  color: #666;
  margin-top: 10rpx;
}

.buy-btn {
  width: 100%;
  padding: 10rpx;
  background-color: #007BFF;
  color: #fff;
  border: none;
  border-radius: 5rpx;
  text-align: center;
}
</style>
