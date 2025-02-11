<template>
  <div class="index-page">
    <a-input-search
      v-model:value="searchParams.text"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch"
    />
  </div>
  <!-- {{ searchParams }} -->
  <!-- {{ postList }} -->
  <MyDivider />
  <a-tabs v-model:activeKey="activeKey" @change="onChange">
    <a-tab-pane key="Post" tab="文章">
      <PostList :post-list="postList" />
    </a-tab-pane>
    <a-tab-pane key="picture" tab="图片">
      <PictureList />
    </a-tab-pane>
    <a-tab-pane key="User" tab="用户">
      <UserList :user-list="userList" />
    </a-tab-pane>
  </a-tabs>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PostList from "../components/PostList.vue";
import PictureList from "../components/PictureList.vue";
import UserList from "../components/UserList.vue";
import MyDivider from "../components/MyDivider.vue";
import { useRoute, useRouter } from "vue-router";
import myAxios from "@/plugins/MyAxios";

// myAxios
//   .get("/post/get/vo", {
//     params: { id: "1887905375068475394" },
//   })
//   .then((res) => {
//     console.log(res);
//   });

const postList = ref([]);
const userList = ref([]);

myAxios.post("/user/list/page/vo", {}).then((res: any) => {
  userList.value = res.records;
});

myAxios.post("/post/list/page/vo", {}).then((res: any) => {
  postList.value = res.records;
});

const route = useRoute();
const router = useRouter();
const activeKey = route.params.category;

const initSearchParams = {
  text: "",
  pageSize: 10,
  pageNum: 1,
};

const searchParams = ref(initSearchParams);

watchEffect(() => {
  searchParams.value = {
    ...initSearchParams,
    text: route.query.text,
  } as any;
});

const onSearch = (value: string) => {
  alert(value);
  router.push({
    query: searchParams.value,
  });
};
const onChange = (key: string) => {
  router.push({
    path: `/${key}`,
    query: searchParams.value,
  });
};
</script>
