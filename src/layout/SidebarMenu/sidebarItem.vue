<script setup lang="ts">
import type { RouteRecordRaw } from "vue-router";
import Link from "./link.vue";
import Title from "./title.vue";
const props = defineProps<{
  parentPath?: string;
  item: RouteRecordRaw;
}>();

const { item, parentPath } = toRefs(props),
  { meta, path, children } = item.value;
const hasChildren = props.item.children?.length ?? false,
  routePath = `${parentPath ? parentPath + "/" : ""}${path}`;
</script>

<template>
  <el-sub-menu v-if="hasChildren" :index="routePath">
    <template #title>
      <Title :title="meta?.title" :icon="meta?.icon" />
    </template>
    <el-menu-item-group>
      <SidebarItem
        v-for="child in children"
        :item="child"
        :key="child.path"
        :parentPath="path"
      />
    </el-menu-item-group>
  </el-sub-menu>
  <Link v-else :route="path" :parentRoute="parentPath ?? ''">
    <el-menu-item :index="routePath">{{ meta?.title }}</el-menu-item>
  </Link>
</template>
