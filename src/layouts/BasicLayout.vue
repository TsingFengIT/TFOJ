<template>
  <div id="basicLayout">
    <vue-particles
      color="#333333"
      :particleOpacity="0.7"
      :particlesNumber="60"
      shapeType="circle"
      :particleSize="4"
      linesColor="#333333"
      :linesWidth="1"
      :lineLinked="true"
      :lineOpacity="0.4"
      :linesDistance="150"
      :moveSpeed="2"
      :hoverEffect="true"
      hoverMode="grab"
      :clickEffect="true"
      clickMode="push"
      class="lizi"
    >
    </vue-particles>
    <a-layout style="min-height: 100vh">
      <a-layout-header class="header">
        <GlobalHeader />
      </a-layout-header>
      <a-layout-content class="content">
        <router-view />
      </a-layout-content>
      <a-layout-footer class="footer">
        <a href="https://TsingFeng.cn" target="_xblank"> 清风小破栈 </a>
      </a-layout-footer>
    </a-layout>
  </div>
</template>

<style scoped>
#basicLayout {
}

input {
  z-index: 999; /* 设置一个比粒子特效容器更高的 z-index 值 */
}

#basicLayout .header {
  margin-bottom: 16px;
  box-shadow: #eee 1px 1px 5px;
}

#basicLayout .content {
  background: linear-gradient(to right, #fefefe, #fff);
  margin-bottom: 16px;
  padding: 20px;
}

#basicLayout .footer {
  background: #efefef;
  padding: 16px;
  position: sticky;
  bottom: 0;
  left: 0;
  right: 0;
  text-align: center;
}

.lizi {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 95%;
  z-index: -1; /* 将粒子容器置于页面的最底层 */
}
</style>
<script>
import GlobalHeader from "@/components/GlobalHeader";
import VueParticles from "vue-particles/src/vue-particles/vue-particles.vue";

export default {
  components: { VueParticles, GlobalHeader },
};
</script>
