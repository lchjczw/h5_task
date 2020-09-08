<script>
/* 如果您不使用 nvue 请将本页面代码全部复制覆盖到 app.vue */
/* 不使用 nvue 请您同时删除 pages 目录下的全部 .nvue 文件 */
export default {
	onLaunch: function() {},
	onShow: function() {},
	onHide: function() {}
}
</script>
<style>
/* 引入核心样式 */
@import "./graceUI/graceUI.css";
@import "./graceUI/graceIcons.css";

/*
如下修改和补充能够保证不修改 GraceUI 框架的核心源码，当 GraceUI 升级时可以直接使用框架核心文件包覆盖旧文件
请根据业务原型或者设计稿规划出自己的 公共样式, 如:
*/
.color-black{color:#333333;}
.color-red{color:#FF0036;}
/* 修改 graceUI 或者其他 UI 框架示例 如, 修改 grace-title: */
.grace-title{color:#333333;}
navigator{opacity:1; background:none;}
.navigator-hover{background-color:none; opacity:0.8;}
</style>