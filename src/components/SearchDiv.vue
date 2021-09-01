<template>
	<v-container class="lighten-5">
		<v-row
		  justify="center"
		  no-gutters
		>
			<v-col lg="2">
				<v-select
				:items="searchOption"
				item-text="label"
				item-key="type"
				item-value="type"
				class="pa-2"
				return-object
				v-model="s_obj"
				></v-select>
			</v-col>
			<v-col md="auto">
				<v-text-field
				  class="pa-2"
				  v-model="s_value"
				></v-text-field>
			</v-col>
			<v-col lg="2">
				<v-btn class="pa-2 mt-3" outlined color="indigo" @click="btnClick">
				검 색
				</v-btn>
			</v-col>
		</v-row>
	</v-container>
</template>

<script>
export default {
	name: 'SearchDiv',
	props: {
		searchOption: Array,
		searchType: Object,
		searchValue: String,
	},
    data: () => ({
		s_obj: {},
		s_value: '',
    }),
	methods: {
		btnClick: function() {
			this.$emit('searchE', this.s_obj.type, this.s_value);
		}
	},
	watch: {
		searchType (val) {
			this.s_obj = val;
		},
		searchValue (val) {
			this.s_value = val;
		},
	},
	created() {
		this.s_obj = this.searchOption[0];
		this.s_value = this.searchValue;
	}
}
</script>
