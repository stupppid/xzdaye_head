<template>
	<div style="padding-bottom:10px ;text-align: center;border-bottom: solid silver;">
		<img src="../image/xiaohui.png" />
		<div style="float: right;position: relative;">
			<div class="tx" title="点击登录" @click="beginLogin"><img :src="txPath[0]"/></div><br />
			<div class="xx">{{userInfo[0]}}<br />{{userName[0]}}</div>
			<div id="login">
				<table>
					<tr>
						<td><label for="id"><i>ID:</i></label></td>
						<td>
							<input type="text" name="id" id="id" placeholder="学号/工号" v-model="id"/>
						</td>
					</tr>
					<tr>
						<td><label for="password"><i>PW:</i></label></td>
						<td>
							<input type="password" name="password" id="password" placeholder="请输入密码" v-model="passwords"/>
						</td>
					</tr>
					<tr>
						<td style="color: gray;"><b><i>{{loginRight[0]}}</i></b></td> 
						<td style="position: absolute;right: 10px;"><button @click="login(id,passwords)">登录</button></td>
					</tr>
				</table>
			</div>
		</div>
	</div>
</template>
<div id="dd">adsad</div>
<style type="text/css">
	.tx {
		position: absolute;
		right: 90px;
		height: 50px;
		width: 50px;
		border-radius: 50px;
		border: 1px solid silver;
		overflow: hidden;
		cursor: pointer;
	}
	.tx img {
		width: 50px;
		height: 50px;
	}
	.xx{
		margin-top: 3px;
		font-size: smaller;
		position: absolute;
		width: 10em;
		right: 50px;
		top: 50px;
	}
	#login{
		border: 1px solid silver;
		z-index: 3;
		background-color:#F0F0F0 ;
		height: 86px;
		padding: 4px;
		position: absolute;
		top: 96px;
		right: 8px;
	}
</style>

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
		data(){
			return {
				txPath:["./static/image/timg.jpg"],
				userInfo:['未登录'],
				userName:[''],
				id:'',
				passwords:'',
				loginRight:['']
			}
		},
		created(){
			$(function(){
				$("#login").hide()
			})
			this.id = gettCookie("xzitID")
			this.passwords = gettCookie("xzitPW")
			if(this.id!=null && this.passwords!=null){
				this.login(this.id,this.passwords)
			}
		},
		methods:{
			beginLogin:function(){
				$('#login').stop().slideToggle()
			},
			login:function(id,passwords){
				this.$http.post('/index.php?c=index&a=login&id='+id+'&password='+passwords).then(response=>{
					if(response.body!=null){
						this.userInfo.splice(0,1,response.body.banji)
						this.userName.splice(0,1,response.body.name)
						this.txPath.splice(0,1,response.body.txUrl)
						this.loginRight.splice(0,1,'')
					}else{
						this.loginRight.splice(0,1,'登录失败！')
					}
				})
			}
		}
	}
	
</script>