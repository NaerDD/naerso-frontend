<template>
  <a-list
    item-layout="horizontal"
    :data-source="$props.pictureList"
    :grid="{ gutter: 16, xs: 1, sm: 2, md: 4, lg: 4, xl: 6, xxl: 3 }"
  >
    <template #renderItem="{ item }">
      <a-list-item>
        <a-card hoverable>
          <template #cover>
            <img alt="example" :src="item.url" />
          </template>
          <a-card-meta :title="item.title" />
        </a-card>
      </a-list-item>
    </template>
  </a-list>
</template>

<script setup lang="ts">
import { withDefaults, defineProps } from "vue";

interface Props {
  pictureList: any[];
}

const pictureList = withDefaults(defineProps<Props>(), {
  pictureList: () => [],
});
</script>

<style scoped>
.gege {
  width: 200px;
}
</style>
