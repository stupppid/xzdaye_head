<template>
	<div class="first">
		<h1>徐州打野学院信息管理系统</h1>
		<div style="position: relative;float: right;">
			<div id="adminInfo">
				<b><i>管理员：<span id="adminName">{{adminName}}</span></i></b>
			</div>
		</div>
	</div>
</template>

<script type="text/javascript">
	export default{
		data(){
			return {
				adminName:null
			}
		}
	}
</script>

<style type="text/css">
	.first{
		background-color: #f0f0f0;
		width: 100%;
		height: 75px;
		text-align: center;
		margin: -8px -8px 0px -8px;
		padding: 8px 16px 0px 0px;
		min-width: 800px;
		border-bottom:solid silver ;
	}
	#adminInfo{
		position: absolute;
		right: 100px;
		width: 10em;
		top: -50px;
	}
</style>