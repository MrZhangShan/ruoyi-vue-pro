<script lang="ts" setup>
import type { InfraRedisApi } from '#/api/infra/redis';

import { onMounted, ref } from 'vue';

import { DocAlert, Page } from '@vben/common-ui';

import { NCard } from 'naive-ui';

import { getRedisMonitorInfo } from '#/api/infra/redis';

import Commands from './modules/commands.vue';
import Info from './modules/info.vue';
import Memory from './modules/memory.vue';

const redisData = ref<InfraRedisApi.RedisMonitorInfo>();

/** 统一加载 Redis 数据 */
const loadRedisData = async () => {
  try {
    redisData.value = await getRedisMonitorInfo();
  } catch (error) {
    console.error('加载 Redis 数据失败', error);
  }
};

onMounted(() => {
  loadRedisData();
});
</script>

<template>
  <Page auto-content-height>
    <template #doc>
      <DocAlert title="Redis 缓存" url="https://doc.iocoder.cn/redis-cache/" />
      <DocAlert title="本地缓存" url="https://doc.iocoder.cn/local-cache/" />
    </template>

    <NCard class="mt-5" title="Redis 概览">
      <Info :redis-data="redisData" />
    </NCard>

    <div class="mt-5 grid grid-cols-1 gap-4 md:grid-cols-2">
      <NCard title="内存使用">
        <Memory :redis-data="redisData" />
      </NCard>

      <NCard title="命令统计">
        <Commands :redis-data="redisData" />
      </NCard>
    </div>
  </Page>
</template>
