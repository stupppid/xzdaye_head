<template>
	<div class="content" @mousemove="moveBottom">
		<div style="overflow: hidden;height:70px;border-bottom: solid silver;background-color:lightgray;">
			<div>
				<h2 style="line-height: 0px;"><span style="color: silver;">&nbsp;》</span>选择你需要管理的信息</h2>
				<div style="position: absolute;right: 40px;top:100px">
					<input style="height:30px;" type="search" id="search" placeholder="找不到？试试搜索一下" v-model="search" />
					<button style="border: 0;height:30px;background-color: silver;" for="search" @click="searchDB(search)">搜索</button>
				</div>
			</div>
		</div>
		<div @keyup.enter="changeFont" style="position: absolute;right: 20px;">
			字体:
			<select v-model="fontFamily">
				<option value="微软雅黑">微软雅黑</option>
				<option value="dingding">柔体</option>
			</select>&nbsp;&nbsp;
			字号:
			<input @keyup.enter="changeFont" type="number" style="width: 5em;" max="24" min="12" v-model="fontSize"/>
		</div>
		<div v-for="(item,key) in items" class="rightColumn">
			<div v-for="(itm,keyy) in item" @click="currentKey=keyy">
				<div style="border-right: solid silver;width:270px;float: left;">
					<div class="selectItems">
						<a :href="'#'+keyy+'top'"><i>{{keyy}}</i></a>
					</div>
				</div>
				<div style="width:calc(100% - 340px);margin-right:30px;margin-bottom: 10px;float:right;"><br />
					<div style="margin-bottom: 20px;">
						<span :id="keyy+'top'" style="margin: 0;padding:2px 20px;cursor: pointer;background-color: #F0F0F0;border-radius: 10px 10px 0 0;">{{keyy}}</span>
						<div style="background-color: #F0F0F0;padding-top: 10px;">
							<div v-for="it in itm" :id="it[1]" class="smallItems" style="text-align:right;">
								<div @click="topEditItem(keyy,it[0],it[1])" style="width: 10em;text-align:center;white-space:nowrap;overflow: hidden;">{{it[1]}}</div>
								<img @click="deleteItem(keyy,it[0],it[1])" class="deleteItem" :class="'deleteItem'+keyy" style="position: absolute;height: 15px;margin-top:-30px;cursor: cell;border-radius:7.5px;background-color: gray;" src="../../assets/image/delete.png" />
							</div>
							<div class="smallItems addSmallItems" :id="'addItem'+keyy" @click="addItem(keyy)" style="background-color: #F0F0F0;;border:1px dashed gray;padding: 4px;">+</div>
							<div id="change" style="display:block;width:100%;float: right;background-color: #F0F0F0;">
								<div class="changeThing" :id="keyy">
									<button @click="editItem(keyy)">编辑条目</button>
									<button @click="stopEditItem(keyy)">编辑完成</button>
									<form action="admin.php?c=admin&a=getFile" method="post" enctype="multipart/form-data">
										<input type="file" name="file" id="file" title="请将名称改为要改的项目名称，如学校简介.txt" /><br />
										<input type="submit" value="上传" />
									</form>
								</div>
								<div class="adminSlide" :id="keyy+'a'" @click="giveOut(keyy)">
									<img src="../../assets/image/slide.png" />
								</div>
								<div @keyup.enter="changePage" style="line-height: 38px;float: right;margin-right: 10px;">
									page &nbsp;<input type="number" min="0" max="9999" v-model="page[keyy]" style="width: 48px;"/>
									<button @click="changePage">确定</button>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
		<div id="addPage" v-show="ifEdit" :style="{height: bottomSize + 'px'}">
			<div id="AdminBottomItem" @mouseup="stopChangeAdminEditSize" @mousedown="changeAdminEditSize" @mouseover="changeAdminEditStyle" style="position: absolute;height: 5px;width: 100%;"></div>
			<span style="width: 1em;display:block;float: left;padding:0px 5px 0px 5px;border-right: solid dimgray 2px;line-height: 15px;"><b>{{currentKey}}</b></span>
			<div style="margin:3px 4px 3px 34px;" id="AdminEditItem" v-for="items in editItems[0]">
				<div v-for="item in items" style="font-size: small;">
					<div v-if="item[0] === 1"><span style="display:inline-block;width: 10em;float: left;"><b>{{item[1]}}:</b></span><input style="width: 30em;" type="text" v-model="item[2]" /></div>
					<div v-else-if="item[0] === 3"><span style="display:inline-block;width: 10em;float: left;"><b>{{item[1]}}:</b></span><input style="width: 30em;" type="text" v-model="item[2]" /></div>
					<div v-else-if="item[0] === 2"><span style="display:inline-block;width: 10em;float: left;"><b>{{item[1]}}:</b></span><textarea style="width: calc(100% - 5px - 10em);" v-model="item[2]"></textarea></div>
				</div>
				<button @click="putInfo" style="float: right;margin-right: 28px;width: 10em;">提交</button>
			</div>
		</div>
		<a href="./index.php"><button style="width: 2em;position: fixed;right: -2px;top:300px">返回首页</button></a>
		<a href="#search"><button style="width: 2em;position: fixed;right: -2px;top: 200px;">返回页首</button></a>
	</div>
</template>

<script type="text/javascript">
	import {bus} from '../js/bus.js'
	import $ from 'jquery'
	export default {
		data() {
				return {
					items: [],
					search: '',
					currentKey: '',
					ifEdit: false,
					editItems: [''],
					ifEditBottomSize: false,
					bottomSize: 150,
					fontFamliy:'柔体',
					fontSize:24,
					page:{}
				}
			},
			beforeCreate() {
				this.$http.get('admin.php?c=admin&a=getItems').then(response => {
					this.items.splice(0, 1, response.body)
					$(document).ready(function() {
						$(".changeThing").hide()
						$(".addSmallItems").hide()
						$(".deleteItem").hide()
					})
				})
			},
			methods: {
				flushItems: function(keyy) {
					var a = this
					this.$http.get('admin.php?c=admin&a=getItems&key='+a.currentKey+'&page=' + this.page[keyy]).then(response => {
						var q = a.currentKey
						var m = this.items[0]
						m[q] = response.body
						this.items.sort()
					})
				},
				searchDB: function(a) {
					this.$http.get('admin.php?c=admin&a=searchItem&msg=' + a, response => {

					})
				},
				giveOut: function(a) {
					$("#" + a).stop().slideToggle();
					if($("#" + a + 'a').css("transform") == 'matrix(1, 0, 0, 1, 0, 0)') {
						$("#" + a + 'a').css("transform", "rotate(180deg)")
					} else {
						$("#" + a + 'a').css("transform", "rotate(0deg)")
					}
				},
				editItem: function(keyy) {
					if(window.xzitAdminPrivalige > 0) {
						this.currentKey = keyy
						$("#addItem" + keyy).stop().show()
						$(".deleteItem" + keyy).stop().show()
					} else {
						alert('你的权限不够编辑条目')
					}
				},
				stopEditItem: function(keyy) {
					$("#addItem" + keyy).stop().hide()
					$(".deleteItem" + keyy).stop().hide()
					this.ifEdit = false
					bus.$emit('changeTailMargin', 0)
					this.currentKey = ''
				},
				addItem: function(keyy) {
					this.currentKey = keyy
					this.ifEdit = true
					bus.$emit('changeTailMargin', 150)
					this.$http({
						method: 'GET',
						url: './admin.php?c=admin&a=getInfo&key=' + keyy,
					}).then(response => {
						this.editItems.splice(0, 1, response.body)
					})
				},
				deleteItem: function(keyy, id, itemId) {
					if(confirm('确认删除？')) {
						this.$http({
							method: 'GET',
							url: './admin.php?c=admin&a=deleteInfo&key=' + keyy + '&id=' + id,
						}).then(response => {
							if(response.body) {
								alert('删除成功' + itemId)
							} else {
								alert('删除失败')
							}
							this.flushItems(this.currentKey)
						}, response => {
							alert('删除失败')
						})
					}
				},
				topEditItem: function(keyy, id, name) {
					this.ifEdit = true
					bus.$emit('changeTailMargin', 150)
					this.currentKey = keyy + '--' + name
					this.$http({
						method: 'GET',
						url: './admin.php?c=admin&a=getInfo&key=' + keyy + '&id=' + id,
					}).then(function(response) {
						this.editItems.splice(0, 1, response.body)
					})
				},
				putInfo: function() {
					var self = this
					if(window.xzitAdminPrivalige > 0) {
						$.ajax({
							url: './admin.php?c=admin&a=putInfo',
							type: 'post',
							data: {
								"msg": self.editItems[0]
							},
							success: function(msg) {
								if(msg) {
									alert('编辑成功')
								} else {
									alert('编辑失败')
								}
							}
						})
					} else {
						alert("你的权限不够编辑条目")
					}
				},
				moveBottom: function(event) {
					if(this.ifEditBottomSize) {
						this.bottomSize = window.innerHeight - event.clientY + 2.5
						bus.$emit('changeTailMargin', this.bottomSize)
					}
				},
				changeAdminEditStyle: function() {
					$('#AdminBottomItem').css('cursor', 'n-resize')
				},
				changeAdminEditSize: function() {
					this.ifEditBottomSize = true
				},
				stopChangeAdminEditSize: function() {
					this.ifEditBottomSize = false
				},
				changePage(){
					this.flushItems(this.currentKey)
				},
				changeFont(){
					$('.content').css({"font-family":this.fontFamily,"font-size":this.fontSize+'px'})
				}
			},
	}
</script>
<style type="text/css">
	.changeThing {
		border-bottom: solid silver;
		border-top: solid silver;
	}
	
	.content {
		font-family: 'dingding';
		font-size: 24px;
		width: 100%;
		background: #F0F0F0;
		min-width: 800px;
	}
	
	.selectItems {
		border-right: solid #666;
		overflow: hidden;
		text-align: center;
		line-height: 80px;
		margin: 30px;
		border-radius: 30px;
		width: 200px;
		height: 80px;
		background-color: silver;
		cursor: pointer;
	}
	
	.smallItems {
		border-radius: 10px 0px;
		text-align: center;
		display: inline-block;
		width: 10em;
		cursor: pointer;
		padding: 5px;
		background-color: silver;
		margin: 0px 10px 10px 10px;
	}
	
	.adminSlide {
		transform: rotate(180deg);
		cursor: pointer;
		margin-left: calc(50% - 37px);
		display: inline-block;
	}
	
	#addPage {
		position: fixed;
		background-color: #F0F0F0;
		height: 150px;
		bottom: 0;
		width: 100%;
		margin-left: -8px;
		overflow-Y: scroll;
		font-family: "微软雅黑";
		font-size: small;
		border-top: solid gray 1px;
		overflow: overlay;
	}
</style>