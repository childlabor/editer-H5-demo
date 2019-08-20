<template>
  <div class="home">
    <div class="mobile-box">
      <render-components :componentsRenderData="renderData" />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import RenderComponents from "@/components/RenderComponents.vue";

export default {
  name: "home",
  components: {
    RenderComponents
  },
  data() {
    return {
      // 拖拽后需要渲染的组件
      renderData: []
    }
  },

  created() {

  },

  mounted() {
    window.addEventListener('message', this.handleMessage)
  },

  methods: {
    handleMessage(event) {
      let data = event.data;
      if(typeof data === 'string') {
        this.renderData = JSON.parse(data)
        // console.log(this.renderData);
      }
    }
  }
};
</script>

<style>
.mobile-box {

}
</style>
