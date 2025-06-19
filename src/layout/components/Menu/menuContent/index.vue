<template>
  <el-scrollbar class="menu-wrapper">
    <el-menu
      :collapse="settingStore.isFold ? true : false"
      :default-active="$route.path"
      :default-openeds="getDefaultOpeneds"
      class="custom-menu"
    >
      <template v-for="item in menuList" :key="item.path">
        <el-menu-item
          v-if="!item.hidden && !item.children"
          :index="item.path"
          @click="goRoute"
          class="menu-item"
        >
          <el-icon class="menu-icon">
            <component :is="item?.meta?.icon"></component>
          </el-icon>
          <template #title>
            <span class="menu-title">
              {{ item?.meta?.title }}
            </span>
          </template>
        </el-menu-item>

        <el-menu-item
          v-if="!item.hidden && item?.children?.length === 1"
          :index="item?.children[0]?.path"
          @click="goRoute"
          class="menu-item"
        >
          <el-icon class="menu-icon">
            <component :is="item?.children[0]?.meta?.icon"></component>
          </el-icon>
          <template #title>
            <span class="menu-title">
              {{ item?.children[0]?.meta?.title }}
            </span>
          </template>
        </el-menu-item>

        <el-sub-menu
          v-if="!item.hidden && item?.children?.length > 1"
          :index="item.path"
          class="sub-menu"
        >
          <template #title>
            <el-icon class="menu-icon">
              <component :is="item?.meta?.icon"></component>
            </el-icon>
            <span class="menu-title">{{ item?.meta?.title }}</span>
          </template>
          <menu-content :menuList="item.children" />
        </el-sub-menu>
      </template>
    </el-menu>
    <!-- 底部渐变遮罩 -->
    <div class="bottom-fade"></div>
  </el-scrollbar>
</template>

<script setup lang="ts" name="MenuContent">
import { computed } from "vue";
import { useRouter, useRoute } from "vue-router";
import { useSettingStore } from "@/store/modules/setting";

const $router = useRouter();
const $route = useRoute();
const settingStore = useSettingStore();

defineProps(["menuList"]);

const getDefaultOpeneds = computed(() => {
  const path = $route.path;
  const lastIndex = path.lastIndexOf("/");
  return [path.slice(0, lastIndex)];
});

const goRoute = (vc: any) => {
  $router.push(vc.index);
};
</script>

<style scoped lang="scss">
.menu-wrapper {
  height: 100%;
  overflow: auto;
  border: none;
  background: transparent;
  box-shadow: none;
  border-radius: 0;
  position: static;
}

.custom-menu {
  border-right: none;
  background: transparent;
  position: relative;
  z-index: 3;
  
  .el-menu-item,
  .el-sub-menu__title {
    color: rgba(255, 255, 255, 0.9);
    border-radius: 8px;
    margin: 4px 8px;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative;
    overflow: hidden;
    backdrop-filter: blur(10px);
    
    &::before {
      content: '';
      position: absolute;
      top: 0;
      left: -100%;
      width: 100%;
      height: 100%;
      background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
      transition: left 0.5s;
    }
    
    &:hover {
      background: rgba(255, 255, 255, 0.15);
      color: #ffffff;
      transform: translateX(4px);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
      
      &::before {
        left: 100%;
      }
      
      .menu-icon {
        transform: scale(1.1);
        color: #ffffff;
      }
      
      .menu-title {
        color: #ffffff;
      }
    }
    
    &.is-active {
      background: linear-gradient(135deg, rgba(255, 255, 255, 0.2), rgba(255, 255, 255, 0.1));
      color: #ffffff;
      box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
      border-left: 4px solid #ffffff;
      
      .menu-icon {
        color: #ffffff;
      }
      
      .menu-title {
        color: #ffffff;
        font-weight: 600;
      }
    }
  }
  
  .menu-item {
    height: 50px;
    line-height: 50px;
  }
  
  .menu-icon {
    font-size: 18px;
    margin-right: 12px;
    transition: all 0.3s ease;
    color: rgba(255, 255, 255, 0.8);
  }
  
  .menu-title {
    font-size: 14px;
    font-weight: 500;
    transition: all 0.3s ease;
    color: rgba(255, 255, 255, 0.9);
  }
  
  .sub-menu {
    .el-sub-menu__title {
      height: 50px;
      line-height: 50px;
    }
    
    .el-menu {
      margin-left: 20px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 8px;
      margin: 4px 8px 4px 20px;
      border: 1px solid rgba(255, 255, 255, 0.05);
      
      .el-menu-item {
        margin: 2px 4px;
        border-radius: 6px;
        
        &:hover {
          background: rgba(255, 255, 255, 0.1);
        }
        
        &.is-active {
          background: rgba(255, 255, 255, 0.15);
          border-left: 3px solid #ffffff;
        }
      }
    }
  }
  
  // 折叠状态下的样式
  &.el-menu--collapse {
    .el-menu-item,
    .el-sub-menu__title {
      margin: 2px 4px;
      border-radius: 6px;
      
      &:hover {
        transform: none;
      }
    }
    
    .menu-icon {
      margin-right: 0;
    }
  }
}

// 响应式设计
@media (max-width: 768px) {
  .menu-wrapper {
    border-radius: 0;
    
    :deep(.el-scrollbar__wrap) {
      border-radius: 0;
    }
  }
  
  .custom-menu {
    .el-menu-item,
    .el-sub-menu__title {
      margin: 2px 4px;
      font-size: 13px;
    }
    
    .menu-icon {
      font-size: 16px;
    }
  }
}

// 深色主题适配
@media (prefers-color-scheme: dark) {
  .menu-wrapper {
    background: linear-gradient(135deg, #2c3e50 0%, #34495e 100%);
    
    &::after {
      background: linear-gradient(to bottom, 
        rgba(44, 62, 80, 0.8) 0%, 
        transparent 100%);
    }
    
    .bottom-fade {
      background: linear-gradient(to top, 
        rgba(52, 73, 94, 0.8) 0%, 
        transparent 100%);
    }
  }
}
</style>
