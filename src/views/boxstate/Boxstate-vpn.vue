<template>
	<div class="boxstate-vpn">
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
	name: 'BoxstateVpn',
	components: {
		ViewTable,
	},
	data() {
		return {
			fields: [
				{ value: 'tname', text: 'VPN 터널명', sortable: true },
				{ value: 'cluster', text: '클러스터', sortable: true },
				{ value: 'local_addr', text: 'Local Address(Network/NetMask)', sortable: true },
				{ value: 'remote_addr', text: 'Remote Address(Network/NetMask)', sortable: true },
				{ value: 'uptime', text: 'UP Time', sortable: true },
				{ value: 'status', text: '상태', sortable: true },
			],
			items: [
				{  tname: 'TEST_VPN', cluster: '사용안함', local_addr: '1.1.1.0/255.255.255.0', remote_addr: '2.2.2.0/255.255.255.0', uptime: '356시간 23분 43초', status: '연결' },
			],
		}
	},
}
</script>

<style scoped>
</style>
