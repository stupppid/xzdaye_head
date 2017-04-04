<template>
	<div>
		<first id="f"></first>
		<contentt id="c"></contentt>
		<tail id="t"></tail>
	</div>
</template>

<script>
import first from './components/first.vue'
import contentt from './components/content.vue'
import tail from './components/tail.vue'
	export default{
		components:{
			first,contentt,tail
		}
	}
</script>

<style type="text/css">
	#f {
		width: 100%;
		min-width: 800px;
		background: #F0F0F0;
		margin: -8px -8px;
		padding: 8px 16px 0 0;
	}
	#c {
		text-align: center;
		width: 70%;
		min-width: 800px;
		margin: 0px auto;
		min-height: 597px;
	}
	#t{
		text-align:  center ;
		font-size: small;
		width: 100%;
		min-width: 800px;
		overflow: auto;
		background-color: #F0F0F0;
		margin: -8px -8px;
		padding: 8px 16px 0 0;
	}
	ul{
		list-style: none;
	}
</style>