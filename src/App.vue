<template>
  <div id="app">
    <DraggableMenu @close="onClose">
      I am Draggable Menu
    </DraggableMenu>
  </div>
</template>

<script>
import DraggableMenu from './components/DraggableMenu.vue'

export default {
  name: 'App',
  components: {
    DraggableMenu
  },
  methods: {
    onClose() {
      console.log("close");
    }
  }
}
</script>
