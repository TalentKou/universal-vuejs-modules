<template>
	<div class="GeneralTableCompo">
		<OperationPanel :operation="operation" @EventDispatch="eventDispatch" />
		<InnerTable :tableData="tableData" @EventDispatch="eventDispatch" />
		<PaginationBar />
	</div>
</template>

<script>
import OperationPanel from './OperationPanel.vue';
import InnerTable from './InnerTable.vue';
import PaginationBar from './PaginationBar.vue';
export default {
	name: 'GeneralTableCompo',
	components: {
		OperationPanel,
		InnerTable,
		PaginationBar,
	},
	data() {
		return {
			operation: {
				inputs: [{ placeholder: '请输入模板名称', value: '', name: 't0' }],
				selects: [
					{
						value: '',
						placeholder: '请选择模板类型',
						name: 's0',
						options: [
							{ label: '模板0', value: 'template0', name: 's0t0' },
							{ label: '模板1', value: 'template1', name: 's0t1' },
							{ label: '模板2', value: 'template2', name: 's0t2' },
						],
					},
					{
						value: '',
						placeholder: '请选择状态',
						name: 's1',
						options: [
							{ label: '状态0', value: 'status0', name: 's1t0' },
							{ label: '状态1', value: 'status1', name: 's1t1' },
							{ label: '状态2', value: 'status2', name: 's1t2' },
						],
					},
					{
						value: '',
						placeholder: '请选择节点',
						name: 's2',
						options: [
							{ label: '节点0', value: 'node0', name: 's2t0' },
							{ label: '节点1', value: 'node1', name: 's2t1' },
							{ label: '节点2', value: 'node2', name: 's2t2' },
						],
					},
				],
				buttons: [
					{ type: 'info', name: '搜索', event: 'search' },
					{ type: 'info', name: '重置', event: 'reset' },
					{ type: 'primary', name: '新增', event: 'add' },
				],
			},
			tableData: {
				props: [
					{ field: 'templateName', label: '模板名称' },
					{ field: 'templateType', label: '模板类型' },
					{ field: 'createDate', label: '创建时间' },
					{ field: 'updateDate', label: '更新时间' },
					{ field: 'status', label: '状态', type: 'switch' },
					{ field: 'operates', label: '操作', type: 'operates' },
				],
				data: [
					{
						id: '0',
						templateName: '客户申请表单',
						templateType: '默认表单',
						createDate: '2021-09-09',
						updateDate: '2021-09-09',
						status: true,
						operates: [
							{ event: 'edit', label: '编辑' },
							{ event: 'delete', label: '删除' },
						],
					},
					{
						id: '1',
						templateName: '客户申请表单1',
						templateType: '默认表单1',
						createDate: '2021-09-10',
						updateDate: '2021-09-10',
						status: false,
						operates: [
							{ event: 'edit', label: '编辑' },
							{ event: 'delete', label: '删除' },
						],
					},
					{
						id: '2',
						templateName: '客户申请表单2',
						templateType: '默认表单2',
						createDate: '2021-09-11',
						updateDate: '2021-09-11',
						status: true,
						operates: [
							{ event: 'edit', label: '编辑' },
							{ event: 'delete', label: '删除' },
						],
					},
				],
			},
		};
	},
	methods: {
		eventDispatch(event, data) {
			this[event](data);
		},
		search() {
			for (let p in this.operation) {
				this.operation[p].forEach((item) => {
					item.value && console.log(item.value);
				});
			}
		},
		reset() {
			for (let p in this.operation) {
				this.operation[p].forEach((item) => {
					item.value && (item.value = '');
				});
			}
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
	},
};
</script>

<style scoped>
.GeneralTableCompo {
	display: block;
}
</style>
