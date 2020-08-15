<template>
	<div class="toast" v-show="isshow">
		<div>{{message}}</div>
	</div>
</template>

<script>
	export default{
		name:"Toast",
		props:{
			// message:{
			// 	type:String,
			// 	default:''
			// },
			// isshow:{
			// 	type:Boolean,
			// 	default:false
			// }
		},
		data(){
			return {
				message:'',
				isshow:false
			}
		},
		methods:{
			show(message='默认文字',duration=2000){
				this.isshow = true;
				this.message = message;
				setTimeout(() => {
					this.isshow = false;
					this.message = ''
				},duration)
			}
		}
	}
</script>

<style scoped>
	.toast{
		position: fixed;
		left: 50%;
		top: 50%;
		transform: translate(-50%,-50%);
		z-index: 999;
		padding: 8px 10px;
		color: #fff;
		background-color: rgba(0,0,0,.75);
	}
</style>
