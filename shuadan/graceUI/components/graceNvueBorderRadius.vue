<template>
	<view class="grace-border-radius" 
	:style="{
		backgroundImage:background, 
		borderTopLeftRadius:radius[0],
		borderTopRightRadius:radius[1],
		borderBottomRightRadius:radius[2],
		borderBottomLeftRadius:radius[3]}">
		<slot></slot>
	</view>
</template>
<script>
export default{
	props:{
		radius : {type:Array, default:function(){
			return['10px', '10px', '10px', '10px']
		}},
		background:{type:String, default:""}
	}
}
</script>
<style>
.grace-border-radius{}
</style>
