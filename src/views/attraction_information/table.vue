<template>
	<el-main class="bg table_wrap comtabel_t">
		<el-form label-position="right" :model="query" class="form p_4" label-width="120">
			<el-row class="rows row1">



							<el-col :xs="24" :sm="24" :lg="8" class="el_form_search_wrap">
					<el-form-item label="景点名称">
									<el-input v-model="query.attraction_name"></el-input>
								</el-form-item>
				</el-col>
									<el-col :xs="24" :sm="24" :lg="8" class="el_form_search_wrap">
					<el-form-item label="景点类型">
									<el-select v-model="query.types_of_attractions">
											<el-option v-for="o in list_types_of_attractions" :key="o.types_of_attractions" :label="o.types_of_attractions"
								:value="o.types_of_attractions">
							</el-option>
										</el-select>
								</el-form-item>
				</el-col>
									<el-col :xs="24" :sm="24" :lg="8" class="el_form_search_wrap">
					<el-form-item label="景点城市">
									<el-input v-model="query.attraction_cities"></el-input>
								</el-form-item>
				</el-col>
									<el-col :xs="24" :sm="24" :lg="8" class="el_form_search_wrap">
					<el-form-item label="景点地址">
									<el-input v-model="query.attraction_address"></el-input>
								</el-form-item>
				</el-col>
																									</el-row>
	<el-row class="rows row2">

		<el-col :xs="24" :sm="24" :lg="24" class="search_btn_wrap search_btns">

				<el-col :xs="24" :sm="10" :lg="8" class="search_btn_1 search_btn_wrap_1 btns">

										<el-button type="primary" @click="search()" class="search_btn_find">查询</el-button>
						<el-button @click="reset()" style="margin-right: 74px;" class="search_btn_reset">重置</el-button>
																		

						<el-button v-if="user_group == '管理员' || $check_action('/attraction_information/table','del') || $check_action('/attraction_information/view','del')" class="search_btn_del" type="danger" @click="delInfo()">删除</el-button>
								
				</el-col>
		</el-col>
	</el-row >

		</el-form>
		<el-table :data="list" @selection-change="selectionChange" @sort-change="$sortChange" style="width: 100%" id="dataTable">
			<el-table-column fixed type="selection" tooltip-effect="dark" width="55">
			</el-table-column>
				<el-table-column prop="attraction_name" @sort-change="$sortChange" label="景点名称"
				v-if="user_group == '管理员' || $check_field('get','attraction_name')" min-width="200">
					</el-table-column>
					<el-table-column prop="types_of_attractions" @sort-change="$sortChange" label="景点类型"
				v-if="user_group == '管理员' || $check_field('get','types_of_attractions')" min-width="200">
					</el-table-column>
					<el-table-column prop="attraction_cities" @sort-change="$sortChange" label="景点城市"
				v-if="user_group == '管理员' || $check_field('get','attraction_cities')" min-width="200">
					</el-table-column>
					<el-table-column prop="attraction_address" @sort-change="$sortChange" label="景点地址"
				v-if="user_group == '管理员' || $check_field('get','attraction_address')" min-width="200">
					</el-table-column>
					<el-table-column prop="admission_price" @sort-change="$sortChange" label="门票价格"
				v-if="user_group == '管理员' || $check_field('get','admission_price')" min-width="200">
					</el-table-column>
					<el-table-column prop="opening_hours" @sort-change="$sortChange" label="开放时间"
				v-if="user_group == '管理员' || $check_field('get','opening_hours')" min-width="200">
					</el-table-column>
					<el-table-column prop="map_link" @sort-change="$sortChange" label="地图链接"
				v-if="user_group == '管理员' || $check_field('get','map_link')" min-width="200">
					<template slot-scope="scope">
				<a :href="scope.row['map_link']" target="_blank">{{ scope.row['map_link'] }}</a>
			</template>
					</el-table-column>
					<el-table-column prop="appointment_points" @sort-change="$sortChange" label="预约积分"
				v-if="user_group == '管理员' || $check_field('get','appointment_points')" min-width="200">
					</el-table-column>
					<el-table-column prop="tourist_attraction_routes" @sort-change="$sortChange" label="景点路线"
				v-if="user_group == '管理员' || $check_field('get','tourist_attraction_routes')" min-width="200">
					</el-table-column>
					<el-table-column prop="cover_photo" @sort-change="$sortChange" label="封面图片"
				v-if="user_group == '管理员' || $check_field('get','cover_photo')" min-width="200">
						<template slot-scope="scope">
					<el-image style="width: 100px; height: 100px" :src="$fullUrl(scope.row['cover_photo'])"
						:preview-src-list="[$fullUrl(scope.row['cover_photo'])]">
						<div slot="error" class="image-slot">
							<img src="../../../public/img/error.png" style="width: 90px; height: 90px" />
						</div>
					</el-image>
				</template>
					</el-table-column>
			



            <el-table-column sortable prop="create_time" label="创建时间" min-width="200">
                <template slot-scope="scope">
                	{{ $toTime(scope.row["create_time"],"yyyy-MM-dd hh:mm:ss") }}
                </template>
            </el-table-column>

			<el-table-column sortable prop="update_time" label="更新时间" min-width="200">
                <template slot-scope="scope">
                	{{ $toTime(scope.row["update_time"],"yyyy-MM-dd hh:mm:ss") }}
                </template>
			</el-table-column>







			<el-table-column fixed="right" label="操作" min-width="200" v-if="user_group == '管理员' || $check_action('/attraction_information/table','set') || $check_action('/attraction_information/view','set') || $check_action('/attraction_information/view','get') || $check_action('/预约景点/table','add') || $check_action('/预约景点/view','add')" >


				<template slot-scope="scope">
					<div class="view_a">
					<router-link class="e-button el-button--small is-plain el-button--success" style="margin: 5px !important;"
					v-if="user_group == '管理员' || $check_action('/attraction_information/table','set') || $check_action('/attraction_information/view','set') || $check_action('/attraction_information/view','get')"
						:to="'./view?' + field + '=' + scope.row[field]"
						 size="small">
						<span>详情</span>
					</router-link>
						<router-link v-if="user_group == '管理员' || $check_comment('/attraction_information/table')" class="e-button el-button--small is-plain el-button--primary"
								  :to="'../comment/table?size=10&page=1&source_table=attraction_information&source_field=' + field + '&source_id=' + scope.row[field]" size="small">
						<span>查看评论</span>
					</router-link>
					</div>
				</template>
			</el-table-column>

		</el-table>

		<!-- 分页器 -->
		<div class="mt text_center">
			<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
				:current-page="query.page" :page-sizes="[7, 10, 30, 100]" :page-size="query.size"
				layout="total, sizes, prev, pager, next, jumper" :total="count">
			</el-pagination>
		</div>
		<!-- /分页器 -->

											
		<div class="modal_wrap" v-if="showModal">
			<div class="modal_box">
				<!-- <div class="modal_box_close" @click="closeModal">X</div> -->
				<p class="modal_box_title">重要提醒</p>
				<p class="modal_box_text">当前有数据达到预警值！</p>
				<div class="btn_box">
					<span @click="closeModal">取消</span>
					<span @click="closeModal">确定</span>
				</div>
			</div>
		</div>


	</el-main>
</template>
<script>
	import mixin from "@/mixins/page.js";

	export default {
		mixins: [mixin],
		data() {
			return {
				// 弹框
				showModal: false,
				// 获取数据地址
				url_get_list: "~/api/attraction_information/get_list?like=0",
				url_del: "~/api/attraction_information/del?",

				// 字段ID
				field: "attraction_information_id",

				// 查询
				query: {
					"size": 7,
					"page": 1,
								"attraction_name": "",
											"types_of_attractions": "",
											"attraction_cities": "",
											"attraction_address": "",
															"login_time": "",
					"create_time": "",
					"orderby": `create_time desc`
				},

				// 数据
				list: [],
																		// 景点类型列表
				list_types_of_attractions: [""],
																							}
		},
		methods: {
			// 关闭弹框
			closeModal(){
				this.showModal = false;
				},


						/**
			 * 获取景点类型列表
			 */
			async get_list_types_of_attractions() {
				var json = await this.$get("~/api/types_of_attractions/get_list?");
				if(json.result){
					this.list_types_of_attractions = json.result.list;
				}else if (json.error){
					console.log(json.error);
				}
			},
																				



																								},
		created() {
						// 初始化景点类型列表
			this.get_list_types_of_attractions();
																						}
	}
</script>

<style type="text/css">
	.bg {
		background: white;
	}

	.form.p_4 {
		padding: 1rem;
	}

	.form .el-input {
		width: initial;
	}

	.mt {
		margin-top: 1rem;
	}

	.text_center {
		text-align: center;
	}

	.float-right {
		float: right;
	}


	.modal_wrap{
		width: 100vw;
		height: 100vh;
		position: fixed;
		top: 0;
		left: 0;
		background: rgba(0,0,0,0.5);
		z-index: 9999999999;
	}
	.modal_wrap .modal_box{
		width: 400px;
		height: 200px;
		background: url("../../assets/modal_bg.jpg") no-repeat center;
		background-size: cover;
		position: absolute;
		top: 50%;
		left: 50%;
		margin-left: -200px;
		margin-top: -100px;
		border-radius: 10px;
		}
	.modal_wrap .modal_box .modal_box_close{
		font-size: 20px;
		position: absolute;
		top: 10px;
		right: 10px;
		cursor: pointer;
		}
	.modal_wrap .modal_box .modal_box_title{
	  text-align: center;
    font-size: 18px;
    margin: 16px auto;
    color: #fff;
    border-bottom: 1px solid rgba(117, 116, 116,0.5);
    padding-bottom: 16px;
    width: 356px;
		}
	.modal_wrap .modal_box .modal_box_text{
			text-align: center;
		font-size: 15px;
		color: #fff;
		margin-top: 25px;
		}
	.modal_wrap .modal_box .btn_box{
		display: flex;
		flex-direction: row;
		justify-content: center;
		margin-top: 42px;
		}
			.modal_wrap .modal_box .btn_box span{
				display: inline-block;
				width: 80px;
				height: 30px;
				line-height: 30px;
				text-align: center;
				border: 1px solid #ccc;
				font-size: 14px;
				cursor: pointer;
				color: #fff;
			}
	.modal_wrap .modal_box .btn_box span:nth-child(2){
		background: #409EFF;
		color: #fff;
		border-color: #409EFF;
		margin-left: 15px;
	}
</style>
