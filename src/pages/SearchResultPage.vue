<template>
  <user-card-list :user-list="userList" :loading="loading" />
  <van-empty v-if="!userList || userList.length < 1" description="搜索结果为空" />
</template>

<script setup lang="ts">
import {onMounted, ref} from 'vue';
import {useRoute} from 'vue-router';
import myAxios from '../plugins/myAxios';
import {Toast} from 'vant';
import qs from 'qs';
import UserCardList from '../components/UserCardList.vue';

const route = useRoute();
const {tags} = route.query;

const userList = ref([]);
const loading = ref(true); // 添加 loading 状态

onMounted(async () => {
  try {
    const response = await myAxios.get('/user/search/tags', {
      params: {
        tagNameList: tags
      },
      paramsSerializer: params => {
        return qs.stringify(params, {indices: false});
      },
      withCredentials: true // 确保请求包含凭证（如 cookies）
    });

    console.log('/user/search/tags succeed', response);
    Toast.success('请求成功');

    const userListData = response?.data;
    if (userListData) {
      userListData.forEach(user => {
        if (user.tags) {
          user.tags = JSON.parse(user.tags);
        }
      });
      userList.value = userListData;
    }
  } catch (error) {
    console.error('/user/search/tags error', error);
    Toast.fail('请求失败');
  } finally {
    loading.value = false; // 请求完成后关闭 loading 状态
  }
});
</script>

<style scoped>
</style>
