<template>
	<gracePage headerBG="#FFFFFF">
		<view slot="gBody" class="grace-flex-v1" id="gBody">
			<view style="padding:5rpx 5rpx;" id="nav">
<!-- 				<graceNavBar :items="tabs" :currentIndex="swiperCurrentIndex" @change="navChange"></graceNavBar> -->
			<graceNavBar :items="tabs" 
							lineHeight="70rpx" :isCenter="true" 
							:currentIndex="swiperCurrentIndex" :size="0" padding="20rpx" 
							activeLineBg="#3688FF" textAlign="center"
							activeColor="#3688FF" activeLineWidth="100%" activeLineHeight="8rpx" 
							:margin="10" @change="navChange"></graceNavBar>
			</view>
			<swiper :style="{height:mainHeight+'px'}" :current="swiperCurrentIndex" @change="swiperChange">
				<swiper-item v-for="(item, index) in tabs" :key="index">
					<scroll-view scroll-y :style="{height:mainHeight+'px'}" @scrolltolower="scrollend">
						<!-- 空数据 -->
						<view style="margin-top:150rpx;" v-if="loadingTypes[index] == 5">
							<graceEmpty>
								<view slot="img" class="empty-view">
									<!-- 请根据您的项目要求制作并更换为空图片 -->
									<image class="empty-img" mode="widthFix" src="https://staticimgs.oss-cn-beijing.aliyuncs.com/empty.png"></image>
								</view>
								<text slot="text" class="grace-text-small grace-margin-top grace-gray">暂无订单信息~</text>
							</graceEmpty>
						</view>
						<view class="grace-order grace-box-shadow" v-for="(order, orderIndex) in orders[index]" :key="orderIndex" @tap="toDetail">
							<view class="grace-space-between grace-flex-center">
								<text class="grace-order-number">订单号 : {{order.orderNumber}}</text>
								<view class="grace-icons icon-close"><text class="grace-text" style="margin-left:10rpx;" @tap.stop="removeorder(order.orderNumber,orderIndex)" >删除订单</text></view>
							</view>
							<!-- 以商铺为单位进行循环 -->
							<block v-for="(shop, indexShop) in order.items" :key="indexShop">
								<view class="grace-title">
									<text class="grace-black">{{shop.shopName}}</text>
								</view>
								<!-- 循环订单商品 -->
								<view class="grace-order-goods" v-for="(goods, indexGoods) in shop.goods" :key="indexGoods">
									<image :src="goods.goods_img" class="grace-order-goods-img" mode="widthFix"></image>
									<text class="grace-order-goods-name">{{goods.goods_name}}</text>
									<view class="grace-order-goods-price">￥{{goods.goods_price}}<text class="grace-order-goods-num"> x {{goods.goods_buynum}}</text></view>
								</view>
							</block>
							<!-- 订单底部 -->
							<view class="grace-order-footer grace-nowrap">
								<text class="grace-order-number">{{order.status}} {{order.orderDate}}</text>
								<text class="grace-order-btn">查看发票</text>
								<text class="grace-order-btn grace-order-btn-red">再次购买</text>
							</view>
						</view>
						<!-- 每个选项卡都有一个自己的加载更多  -->
						<graceLoading :loadingType="loadingTypes[index]"></graceLoading>
					</scroll-view>
				</swiper-item>
			</swiper>
		</view>
	</gracePage>
</template>
<script>
import gracePage from "../../graceUI/components/gracePage.vue";
import graceEmpty from "../../graceUI/components/graceEmptyNew.vue";
import graceNavBar from "../../graceUI/components/graceNavBar.vue";
import graceLoading from "../../graceUI/components/graceLoading.vue";
var request = require('../../graceUI/jsTools/request.js');
export default {
    data() {
    	return {
			swiperCurrentIndex: 0,
			tabs : [ '全部', '待提交', '审核中', '审核通过', '未通过'],
			mainHeight : 300,
			// 订单数据  订单数组和订单状态数组元素一致
			orders : [[], [], [], [], []],
			// 订单页码
			pages : [1,1,1,1,1],
			// 加载状态
			loadingTypes : [3,3,3,3,3]
    	}
    },
	onReady:function(){
		setTimeout(()=>{
			uni.createSelectorQuery().select('#gBody').fields(
				{size: true}, (res) => {
					uni.createSelectorQuery().select('#nav').fields(
						{size: true}, (res2) => {
							this.mainHeight = res.height - res2.height;
						}
					).exec();
				}
			).exec();
		},1000);
	},
	onLoad:function () {
		this.getOrders();
	},
	watch : {
		swiperCurrentIndex : function(val){
			// 如果切换时尚未读取数据则读取
			if(this.orders[this.swiperCurrentIndex].length < 1 && this.loadingTypes[this.swiperCurrentIndex] == 3){ this.getOrders(); }
		}
	},
	methods:{
		//定义方法
		tohome:function(e){
			console.log("toHome")
			uni.navigateTo({
				url:"info"
			})
		},
		toDetail:function(e){
			console.log("toDetail")
			uni.navigateTo({
				url:"info"
			})
		},
		navChange: function (e) {
			this.swiperCurrentIndex = e;
		},
		swiperChange: function (e) {
			var index = e.detail.current;
			this.swiperCurrentIndex = index;
		},
		scrollend : function(e){
			console.log(e);
			// 避免重复加载
			if(this.loadingTypes[this.swiperCurrentIndex] != 3){return ;}
			this.getOrders();
		},
		getOrders : function (){
			console.log('类型 : ' + this.tabs[this.swiperCurrentIndex] + ' 第'+ this.pages[this.swiperCurrentIndex] +'页');
			this.loadingTypes.splice(this.swiperCurrentIndex, 1, 1);
			// this.tabs[this.swiperCurrentIndex] 代表向 api 接口传递订单要求返回的订单状态
			// page 代表第几页
			// 接口地址组合
			var url = "http://grace.hcoder.net/api/index/orders/"+this.tabs[this.swiperCurrentIndex]+'/'+this.pages[this.swiperCurrentIndex];
			console.log(url)
			request.get(url,{}, res => {
				if(res.status == 'ok'){
					// 第一页
					if(this.pages[this.swiperCurrentIndex] == 1){
						this.orders.splice(this.swiperCurrentIndex, 1 , res.data);
					}
					// 之后的加载页
					else{
						this.orders[this.swiperCurrentIndex] = this.orders[this.swiperCurrentIndex].concat(res.data);
					}
					// 页码增加
					this.pages[this.swiperCurrentIndex]++;
					this.loadingTypes.splice(this.swiperCurrentIndex, 1, 3);
				}else if(res.status == 'empty'){
					console.log('empty');
					this.loadingTypes.splice(this.swiperCurrentIndex, 1, 5);
				}else if(res.status == 'nomore'){
					console.log('nomore');
					this.loadingTypes.splice(this.swiperCurrentIndex, 1, 2);
				}
			});
		},
		removeorder : function(oid,index){
			console.log(oid);
			uni.showModal({
				title: '确认提醒',
				content: '您确定要移除订单 [ ' + oid + ' ] 吗？',
				success:function(e){
					console.log("success")
					if (e.confirm) {
						for(var i=0;i<this.orders[this.swiperCurrentIndex].length;i++){
							console.log(this.orders[this.swiperCurrentIndex].get(i).orderNumber)	
						}
						//自行完善删除代码
						this.orders[this.swiperCurrentIndex].splice(index, 1);
						console.log("删除后")
					
						for(var m=0;m<this.orders[this.swiperCurrentIndex].length;m++){
							console.log(this.orders[this.swiperCurrentIndex].get(m).orderNumber)	
						}
						
					}else{
						console.log("else")
					}
				}
			});
		}
	},
	components:{gracePage, graceNavBar, graceEmpty, graceLoading}
}
</script>
<style>
.empty-view{width:280rpx; height:280rpx; border-radius:280rpx; background-color:#F6F7F8; margin-top:30rpx;}
.empty-img{width:220rpx; height:200rpx; margin:40rpx; border-radius:200rpx;}

.grace-order{background-color:#FFFFFF; padding:20rpx; margin:0 25rpx; border-radius:10rpx; margin-top:30rpx; width:660rpx;}
.grace-shop{line-height:60rpx; font-size:28rpx; font-weight:bold; color:#333333;}
.grace-order-goods{font-size:0; overflow:hidden; width:660rpx; display:flex; flex-direction:row; flex-wrap:nowrap; align-items:center; padding-bottom:20rpx;}
.grace-order-goods-img{width:100rpx; height:100rpx;}
.grace-order-goods-name{line-height:40rpx; width:100rpx; flex:auto; overflow:hidden; font-size:26rpx; margin:0 20rpx; color:#333333;}
.grace-order-goods-price{font-size:30rpx; line-height:50rpx; color:#333333; padding-left:10rpx; font-weight:bold;}
.grace-order-goods-num{font-size:22rpx; color:#999999; margin-left:10rpx;}
.icon-close{font-size:28rpx; color:#999999; margin-left:30rpx;}
.grace-order-footer{margin-top:2px;}
.grace-order-number{line-height:50rpx; color:#999999; font-size:26rpx; width:100rpx; flex:auto;}
.grace-order-status{line-height:50rpx; color:#999999; font-size:26rpx; width:100rpx; flex:auto; text-align:center;}
.grace-order-btn{display:block; width:150rpx; height:50rpx; line-height:50rpx; color:#999999; font-size:22rpx; text-align:center; border-radius:60rpx; border:1px solid #999999; margin-left:20rpx;}
.grace-order-btn-red{border-color:#FF0036; color:#FF0036;}
</style>