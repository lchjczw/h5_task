<template>
	<view class="graceSlideToUnlock" :style="{backgroundColor:bgColor, width:width+'rpx'}">
		<view :style="{width:(width - padding* 2) +'rpx', padding:padding+'rpx'}">
			<movable-area :style="{backgroundColor:bgColor, width:(width - padding* 2) +'rpx', height:size+'rpx', borderRadius:borderRadius}">
				<movable-view class="graceSlideToUnlock-block" direction="horizontal" @change="change" :x="moveX" :disabled="disabled" 
				:style="{backgroundColor:disabled ? blockActiveColor : blockColor, borderRadius:borderRadius, width:size+'rpx', height:size+'rpx', lineHeight:size+'rpx', color:iconColor, fontSize:iconSize}">
				<text class="grace-icons icon-pwd" v-if="!disabled"></text>
				<text class="grace-icons icon-right" v-if="disabled"></text>
				</movable-view>
				<text class="graceSlideToUnlock-text" :style="{width:(width - padding* 2) +'rpx', lineHeight:size+'rpx', color:disabled?blockActiveColor:'#898989'}">{{disabled?msgUnlock:msg}}</text>
			</movable-area>
		</view>
	</view>
</template>
<script>
/*
GraceUI 滑动解锁组件添加于 2020-02-27
近期发现个别 UI 抄袭 GraceUI，警告：GraceUI 已经申请软著，抄袭也是一种侵权! 我们将举起法律武器维护我们的版权！后果自负 !
*/
export default{
	props:{
		width:{type : Number, default:700},
		padding:{type : Number, default:6},
		size : {type : Number, default:60},
		bgColor : {type : String, default:'#F6F7F8'},
		blockColor : {type : String, default:'#3688FF'},
		blockActiveColor :{type : String, default:'#1ab16c'},
		iconSize : {type : String, default:'30rpx'},
		iconColor : {type : String, default:'#FFFFFF'},
		borderRadius:{type : String, default:'6rpx'},
		msg:{type : String, default:'请向右滑动滑块解锁'},
		msgUnlock:{type : String, default:'解锁成功'}
	},
	data() {
		return {
			maxWidth:300,
			moveX : 0,
			disabled:false
		}
	},
	created:function(){
		this.maxWidth = uni.upx2px(this.width - this.padding*2 - this.size - 2);
	},
	methods:{
		change:function(e){
			if(this.disabled){return ;}
			if(e.detail.x >= this.maxWidth){
				this.moveX = this.width;
				this.disabled = true;
				this.$emit('unlock');
			}
		}
	}
}
</script>
<style>
.graceSlideToUnlock{}
.graceSlideToUnlock-block{text-align:center;}
.graceSlideToUnlock-text{text-align:center; color:#898989; font-size:24rpx; display:block;}
</style>
