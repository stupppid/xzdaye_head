<template>
	<div>
		<selectt class="selectt"></selectt>
		<objectt class="objectt"></objectt>
	</div>
</template>

<script type="text/javascript">
	import selectt from './content/select.vue'
	import objectt from './content/object.vue'
	export default{
		components:{
			selectt,objectt
		}
	}
</script>

<style type="text/css">
	.selectt{
		width: 22%;
		max-width: 240px;
		float: left;
	}
	.objectt{
		width: 76%;
		float: right;
		margin: 16px 0;
		max-width: 800px;
		padding-bottom: 20px;
	}
</style>