<template>
	<div>
		<ul>
			<li class="upSelect" v-for="(item,key) in items">
				<a style="line-height: 30px;" href="javascript:"><div id="outItem" @click="slideTo(key)">{{key}}
				<img style="width: 15px;position: absolute;right: 12px;bottom: 10px;" src="../../image/down.png"/>
			</div></a>
			<div style="cursor: pointer;"><div class="innerFrame" :id="key">
				<p class="li"  @click="tranHtml(itm)" v-for="(keyy,itm) in item" >{{itm}}</p>
			</div></div>
			</li>
		</ul>
	</div>
</template>

<style>
	a{
		text-decoration: none;
		color: initial;
	}
	.li{
		line-height: 30px;
		margin: 1px 0;
		background: #F0F0F0;
	}
	.upSelect{
		background: silver;
		margin: 1px 0;
	}
	#outItem{
		padding: 4px 0;
		position: relative;
	}
</style>

<script type="text/javascript">
	import $ from 'jquery'
	import selectItem from '../../json/select.json'
	import {bus} from '../../js/bus.js'
	export default{
		data() {
			return {
				items:selectItem,
			}
		},
		methods:{
			slideTo:function(msg){
				$('#'+msg).stop().slideToggle()
			},
			tranHtml:function(msg){
				bus.$emit('eleInfo','select/'+msg)
			}
		}
	}
	$(function(){
		$('.innerFrame').hide()
	})
</script>