<template>
	<div id="ss" style="margin: 30px 0px 30px 0px;">
		<div v-for="(a,key) in items[0]">
			<span style="display: block;border-bottom: solid #666;width: 100%;font-size: large;color: #444;">》{{key}}《</span>
			<div class="gotNewsList" v-for="item in a" @click="getNews(key+'/'+item[0])">
				<p style="margin: 8px 10px;overflow: hidden;">
					<span style="color: #444;">(<i>{{item[3]}}</i>)<b>{{item[1]}}:</b></span>
					<span style="overflow: hidden;height: 80px;line-height: 23px;">{{item[2]}}</span>
				</p>
			</div>
			<div style="float: right;margin-bottom: 10px;">
				page:<input @keyup.enter="getNewsListt(key)" style="width: 60px;" min="0" max="9999" type="number" v-model="page"/>
				<button @click="getNewsListt(key)">确定</button>
			</div>
		</div>
	</div>
</template>
	
<style type="text/css">
	.gotNewsList{
		text-align: left;
		width: initial;
		margin: 5px 20px 20px 20px;
		height: 80px;
		background-color: #F0F0F0;
		display: block;
		word-break: break-all;
		cursor: pointer;
		overflow: hidden;
	}
</style>

<script type="text/javascript">
	import Vue from 'vue'
	import {bus} from '../../js/bus.js'
	import $ from 'jquery'
	export default{
		data(){
			return {
				items:[],
				page:0
			}
		},
		created(){
			var a = this
			bus.$on('getNewsList',function(msg){
				a.$http.get('index.php?c=index&a=getNewsList&msg='+msg+'&page='+a.page).then(response=>{
					a.items.splice(0,1,response.body)
				})
			})
		},
		methods:{
			getNews:function(msg){
				console.log(msg)
				bus.$emit('eleInfo',msg)
			},
			getNewsListt:function(msg){
				bus.$emit('getNewsList',msg)
			}
		}
	}
</script>