<template>
	<div class="OperationPanel">
		<el-row type="flex" :gutter="6" class="first-row">
			<el-col v-for="inputItem in operation.inputs" :key="inputItem.name">
				<el-input
					:placeholder="inputItem.placeholder"
					v-model="inputItem.value"
				></el-input>
			</el-col>
			<el-col v-for="selectItem in operation.selects" :key="selectItem.name">
				<el-select
					v-model="selectItem.value"
					:placeholder="selectItem.placeholder"
				>
					<el-option
						v-for="option in selectItem.options"
						:key="option.name"
						:label="option.label"
						:value="option.value"
					>
					</el-option>
				</el-select>
			</el-col>
			<el-col v-for="buttonItem in operation.buttons" :key="buttonItem.name">
				<el-button
					:type="buttonItem.type"
					@click="$emit('EventDispatch', buttonItem.event)"
					>{{ buttonItem.name }}</el-button
				>
			</el-col>
		</el-row>
	</div>
</template>

<script>
export default {
	name: 'OperationPanel',
	props: ['operation'],
};
</script>

<style scoped lang="less">
.OperationPanel {
	.first-row {
		flex-flow: wrap;
	}
	.first-row > .el-col {
		margin-bottom: 6px;
		width: auto;
		.el-input {
			min-width: 202px;
		}
		.el-select {
			min-width: 217px;
		}
	}
}
</style>
