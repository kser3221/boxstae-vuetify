<template>
	<div class="realtime_alm" id="alarmC">
		<v-row dense>
			<v-col cols="12">
				<label class="float_l">알람 모니터</label>

				<div class="float_r">
					<v-menu offset-y
					  :close-on-content-click="false"
					  v-model="amenu"
					>
					  <template v-slot:activator="{ on, attrs }">
						<v-btn
						  color="error"
						  class="ml-5"
						  dark
						  :outlined="set_amenu"
						  v-bind="attrs"
						  v-on="on"
						>
						  알람종류
						</v-btn>
					  </template>
					  <v-list>
						<v-list-item
						  v-for="(item, index) in alm"
						  :key="index"
						>
						  <v-list-item-title>
							<v-checkbox
							v-model="item.value"
							:label="item.text"
							color="orange"
							></v-checkbox>
						  </v-list-item-title>
						</v-list-item>
						<v-list-item>
						  <v-list-item-title>
							  <v-btn
							  color="primary"
							  @click="applyClick"
							  >
							  적 용
							  </v-btn>
						  </v-list-item-title>
						</v-list-item>
					  </v-list>
					</v-menu>

					<v-menu offset-y
					  :close-on-content-click="false"
					  v-model="smenu"
					>
					  <template v-slot:activator="{ on, attrs }">
						<v-btn
						  color="warning"
						  class="ml-2"
						  dark
						  :outlined="set_smenu"
						  v-bind="attrs"
						  v-on="on"
						>
						  상태
						</v-btn>
					  </template>
					  <v-list>
						<v-list-item
						  v-for="(item, index) in alm_status"
						  :key="index"
						>
						  <v-list-item-title>
							<v-checkbox
							v-model="item.value"
							:label="item.text"
							color="orange"
							></v-checkbox>
						  </v-list-item-title>
						</v-list-item>
						<v-list-item>
						  <v-list-item-title>
							  <v-btn
							  color="primary"
							  @click="applyClick"
							  >
							  적 용
							  </v-btn>
						  </v-list-item-title>
						</v-list-item>
					  </v-list>
					</v-menu>

					<v-select
					class="w120 dp_ib ml-2"
					:items="mtime"
					v-model="sel_mtime"
					dense
					></v-select>
				</div>
			</v-col>
		</v-row>
		<v-row class="minus-top-mg">
			<v-col cols="12" class="alm-vs-table">
				<v-data-table
				  fill-height
				  id="alm-virtual-scroll-table"
				  v-scroll:#alm-virtual-scroll-table="a_onScroll"
				  dense
				  :headers="fields"
				  :items="a_listsLimited"
				  item-key="name"
				  disable-pagination
				  hide-default-header
				  hide-default-footer
				  fixed-header
				  @click:row="a_popOpen"
				  class="elevation-1 alm-vs-table-wrap"
				>
					<template
					  v-slot:header="{props: {headers}}"
					>
						<thead class="v-data-table-header">
							<tr>
								<th v-for="(h, i) in fields" :key="h.value"
								class="text-center parent-header td-border-style"
								>{{ h.text }}</th>
							</tr>
						</thead>
					</template>

					<template
					  v-if="a_start > 0"
					  v-slot:body.prepend
					>
						<tr>
							<td
							  :style="'padding-top:' + a_startHeight + 'px'"
							></td>
						</tr>
					</template>
					<template
					  v-if="a_start + a_perPage < this.datas.length"
					  v-slot:body.append
					>
						<tr>
							<td
							  :style="'padding-top:' + a_endHeight + 'px'"
							></td>
						</tr>
					</template>
				</v-data-table>
			</v-col>
		</v-row>

		<v-dialog v-model="a_dialog" max-width="850px">
			<v-card>
				<v-card-title>
					<span class="headline">{{box_info.bname}}</span>
				</v-card-title>

				<v-card-text>
				  <v-container class="p_15">
					  <v-input>장비 정보</v-input>
					  <v-row dense>
						<v-col cols="3">
						Model
						<v-input>{{box_info.model}}</v-input>
						</v-col>
						<v-col cols="3">
						Image Version
						<v-input>{{box_info.img}}</v-input>
						</v-col>
						<v-col cols="3">
						IP
						<v-input>{{box_info.ip}}</v-input>
						</v-col>
						<v-col cols="3">
						Serial
						<v-input>{{box_info.serial}}</v-input>
						</v-col>
					  </v-row>
					  <v-row dense>
						<v-col cols="3">
						Lan IP
						<v-input>{{box_info.lan}}</v-input>
						</v-col>
						<v-col cols="3">
						동작시간
						<v-input>{{box_info.time}}</v-input>
						</v-col>
						<v-col cols="3">
						SNMP Version
						<v-input>{{box_info.snmp_ver}}</v-input>
						</v-col>
						<v-col cols="3">
						SNMP Community
						<v-input>{{box_info.snmp_com}}</v-input>
						</v-col>
					  </v-row>
					  <v-input class="mt-5">지점 정보</v-input>
					  <v-row dense>
						<v-col cols="3">
						상위그룹
						<v-input>{{box_info.pname}}</v-input>
						</v-col>
						<v-col cols="3">
						지점명
						<v-input>{{box_info.bch}}</v-input>
						</v-col>
						<v-col cols="3">
						지점코드
						<v-input>{{box_info.code}}</v-input>
						</v-col>
					  </v-row>
					  <v-row dense>
						<v-col cols="3">
						담당자
						<v-input>{{box_info.name}}</v-input>
						</v-col>
						<v-col cols="3">
						e-mail
						<v-input>{{box_info.email}}</v-input>
						</v-col>
						<v-col cols="3">
						지점 연락처1
						<v-input>{{box_info.tel1}}</v-input>
						</v-col>
						<v-col cols="3">
						지점 연락처2
						<v-input>{{box_info.tel2}}</v-input>
						</v-col>
					  </v-row>
					  <v-row dense>
						<v-col cols="9">
						주소
						<v-input>{{box_info.addr}}</v-input>
						</v-col>
						<v-col cols="3">
						등록일시
						<v-input>{{box_info.date}}</v-input>
						</v-col>
					  </v-row>
				  </v-container>
				</v-card-text>

				<v-card-actions>
				  <v-spacer></v-spacer>
				  <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>
  	</div>
</template>

<script>
export default {
	data: () => ({
		a_start: 0,
		a_timeout: null,
		a_rowHeight: 24,
		a_perPage: 25,
		fields: [
			{ value: 'gname', text: '장비명', sortable: true },
			{ value: 'pname', text: '알람종류', sortable: true },
			{ value: 'sdate', text: '발생시간', sortable: true },
			{ value: 'odate', text: '복구시간', sortable: true },
			{ value: 'status_text', text: '상태', sortable: true },
		],
		datas: [],
		tmp_alm_datas: [],
		a_items: [
			{  id: 1, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 0, gname: 'BOX1', pname: '인터페이스 다운' 
				, box_info: {
					id: '1',
					bname: 'BOX1',
					model: 'Neobox_M106a',
					img: 'XIOS FW V2.1(r32473)',
					ip: '172.31.13.232',
					serial: 'M106-a-L1707-0110',
					lan: '40.0.0.1',
					time: '68일 0시간 27분 59초',
					snmp_ver: '2c',
					snmp_com: '68일 0시간 27분 59초',

					pname: 'grp_2_grp',
					bch: 'bch_2',
					code: '-',
					tel1: '-',
					tel2: '-',
					addr: '부산광역시 수영구 광안로49번길 74',
					name: '-',
					email: '-',
					date: '2021-01-25 14:07:03',
				},
			},
			{  id: 2, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 1, gname: 'Larsen', pname: '모든 인터페이스 다운' , box_info: {} },
			{  id: 3, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 2, gname: 'Geneva', pname: '터널 다운' },
			{  id: 4, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 3, gname: 'Dickerson', pname: 'CPU 임계치 초과' , box_info: {} },
			{  id: 5, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 4, gname: 'Larsen', pname: 'Memory 임계치 초과' , box_info: {} },
			{  id: 6, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 5, gname: 'Geneva', pname: 'Session 임계치 초과' , box_info: {} },
			{  id: 7, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 6, gname: 'Dickerson', pname: '입력 트래픽 임계치초과' , box_info: {} },
			{  id: 8, status_text: "발생", status: 3, sdate: '2020-02-05', odate: '2020-02-05', code: 7, gname: 'Larsen', pname: '출력 트래픽 임계치 초과' , box_info: {} },
			{  id: 9, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 8, gname: 'Geneva', pname: '동작환경 검사' , box_info: {} },
			{ id: 10, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 9, gname: 'Dickerson', pname: '터널 백업 동작' , box_info: {} },
			{ id: 11, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 10, gname: 'Larsen', pname: '포트 다운' , box_info: {} },
			{ id: 12, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 0, gname: 'Geneva', pname: '인터페이스 다운' , box_info: {} },
			{ id: 13, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 1, gname: 'Dickerson', pname: '모든 인터페이스 다운' , box_info: {} },
			{ id: 14, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 2, gname: 'Larsen', pname: '터널 다운' , box_info: {} },
			{ id: 15, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 3, gname: 'Geneva', pname: 'CPU 임계치 초과' , box_info: {} },
			{ id: 16, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 4, gname: 'Dickerson', pname: 'Memory 임계치 초과' , box_info: {} },
			{ id: 17, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 5, gname: 'Larsen', pname: 'Session 임계치 초과' , box_info: {} },
			{ id: 18, status_text: "복구", status: 2, sdate: '2020-02-05', odate: '2020-02-05', code: 6, gname: 'Geneva', pname: '임력 트래픽 임계치 초과' , box_info: {} },
			{ id: 19, status_text: "발생", status: 3, sdate: '2020-02-05', odate: '2020-02-05', code: 7, gname: 'Dickerson', pname: '출력 트래픽 임계치 초과' , box_info: {} },
			{ id: 20, status_text: "발생", status: 3, sdate: '2020-02-05', odate: '2020-02-05', code: 8, gname: 'Larsen', pname: '동작환경 검사' , box_info: {} },
			{ id: 21, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 9, gname: 'Geneva', pname: '터널 백업 동작' , box_info: {} },
			{ id: 22, status_text: "발생", status: 3, sdate: '2020-02-05', odate: '2020-02-05', code: 10, gname: 'Dickerson', pname: '포트 다운' , box_info: {} },
			{ id: 23, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 0, gname: 'Larsen', pname: '인터페이스 다운' , box_info: {} },
			{ id: 24, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 1, gname: 'Geneva', pname: '모든 인터페이스 다운' , box_info: {} },
			{ id: 25, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 2, gname: 'Dickerson', pname: '모든 인터페이스 다운' , box_info: {} },
			{ id: 26, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 3, gname: 'Larsen', pname: 'CPU 임계치 초과' , box_info: {} },
			{ id: 27, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 4, gname: 'Geneva', pname: 'Memory 임계치 초과' , box_info: {} },
			{ id: 28, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 5, gname: 'Dickerson', pname: 'Session 임계치 초과' , box_info: {} },
			{ id: 29, status_text: "발생", status: 3, sdate: '2020-02-05', odate: '2020-02-05', code: 6, gname: 'Larsen', pname: '입력 트래픽 임계치 초과' , box_info: {} },
			{ id: 30, status_text: "발생", status: 3, sdate: '2020-02-05', odate: '2020-02-05', code: 7, gname: 'Geneva', pname: '출력 트래픽 임계치 초과' , box_info: {} },
			{ id: 31, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 8, gname: 'Dickerson', pname: '동작환경 검사' , box_info: {} },
			{ id: 32, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 9, gname: 'Larsen', pname: '터널 백업 설정' , box_info: {} },
			{ id: 33, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 10, gname: 'Geneva', pname: '포트 다운' , box_info: {} },
			{ id: 34, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 0, gname: 'Dickerson', pname: '인터페이스 다운' , box_info: {} },
			{ id: 35, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 1, gname: 'Larsen', pname: '모든 인터페이스 다운' , box_info: {} },
			{ id: 36, status_text: "발생", status: 1, sdate: '2020-02-05', odate: '2020-02-05', code: 2, gname: 'Geneva', pname: '터널 다운' , box_info: {} },
		],

		sel_mtime: 0,
		mtime: [
			{ text: '사용안함', value: 0 },
			{ text: '최근 1분', value: 1 },
			{ text: '최근 5분', value: 2 },
			{ text: '최근 10분', value: 3 },
			{ text: '최근 1시간', value: 4 },
			{ text: '최근 6시간', value: 5 },
		],

		amenu: false,
		smenu: false,
		alm: [
			{ id: 0, text: "인터페이스 다운", value: false, },
			{ id: 1, text: "모든 인터페이스 다운", value: false, },
			{ id: 2, text: "터널 다운", value: false, },
			{ id: 3, text: "CPU 임계치 초과", value: false, },
			{ id: 4, text: "Memory 임계치 초과", value: false, },
			{ id: 5, text: "Session 임계치 초과", value: false, },
			{ id: 6, text: "입력 트래픽 임계치 초과", value: false, },
			{ id: 7, text: "출력 트래픽 임계치 초과", value: false, },
			{ id: 8, text: "동작환경 검사", value: false, },
			{ id: 9, text: "터널 백업 동작", value: false, },
			{ id: 10, text: "포트 다운", value: false, },
		],
		alm_status: [
			{ id: 1, text: '발생', value: false },
			{ id: 2, text: '복구', value: false },
			{ id: 3, text: '기타', value: false },
		],

		set_amenu: true,
		set_smenu: true,

		a_dialog: false,
		box_info: {
			id: '1',
			bname: 'BOX1',
			model: 'Neobox_M106a',
			img: 'XIOS FW V2.1(r32473)',
			ip: '172.31.13.232',
			serial: 'M106-a-L1707-0110',
			lan: '40.0.0.1',
			time: '68일 0시간 27분 59초',
			snmp_ver: '2c',
			snmp_com: '68일 0시간 27분 59초',

			pname: 'grp_2_grp',
			bch: 'bch_2',
			code: '-',
			tel1: '-',
			tel2: '-',
			addr: '부산광역시 수영구 광안로49번길 74',
			name: '-',
			email: '-',
			date: '2021-01-25 14:07:03',
		},
	}),
	methods: {
		a_popOpen (item) {
			//this.box_info = item.box_info;

			this.a_dialog = true;
		},
		close() {
			this.a_dialog = false;
		},
		aMenuClick(list) {
			var idx = "";
			for(var key in this.alm) {
				if(this.alm[key].value) {
					idx += this.alm[key].id;
				}
			}
			if(idx.length) {
				var item = list.filter(d => {
					return (idx.indexOf(d.code) != -1)
				});
				this.tmp_alm_datas = item;
				this.set_amenu = false;
			} else {
				this.tmp_alm_datas = list;
				this.set_amenu = true;
			}
			this.amenu = false;
		},
		sMenuClick(list) {
			var idx = "";
			for(var key in this.alm_status) {
				if(this.alm_status[key].value) {
					idx += this.alm_status[key].id;
				}
			}
			if(idx.length) {
				var item = list.filter(d => {
					return (idx.indexOf(d.status) != -1)
				});
				this.tmp_alm_datas = item;
				this.set_smenu = false;
			} else {
				this.tmp_alm_datas = list;
				this.set_smenu = true;
			}
			this.smenu = false;
		},
		applyClick() {
			var list = this.a_items.slice();

			this.aMenuClick(list);
			this.sMenuClick(this.tmp_alm_datas);

			this.datas = this.tmp_alm_datas;
		},
		a_onScroll(e) {
			// debounce if scrolling fast
			this.a_timeout && clearTimeout(this.a_timeout);
		
			this.a_timeout = setTimeout(() => {
				const { scrollTop } = e.target;
				const rows = Math.ceil(scrollTop / this.a_rowHeight);
		
				this.s_tart = rows + this.a_perPage > this.lists.length ?
				this.lists.length - this.a_perPage: rows;
		
				this.$nextTick(() => {
					e.target.scrollTop = scrollTop;
				});
			 }, 20);
		},
	},
	computed: {
		a_listsLimited() {
			return this.datas.slice(this.a_start, this.a_perPage+this.a_start);
		},
		a_startHeight() {
			return this.a_start * this.a_rowHeight - 32;
		},
		a_endHeight() {
			return this.a_rowHeight * (this.datas.length - this.a_start);
		},
	},
	created() {
		this.datas = this.a_items;
	},

}
</script>

<style scoped>
.realtime_alm {
	position: relative;
	width: 100%;
	height: 100%;
	padding: 10px 20px 20px 20px;
}
.alm-vs-table {
	position: absolute;
	width: 100%;
	height: calc(100% - 50px);
}
.alm-vs-table-wrap {
	position: relative;
	width: 100%;
	height: 100%;
}
#alm-virtual-scroll-table {
	min-height: 120px;
	max-height: 100%;
	overflow: auto;
}
.v-data-table /deep/ .sticky-header {
	position: sticky;
	/* top: 196px; */
	/* top: var(--toolbarHeight); */
}

.v-data-table /deep/ .v-data-table__wrapper {
  overflow: unset;
}
.float_l {
	float: left;
}
.float_r {
	float: right;
}
.p_15 {
	padding: 15px;
}
.mt-5 {
	margin-top: 5px;
}
.w120 {
	width: 120px;
}
.dp_ib {
	display: inline-block;
}
.minus-top-mg {
	margin-top: -15px;
}
</style>

