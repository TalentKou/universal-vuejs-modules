<template>
	<div class="OperationPanel">
		<el-row type="flex" :gutter="6" class="first-row">
			<el-col v-for="inputItem in operation.inputs" :key="inputItem.id">
				<el-form inline>
					<el-form-item :label="inputItem.label">
						<el-input
							v-if="inputItem.type == 'tel'"
							:maxlength="inputItem.maxlength || 11"
							:placeholder="inputItem.placeholder"
							:value="inputItem.value"
							@input="
								(value) => {
									if (!value.match(/[^\d]/)) {
										inputItem.value = value;
									}
								}
							"
							@change="telephoneChangeHandle"
						></el-input>
						<el-input
							v-else
							:placeholder="inputItem.placeholder"
							v-model="inputItem.value"
						></el-input>
						<el-alert
							v-if="inputItem.type == 'tel'"
							v-show="!telephoneIsValid"
							title="格式不正确"
							type="error"
							:closable="false"
						>
						</el-alert>
					</el-form-item>
				</el-form>
			</el-col>
			<el-col
				v-for="datePickerItem in operation.datePickers"
				:key="datePickerItem.id"
			>
				<el-form inline>
					<el-form-item :label="datePickerItem.label">
						<el-date-picker
							v-model="datePickerItem.value"
							type="date"
							:placeholder="datePickerItem.placeholder"
						>
						</el-date-picker>
					</el-form-item>
				</el-form>
			</el-col>
			<el-col v-for="selectItem in operation.selects" :key="selectItem.id">
				<el-form inline>
					<el-form-item :label="selectItem.label">
						<el-select
							v-model="selectItem.value"
							:placeholder="selectItem.placeholder"
						>
							<el-option
								v-for="option in selectItem.options"
								:key="option.id"
								:label="option.label"
								:value="option.value"
							>
							</el-option>
						</el-select>
					</el-form-item>
				</el-form>
			</el-col>
			<el-col v-for="buttonItem in operation.buttons" :key="buttonItem.id">
				<el-button
					:type="buttonItem.type"
					@click="$emit('EventDispatch', buttonItem.event)"
					>{{ buttonItem.label }}</el-button
				>
			</el-col>
		</el-row>
	</div>
</template>

<script>
export default {
	name: 'OperationPanel',
	props: ['operation'],
	data() {
		return {
			telephoneIsValid: true,
		};
	},
	methods: {
		telephoneChangeHandle(tel) {
			this.telephoneIsValid =
				/^(13[0-9]|14[01456879]|15[0-35-9]|16[2567]|17[0-8]|18[0-9]|19[0-35-9])\d{8}$/.test(
					tel
				);
		},
	},
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
