<template>
  <div id="boxstate">
    <SplitGrid
      class="sb_sub-grid"
      v-bind="splitGridOptions"
      :animation="animation"
      direction="row"
    >
	  <SplitGridArea>
	    <BoxMonitor/>
	  </SplitGridArea>

      <SplitGridGutter/>

      <SplitGridArea :size-unit="size.unit" :size-value="size.value">
      <!--SplitGridArea-->
		<RealtimeAlm/>
	  </SplitGridArea>
    </SplitGrid>
  </div>
</template>
<script>
import { SplitGrid, SplitGridArea, SplitGridGutter } from "vue-split-grid";

import BoxMonitor from '@/views/BoxState-monitor.vue'
import RealtimeAlm from '@/views/RealtimeAlm.vue'

export default {
  name: "BoxState",
  components: {
    SplitGrid,
    SplitGridArea,
    SplitGridGutter,

	BoxMonitor,
	RealtimeAlm,
  },
  data() {
    return {
      /**
       * Vue Split Grid properties
       */
      animation: {
        duration: 500,
        easing: "easeInOutQuint"
      },
      splitGridOptions: {
		rowMinSizes: { 0: 500, 1: 200},
        snapOffset: 0,
        dragInterval: 0
      },
    };
  },
  computed: {
    size() {
      return { value: 400, unit: "px" };
    }
  }
};
</script>
<style lang="scss">
body {
  margin: 0;
}

#boxstate {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
  overflow: hidden;

  label {
    margin-right: 1rem;
  }
}

.sb_split-grid {
  height: calc(100% - 30px);
}

.sb_sub-grid {
	width: 100%;
	height: 100%;
}
.sb_sub-grid .vsg_area {
	width: 100%;
	position: relative;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.8s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.slide-to-right-enter-active,
.slide-to-right-leave-active {
  transition: opacity 0.2s, transform 0.2s ease;
}
.slide-to-right-enter,
.slide-to-right-leave-to {
  opacity: 0;
  transform: translateX(-20px);
}
.slide-to-left-enter-active,
.slide-to-left-leave-active {
  transition: opacity 0.2s, transform 0.2s ease;
}
.slide-to-left-enter,
.slide-to-left-leave-to {
  opacity: 0;
  transform: translateX(20px);
}
</style>
