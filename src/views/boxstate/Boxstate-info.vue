<template>
	<div class="boxstate-info">
		<div>
			<v-btn
			  class="ma-2"
			  outlined
			  color="indigo"
			  @click="info_type = 1"
			>
			  장비정보
			</v-btn>
			<v-btn
			  class="ma-2"
			  outlined
			  color="indigo"
			  @click="info_type = 2"
			>
			  지점정보
			</v-btn>
			<v-btn
			  class="ma-2"
			  outlined
			  color="indigo"
			  @click="info_type = 3"
			>
			  회선정보
			</v-btn>
			<v-btn
			  class="ma-2"
			  outlined
			  color="indigo"
			  @click="info_type = 4"
			>
			  인터페이스
			</v-btn>
			<v-btn
			  class="ma-2"
			  outlined
			  color="indigo"
			  @click="info_type = 5"
			>
			  LTE상태
			</v-btn>
			<v-btn
			  class="ma-2"
			  outlined
			  color="indigo"
			  @click="info_type = 6"
			>
			  사용자목록
			</v-btn>
			<v-btn
			  class="ma-2"
			  outlined
			  color="indigo"
			  @click="boxWeb_Window()"
			>
			  WEB접속
			</v-btn>
			<v-btn
			  class="ma-2"
			  outlined
			  color="indigo"
			  @click="boxCli_Window()"
			>
			  CLI접속
			</v-btn>
		</div>
		<div v-if="info_type < 3 || info_type == 5">
			<v-list dense>
				<v-list-item
				  v-for="(item, i) in items"
				  :key="i"
				>
					<v-list-item-title v-text="item.title"></v-list-item-title>
					<v-list-item-subtitle v-text="item.text"></v-list-item-subtitle>
				</v-list-item>
			</v-list>
		</div>
		<div v-if="info_type == 3 || info_type == 4 || info_type == 6">
			<v-tabs v-model="tab"
			v-if="info_type == 6"
			>
				<v-tab
				v-for="item in tabs"
				:key="item.tab"
				>
					{{ item.text }}
				</v-tab>
			</v-tabs>
			<ViewTable 
			:fields="fields"
			:items="items"
			/>
		</div>
	</div>
</template>

<script>
// @ is an alias to /src
import ViewTable from '@/components/ViewTable.vue'

export default {
	name: 'BoxstateInfo',
	components: {
		ViewTable,
	},
	data() {
		return {
			info_type: 0, //1: 장비정보, 2: 지점정보, 3: 회선정보, 4: 인터페이스, 5: LTE상태, 6: 사용자목록, 7: WEB접속, 8: CLI접속
			selectedItem: 1,
			items: [],
			box_info: [
				{ title: 'Model', text: 'Neobox_M106a'},
				{ title: 'Image Version', text: 'XIOS FW V2.1(r32473)'},
				{ title: 'IP', text: '1.1.1.1'},
				{ title: 'Serial', text: 'M106-a-L1707-0110'},
				{ title: 'WAN0 IP', text: '1.1.1.1'},
				{ title: 'WAN1 IP', text: '1.1.1.2'},
				{ title: 'WAN2 IP', text: '1.1.1.3'},
				{ title: 'WAN3 IP', text: '-'},
				{ title: 'Lan IP', text: '10.1.1.10'},
				{ title: 'SNMP Version', text: '2c'},
				{ title: 'SNMP Community', text: 'XNSYSTEMS'},
				{ title: '동작시간', text: '124일 19시간 47분 34초'},
			],
			bch_info: [
				{ title: '상위그룹', text: 'grp'},
				{ title: '지점명', text: 'gName1'},
				{ title: '서비스번호', text: ''},
				{ title: '지점 연락처1', text: ''},
				{ title: '지점 연락처2', text: ''},
				{ title: '주소', text: '부산광역시 수영구 광안로49번길 74'},
				{ title: '담당자', text: ''},
				{ title: 'e-mail', text: ''},
				{ title: '등록일시', text: '2021-01-25 14:07:03'},
			],
			lte_info: [
				{ title: 'LTE IP', text: '0.0.0.0'},
				{ title: '망상태', text: 'NO_SRV'},
				{ title: 'LTE Band', text: '1'},
				{ title: '전화번호', text: ''},
				{ title: 'LTE 신호세기', text: '-121dBm'},
				{ title: 'LTE UpChannel', text: '3200'},
				{ title: 'LTE DownChannel', text: '3200'},
				{ title: 'LTE USIM 상태', text: 'USIM 미인식'},
			],
			line_items: [
				{ isp_name: 'ISP0', isp: 'SKB', isp_sv: 'SKB_케이블', sv_type: 'ST', date: '2021-01-25 01:01:01', cnt: 3, num: '', tel: '106', etc1: '', etc2: '' },
			],
			if_items: [
				{ iface: 'eth0', ip: '1.1.1.1' },
				{ iface: 'eth1.2', ip: '10.1.1.10' },
			],
			user_all_items: [
				{ iface: 'eth0', ip: '1.1.1.1', mac: 'AB:1G:32:C3:1D:EF', os: 'Windows', ssid: 'testSSID', mode: '-', use_802: 'X', use_mab: 'O', hz: '15Hz', time: '12시간 24분 05초' },
				{ iface: 'eth1.2', ip: '10.1.1.10', mac: '-', os: '-', ssid: '-', mode: '-', use_802: '-', use_mab: '-', hz: '-', time: '-' },
			],
			user_w_items: [
				{ iface: 'eth1.2', ip: '10.1.1.10', mac: '-' },
			],
			user_wl_items: [
				{ iface: 'eth0', ip: '1.1.1.1', mac: 'AB:1G:32:C3:1D:EF', os: 'Windows', ssid: 'testSSID', mode: '-', use_802: 'X', use_mab: 'O', hz: '15Hz', time: '12시간 24분 05초' },
			],

			fields: [],
			line_fields: [
				{ value: 'isp_name', text: '회선이름', sortable: true },
				{ value: 'isp', text: 'ISP', sortable: true },
				{ value: 'isp_sv', text: '회선 서비스', sortable: true },
				{ value: 'sv_type', text: '회선 유형', sortable: true },
				{ value: 'date', text: '개통일', sortable: true },
				{ value: 'cnt', text: '회선수', sortable: true },
				{ value: 'num', text: '회선번호', sortable: true },
				{ value: 'tel', text: '장애신고전화', sortable: true },
				{ value: 'etc1', text: '추가정보1', sortable: true },
				{ value: 'etc2', text: '추가정보2', sortable: true },
			],
			if_fields: [
				{ value: 'iface', text: '인터페이스', sortable: true },
				{ value: 'ip', text: 'IP', sortable: true },
			],
			user_all_fields: [
				{ value: 'iface', text: '인터페이스', sortable: true },
				{ value: 'ip', text: 'IP', sortable: true },
				{ value: 'mac', text: 'MAC', sortable: true },
				{ value: 'os', text: 'OS 버전', sortable: true },
				{ value: 'ssid', text: '무선 SSID명', sortable: true },
				{ value: 'mode', text: '무선 보안모드', sortable: true },
				{ value: 'use_802', text: '무선 802.1x 사용 여부', sortable: true },
				{ value: 'use_mab', text: 'MAB 사용 여부', sortable: true },
				{ value: 'hz', text: '무선 주파수', sortable: true },
				{ value: 'time', text: '무선 접속 유지 시간', sortable: true },
			],
			user_w_fields: [
				{ value: 'iface', text: '인터페이스', sortable: true },
				{ value: 'ip', text: 'IP', sortable: true },
				{ value: 'mac', text: 'MAC', sortable: true },
			],

			tab: null,
			tabs: [
			  { tab: 1, text: '전체'},
			  { tab: 2, text: '유선'},
			  { tab: 3, text: '무선'},
			],
		}
	},
	methods: {
		boxWeb_Window() {
			var box_ip = '';

			for(var key in this.box_info) {
				if(this.box_info[key].title == 'IP') {
					box_ip = 'https://' + this.box_info[key].text;
				}
			}
			box_ip = 'https://192.168.0.90';
			console.log(box_ip);
			var webScreenNum = this.$newWindow_web.length;
			this.$newWindow_web.push(window.open(box_ip, "WEB_SCREEN"+webScreenNum, "menubar=no, status=no, location=no, fullscreen=yes, toolbar=no, scrollbars=no"));
		},
		boxCli_Window() {
			var box_ip = '';

			for(var key in this.box_info) {
				if(this.box_info[key].title == 'IP') {
					box_ip = this.box_info[key].text;
				}
			}
			box_ip = '192.168.0.90';

			var opt;
			if(window.location.protocol == "http:") {
				opt = "-t -s";
			} else {
				opt = "-s";
			}

			window.open(`/cli?box_ip=${box_ip}&protocol=${window.location.protocol}`, "CLI_SCREEN", "menubar=no, status=no, location=no, fullscreen=yes, toolbar=no, scrollbars=no");
			//var win = window.open("", "CLI_SCREEN", "menubar=no, status=no, location=no, fullscreen=yes, toolbar=no, scrollbars=no");
			//win.document.body.innerHTML = "<iframe name='ifr_cli1' src='`/usr/bin/shellinabox " + opt +" /:SSH:" + box_ip +"`' width='0' height='0' frameborder='0' border='0' scrolling='no'></iframe>";
		},
	},
	watch: {
		info_type (val) {
			if(val == 1) {
				this.items = this.box_info;
			} else if(val == 2) {
				this.items = this.bch_info;
			} else if(val == 3) {
				this.fields = this.line_fields;
				this.items = this.line_items;
			} else if(val == 4) {
				this.fields = this.if_fields;
				this.items = this.if_items;
			} else if(val == 5) {
				this.items = this.lte_info;
			} else if(val == 6) {
				this.fields = this.user_all_fields;
				this.items = this.user_all_items;
			}
		},

		tab (val) {
			if(val == 0) {
				this.fields = this.user_all_fields;
				this.items = this.user_all_items;
			} else if(val == 1) {
				this.fields = this.user_w_fields;
				this.items = this.user_w_items;
			} else if(val == 2) {
				this.fields = this.user_all_fields;
				this.items = this.user_wl_items;
			}
		}
	},
	created() {
		this.info_type = 1;
	}
}
</script>

<style scoped>
</style>
