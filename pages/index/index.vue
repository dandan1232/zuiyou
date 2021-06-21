<template>
	
	<view class="content mb-3">
	
		<view class="status_bar">
			<!-- 这里是状态栏 -->
		</view>
		
		<!-- 自定义状态栏 -->
		<view class="status_title"  v-show="scrollTop > 60">
			<text class="status_center text-white mt-1">选择小区</text>
		</view>
		<view class="Img-Swiper">
			<swiper class="swiper" indicator-dots="true" indicator-color="#CFD8DC" indicator-active-color="#ffffff" autoplay="true"
			 interval="5000" duration="1500">
				<swiper-item v-for="(item , index) in homeSlide" :key="index">
					<image class="sImg" :src="item.img" mode="aspectFill"></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="content-center">
			<view class="info">
				
				<image class="red ml-2 mt-1" src="../../static/index/icon_voice_red.png"></image>
				<text class="info-content text-muted">「正&丹」感谢相遇，要一直一直一直在一起哦</text>
			   
			</view>
			<divider class="mt-1 ml-2"></divider>
			<view class="choose-all flex flex-row mt-2">
				<view class="info-choose1 flex flex-column">
					<view class="info-icon flex flex-row" @click="openDoor">
						<image class="info-img mt-2 ml-2" src="../../static/index/phone.png"></image>
						<view class="info-text flex flex-column">
							<text class="text-dark font font-weight-bold mt-3">安全设备</text>
							<text class="text-muted font-sm mt-1">实时保卫控制</text>
						</view>
					</view>
					<view class="info-icon flex flex-row" @click="gotoParking">
						<image class="info-img mt-2 ml-2" src="../../static/index/car.png"></image>
						<view class="info-text flex flex-column">
							<text class="text-dark font font-weight-bold mt-3">快速学习</text>
							<text class="text-muted font-sm mt-1">快速学习定位</text>
						</view>
					</view>
				</view>
				<view class="info-choose2 flex flex-column ml-3">
					<view class="info-icon flex flex-row" @click="gotoSecurity">
						<image class="info-img mt-2 ml-2" src="../../static/index/sure.png"></image>
						<view class="info-text flex flex-column">
							<text class="text-dark font font-weight-bold mt-3">智能家具</text>
							<text class="text-muted font-sm mt-1">智能遥控设备</text>
						</view>
					</view>

					

					<view class="info-icon flex flex-row" @click="gotoProperty">
						<image class="info-img mt-2 ml-2" src="../../static/index/home.png"></image>
						<view class="info-text flex flex-column">
							<text class="text-dark font font-weight-bold mt-3">咨询服务</text>
							<text class="text-muted font-sm mt-1">咨询消费学习</text>

						</view>
					</view>
				</view>
			</view>
		</view>	
		
		<view class="content-end flex flex-row mt-2 ml-2 mb-2">
			<text class="font-weight-bold font">民生资讯</text>
			<text class="see-more font-sm text-muted">查看全部</text>
		</view>
		
		<view v-for="(info,index) in infos" :key="index">
			<index-card :info="info" class="mt-3" @open="news(info.pkId)"></index-card>
		</view>
		
	</view>
</template>

<script>
	import divider from '@/components/common/divider.vue'
	import indexCard from '@/components/index-card/index-card.vue'
	import {
		test
	} from '../../api/api.js'
	export default {
		data() {
			return {
				infos: [],
				title: 'Hello',
				scrollTop: 0,
				homeSlide: [{
						index: 1,
						img: "http://bnnbucket.oss-cn-hangzhou.aliyuncs.com/fulailai/s4.png"
					},
					{
						index: 2,
						img: "http://bnnbucket.oss-cn-hangzhou.aliyuncs.com/fulailai/s5.png"
					},
					{
						index: 3,
						img: "http://bnnbucket.oss-cn-hangzhou.aliyuncs.com/fulailai/s6.png"
					}
				]
			}
		},

		components: {
			divider,
			indexCard
		},
		onLoad() {
			// console.log(JSON.parse(localStorage.getItem("userInfo")))
			// this.test(),
			uni.getSystemInfo({
				success: res => {
					this.scrollH = res.windowHeight - uni.upx2px(101);
				}
			});
			uni.request({
				url:'http://8.136.186.71:8082/api/news/info/all',
				success: (res) => {
					// console.log(res);
					console.log(',.,,.,.,,.'+JSON.stringify(res.data));
					this.infos = res.data
					// console.log(res.data.pkId)
				}
			});
		},
		onPageScroll(e){
				this.scrollTop = e.scrollTop;
				// console.log(this.scrollTop)
			},
		
		methods: {
			// test() {
			// 	test().then(res => {
			// 		console.log(res);
			// 	})
			// },
			openDoor() {
				uni.navigateTo({
					url:'../door/door'
				})
			},


			gotoProperty() {
				uni.navigateTo({
					url: "../property/property"

				})
			},
			gotoParking() {
				uni.navigateTo({
					url: "../parking/community_parking"
				})
			},
			gotoSecurity() {
				uni.navigateTo({
					url: "../parking/smart_security"
				})
			},
			news(pkId) {
				console.log('文章id：' + pkId);
				uni.navigateTo({
					url:'../newsreport/news?id=' + pkId
				})
			},

		}
	}
</script>

<style lang="scss" scoped>
	.status_title {
		box-sizing: border-box;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		height: 60px;
		position: fixed;
		padding: 0 16px;
		top: 0;
		z-index: 999;
		background-color: #F57C00;
	}

	
	.status_center {
		font-size: 17px;
		font-weight: 700;
	}
	.content {
		background-color: #F5F5F5;
		// height: 2000px;
		margin-bottom: 70px;
	}

	.swiper {
		height: 400upx;
		margin-bottom: 15px;
	}

	.sImg {
		width: 100%;
		height: 400upx;
	}

	.content-center {
		height: 230px;
		width: 95%;
		background-color: white;
		margin: auto;
		border-radius: 3%;
	}
	

	.info {
		width: 100%;
		height: 40px;
		display: flex;
		flex-direction: row;
		white-space: nowrap; //不换行
	}

	.red {
		height: 30px;
		width: 30px;
		background-color: white;
	}

	.info-content {
		margin-top: 10px;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.info-img {
		height: 70px;
		width: 70px;
		background-color: white;
	}
	
	.see-more {
		margin-left: 240px;
	}
	page,.main{
	        width: 100%;
	        height: 100%;
	    }
</style>
