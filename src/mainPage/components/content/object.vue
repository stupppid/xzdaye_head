<template>
	<div style="background: silver;position: relative;">
		<div id="mp">
			<xyxw></xyxw>
			<ksdh></ksdh>
		</div>
		<div id="content">
			<div style="border-bottom: solid 1px gray;">
			<h2 style="color: #444;text-align: center;display: inline-block;" id="newsTitle"></h2>
			&nbsp;&nbsp;&nbsp;&nbsp;<i style="font-size: small;display: inline-block;" id="newsDate"></i>
			</div>
			<pre id="ccontent" style="white-space: pre-wrap;word-break: break-word;text-align: left;width: 96%;"></pre>
		</div>
		<div id="newsList">
			<newsList></newsList>
		</div>
		<a href="/" style="position: absolute;right: 0; bottom: 0;">
			<button>返回首页</button>
		</a>
	</div>
</template>

<style type="text/css">
	.mp{	
		float: left;
		width: 42%;
		height: 260px;
		margin: 20px 0 20px 40px;
		background-color: #F0F0F0;
		text-align: left;
	}
	#content{
		margin: 20px 0px 20px 40px;
	}
</style>

<script type="text/javascript">
	import {bus} from '../../js/bus.js'
	import Vue from 'vue'
	import $ from 'jquery'
	import xyxw from './xyxw.vue'
	import newsList from './list.vue'
	import ksdh from './ksdh.vue'
	export default{
		data(){
			return {
			}
		},
		components:{
			xyxw,ksdh,newsList
		},
		created(){
			$(function(){
				$('#newsList').hide()
				$('#mp').hide()
				setTimeout(function(){
					$('#mp').slideDown()
				},50)
			})
			bus.$on('eleInfo',function(msg){
				$.ajax({
					type:"get",
					url:"./index.php?c=index&a=getInfo",
					async:true,
					data:{"msg":msg},
					dataType:'json',
					success:function(fileInfo){
						console.log(fileInfo)
						$('#newsTitle').html(fileInfo[1])
						$('#newsDate').html(fileInfo[3])
						$('#ccontent').html(fileInfo[2])
						$('#mp').hide()
						$('#newsList').hide()
						$('#content').show()
					}
				});
			}),
			bus.$on('newsList',function(){
				$('#content').hide()
				$('#mp').hide()
				$('#newsList').show()
			})
		},
	}
</script>