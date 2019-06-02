<!-- html部分 -->
<template>
	<div style="width: 830px;margin: 0px auto;">
		<h1 style="margin: 25px auto;">人员信息</h1>
		<div style="text-align: left;margin-bottom: 25px;">
			<el-input style='width: 200px;' v-model="username" placeholder="请输入用户名"></el-input>
			<el-select style='width: 200px;margin-right: 10px;' v-model="compid" clearable placeholder="请选择所属企业">
				<el-option v-for='obj in compArr' :key="obj.compid" :label='obj.compname' :value='obj.compid'></el-option>
			</el-select>
			<el-button @click='search'>查询</el-button>
			<el-button @click='add'>增加</el-button>
		</div>
		<el-table :data="list" style='width: 830px;'>
			<el-table-column align='center' prop="userid" label="用户ID" width='70'></el-table-column>
			<el-table-column align='center' prop="cname" label="姓名" width='80'></el-table-column>
			<el-table-column align='center' prop="username" label="用户名" width='80'></el-table-column>
			<el-table-column align='center' prop="password" label="密码" width='80'></el-table-column>
			<el-table-column align='center' prop="telno" label="手机号" width='120'></el-table-column>
			<el-table-column align='center' prop="email" label="邮箱" width='140'></el-table-column>
			<el-table-column align='center' prop="sex" label="性别" width='80'></el-table-column>
			<el-table-column align='center' prop="baseCompany.compname" label="所属企业" width='80'></el-table-column>
			<el-table-column align='center' label="操作" width='100'>
				<template slot-scope="scope">
					<el-button size='small' type="text" @click='edit(scope.row)'>编辑</el-button>
					<el-button size='small' type="text" @click="del(scope.row.userid)">删除</el-button>
				</template>
			</el-table-column>
		</el-table>
	</div>
</template>
<!-- js部分 -->
<script>
	export default {
		data() {
			return {
				compid:'',
				username:'',
				list:[],
				compArr:[]
			};
		},
		methods:{
			add(){
				//路由跳转
				this.$router.push({path:'/UserAdd'});
			},
			del(userid){
				// 后端网址
				var url = this.baseUrl+"/baseUser/delete"
				// 传递给后端的数据
				var data = {userid:userid};
				this.$axios.post(url,this.$qs.stringify(data),{
					headers: {
						'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8'
					}
				}).then(res=>{
					// res是后端的响应
					this.$message("删除成功");
					this.search();
				})
			},
			edit(row){
				//路由跳转
				this.$router.push({name:'UserEdit',params:{row:row.userid}});
			},
			search(){
				// 后端网址
				var url = this.baseUrl+"/baseUser/search"
				// 传递给后端的数据
				var data = {compid:this.compid,username:this.username};
				this.$axios.post(url,this.$qs.stringify(data),{
					headers: {
						'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8'
					}
				}).then(res=>{
					this.list = res.data;
				})
			}
		},
		mounted:function(){
			this.search();
			// 后端网址
			var url = this.baseUrl+"/baseCompany/list"
			this.$axios.post(url,{
				headers: {
					'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8'
				}
			}).then(res=>{
				this.compArr = res.data;
			})
		}
	}
</script>
<!-- css部分 -->
<style>
	
</style>
