<template>
	<div class="PaginationBar">
		<el-pagination
			@size-change="handleSizeChange"
			@current-change="handleCurrentChange"
			:current-page="currentPage"
			:page-sizes="[10, 20, 30, 40]"
			:page-size="10"
			layout="total, sizes, prev, pager, next, jumper"
			:total="400"
		>
		</el-pagination>
	</div>
</template>

<script>
export default {
	name: 'PaginationBar',
	data() {
		return {
			currentPage: 1,
		};
	},
	methods: {
		handleSizeChange() {},
		handleCurrentChange() {},
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
