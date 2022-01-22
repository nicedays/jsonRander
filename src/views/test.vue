<template>
	<div class="test_main">
		<div class="modle">
			<button @click="changType('fast')">快速注册</button>
			<button @click="changType('all')">完整注册</button>
		</div>
		<component v-for="(item,index) in comArr" :is="item.comsName" :key="index" @getParm="getParm(arguments)"
			:jsonObj="item.comsObj">
		</component>
		<button @click="onSumit" class="sumit">提交</button>
	</div>
</template>

<script>
	import coms from '../components/options.json'
	export default {
		name: "test",
		data() {
			return {
				comArr: [],// 组件
				showType: "fast",//当前模式
				parms: {}// 所有参数
			}
		},
		methods: {
			// 切换模式
			changType(type) {
				this.showType = type;
				this.getComs();
			},
			// 获取组件返回的值
			getParm(arg) {
				this.parms[arg[0]] = arg[1];
			},
			// 获取当前模式下上送参数
			getReqParms(){
				var newObj={};
				coms.components.forEach(item=>{
					if (item.showType.includes(this.showType)) {
						newObj[item.name]=this.parms[item.name]
					}
				})
				return newObj;
			},
			// 提交
			onSumit() {
				if (this.checkInput()) {
					console.log(this.getReqParms())
				} else {
					console.log("请输入完整");
				}
			},
			// 判断是否完整输入必输项
			checkInput() {
				var res=true;
				coms.components.forEach(item => {
					if (item.showType.includes(this.showType)) {
						if (item.required && (this.parms[item.name] === '' || this.parms[item.name] ===
							undefined)) {
							res= false;
						}
					}

				})
				return res;
			},
			// 动态渲染组件
			getComs() {
				this.comArr = []
				coms.components.forEach((item) => {
					if (item.showType.includes(this.showType)) {
						var comItem = require(`@components/${item.path}`)
						this.comArr.push({
							comsName: comItem.default,
							comsObj: item
						})
					}

				})
			}

		},
		// 第一次进来动态渲染组件
		created() {
			this.getComs()
		},


	}
</script>

<style scoped>
	.test_main {
		width: 500px;
		margin: auto auto;
	}

	.modle {
		width: 500px;
		display: flex;
		margin-bottom: 20px;
	}

	.modle button {
		width: 50%;
	}

	.sumit {
		width: 500px;
		margin-top: 20px;
	}
</style>
