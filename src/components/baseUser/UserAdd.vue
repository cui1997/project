<!-- html部分 -->
<template>
	<div>
		<h1 style="margin: 25px auto 50px auto;">人员信息添加页面</h1>
		<el-row style='margin-bottom: 20px;'>
			<el-col :span="12"><el-input style='width:45%' v-model='cname' placeholder='请输入姓名'></el-input></el-col>
			<el-col :span="12"><el-input style='width:45%' v-model='username' placeholder='请输入用户名'></el-input></el-col>
		</el-row>
		<el-row style='margin-bottom: 20px;'>
			<el-col :span="12"><el-input style='width:45%' v-model='password' placeholder='请输入密码'></el-input></el-col>
			<el-col :span="12"><el-input style='width:45%' v-model='telno' placeholder='请输入手机号'></el-input></el-col>
		</el-row>
		<el-row style='margin-bottom: 20px;'>
			<el-col :span="12"><el-input style='width:45%' v-model='email' placeholder='请输入邮箱'></el-input></el-col>
			<el-col :span="12">
				<el-select v-model='sex' style='width:45%' placeholder="请选择性别">
					<el-option value='男'>男</el-option>
					<el-option value='女'>女</el-option>
				</el-select>
			</el-col>
		</el-row>
		<el-row style='margin-bottom: 50px;'>
			<el-col :span="12">
				<el-select v-model='compid' style='width:45%' placeholder="请选择所属企业">
					<el-option v-for='obj in userArr' :key="obj.compid" :label='obj.compname' :value='obj.compid'></el-option>
				</el-select>
			</el-col>
		</el-row>
		<el-row>
			<el-col :span="24">
				<el-button @click='save'>添加</el-button>
				<el-button @click='cancel'>返回</el-button>
			</el-col>
		</el-row>
	</div>
</template>
<!-- js部分 -->
<script>
	export default {
		data() {
			return {
				cname:'',
				username:'',
				password:'',
				telno:'',
				email:'',
				sex:'',
				compid:'',
				userArr:[]
			};
		},
		methods:{
			save(){
				// 后端网址
				var url = this.baseUrl+"/baseUser/insert"
				// 传递给后端的数据
				var data = {cname:this.cname,username:this.username,password:this.password,telno:this.telno,email:this.email,sex:this.sex,compid:this.compid};
				this.$axios.post(url,this.$qs.stringify(data),{
					headers: {
						'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8'
					}
				}).then(res=>{
					// res是后端的响应
					this.$message("添加成功");
					this.$router.push({path:'/UserList'});
				})
			},
			cancel(){
				this.$router.go(-1);
			}
		},
		mounted:function(){
			// 后端网址
			var url = this.baseUrl+"/baseCompany/list"
			this.$axios.post(url,{
				headers: {
					'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8'
				}
			}).then(res=>{
				this.userArr = res.data;
			})
		}
	}
</script>
<!-- css部分 -->
<style>

</style>
