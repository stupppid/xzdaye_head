<template>
	<div style="min-width: 900px;">
		<first></first>
		<contentt></contentt>
		<tail></tail>
	</div>
</template>

<script>
	import first from './components/first.vue'
	import contentt from './components/content.vue'
	import tail from './components/tail.vue'
	import $ from 'jquery'
	export default{
		components:{
			first,contentt,tail
		},
		beforeCreate(){
			$(document).ready(function(){
				var a = window.location.href;
				if(a.split('/')[3] == "admin.html"){
					alert('未验证！')
					window.location.href = "xzdaye.top/admin.php"
				}
			})
		}
	}
</script>

<style type="text/css">
	@font-face {
		font-family:'dingding';
		src: url('/static/font/dingding.otf');
	}
</style>