<template>
	<div>
		<div v-for="(itema,keyy) in items" >
			<div class="mp" v-for="(itemb,key) in itema" >
				<p class="title" >
					<span style="color: silver;">》</span>
					<a href="javascript:" @click="newsList(key)">{{key}}</a>
					<hr style="color:#E0E0E0;" />
				</p>
				<ul  style="text-align: left;">
					<li v-for="item in itemb" style="margin-bottom: 10px;">
						<span style="color: silver;">>&nbsp;</span>
						<a href="javascript:" @click="getNews(key+'/'+item[0])">{{item[1]}}</a>
						<div style="max-height: 52px;margin-bottom:20px; overflow: hidden;display:block;">
							<p @click="getNews(key+'/'+item[0])" style="text-indent: 1em;word-break: break-all;line-height: 20px;width:86%;font-size: small;cursor: pointer;">
								{{item[2]}}
							</p>
						</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<style type="text/css">
	.title {
		padding-left: 20px;
		font-size: larger;
		line-height: 15px;
	}
</style>

<script type="text/javascript">
	import $ from 'jquery'
	import {bus} from '../../js/bus.js'
	export default {
		data(){
			return {
				items:[]
			}
		},
		created() {
			this.$http.get('/index.php?c=index&a=index').then(response =>{
				this.items.splice(0,1,response.body)
			})
		},
		methods:{
			getNews:function(msg){
				console.log(msg)
				bus.$emit('eleInfo',msg)
			},
			newsList:function(msg){
				bus.$emit('newsList')
				bus.$emit('getNewsList',msg)
			}
		}
	}
</script>