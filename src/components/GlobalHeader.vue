<template>
  <a-row id="globalHeader" align="center" :wrap="false">
    <a-col flex="auto">
      <a-menu
        mode="horizontal"
        :selected-keys="selectedKeys"
        @menu-item-click="doMenuClick"
      >
        <a-menu-item
          key="0"
          :style="{ padding: 0, marginRight: '38px' }"
          disabled
        >
          <div class="title-bar">
            <img class="logo" src="../assets/logo.png" />
            <div class="title">OJ</div>
          </div>
        </a-menu-item>
        <a-menu-item v-for="item in visibleRoutes" :key="item.path">
          {{ item.name }}
        </a-menu-item>
      </a-menu>
    </a-col>
    <a-col flex="150px">
      <div class="title-bar">
        <button @click="goLoginOrOut">
          {{ store.state.user?.loginUser?.userName ?? "未登录" }}
        </button>
        <img :src="store.state.user?.loginUser?.userAvatar" style="zoom: 15%" />
      </div>
    </a-col>
  </a-row>
</template>

<script setup lang="ts">
import { routes } from "../router/routes";
import { useRoute, useRouter } from "vue-router";
import { computed, ref } from "vue";
import { useStore } from "vuex";
import checkAccess from "@/access/checkAccess";
import ACCESS_ENUM from "@/access/accessEnum";
import { UserControllerService } from "../../generated";
import message from "@arco-design/web-vue/es/message";
import VueParticles from "vue-particles/src/vue-particles/vue-particles.vue";

const router = useRouter();
const store = useStore();

// 展示在菜单的路由数组
const visibleRoutes = computed(() => {
  return routes.filter((item, index) => {
    if (item.meta?.hideInMenu) {
      return false;
    }
    // 根据权限过滤菜单
    if (
      !checkAccess(store.state.user.loginUser, item?.meta?.access as string)
    ) {
      return false;
    }
    return true;
  });
});

// 默认主页
const selectedKeys = ref(["/"]);

// 路由跳转后，更新选中的菜单项
router.afterEach((to, from, failure) => {
  selectedKeys.value = [to.path];
});

console.log();

// setTimeout(() => {
//   store.dispatch("user/getLoginUser", {
//     userName: "清风管理员",
//     userRole: ACCESS_ENUM.ADMIN,
//   });
// }, 3000);
const goLoginOrOut = async () => {
  if (store.state.user?.loginUser?.userName == "未登录") {
    router.push({ path: "/user/login" });
  } else {
    const res = await UserControllerService.userLogoutUsingPost();
    if (res.code === 0) {
      router
        .push({
          path: "/",
          replace: true,
        })
        .then(() => {
          window.location.reload();
          message.info("注销成功");
        });
    } else {
      message.error("注销失败，" + res.message);
    }
  }
};
const doMenuClick = (key: string) => {
  router.push({
    path: key,
  });
};
</script>

<style scoped>
#globalHeader {
  background-color: rgba(255, 255, 255, 0.1);
}

.title-bar {
  display: flex;
  align-items: center;
}

.title {
  color: #444;
  margin-left: 16px;
}

.logo {
  height: 48px;
}
</style>
