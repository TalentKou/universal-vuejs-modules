<template>
	<div class="InnerTable">
		<div class="table">
			<div class="row head">
				<div
					class="column head"
					v-for="prop in tableData.props"
					:key="prop.field"
				>
					{{ prop.label }}
				</div>
			</div>
			<div class="row" v-for="row in tableData.data" :key="row.id">
				<div class="column" v-for="prop in tableData.props" :key="prop.field">
					<el-switch
						v-if="prop.type == 'switch'"
						v-model="row[prop.field]"
						active-color="#13ce66"
						inactive-color="#ff4949"
						disabled
					>
					</el-switch>
					<div v-else-if="prop.type == 'operates'">
						<el-button
							v-for="buttonItem in row[prop.field]"
							:key="buttonItem.event"
							@click="$emit('EventDispatch', buttonItem.event, row.id)"
							type="text"
							>{{ buttonItem.label }}</el-button
						>
					</div>
					<span v-else>{{ row[prop.field] }}</span>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'InnerTable',
	props: ['tableData'],
};
</script>

<style scoped lang="less">
.InnerTable {
	.table {
		width: 100%;
		.row {
			width: 100%;
			min-height: 48px;
			display: flex;
			align-items: center;
			border-bottom: 1px solid #ebeef5;
			transition: background-color 300ms ease-in-out;
			background-color: #ffffff;
			&:hover {
				transition: background-color 300ms ease-in-out;
				background-color: #ebeef5;
			}
			&.head:hover {
				transition: none;
				background-color: #ffffff;
			}
			.column {
				flex: 1 1 100px;
				text-align: left;
				color: #606266;
				font-size: 14px;
				font-weight: 400;
				font-family: Helvetica Neue, Helvetica, PingFang SC, Hiragino Sans GB,
					Microsoft YaHei, SimSun, sans-serif;
				padding-left: 10px;
				padding-right: 10px;
				&.head {
					color: #909399;
					font-weight: bold;
				}
			}
		}
	}
}
</style>
