<template>
	<view class="grace-drawer-shade" v-if="show" @tap.stop="closeDrawer" @touchmove.stop="" :style="{background:background,zIndex:zIndex, top:top+'px'}">
		<view  :class="['grace-drawer-shade-nav', direction == 'left' ? 'gdSlideLeft' : 'gdSlideRight']" :style="{width:width,padding:padding, zIndex:zIndex+1, background:slotBg}" @tap.stop="stopFun">
			<slot name="links"></slot>
		</view>
	</view>
</template>
<script>
export default {
	name: "graceDrawer",
	props: {
		show : {
			type : Boolean,
			default : false
		},
		direction : {
			type : String,
			default : 'left'
		},
		width : {
			type : String,
			default : '60%'
		},
		background:{
			type : String,
			default : 'rgba(0, 0, 0, 0.5)'
		},
		slotBg:{
			type : String,
			default : 'rgba(255, 255, 255, 1)'
		},
		padding :{
			type : String,
			default : '30rpx'
		},
		zIndex : {
			type : Number,
			default : 1
		},
		// #ifdef H5
		top:{
			type : Number,
			default : 44
		},
		// #endif
		// #ifndef H5
		top:{
			type : Number,
			default : 0
		},
		// #endif
	},
	methods:{
		closeDrawer : function(){
			this.$emit('closeDrawer');
		},
		stopFun : function(){}
	}
}
</script>
<style>
@keyframes gdSlideLeft{ from {left:-300px; } 100% { left:0px; }}
.gdSlideLeft {animation:gdSlideLeft 200ms linear; left:0;}
@keyframes gdSlideRight{ from {right:-300px; } 100% { right:0px; }}
.gdSlideRight {animation:gdSlideRight 200ms linear; right:0;}
.grace-drawer-shade{position:fixed; width:100%; height:100%; top:0; left:0; z-index:1; background:rgba(0, 0, 0, 0.5);}
.grace-drawer-shade-nav{height:100%; background:#FFFFFF; position:absolute; top:0; z-index:2; box-sizing:border-box;}
</style>