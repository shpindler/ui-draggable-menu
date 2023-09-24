<template>
  <div class="ui-draggable-menu" :style="{ top: this.top + 'vh' }">
    <div
      :class="[
        'ui-draggable-menu__holder',
        {
          'ui-draggable-menu__holder_hidden': this.top === this.fullScreenBreakpoint,
        },
      ]"
      @touchstart="onTouchStart"
      @touchmove="onTouchMove"
      @touchend="onTouchEnd"
    ></div>
    <slot></slot>
  </div>
</template>

<script>
export default {
  data() {
    return {
      lowestBreakpoint: 90,
      averageBreakpoint: 50,
      fullScreenBreakpoint: 0,
      startY: 0,
      currentY: 0,
      top: 50,
      dragDirection: 0,
    };
  },
  watch: {
    top(newValue) {
      if (newValue === this.lowestBreakpoint) {
        this.$emit("close");
      }
    }
  },
  methods: {
    onTouchStart(e) {
      this.startY = e.touches[0].clientY;
    },
    onTouchMove(e) {
      this.currentY = e.touches[0].clientY;
      let diff = ((this.startY - this.currentY) / window.innerHeight) * 100;
      this.top = Math.min(Math.max(this.top - diff, this.fullScreenBreakpoint), this.lowestBreakpoint);
      this.dragDirection = this.currentY - this.startY;
      this.startY = this.currentY;
    },
    onTouchEnd() {
      if (this.dragDirection > 0) {
        if (this.top < this.averageBreakpoint) {
          this.top = this.averageBreakpoint;
        } else {
          this.top = this.lowestBreakpoint;
        }
      } else {
        if (this.top > this.averageBreakpoint) {
          this.top = this.averageBreakpoint;
        } else {
          this.top = this.fullScreenBreakpoint;
        }
      }
    },
  },
};
</script>

<style>
.ui-draggable-menu {
  position: fixed;
  top: 100vh;
  left: 0;
  right: 0;
  transition: top 0.3s ease-out;
}

.ui-draggable-menu__holder {
  width: 40px;
  height: 5px;
  margin: 8px auto;
  background-color: #bdbdbd;
  border-radius: 2.5px;
  cursor: grab;
}

.ui-draggable-menu__holder:active {
  cursor: grabbing;
}

.ui-draggable-menu__holder_hidden {
  opacity: 0;
}
</style>
