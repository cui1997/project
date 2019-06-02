<!-- html部分 -->
<template>
	<div style="width: 630px;margin: 0px auto;">
		<h1 style="margin: 25px auto;">维修记录</h1>
		<div style="text-align: left;margin-bottom: 25px;">
			<el-input style='width: 200px;margin-right: 10px;' v-model="cost" placeholder="请输入维修费用"></el-input>
			<el-button @click='search'>查询</el-button>
			<el-button @click='add'>增加</el-button>
		</div>
		<el-table :data="list" style="width: 630px;">
			<el-table-column align='center' prop="repairid" label="维修ID" width='70'></el-table-column>
			<el-table-column align='center' prop="devname" label="设备名称" width='120'></el-table-column>
			<el-table-column align='center' prop="cause" label="维修原因" width='140'></el-table-column>
			<el-table-column align='center' prop="cost" label="维修费用" width='100'></el-table-column>
			<el-table-column align='center' prop="repairdate" label="维修日期" width='100'></el-table-column>
			<el-table-column align='center' label="操作" width='100'>
				<template slot-scope="scope">
					<el-button size='small' type="text" @click='edit(scope.row)'>编辑</el-button>
					<el-button size='small' type="text" @click="del(scope.row.repairid)">删除</el-button>
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
				cost:'',
				list:[]
			};
		},
		methods:{
			add(){
				//路由跳转
				this.$router.push({path:'/RepairAdd'});
			},
			del(repairid){
				// 后端网址
				var url = this.baseUrl+"/produceRepair/delete"
				// 传递给后端的数据
				var data = {repairid:repairid};
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
				this.$router.push({name:'RepairEdit',params:{row:row.repairid}});
			},
			search(){
				// 后端网址
				var url = this.baseUrl+"/produceRepair/search"
				// 传递给后端的数据
				var data = {cost:this.cost};
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
		}
	}
</script>
<!-- css部分 -->
<style>
	
</style>
