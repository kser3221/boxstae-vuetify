<script>
export default {
	name: 'connect',
  	props: {
		box_ip: String,
		box_method: String,
		protocol: String,
	},
	created() {
		var opt;
		var exec_str;

		var kill_process = "killall -KILL shellinaboxd";
		shell.exec(kill_process);

		if(this.protocol == "http:") {
			opt = "-t -s";
		} else {
			opt = "-s";
		}

		if(this.box_method == "TELNET") {
			exec_str = "/usr/local/bin/shellinaboxd " + opt + " '/:www-data:www-data:/CMD:/usr/bin/telnet " + this.box_ip + "'";
		} else {
			exec_str = "/usr/local/bin/shellinaboxd " + opt + " /:SSH:" + this.box_ip;
		}

		shell.exec(exec_str);
	}
}
</script>
