<template>
  <div>
    <van-skeleton v-for="user in props.userList" :key="user.id" title avatar :row="3" :loading="loading">
      <van-card
          :desc="user.profile"
          :title="`${user.username}（${user.planetCode}）`"
          :thumb="user.avatarUrl"
      >
        <template #tags>
          <van-tag plain type="danger" v-for="tag in user.tags" :key="tag" style="margin-right: 8px; margin-top: 8px">
            {{ tag }}
          </van-tag>
        </template>
        <template #footer>
          <van-button size="mini">联系我</van-button>
        </template>
      </van-card>
    </van-skeleton>
  </div>
</template>

<script setup lang="ts">
import {UserType} from '../models/user';
import {defineProps, withDefaults} from 'vue';

interface UserCardListProps {
  loading: boolean;
  userList: UserType[];
}

const props = withDefaults(defineProps<UserCardListProps>(), {
  loading: true,
  userList: [] as UserType[],
});
</script>

<style scoped>
</style>
