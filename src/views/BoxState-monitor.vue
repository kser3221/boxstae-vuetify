<template>
	<div class="box-state">
		<SearchDiv
			:searchOption="search_opt"
			:searchType="search_type"
			:searchValue="search_val"
			@searchE="searchClick"
		/>

		<v-row dense>
			<v-col cols="12">
				<v-btn
				  class="ma-2"
				  outlined
				  color="indigo"
				  @click="allList.status = !allList.status; filterClick"
				>
					<v-icon>{{ allList.icon }}</v-icon>
					전체
				</v-btn>
				<v-btn
				  class="ma-2"
				  outlined
				  color="indigo"
				  @click="ifList.status = !ifList.status"
				>
					<v-icon>{{ ifList.icon }}</v-icon>
					연결장애
				</v-btn>
				<v-btn
				  class="ma-2"
				  outlined
				  color="indigo"
				  @click="lineList.status = !lineList.status"
				>
					<v-icon>{{ lineList.icon }}</v-icon>
					회선장애
				</v-btn>
				<v-btn
				  class="ma-2"
				  outlined
				  color="indigo"
				  @click="tunList.status = !tunList.status"
				>
					<v-icon>{{ tunList.icon }}</v-icon>
					터널장애
				</v-btn>
			</v-col>
		</v-row>
		<v-row class="minus-top-mg" dense>
			<v-col cols="10">
				<div class="advice">
				<label>검색 장비: 총 {{ lists.length }} 대</label>

				<span>인터페이스상태-</span>
				<v-img src="@/img/box_state/up.png" width="18" height="18"></v-img>
				<span>Up</span>
				<v-img src="@/img/box_state/down.png" width="18" height="18"></v-img>
				<span>Down</span>

				<span>/ LTE망상태-</span>
				<v-img src="@/img/box_state/lte_normal.png" width="18" height="18"></v-img>
				<span>Normal</span>
				<v-img src="@/img/box_state/lte_error.png" width="18" height="18"></v-img>
				<span>Error</span>

				<span>/ 연결상태-</span>
				<v-img src="@/img/box_state/line_normal.png" width="18" height="18"></v-img>
				<span>Normal</span>
				<v-img src="@/img/box_state/line_serv_error.png" width="18" height="18"></v-img>
				<span>Service Error</span>
				<v-img src="@/img/box_state/line_error.png" width="18" height="18"></v-img>
				<span>Error</span>

				<span>/ 전원상태-</span>
				<v-img src="@/img/box_state/on.png" width="18" height="18"></v-img>
				<span>On</span>
				<v-img src="@/img/box_state/off.png" width="18" height="18"></v-img>
				<span>Off</span>
				<v-img src="@/img/box_state/normal.png" width="18" height="18"></v-img>
				<span>Normal</span>
				<v-img src="@/img/box_state/error.png" width="18" height="18"></v-img>
				<span>Error</span>
				</div>
			</v-col>
			<v-col cols="2">
				<!--v-select class="select_150" v-model="selectedHeaders" :items="headers" multiple dense outlined return-object-->
				<v-select class="select_150" v-model="selectedHeaders" :items="col_label" multiple dense outlined return-object>
					<template v-slot:selection="{ item, index }">
						<span v-if="index === 0" class="grey--text caption">컬럼편집</span>
					</template>
				</v-select>
			</v-col>
		</v-row>
		<v-row class="minus-top-mg">
			<v-col cols="12" class="vs-table">
				<v-data-table
				  fill-height
				  id="virtual-scroll-table"
				  v-scroll:#virtual-scroll-table="onScroll"
				  dense
				  :headers="showHeaders"
				  :items="listsLimited"
				  item-key="name"
				  disable-pagination
				  hide-default-header
				  hide-default-footer
				  fixed-header
				  @click:row="dialog = true"
				  class="elevation-1 vs-table-wrap"
				>
					<template
					  v-slot:header="{props: {headers}}"
					>
						<thead class="v-data-table-header">
							<tr>
								<th v-for="(h, i) in showHeaders" :key="h.value"
								v-if="(!h.parent && !h.children) || (h.children > 0)"
								class="text-center parent-header td-border-style"
								:rowspan="h.children?1:2" :colspan="(h.children>0)?h.children:1"
								>{{ h.parent ? h.parent : h.text }}</th>
							</tr>
							<tr>
								<th v-for="(h, i) in showHeaders" :key="h.value"
								v-if="h.parent"
								class="text-center parent-header td-border-style"
								>{{ h.text }}</th>
							</tr>
						</thead>
					</template>

					<template
					  v-if="start > 0"
					  v-slot:body.prepend
					>
						<tr>
							<td
							  :style="'padding-top:' + startHeight + 'px'"
							></td>
						</tr>
					</template>
					<template
					  v-if="start + perPage < this.lists.length"
					  v-slot:body.append
					>
						<tr>
							<td
							  :style="'padding-top:' + endHeight + 'px'"
							></td>
						</tr>
					</template>
				</v-data-table>
			</v-col>
		</v-row>


		<v-dialog
		  v-model="dialog"
		  scrollable
		  max-width="90%"
		>
			<v-card>
				<v-tabs v-model="tab"
				fixed-tabs
				>
					<v-tab
					v-for="item in tabs"
					:key="item.tab"
					>
						{{ item.text }}
					</v-tab>
				</v-tabs>
				<v-divider></v-divider>
				<v-card-text style="height: 600px;">
					<v-tabs-items v-model="tab" class="tab-item">
						<v-tab-item
						v-for="item in tabs"
						:key="item.tab"
						>
							<v-card flat>
								<keep-alive>
									<component v-bind:is="item.content"></component>
								</keep-alive>
							</v-card>
						</v-tab-item>
					</v-tabs-items>
				</v-card-text>
				<v-divider></v-divider>
				<v-card-actions>
					<v-btn
					  color="blue darken-1"
					  text
					  @click="dialog = false"
					>
					닫 기
					</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>
  	</div>
</template>

<script>
import Info from '@/views/boxstate/Boxstate-info.vue'
import Alarm from '@/views/boxstate/Boxstate-alarm.vue'
import VPN from '@/views/boxstate/Boxstate-vpn.vue'
import Resource from '@/views/boxstate/Boxstate-resource.vue'
import Traffic from '@/views/boxstate/Boxstate-traffic.vue'
import SearchDiv from '@/components/SearchDiv.vue'

export default {
    components: {
		Info,
		Alarm,
		VPN,
		Resource,
		Traffic,
		SearchDiv,
    },

	data: () => ({
		search_opt: [
			{ type: 1, label: "장비명" },
			{ type: 2, label: "모델명" },
			{ type: 3, label: "지점명" },
			{ type: 4, label: "IP" },
			{ type: 5, label: "서비스번호" },
			{ type: 6, label: "회선번호" },
		],
		search_type: {},
		search_val: '',

		dialog: false,
		popOpen: false,
		allList: { status: true, icon: 'mdi-checkbox-blank-circle'},
		ifList: { status: false, icon: 'mdi-checkbox-blank-circle-outline'},
		lineList: { status: false, icon: 'mdi-checkbox-blank-circle-outline'},
		tunList: { status: false, icon: 'mdi-checkbox-blank-circle-outline'},
		cmenu: false,
		set_cmenu: true,

		col_edit: [
			{  id: 1, value: 'gname',  text: '지점명' },
			{  id: 2, value: 'bname',  text: '장비명' },
			{  id: 3, value: 'ip',  text: 'IP' },
			{  id: 4, value: 'lan',  text: 'LAN' },
			{  id: 5, value: 'alldown',  text: '연결상태' },

			{  id: 6, parent: 'WAN0',
				children:3,
				text: '상태', value: 'wan0_status'},
			{  id: 7, parent: 'WAN0',
				children:0,
				text: 'UP 회선품질', value: 'wan0_up'},
			{  id: 8, parent: 'WAN0',
				children:0,
				text: 'DOWN 회선품질', value: 'wan0_down'},
			{  id: 9, parent: 'WAN1',
				children:3,
				text: '상태', value: 'wan1_status'},
			{  id: 10, parent: 'WAN1',
				children:0,
				text: 'UP 회선품질', value: 'wan1_up'},
			{  id: 11, parent: 'WAN1',
				children:0,
				text: 'DOWN 회선품질', value: 'wan1_down'},
			{  id: 12, parent: 'WAN2',
				children:3,
				text: '상태', value: 'wan2_status'},
			{  id: 13, parent: 'WAN2',
				children:0,
				text: 'UP 회선품질', value: 'wan2_up'},
			{  id: 14, parent: 'WAN2',
				children:0,
				text: 'DOWN 회선품질', value: 'wan2_down'},
			{  id: 15, parent: 'WAN3',
				children:3,
				text: '상태', value: 'wan3_status'},
			{  id: 16, parent: 'WAN3',
				children:0,
				text: 'UP 회선품질', value: 'wan3_up'},
			{  id: 17, parent: 'WAN3',
				children:0,
				text: 'DOWN 회선품질', value: 'wan3_down'},

			{ id: 18, value: 'lte_ip',  text: 'LTE IP' },
			{ id: 19, value: 'lte_status',  text: 'LTE 망상태' },
			{ id: 20, value: 'lte_usim',  text: 'LTE USIM' },
			{ id: 21, value: 'rsrp',  text: 'LTE 신호세기' },
			{ id: 22, value: 'vpn_cnt',  text: 'VPN 개수' },

			{ id: 23, parent: 'VPN 트래픽',
				children:2,
				text: 'UP 트래픽', value: 'vpn_up'},
			{ id: 24, parent: 'VPN 트래픽',
				children:0,
				text: 'DOWN 트래픽', value: 'vpn_down'},

			{ id: 25, value: 'cpu',  text: 'CPU' },
			{ id: 26, value: 'mem',  text: 'Memory' },
			{ id: 27, value: 'ses',  text: 'Session' },
			{ id: 28, value: 'disk',  text: 'Disk' },
			{ id: 29, parent: '전원컨트롤',
				children:4,
				text: '전원상태', value: 'p1_operation1'},
			{ id: 30, parent: '전원컨트롤',
				children:0,
				text: '전원상태', value: 'p1_operation2'},
			{ id: 31, parent: '전원컨트롤',
				children:0,
				text: '전원상태', value: 'p1_operation3'},
			{ id: 32, parent: '전원컨트롤',
				children:0,
				text: '전원상태', value: 'p1_operation4'},
			{ id: 33, parent: '전원이중화',
				children:2,
				text: 'Power A', value: 'dual_power_a'},
			{ id: 34, parent: '전원이중화',
				children:0,
				text: 'Power B', value: 'dual_power_b'},
		],

		col_label: [
			{ value: 'gname',  text: '지점명' },
			{ value: 'bname',  text: '장비명' },
			{ value: 'ip',  text: 'IP' },
			{ value: 'lan',  text: 'LAN' },
			{ value: 'alldown',  text: '연결상태' },

			{ text: 'WAN0', value: 'wan0_status'},
			{ text: 'WAN0 회선품질', value: 'wan0_up,wan0_down'},
			{ text: 'WAN1', value: 'wan1_status'},
			{ text: 'WAN1 회선품질', value: 'wan1_up,wan1_down'},
			{ text: 'WAN2', value: 'wan2_status'},
			{ text: 'WAN2 회선품질', value: 'wan2_up,wan2_down'},
			{ text: 'WAN3', value: 'wan3_status'},
			{ text: 'WAN3 회선품질', value: 'wan3_up,wan3_down'},

			{ value: 'lte_ip',  text: 'LTE IP' },
			{ value: 'lte_status',  text: 'LTE 망상태' },
			{ value: 'lte_usim',  text: 'LTE USIM' },
			{ value: 'rsrp',  text: 'LTE 신호세기' },
			{ value: 'vpn_cnt',  text: 'VPN 개수' },

			{ text: 'VPN 트래픽', value: 'vpn_up,vpn_down'},

			{ value: 'cpu',  text: 'CPU' },
			{ value: 'mem',  text: 'Memory' },
			{ value: 'ses',  text: 'Session' },
			{ value: 'disk',  text: 'Disk' },
			{ text: '전원컨트롤', value: 'p1_operation1,p1_operation2,p1_operation3,p1_operation4'},
			{ text: '전원이중화', value: 'dual_power_a,dual_power_b'},
		],


		lists: [],
		tmp_datas: [],
		tmp_lists: Array.from({ length: 15000 }).map((v, k) => ({
			gname: `gName${k}`,
			bname: `BOX${k}`,
			gw_type: new Array( 'Neobox M106a', 'Neobox X1b', 'Neobox M22' )[Math.floor((Math.random()*3))],
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			lan: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			alldown: new Array( 'Normal', 'Service Error', 'Error' )[Math.floor((Math.random()*3))],
			wan0_status: new Array( 'Up', 'Down' )[Math.floor((Math.random()*2))],
			wan0_up: '-',
			wan0_down: '-',
			wan1_status: new Array( 'Up', 'Down' )[Math.floor((Math.random()*2))],
			wan1_up: '-',
			wan1_down: '-',
			wan2_status: new Array( 'Up', 'Down' )[Math.floor((Math.random()*2))],
			wan2_up: '-',
			wan2_down: '-',
			wan3_status: new Array( 'Up', 'Down' )[Math.floor((Math.random()*2))],
			wan3_up: '-',
			wan3_down: '-',
			sv_num: Math.floor((Math.random()*100)),
			tel: Math.floor((Math.random()*100)),
			lte_ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			lte_status: '-',
			lte_usim: '-',
			rsrp: Math.floor((Math.random()*100)),
			vpn_cnt: '0/0',
			vpn_up: 0,
			vpn_down: 0,
			cpu: Math.floor((Math.random()*100)) + '%',
			mem: Math.floor((Math.random()*100)) + '%',
			ses: Math.floor((Math.random()*100)) + '%',
			disk: Math.floor((Math.random()*100)) + '%',
			p1_operation1: '-',
			p1_operation2: '-',
			p1_operation3: '-',
			p1_operation4: '-',
			dual_power_a: '-',
			dual_power_b: '-',
		})),
		headers: [],
		selectedHeaders: [],

		start: 0,
		timeout: null,
		rowHeight: 24,
		perPage: 25,

		tab: null,
        tabs: [
          { tab: 1, text: '상세정보', content: 'Info' },
          { tab: 2, text: '알람', content: 'Alarm' },
          { tab: 3, text: 'VPN현황', content: 'VPN' },
          { tab: 4, text: '자원사용율 추이', content: 'Resource' },
          { tab: 5, text: '트래픽 추이', content: 'Traffic' },
        ],
	}),
	methods: {
		searchClick: function(type, val) {
			if(val.length) {
				this.tmp_datas = this.tmp_lists;
				var item = this.tmp_datas.filter(d => {
					if(type == 1) {
						return (d.bname.trim().toUpperCase().indexOf(val.trim().toUpperCase()) != -1)
					} else if(type == 2) {
						return (d.gw_type.trim().toUpperCase().indexOf(val.trim().toUpperCase()) != -1)
					} else if(type == 3) {
						return (d.gname.trim().toUpperCase().indexOf(val.trim().toUpperCase()) != -1)
					} else if(type == 4) {
						return (d.ip.trim().toUpperCase().indexOf(val.trim().toUpperCase()) != -1)
					} else if(type == 5) {
						return (d.sv_num.toString().trim().toUpperCase().indexOf(val.trim().toUpperCase()) != -1)
					} else if(type == 6) {
						return (d.tel.toString().trim().toUpperCase().indexOf(val.trim().toUpperCase()) != -1)
					}
				});
				this.lists = item;
			} else {
				this.lists = this.tmp_lists;
			}
		},
		onScroll(e) {
			// debounce if scrolling fast
			this.timeout && clearTimeout(this.timeout);
		
			this.timeout = setTimeout(() => {
				const { scrollTop } = e.target;
				const rows = Math.ceil(scrollTop / this.rowHeight);
		
				this.start = rows + this.perPage > this.lists.length ?
				this.lists.length - this.perPage: rows;
		
				this.$nextTick(() => {
					e.target.scrollTop = scrollTop;
				});
			 }, 20);
		},

		allDown(list) {
			if(this.ifList.status) {
				var item = list.filter(d => {
					return (d.alldown== 'Error')
				});
				if(item.length) {
					this.tmp_datas = item;
				} else {
					this.tmp_datas = list;
				}
			} else {
				this.tmp_datas = list;
			}
		},
		wanErr(list) {
			if(this.lineList.status) {
				var item = list.filter(d => {
					return (d.wan0_status== 'Down' && d.wan1_status== 'Down' && d.wan2_status== 'Down' && d.wan3_status== 'Down')
				});
				if(item.length) {
					this.tmp_datas = item;
				} else {
					this.tmp_datas = list;
				}
			} else {
				this.tmp_datas = list;
			}
		},
		tunErr(list) {
			if(this.tunList.status) {
				var item = list.filter(d => {
					return (d.vpn_down > 0)
				});
				if(item.length) {
					this.tmp_datas = item;
				} else {
					this.tmp_datas = list;
				}
			} else {
				this.tmp_datas = list;
			}
		},
		filterClick() {
			var list = this.tmp_lists.slice();

			this.allDown(list);
			this.wanErr(this.tmp_datas);
			this.tunErr(this.tmp_datas);

			this.lists = this.tmp_datas;
		},
	},
	computed: {
		listsLimited() {
			return this.lists.slice(this.start, this.perPage+this.start);
		},
		startHeight() {
			return this.start * this.rowHeight - 32;
		},
		endHeight() {
			return this.rowHeight * (this.lists.length - this.start);
		},
		showHeaders () {
			var ind = [];
			var len = 0;
			this.selectedHeaders.forEach(function(d, i) {
				if(d.chk) {
					ind.push(i);
				}
			});
			
			len = ind.length;
			for(var i = len - 1; i > -1; i--) {
				this.selectedHeaders.splice(ind[i], 1);
			}
			var tmp_sel = [];
			var tmp_header = [...this.col_edit];
			var tmp_sel_header = [...this.selectedHeaders];

			var obj = {};
			tmp_sel_header.forEach(function(s) {
				var col_arr = s.value.split(",");
				var col_len = col_arr.length;

				col_arr.forEach(function(c) {
					var isExist = tmp_header.indexOf(tmp_header.filter(function(h) {
									return h.value == c
								})[0]);

					obj = tmp_header[isExist];
					if(col_len > 1) {
						obj.chk = true;
					}
					tmp_sel.push(obj);
				});

				if(col_arr.length > 1) {
					tmp_sel.push(s);
				}
			});

			///////////////////////////////////////////////////////////////////////////////////

			var wan = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan0_status" 
						})[0]);
			var wan_up = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan0_up" 
						})[0]);
			var wan_down = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan0_down" 
						})[0]);
			if(wan_up != -1 || wan_down != -1) {
				if(wan == -1) {
					if(wan_up > wan_down) {
						tmp_sel.splice(wan_up, 1);
						tmp_sel.splice(wan_down, 1);
					} else {
						tmp_sel.splice(wan_down, 1);
						tmp_sel.splice(wan_up, 1);
					}
					var wan = tmp_sel.indexOf(tmp_sel.filter(function(h) {
									return h.value == "wan0_up,wan0_down" 
								})[0]);
					tmp_sel.splice(wan, 1);
					wan = this.col_label.indexOf(this.col_label.filter(function(h) {
									return h.value == "wan0_up,wan0_down" 
								})[0]);
					//this.col_label[wan].disabled = true;
				} else {
					tmp_sel[wan].children = 3;
					wan = this.col_label.indexOf(this.col_label.filter(function(h) {
									return h.value == "wan0_up,wan0_down" 
								})[0]);
					//this.col_label[wan].disabled = false;
				}
			} else if(wan != -1) {
				tmp_sel[wan].children = 1;
			}

			var wan = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan1_status" 
						})[0]);
			var wan_up = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan1_up" 
						})[0]);
			var wan_down = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan1_down" 
						})[0]);
			if(wan_up != -1 || wan_down != -1) {
				if(wan == -1) {
					if(wan_up > wan_down) {
						tmp_sel.splice(wan_up, 1);
						tmp_sel.splice(wan_down, 1);
					} else {
						tmp_sel.splice(wan_down, 1);
						tmp_sel.splice(wan_up, 1);
					}
					var wan = tmp_sel.indexOf(tmp_sel.filter(function(h) {
									return h.value == "wan1_up,wan1_down" 
								})[0]);
					tmp_sel.splice(wan, 1);
					wan = this.col_label.indexOf(this.col_label.filter(function(h) {
									return h.value == "wan1_up,wan1_down" 
								})[0]);
					//this.col_label[wan].disabled = true;
				} else {
					tmp_sel[wan].children = 3;
					wan = this.col_label.indexOf(this.col_label.filter(function(h) {
									return h.value == "wan1_up,wan1_down" 
								})[0]);
					//this.col_label[wan].disabled = false;
				}
			} else if(wan != -1) {
				tmp_sel[wan].children = 1;
			}

			var wan = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan2_status" 
						})[0]);
			var wan_up = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan2_up" 
						})[0]);
			var wan_down = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan2_down" 
						})[0]);
			if(wan_up != -1 || wan_down != -1) {
				if(wan == -1) {
					if(wan_up > wan_down) {
						tmp_sel.splice(wan_up, 1);
						tmp_sel.splice(wan_down, 1);
					} else {
						tmp_sel.splice(wan_down, 1);
						tmp_sel.splice(wan_up, 1);
					}
					var wan = tmp_sel.indexOf(tmp_sel.filter(function(h) {
									return h.value == "wan2_up,wan2_down" 
								})[0]);
					tmp_sel.splice(wan, 1);
					wan = this.col_label.indexOf(this.col_label.filter(function(h) {
									return h.value == "wan2_up,wan2_down" 
								})[0]);
					//this.col_label[wan].disabled = true;
				} else {
					tmp_sel[wan].children = 3;
					wan = this.col_label.indexOf(this.col_label.filter(function(h) {
									return h.value == "wan2_up,wan2_down" 
								})[0]);
					//this.col_label[wan].disabled = false;
				}
			} else if(wan != -1) {
				tmp_sel[wan].children = 1;
			}

			var wan = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan3_status" 
						})[0]);
			var wan_up = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan3_up" 
						})[0]);
			var wan_down = tmp_sel.indexOf(tmp_sel.filter(function(h) {
							return h.value == "wan3_down" 
						})[0]);
			if(wan_up != -1 || wan_down != -1) {
				if(wan == -1) {
					if(wan_up > wan_down) {
						tmp_sel.splice(wan_up, 1);
						tmp_sel.splice(wan_down, 1);
					} else {
						tmp_sel.splice(wan_down, 1);
						tmp_sel.splice(wan_up, 1);
					}
					var wan = tmp_sel.indexOf(tmp_sel.filter(function(h) {
									return h.value == "wan3_up,wan3_down" 
								})[0]);
					tmp_sel.splice(wan, 1);
					wan = this.col_label.indexOf(this.col_label.filter(function(h) {
									return h.value == "wan3_up,wan3_down" 
								})[0]);
					//this.col_label[wan].disabled = true;
				} else {
					tmp_sel[wan].children = 3;
					wan = this.col_label.indexOf(this.col_label.filter(function(h) {
									return h.value == "wan3_up,wan3_down" 
								})[0]);
					//this.col_label[wan].disabled = false;
				}
			} else if(wan != -1) {
				tmp_sel[wan].children = 1;
			}


			///////////////////////////////////////////////////////////////////////////////////

			//this.selectedHeaders = this.selectedHeaders.sort((a, b) => a.id - b.id);
			this.selectedHeaders = tmp_sel.sort((a, b) => a.id - b.id);
			return this.headers.filter(s => this.selectedHeaders.includes(s));
		},
	},
	watch: {
		allList: {
			deep: true,
			immediate: true,

			handler(val) {
				if(val.status) {
					this.allList.icon = 'mdi-checkbox-blank-circle';

					this.ifList.status = false;
					this.lineList.status = false;
					this.tunList.status = false;
					this.ifList.icon = 'mdi-checkbox-blank-circle-outline';
					this.lineList.icon = 'mdi-checkbox-blank-circle-outline';
					this.tunList.icon = 'mdi-checkbox-blank-circle-outline';

					this.lists = this.tmp_lists;
				}
			}
		},
		ifList: {
			deep: true,
			immediate: true,

			handler(val) {
				if(val.status) {
					if(this.lineList.status && this.tunList.status) {
						this.allList.status = false;
						this.allList.icon = 'mdi-checkbox-blank-circle';

						this.ifList.status = false;
						this.lineList.status = false;
						this.tunList.status = false;
						this.ifList.icon = 'mdi-checkbox-blank-circle-outline';
						this.lineList.icon = 'mdi-checkbox-blank-circle-outline';
						this.tunList.icon = 'mdi-checkbox-blank-circle-outline';

						this.lists = this.tmp_lists;
					} else {
						this.ifList.icon = 'mdi-checkbox-blank-circle';

						this.allList.status = false;
						this.allList.icon = 'mdi-checkbox-blank-circle-outline';

						this.filterClick();
					}
				}
			}
		},
		lineList: {
			deep: true,
			immediate: true,

			handler(val) {
				if(val.status) {
					if(this.ifList.status && this.tunList.status) {
						this.allList.status = false;
						this.allList.icon = 'mdi-checkbox-blank-circle';

						this.ifList.status = false;
						this.lineList.status = false;
						this.tunList.status = false;
						this.ifList.icon = 'mdi-checkbox-blank-circle-outline';
						this.lineList.icon = 'mdi-checkbox-blank-circle-outline';
						this.tunList.icon = 'mdi-checkbox-blank-circle-outline';

						this.lists = this.tmp_lists;
					} else {
						this.lineList.icon = 'mdi-checkbox-blank-circle';

						this.allList.status = false;
						this.allList.icon = 'mdi-checkbox-blank-circle-outline';

						this.filterClick();
					}
				}
			}
		},
		tunList: {
			deep: true,
			immediate: true,

			handler(val) {
				if(val.status) {
					if(this.ifList.status && this.lineList.status) {
						this.allList.status = false;
						this.allList.icon = 'mdi-checkbox-blank-circle';

						this.ifList.status = false;
						this.lineList.status = false;
						this.tunList.status = false;
						this.ifList.icon = 'mdi-checkbox-blank-circle-outline';
						this.lineList.icon = 'mdi-checkbox-blank-circle-outline';
						this.tunList.icon = 'mdi-checkbox-blank-circle-outline';

						this.lists = this.tmp_lists;
					} else {
						this.tunList.icon = 'mdi-checkbox-blank-circle';

						this.allList.status = false;
						this.allList.icon = 'mdi-checkbox-blank-circle-outline';

						this.filterClick();
					}
				}
			}
		},
	},
	created() {
		this.headers = Object.values(this.col_edit);
		this.headers[0].align = 'start';

		//this.selectedHeaders = this.headers;
		this.selectedHeaders = this.col_label;
	},

}
</script>

<style scoped>
.box-state {
	height: 100%;
	width: 100%;
	padding: 0 20px 20px 20px;
	position: relative;
}


.vs-table {
	position: absolute;
	width: 100%;
	height: calc(100% - 170px);
}
.vs-table-wrap {
	position: relative;
	width: 100%;
	height: 100%;
}
#virtual-scroll-table {
	min-height: 290px;
	max-height: 100%;
	overflow: auto;
}
.v-image {
	display: inline-block;
}
.advice {
	width: 100%;
	font-size: 8pt;
	font-weight: bold;
	margin-top: 20px;
}
.v-data-table /deep/ .sticky-header {
	position: sticky;
	/* top: 196px; */
	/* top: var(--toolbarHeight); */
}

.v-data-table /deep/ .v-data-table__wrapper {
  overflow: unset;
}

.select_150 {
	width: 150px;
	display: inline-block;
}
.minus-top-mg {
	margin-top: -30px;
}
</style>
