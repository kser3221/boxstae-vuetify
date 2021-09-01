<template>
  <v-data-table
  	dense
    :headers="fields"
    :items="items"
    sort-by="calories"
    class="elevation-1"
	:footer-props="listSize"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>검색결과: 총 {{ items.length }} 개</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
      </v-toolbar>
    </template>
    <template v-slot:no-data>
      No Data
    </template>
  </v-data-table>
</template>

<script>
export default {
	name: 'ViewTable',
  	props: {
		fields: Array,
		items: Array,
	},
	data: () => ({
		listSize: {'items-per-page-options': [10, 20, 30, 50, 100]},
	}),
}
</script>
