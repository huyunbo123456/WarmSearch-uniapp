<template>
	<view class="goods_detail">
		<!-- 轮播图 -->
		<swiper indicator-dots="true">
			<swiper-item v-for="item in swipers" :key="item.id">
				<image :src="item.imgurl"></image>
			</swiper-item>
			
			<swiper-item v-for="item in swipers" :key="item.id+1">
				<image :src="item.d_imgurl"></image>
			</swiper-item>
		</swiper>
		
		<view class="box1">
			<view class="dname">
				<text>{{info.imgname}}</text>
				<text>{{info.status}}</text>
			</view>
			<view class="goods_name">{{info.imgdesc}}</view>
		</view>
		
		<view class="line"></view>
		
		<view class="box2">
			<view>丢失时间:{{info.create_time}}</view>
			<view>联系人:{{info.lostname}}</view>
			<view @click="phone">联系方式:{{info.contact}}(点击拨打)</view>
		</view>
		<view class="line"></view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: 0,
				swipers: [],
				info: {}
			}
		},
		methods: {
			phone() {
				uni.makePhoneCall({
					phoneNumber: '该手机号'
				})
			},
			async getSwipers (){
				const res = await this.$myRequest({
					url: '/goodsdetail?limit=1&page=1&sort=1&id='+this.id
				})
				console.log(res)
				this.swipers = res.data.data.items
			},
		async getInfo () {
			const res = await this.$myRequest({
				url:'/goodsdetail?limit=1&page=1&sort=1&id='+this.id
			})
			console.log(res)
			this.info = res.data.data.items[0]
		},
		onLoad (options) {
			console.log(options)
			this.id = options.id
			this.getSwipers()
			this.getInfo()
		}
	},
}
</script>

<style lang="scss">
.goods_detail {
	swiper {
		height: 700rpx;
		image {
			width: 100%;
			height: 100%;
		}
	}
	.box1 {
		padding: 10px;
		.dname {
			font-size: 35rpx;
			color: $search-color;
			line-height: 80rpx;
			text:nth-child(2) {
				color: #ccc;
				font-size: 28rpx;
				// text-decoration: line-through;
				margin-left: 20rpx;
			}
		}
		.goods_name {
			font-size: 32rpx;
			line-height: 60rpx;
		}
	}
	.box2 {
		padding: 0 10px;
		font-size: 32rpx;
		line-height: 70rpx;
	}
	
.line {
		height: 10rpx;
		width: 750rpx;
		background: #eee;
	}
}
</style>
