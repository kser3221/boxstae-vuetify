<template>
	<div class="connect">
		{{$route.query.box_ip}}
		{{$route.query.protocol}}
		<!--iframe name='ifr_cli1' :src='cli_server' width='0' height='0' frameborder='0' border='0' scrolling='no'></iframe-->
	</div>
</template>

<script>

export default {
	name: 'connect',
	computed: {
		cli_server: function() {
			console.log('cli_server');
			var opt;
			var exec_str;

			var kill_process = "killall -KILL shellinaboxd";
			shell.exec(kill_process);

			if(this.$route.query.protocol == "http:") {
				opt = "-t -s";
			} else {
				opt = "-s";
			}

			if(this.$route.query.box_method == "TELNET") {
				exec_str = "/usr/local/bin/shellinaboxd " + opt + " '/:www-data:www-data:/CMD:/usr/bin/telnet " + this.$route.query.box_ip + "'";
			} else {
				exec_str = "/usr/local/bin/shellinaboxd " + opt + " /:SSH:" + this.$route.query.box_ip;
			}
			console.log(exec_str);

			shell.exec(exec_str);

			return;
		}
	},
	created() {
		setTimeout(() => {
			document.location=this.$route.query.protocol + "//" + this.$route.query.box_ip + ":" + 4200;
			console.log(window.location.host);
			console.log(this.$route.query);
		}, 1000)
	}
}
</script>
