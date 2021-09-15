<template>
	<div class="GeneralTableCompo" v-loading="isLoading">
		<OperationPanel :operation="operation" @EventDispatch="eventDispatch" />
		<InnerTable :tableData="computedTableData" @EventDispatch="eventDispatch" />
		<PaginationBar
			:pageData="computedPageData"
			@EventDispatch="eventDispatch"
		/>
	</div>
</template>

<script>
import moment from 'moment';

import OperationPanel from './OperationPanel.vue';
import InnerTable from './InnerTable.vue';
import PaginationBar from './PaginationBar.vue';

import mockData from './mockData';

export default {
	name: 'GeneralTableCompo',
	components: {
		OperationPanel,
		InnerTable,
		PaginationBar,
	},
	data() {
		return {
			isLoading: false,
			originalData: null,
			operation: {
				inputs: [
					{
						id: 'input-0',
						type: 'number',
						label: '编号:',
						placeholder: '请输入编号',
						value: '',
						name: 'id',
					},
					{
						id: 'input-1',
						//type: 'text'
						label: '名字:',
						placeholder: '请输入名字',
						value: '',
						name: 'name',
					},
				],
				selects: [
					{
						id: 'select-0',
						label: '状态:',
						placeholder: '请选择状态',
						value: '',
						name: 'isMarried',
						options: [
							{ id: 'option-0', label: '已婚', value: true },
							{ id: 'option-1', label: '未婚', value: false },
						],
					},
				],
				datePickers: [
					{
						id: 'date-picker-0',
						label: '创建时间:',
						placeholder: '请选择创建时间',
						value: '',
						name: 'createDate',
					},
					{
						id: 'date-picker-1',
						label: '更新时间:',
						placeholder: '请选择更新时间',
						value: '',
						name: 'updateDate',
					},
				],
				buttons: [
					{ id: 'button-0', type: 'info', label: '搜索', event: 'search' },
					{ id: 'button-1', type: 'info', label: '重置', event: 'reset' },
					{ id: 'button-2', type: 'primary', label: '新增', event: 'add' },
				],
			},
			searchData: {
				id: '',
				name: '',
				createDate: '',
				updateDate: '',
				isMarried: '',
			},
			pageData: {
				pageSize: 10, //每页数据条数
				currentPage: 1, //当前页数
			},
		};
	},
	created() {
		//从服务器请求数据
		this.isLoading = true;
		setTimeout(() => {
			let data = mockData.data;
			//格式化数据
			this.originalData = this.formatData(data);

			this.isLoading = false;
		}, 1000);
	},
	computed: {
		filteredData() {
			try {
				//来自服务器的经过格式转换的列表数据
				let list = this.originalData.data;

				//由过滤器过滤出来的列表数据
				let searchData = this.searchData;
				list = list.filter((item) => {
					let res =
						(searchData.id === '' || item.id == searchData.id) &&
						(searchData.name === '' || item.name == searchData.name) &&
						(searchData.createDate === '' ||
							item.createDate == this.dateFormatter(searchData.createDate)) &&
						(searchData.updateDate === '' ||
							item.updateDate == this.dateFormatter(searchData.updateDate)) &&
						(searchData.isMarried === '' ||
							item.isMarried === searchData.isMarried);

					return res;
				});

				return list;
			} catch (error) {
				return [];
			}
		},
		computedPageData() {
			return Object.assign({ total: this.filteredData.length }, this.pageData);
		},
		computedTableData() {
			try {
				//由分页器定位的当前页的列表数据
				let list = this.filteredData;
				let pageData = this.computedPageData;
				list = list.slice(
					pageData.pageSize * (pageData.currentPage - 1),
					pageData.pageSize * pageData.currentPage
				);

				return { props: this.originalData.props, data: list };
			} catch (error) {
				return { props: [], data: [] };
			}
		},
	},
	methods: {
		dateFormatter(date, fmt) {
			return moment(date).format(fmt || 'YYYY-MM-DD');
		},
		formatData(data) {
			//格式化数据以满足各个组件中的数据结构要求
			let result = {
				props: [
					{ field: 'id', label: '编号' },
					{ field: 'name', label: '名字' },
					{ field: 'createDate', label: '创建时间' },
					{ field: 'updateDate', label: '更新时间' },
					{
						field: 'isMarried',
						label: '状态',
						type: 'switch',
						activeText: '已婚',
						inactiveText: '未婚',
					},
					{ field: 'operates', label: '操作', type: 'operates' },
				],
				data: [],
			};

			result.data = data.map((item) => {
				item.operates = [
					{ event: 'edit', label: '编辑' },
					{ event: 'delete', label: '删除' },
				];
				return item;
			});

			return result;
		},
		eventDispatch(event, data) {
			this[event](data);
		},
		search() {
			for (let p in this.operation) {
				this.operation[p].forEach((item) => {
					if (item.name) {
						this.searchData[item.name] = item.value;
					}
				});
			}
			this.pageData.currentPage = 1;
		},
		reset() {
			for (let p in this.operation) {
				this.operation[p].forEach((item) => {
					if (item.name) {
						item.value = '';
						this.searchData[item.name] = '';
					}
				});
			}
			this.pageData.currentPage = 1;
		},
		add() {
			console.log('添加');
		},
		edit(data) {
			console.log(`编辑 ${data}`);
		},
		delete(data) {
			console.log(`删除 ${data}`);
		},
		pageSizeChanged(val) {
			console.log(`改变每页数据条数为：${val}`);
			this.pageData.pageSize = val;
		},
		currentPageChanged(val) {
			this.pageData.currentPage = val;
		},
	},
};
</script>

<style scoped>
.GeneralTableCompo {
	display: block;
}
</style>
