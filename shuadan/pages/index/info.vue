<template>
	<gracePage headerBG="#FFFFFF">
		<view slot="gBody" class="grace-flex-v1" id="gBody">
			<view class="SegmentedControlIn">
				<graceSegmentedControl :items="cates" :current="current" @change="change"></graceSegmentedControl>
			</view>
			<scroll-view scroll-y class="grace-list grace-margin-top" :style="{height:scrollHeight+'px'}">
				<view class="grace-list-items grace-body" 
				v-for="(item, index) in logs" :key="index" 
				v-if="current == 0 || cates[current] == item[0]">
					<view class="grace-list-body grace-border-b">
						<view class="grace-list-title">
							<text class="grace-list-title-text">{{item[0]}}</text>
							<text class="grace-list-title-desc">￥{{item[1]}}</text>
						</view>
						<view class="grace-list-body-desc">{{item[2]}}</view>
					</view>
				</view>
			</scroll-view>
		</view>
	</gracePage>
</template>
<script>
import gracePage from "../../graceUI/components/gracePage.vue";
import graceSegmentedControl from '../../graceUI/components/graceSegmentedControl.vue';
export default {
    data() {
    	return {
			cates : ["全部", "充值", "消费"],
			current : 0,
			scrollHeight : 500,
			logs : [
				['充值', 200, '2019-12-11'],
				['充值', 500, '2019-12-10'],
				['消费', 198, '2019-12-10'],
				['消费', 300, '2019-12-09'],
				['充值', 188, '2019-12-08'],
				['充值', 299, '2019-12-07'],
				['消费', 200, '2019-12-07'],
				['充值', 600, '2019-12-06'],
				['消费', 318, '2019-12-05'],
				['充值', 190, '2019-12-05'],
				['充值', 599, '2019-12-04'],
				['消费', 286, '2019-12-03'],
				['充值', 199, '2019-12-02'],
				['消费', 688, '2019-12-01']
			]
		}
    },
	onReady:function(){
		setTimeout(()=>{
			uni.createSelectorQuery().select('#gBody').fields(
				{size: true}, (res) => {
					this.scrollHeight = res.height - uni.upx2px(180);
				}
			).exec();
		},1000);
	},
	methods:{
		change : function(e){
			this.current = e;
		}
	},
	components:{gracePage,graceSegmentedControl}
}
</script>
<style>
.SegmentedControlIn{width:500rpx; padding:0 125rpx; }
</style>