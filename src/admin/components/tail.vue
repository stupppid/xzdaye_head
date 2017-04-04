<template>
	<div class="tail">
		<ul>
			<li>徐州工程学院版权所有 © 2016 信息化中心制作维护</li>
			<li>电话：0516-83105021&nbsp;&nbsp; 传真：0516-83105000 &nbsp;&nbsp;邮箱：office@xzit.edu.cn</li>
			<li>地址：江苏省徐州市云龙区丽水路2号&nbsp;&nbsp; 邮政编码：221018&nbsp;&nbsp; 苏ICP备15063436号-1</li>
		</ul>
	</div>
</template>
<style type="text/css">
	ul{
		list-style: none;
		text-align: center;
		font-size: small;
	}
	.tail{
		text-align:  center ;
		font-size: small;
		width: 100%;
		min-width: 800px;
		background-color: #F0F0F0;
		margin: -8px -8px;
		padding: 8px 16px 0px 0px;
		border-top: solid silver;
		overflow: hidden;
		min-width: 800px;
	}
</style>
<script type="text/javascript">
	import {bus} from '../js/bus.js'
	import $ from 'jquery'
	export default{
		created(){
			bus.$on('changeTailMargin',function(msg){
				$('.tail').css("margin-bottom",msg+'px')
			})
		}
	}
</script>