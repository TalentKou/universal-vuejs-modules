<template>
	<div class="PaginationBar">
		<el-pagination
			@size-change="handleSizeChange"
			@current-change="handleCurrentChange"
			:current-page="pageData.currentPage"
			:page-sizes="[10, 20, 30, 40, 50, 60, 70, 80, 90, 100]"
			:page-size="pageData.pageSize"
			layout="total, sizes, prev, pager, next, jumper"
			:total="pageData.total"
		>
		</el-pagination>
	</div>
</template>

<script>
export default {
	name: 'PaginationBar',
	props: ['pageData'],
	methods: {
		handleSizeChange(val) {
			this.$emit('EventDispatch', 'pageSizeChanged', val);
		},
		handleCurrentChange(val) {
			this.$emit('EventDispatch', 'currentPageChanged', val);
		},
	},
};
</script>

<style scoped>
.PaginationBar {
	padding-top: 10px;
	padding-bottom: 10px;
	min-height: 48px;
	display: flex;
	align-items: center;
	justify-content: center;
}
</style>
