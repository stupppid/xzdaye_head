<template>
	<div>
		<form action="admin.php?c=admin&a=admin" method="post" style="overflow: hidden;padding: 10px;width: 220px;background-color: silver;">
			<label for="name">账号：</label>
			<input type="text" name="name" id="name" placeholder="输入账号"/><br />
			<label for="passwd">密码：</label>
			<input type="password" name="passwd" id="passwd" placeholder="输入密码"/>
			<br />
			<input type="submit" style="float: right;" value="登录"/>
		</form>
	</div>
</template>
<script type="text/javascript">
	import $ from 'jquery'
	function gettCookie(msg){
		var pa = new RegExp(msg)
		var arr = document.cookie.split(';')
		for(var i=0;i<arr.length;i++){
			if(pa.test(arr[i])){
				return arr[i].split("=")[1]
			}
		}
	}
	
	export default{
		created(){
			var a = gettCookie('xzitAdminId')
			var b = gettCookie('xzitAdminPasswd')
			if(a!=null && b!=null){
				$("#name").html(a)
				$("#passwd").html(b)
				console.log('a')
				this.$http.post('/admin.php?c=admin&a=test',{"name":a,"passwd":b}).then(response=>{
					console.log(response.body)
				})
			}
		}
	}
</script>