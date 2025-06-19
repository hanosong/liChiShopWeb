<template>
  <el-scrollbar class="menu-wrapper">
    <menu-content :menuList="routes" />
  </el-scrollbar>
</template>

<script setup lang="ts" name="Menu">
import { usePermissionStore } from "@/store/modules/permission";
import MenuContent from "./menuContent/index.vue";

const permissionStore = usePermissionStore();
const routes = permissionStore.routes;
</script>

<style scoped lang="scss">
.menu-wrapper {
  width: 220px;
  height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 0 16px 16px 0;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
  overflow: hidden;
  position: relative;
  border: 1px solid rgba(255,255,255,0.08);
}
</style>
