<template>
  <div id="app">
    <sidebar></sidebar>
    <div
      class="list"
      id="list"
    >
    <div class="bg" :style="{ backgroundImage: 'url(' + background.url + ')' }">
      <VueDragResize
        v-for="(rect, index) in rects"
        :key="index"
        :w="rect.width"
        :h="rect.height"
        :x="rect.left"
        :y="rect.top"
        :parentW="listWidth"
        :parentH="listHeight"
        :axis="rect.axis"
        :isActive="rect.active"
        :minw="rect.minw"
        :minh="rect.minh"
        :angle="rect.angle"
        :isDraggable="rect.draggable"
        :isResizable="rect.resizable"
        :parentLimitation="rect.parentLim"
        :snapToGrid="rect.snapToGrid"
        :aspectRatio="rect.aspectRatio"
        :z="rect.zIndex"
        :contentClass="rect.class"
        v-on:activated="activateEv(index)"
        v-on:deactivated="deactivateEv(index)"
        v-on:dragging="changePosition($event, index)"
        v-on:resizing="changeSize($event, index)"
        v-on:rotating="changeRotate($event, index)"
      >
        <div class="filler" :style="{ backgroundImage: 'url(' + rect.img_url + ')' }">
        </div>
      </VueDragResize>
      </div>
    </div>

  </div>
</template>

<style>
body {
}

#app {
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  position: relative;
  font-family: 'Lato', sans-serif;
  display: flex;
  justify-content: space-between;
  align-items: stretch;
}

.filler {
  width: 100%;
  height: 100%;
  display: inline-block;
  position: absolute;
  background-size: 100% 100%;
}

.list {
  box-shadow: 0 0 2px #aaa;
  background-color: white;
  flex: 0 0 1312px;
  
}
.list .bg{
    position: relative;
    background-repeat: no-repeat;
    background-size: contain;
    background-position: top center;
    width: 100%;
    padding-bottom: 100%;
}

.box-shaddow {
  box-shadow: 10px 10px 15px 0px rgba(125, 125, 125, 1);
}
</style>

<script>
import VueDragResize from '../components/vue-drag-resize.vue'
import toolbar from './components/toolbar/toolbar.vue'
import sidebar from './components/sidebar/sidebar.vue'
import './icons'

export default {
  name: 'app',

  components: {
    VueDragResize,
    toolbar,
    sidebar,
  },

  data() {
    return {
      listWidth: 0,
      listHeight: 0,
    }
  },

  mounted() {
    let listEl = document.getElementById('list')
    this.listWidth = listEl.clientWidth
    this.listHeight = listEl.clientHeight

    window.addEventListener('resize', () => {
      this.listWidth = listEl.clientWidth
      this.listHeight = listEl.clientHeight
    })
  },

  computed: {
    rects() {
      return this.$store.state.rect.rects
    },
    background(){
        return this.$store.state.rect.background
    }
  },

  methods: {
    activateEv(index) {
      this.$store.dispatch('rect/setActive', { id: index })
    },

    deactivateEv(index) {
      this.$store.dispatch('rect/unsetActive', { id: index })
    },

    changePosition(newRect, index) {
      this.$store.dispatch('rect/setTop', { id: index, top: newRect.top })
      this.$store.dispatch('rect/setLeft', { id: index, left: newRect.left })
      this.$store.dispatch('rect/setWidth', { id: index, width: newRect.width })
      this.$store.dispatch('rect/setHeight', {
        id: index,
        height: newRect.height,
      })
    },

    changeSize(newRect, index) {
      this.$store.dispatch('rect/setTop', { id: index, top: newRect.top })
      this.$store.dispatch('rect/setLeft', { id: index, left: newRect.left })
      this.$store.dispatch('rect/setWidth', { id: index, width: newRect.width })
      this.$store.dispatch('rect/setHeight', {
        id: index,
        height: newRect.height,
      })
    },
    changeRotate(rotate, index) {
      this.$store.dispatch('rect/setRotate', { id: index, angle: rotate })
    },
  },
}
</script>
