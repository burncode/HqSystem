<template lang="html">
	<div class="login">
	    <div class="login-header">
	      <div class="title">
	      	<p class="en-title">HQMS</p>
	      	<p class="zh-title">排队叫号管理系统</p>
	      </div>
	    </div>
	    <vue-form :state="formstate" @submit.prevent="saveForm" class="saveForm">
	        <validate  class="form-group">
	          <span>用户名</span>
	          <input v-model="form.userName" required name="userName" class="form-control"  :class="[fieldClassName(formstate.userName)]"/>
	        </validate>
	        <validate  class="form-group">
	          <span>密码</span>
	          <input v-model="form.passwd" class="form-control" name="passwd" type="password" required :class="[fieldClassName(formstate.passwd)]" />
	        </validate>
	        <button type="submit" class="btn btn-primary btn-block">登录</button>
	      </vue-form>
	</div>
</template>
<script>
    import Vue from 'vue'
	import VueForm from 'vue-form'
	import utils from 'common/utils/utils.js'
	Vue.use(VueForm)
	export default {
		name: 'login',
		data() {
			return {
				formstate: {},
				form: {
					userName: '',
					passwd: ''
				},
				formClass: {
					formControl: true
				}
			}
		},
		computed: {
			serverUrl() {
				return this.$store.getters.postUrl('login')
			}
		},
		components: {
		},
		mounted() {
			console.log(this.$router)
		},
		methods: {
			saveForm() {
				if (this.formstate.$invalid) {
					return;
				}
				console.log(this.serverUrl)
				this.axios.post(this.serverUrl, {
					action: 'GetToken',
					userName: this.form.userName,
					passwd: this.form.passwd
				}).then((res) => {
						this.$store.commit('login', res.token);
						// 路由跳转
						// todo
						// this.$route 和 this.$router不一样
						this.$router.push('/manage')
				}, (res) => {
					console.log('failed')
				})
			},
			fieldClassName(field) {
               return utils.fieldClassName(field)
			}
		}
	}
</script>

<style scoped>
.login-header {
	height: 280px;
	background: #0097FB;
	width:100%;
}
.title {
	width:500px;
	margin:0 auto;
	text-align: center;
	position: relative;
	top:50%;
	transform: translateY(-50%);
}
.en-title {
	font-family: 'Arial-Black';
	color: #fff;
	font-size: 52px;
	height:73px;
}
.zh-title {
	font-family: 'PingFangSC-Regular';
	color: #fff;
	font-size: 30px;
	height:42px;
	text-shadow: 0 2px 4px;
}
.saveForm {
	width:400px;
	margin:20px auto 0;
}


</style>