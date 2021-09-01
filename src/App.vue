<template>
	<v-app id="neo">
		<v-main>
			<Boxstate/>
		</v-main>
	</v-app>
</template>

<script>
import Boxstate from './views/BoxState.vue'


export default {
	name: 'App',
	
	components: {
	    Boxstate,
	},
	
	data: () => ({
	}),
};
</script>

<style>
#neo {
	position: relative;
	width: 100%;
	height: 100%;
	overflow: hidden;
}
#neo .v-main {
	/* padding: 20px !important; */
	position: absolute;
	width: 100%;
	height: 100%;
}
</style>
