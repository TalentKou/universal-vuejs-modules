<template>
	<div class="TelephoneInput">
		<el-form inline>
			<el-form-item label="手机号">
				<el-input
					type="tel"
					:value="telephone"
					maxlength="11"
					placeholder="请输入手机号"
					clearable
					@input="inputHandle"
				></el-input>
			</el-form-item>
		</el-form>
	</div>
</template>

<script>
export default {
	name: 'TelephoneInput',
	data() {
		return {
			telephone: '',
		};
	},
	methods: {
		inputHandle(value) {
			if (!value.match(/[^\d]/)) {
				this.telephone = value;
			}
		},
	},
};
</script>

<style scoped>
.TelephoneInput {
	display: block;
}
</style>
