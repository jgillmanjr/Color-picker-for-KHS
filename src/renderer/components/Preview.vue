<template>
  <div class="preview-wrapper">
    <div class="top mb-3">
      <h3
        class="text-white toggle"
        :class="{disabled: collapsed}"
        @click="collapsed = !collapsed"
      >Preview</h3>
      <div v-if="!collapsed">
        <span class="text-white mx-2 mb-n2">Background</span>
        <div class="btn-group" role="group">
          <button
            class="btn btn-secondary"
            :class="{active: target == 'Phase_Plant'}"
            @click="target = 'Phase_Plant'"
          >PP</button>
          <button
            class="btn btn-secondary"
            :class="{active: target == 'Multipass'}"
            @click="target = 'Multipass'"
          >MP</button>
          <button
            class="btn btn-secondary"
            :class="{active: target == 'Snap_Heap'}"
            @click="target = 'Snap_Heap'"
          >SH</button>
        </div>
        <span class="text-white mx-2 mb-n2">Current view</span>
        <div class="btn-group" role="group" v-if="!collapsed">
          <button
            class="btn btn-secondary"
            :class="{active: showEQ == false}"
            @click="showEQ = false"
          >Main</button>
          <button
            class="btn btn-secondary"
            :class="{active: showEQ == true}"
            @click="showEQ = true"
          >EQ</button>
        </div>
      </div>
    </div>
    <div
      class="window-wrapper"
      :class="{hide: collapsed}"
      :style="{backgroundColor: getColor('panel_background')}"
    >
      <TopBar :title="showEQ ? 'SnapEQ' : 'ColorPicker'"></TopBar>

      <div class="window-inner" v-if="!showEQ">
        <div class="background-image" :style="{backgroundImage: `url(${getPluginImage(target)})`}"></div>
        <div class="main" :style="{border: `10px solid ${getColor('main_background')}`}"></div>

        <div class="footer"></div>
      </div>

      <div class="window-inner" v-if="showEQ">
        <div class="main" :style="{border: `10px solid ${getColor('main_background')}`}">
          <Slice></Slice>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapGetters, mapActions } from "vuex";

//Components
import ActiveKnob from "./preview-assets/ActiveKnob";
import Slice from "./preview-assets/Slice";
import TopBar from "./preview-assets/TopBar";

import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { fas } from "@fortawesome/free-solid-svg-icons";
export default {
  data() {
    return {
      collapsed: false,
      showEQ: false,
      target: "Phase_Plant"
    };
  },
  methods: {
    ...mapGetters(["getColorByLabel", "getComputedColor", "getRenderedImage"]),
    getPluginImage: function(name) {
      return this.getRenderedImage()(name);
    },
    getColor: function(label) {
      return this.getComputedColor()(this.getColorByLabel()(label), true);
    }
  },
  components: { ActiveKnob, TopBar, Slice }
};
</script>

<style scoped lang="scss">
.toggle {
  cursor: pointer;
  user-select: none;
}

.disabled {
  color: #707070 !important;
}

.top {
  width: 100%;
  display: flex;
  justify-content: space-between;
}

.hide {
  display: none;
}

.window-wrapper {
  width: 100%;
  overflow: hidden;
  position: relative;

  .window-inner {
    position: relative;
    z-index: 2;

    .background-image {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-repeat: no-repeat;
      background-position: center;
      background-size: 100% auto;
      z-index: -1;
    }
  }
}

.main {
  width: 100%;
  min-height: 382px;
  resize: vertical;
  overflow: auto;
}
</style>
