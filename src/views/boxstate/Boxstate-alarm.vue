<template>
	<div class="boxstate-alarm">
		<ViewTable 
		:fields="fields"
		:items="items"
		/>
	</div>
</template>

<script>
// @ is an alias to /src
import ViewTable from '@/components/ViewTable.vue'

export default {
	name: 'BoxstateAlarm',
	components: {
		ViewTable,
	},
	data() {
		return {
			fields: [
				{ value: 'type', text: '알람 유형', sortable: true },
				{ value: 'status', text: '알람 상태', sortable: true },
				{ value: 'otime', text: '발생시간', sortable: true },
				{ value: 'ctime', text: '복구시간', sortable: true },
				{ value: 'content', text: '알람내용', sortable: true },
				{ value: 'info', text: '알람정보', sortable: true },
			],
			items: [
				{  type: 'CPU 임계치 초과', status: '발생', otime: '2021-05-02 12:02:22', ctime: '', content: '임계치 초과 알람', info: 'cpu' },
				{  type: 'CPU 임계치 초과', status: '발생', otime: '2021-05-02 12:02:22', ctime: '', content: '임계치 초과 알람', info: 'cpu' },
				{  type: 'CPU 임계치 초과', status: '발생', otime: '2021-05-02 12:02:22', ctime: '', content: '임계치 초과 알람', info: 'cpu' },
				{  type: 'CPU 임계치 초과', status: '발생', otime: '2021-05-02 12:02:22', ctime: '', content: '임계치 초과 알람', info: 'cpu' },
				{  type: 'CPU 임계치 초과', status: '발생', otime: '2021-05-02 12:02:22', ctime: '', content: '임계치 초과 알람', info: 'cpu' },
			],
		}
	},
}
</script>

<style scoped>
</style>
