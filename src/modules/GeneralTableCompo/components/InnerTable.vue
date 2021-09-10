<template>
	<div class="InnerTable">
		<el-popover class="column-control" placement="left-start" trigger="click">
			<el-checkbox :value="isAllchecked" @change="checkAllChanged">{{
				isAllchecked ? '全选' : '全不选'
			}}</el-checkbox>
			<el-checkbox-group
				class="column-choose-checkbox-group"
				v-model="checkList"
			>
				<el-checkbox
					v-for="prop in tableData.props"
					:key="prop.field"
					:label="prop.label"
				></el-checkbox>
			</el-checkbox-group>
			<i class="el-icon-s-tools" slot="reference"></i>
		</el-popover>
		<div class="table">
			<div class="row head">
				<div
					class="column head"
					v-for="prop in computedTableProps"
					:key="prop.field"
				>
					{{ prop.label }}
				</div>
			</div>
			<div class="row" v-for="row in tableData.data" :key="row.id">
				<div
					class="column"
					v-for="prop in computedTableProps"
					:key="prop.field"
				>
					<el-switch
						v-if="prop.type == 'switch'"
						v-model="row[prop.field]"
						active-text="有效"
						inactive-text="禁用"
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
	data() {
		return {
			isAllchecked: false,
			checkList: [],
		};
	},
	computed: {
		computedTableProps() {
			let checkList = this.checkList;
			let cProps = this.tableData.props.filter((item) => {
				return checkList.includes(item.label);
			});
			return cProps;
		},
	},
	mounted() {
		this.checkAllChanged(true);
	},
	methods: {
		checkAllChanged(status) {
			this.isAllchecked = status;
			if (status) {
				this.checkList = this.tableData.props.map((item) => item.label);
			} else {
				this.checkList.splice(0);
			}
		},
	},
};
</script>

<style scoped lang="less">
.InnerTable {
	.column-control {
		float: right;
		&::after {
			content: '';
			clear: both;
		}
	}
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

<style lang="less">
.column-choose-checkbox-group {
	display: flex;
	flex-flow: column;
}
</style>
