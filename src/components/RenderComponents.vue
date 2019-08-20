<template>
  <div class="hello">
    <div
      class="group"
      v-for="(element, index) in componentsRenderData"
      :key="index"
    >
      <component :is="element.comps" :options="element.property">
        <render-components :componentsRenderData="element.childs" />
      </component>
    </div>
  </div>
</template>

<script>
export default {
  name: "RenderComponents",
  props: {
    componentsRenderData: {
      require: true
    }
  },
  data() {
    return {

    }
  },

  watch: {
    // 监听父页面传递了新数据 更新视图
    componentsRenderData(newValue) {
      this.requireComps(this.componentsRenderData)
    }
  },

  created() {
    console.log('init comps');
    this.requireComps(this.componentsRenderData)
  },

  methods: {
    // 动态加载组件-视窗
    requireComps(renderObj) {
      if(renderObj instanceof Array) {
        renderObj.forEach(item => {
          // 存在组件名则动态加载组件
          if(item.name) {
            this.$set(item, 'comps',
              function(resolve) {
                require([`./labs/${item.name}.vue`], resolve)
              }
            )
          }
          // 存在嵌套 递归
          if(item.childs) {
            this.requireComps(item.childs);
          }
          // console.log(item);
        })
      }
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
