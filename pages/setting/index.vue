<template>
	<div class="setting">
		<div class="flex">
			<div :class="[activeIndex==item.value?'active':'']" @click="tabClick(index)" v-for="(item,index) in tabList" :key="index">{{item.name}}</div>
		</div>
		<div class="view" v-if="activeIndex == 0">
			<div v-for="(item,index) in honestList" :key="index">
				<view class="uni-form-item uni-column">
					<div style="font-weight:600">选项{{index+1}}：</div>
					<input v-model="item.tabName" class="uni-input" maxlength="5" placeholder="选项卡名称,最大长度为5" />
				</view>
				<view class="uni-textarea">
					<textarea v-model="item.tabContent" maxlength='100' auto-height placeholder="选项内容"/>
				</view>
			</div>
			<div class="btnGroup">
				<button @click="add('honest')" type="success">添加</button>
				<button @click="del('honest')" type="warn">删除</button>
				<button @click="save('honest')" type="primary">保存</button>
			</div>
		</div>
		<div class="view" v-else>
			<div v-for="(item,index) in adventureList" :key="index">
				<view class="uni-form-item uni-column">
					<div style="font-weight:600">选项{{index+1}}：</div>
					<input v-model="item.tabName" class="uni-input" maxlength="5" placeholder="选项卡名称,最大长度为5" />
				</view>
				<view class="uni-textarea">
					<textarea v-model="item.tabContent" maxlength='100' auto-height placeholder="选项内容"/>
				</view>
			</div>
			<div class="btnGroup">
				<button @click="add('adventure')" type="success">添加</button>
				<button @click="del('adventure')" type="warn">删除</button>
				<button @click="save('adventure')" type="primary">保存</button>
			</div>
		</div>
		<yu-toast
      :message="message"
      verticalAlign="center"
      ref="toast"
    ></yu-toast>
	</div>
</template>

<script>
  import yuToast from '@/components/yu-toast/yu-toast'
	export default {
	components: {
    yuToast
  },
		data() {
			return {
				tabList:[{
					name:'真心话',
					value:'0'
				},{
					name:'大冒险',
					value:'1'
				}],
				activeIndex:0,
				// 真心话
				honestList:[],
				// 大冒险
				adventureList:[],
				message:''
			}
		},
		onShow() {
			let _this = this
			uni.getStorage({
				key: 'honestList',
				complete:function (res) {
					if(res.data==''){
						_this.honestList = [{
							tabName:'真心话1',
							tabContent:'内容1'
						},{
							tabName:'真心话2',
							tabContent:'内容2'
						},{
							tabName:'真心话3',
							tabContent:'内容3'
						}]
					}else{
						_this.honestList = JSON.parse(res.data)
					}
				},
			});
			uni.getStorage({
				key: 'adventureList',
				complete:function (res) {
					if(res.data==''){
						_this.adventureList = [{
							tabName:'大冒险1',
							tabContent:'内容1'
						},{
							tabName:'大冒险2',
							tabContent:'内容2'
						},{
							tabName:'大冒险3',
							tabContent:'内容3'
						}]
					}else{
						_this.adventureList = JSON.parse(res.data)
					}
				},
			});
		},
		methods: {
			tabClick(index){
				this.activeIndex = index
			},
			add(type){
				if(type=="honest"){
					if(this.honestList.length>=10){
						this.message = '最多是个选项'
						this.$refs.toast.show()
					}else{
						this.honestList.push({
							tabName:'',
							tabContent:''
						})
					}
				}else{
					if(this.adventureList.length>=10){
						this.message = '最多是个选项'
						this.$refs.toast.show()
					}else{
						this.adventureList.push({
							tabName:'',
							tabContent:''
						})
					}
				}
			},
			del(type){
				if(type=="honest"){
					if(this.honestList.length<=3){
						this.message = '最少三个选项'
						this.$refs.toast.show()
					}else{
						this.honestList.pop()
					}
				}else{
					if(this.adventureList.length<=3){
						this.message = '最少三个选项'
						this.$refs.toast.show()
					}else{
						this.adventureList.pop()
					}
				}
			},
			save(type){
				this.message = '保存成功'
				this.$refs.toast.show()
				if(type=='honest'){
					uni.setStorage({
						key: 'honestList',
						data: JSON.stringify(this.honestList),
					});
				}else{
					uni.setStorage({
						key: 'adventureList',
						data: JSON.stringify(this.adventureList),
					});
				}
			}
		}
	}
</script>

<style scope>	
	.setting{
		background:#f4f4f4;
		min-height: 100vh;
	}
	.setting .flex{
		display:flex;
		justify-content: space-between;
		height: 45px;
		line-height: 45px;
		text-align: center;
		flex-grow: 1;
		border-bottom:1px solid #ddd;
		background-color: #fff;
	}
	.setting .flex div{
		flex-grow: 1;
		margin:0 15px
	}
	.setting .flex .active{
		color:red;
		border-bottom:2px solid red;
	}
	.setting .view{
		padding:15px;
	}
	.uni-input{
		height: 20px;
		background-color: #fff;
		padding:10px 15px;
		margin:8px 0;
	}
	.btnGroup{
		display:flex;
		justify-content: space-between;
		padding:15px
	}
	.btnGroup button{
		flex-grow: 1;
		margin: 0 10px
	}
	.uni-textarea{
		background-color: #fff;
		padding:10px 15px;
		margin:8px 0;
	}
</style>
