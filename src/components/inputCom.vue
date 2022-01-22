<template>
	<div class="list_main">
		<div class="list_left">
			<span v-if="jsonObj.required">*</span>{{jsonObj.text}}
		</div>
		<div class="list_right">
			<input @blur="onBlur" :type="jsonObj.type" :maxlength="jsonObj.length" v-model="myInput" name="" />
		</div>
	</div>
</template>

<script>
	export default{
		name:'inputCom',
		data(){
			return{
				myInput:""
			}
		},
		props:{
			jsonObj:{
				type:Object,
				required:true
			}
		},
		methods:{
			onBlur(){
				this.$emit("getParm",this.jsonObj.name,this.myInput)
			}
		}
	}
</script>

<style>
	.list_main{
		width: 500px;
		display: flex;
	}
	.list_left{
		width: 30%;
	}
	.list_left span{
		color: red;
		float: left;
	}
	.list_right{
		width: 70%;
	}
</style>
