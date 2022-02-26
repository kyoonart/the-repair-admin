<template>
  <div class="index-conntainer">
    <div class="head-card">
      <div class="avatar">
        <el-avatar :size="50" :src="avatar"></el-avatar>
      </div>
      <div class="head-card-content">
        <h2 class="title">{{ sayHi }}! {{ t('indexPage.descTitle') }}</h2>
      </div>
    </div>
    <div class="content">
      <el-row :gutter="10">
        <el-col :xs="24" :sm="24" :md="24" :lg="16" :xl="16">
          <el-card class="card" shadow="hover">
            <template #header>
              <h3 class="title">信息一览</h3>
            </template>
            <div class="card-body" :class="{ mobile: isMobile }">
              <Echarts />
            </div>
          </el-card>
          <el-card class="card" shadow="hover">
            <template #header>
              <h3 class="title">完成情况</h3>
            </template>
            <el-descriptions class="margin-top" :column="3" :size="size" border>
              <el-descriptions-item v-for="(value, key) in packpage.dependencies" :key="key">
                <template #label>
                  {{ key }}
                </template>
                {{ value }}
              </el-descriptions-item>
            </el-descriptions>
          </el-card>
        </el-col>
        <el-col :xs="24" :sm="24" :md="24" :lg="8" :xl="8">
          <el-card class="card" shadow="hover">
            <template #header>
              <h3 class="title">完成情况</h3>
            </template>
            <div class="count-box">
              <div class="item" v-for="(item, index) in state.orderList" :key="index">
                <span class="label">{{ t('indexPage.order.' + item.key) }}</span>
                <CountTo
                  class="count"
                  :class="item.status"
                  :startVal="0"
                  :endVal="item.value"
                  :duration="3000"
                ></CountTo>
              </div>
            </div>
          </el-card>
          <el-card class="card" shadow="hover">
            <template #header>
              <h3 class="title">{{ t('indexPage.skillTitle') }}</h3>
            </template>
            <div v-for="(item, index) in state.skillList" :key="index">
              <div class="skill-title">{{ item.title }}</div>
              <el-progress
                :stroke-width="8"
                :percentage="item.percentage"
                :color="item.color"
              ></el-progress>
            </div>
          </el-card>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Index',
  };
</script>

<script setup>
  import { ref, computed, reactive, onBeforeMount } from 'vue';

  import { CountTo } from 'vue3-count-to';
  import packpage from '../../../package.json';
  import Echarts from '../echarts/line.vue';

  import { useStore } from 'vuex';
  const store = useStore();

  import { useI18n } from 'vue-i18n';
  const { t } = useI18n();

  const state = reactive({
    list: [],
    prefix: '',
    orderList: [],
    skillList: [],
    series: [{ data: [10, 20, 30, 40, 50, 60, 260], type: 'line' }],
    xAxis: {
      type: 'category',
      data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
    },
    yAxis: {
      type: 'value',
    },
    toolbox: {
      show: true,
    },
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'cross',
        label: {
          backgroundColor: '#6a7985',
        },
      },
    },
  });

  import { getResouceList } from '@/api/index';

  const hour = new Date().getHours();
  const thisTime =
    hour < 8
      ? t('sayHi.early')
      : hour <= 11
      ? t('sayHi.morning')
      : hour <= 13
      ? t('sayHi.noon')
      : hour < 18
      ? t('sayHi.afternoon')
      : t('sayHi.evening');
  const sayHi = ref(thisTime);
  const avatar = ref(
    'https://th.bing.com/th/id/R.2bea6e7b06c072e3072a531bfc1d426b?rik=585uPQSWmziNvA&riu=http%3a%2f%2fwww.gx8899.com%2fuploads%2fallimg%2f171206%2f3-1G206093252.jpg&ehk=JDR0sMKc9IGD99zdvEiOfjtrCaJiVqX9pfOpwzGK33I%3d&risl=&pid=ImgRaw&r=0'
  );

  const series2 = reactive([
    {
      data: [820, 932, 901, 934, 1290, 1330, 1320],
      type: 'line',
      smooth: true,
    },
  ]);

  const xAxis = reactive({
    type: 'category',
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
  });

  const yAxis = reactive({
    type: 'value',
  });

  const toolbox = reactive({
    show: true,
  });

  const isMobile = computed(() => {
    return store.getters['setting/isMobile'];
  });

  const onGetResouceList = () => {
    getResouceList().then((res) => {
      const { list, prefix, orderList, skillList } = res.data;
      Object.assign(state, { list, prefix, orderList, skillList });
    });
  };

  const handleToDetail = (url) => {
    window.open(url);
  };

  onBeforeMount(() => {
    onGetResouceList();
  });
</script>

<style lang="scss" scoped>
  .index-conntainer {
    width: $base-width;
    .head-card {
      display: flex;
      align-items: center;
      padding: $base-main-padding;
      background-color: $base-color-white;
      &-content {
        padding-left: 15px;
        .desc {
          color: $base-font-color;
        }
      }
    }
    .content {
      margin: 15px 0;
      .count-box {
        display: flex;
        align-items: center;
        justify-content: space-between;
        .item {
          display: flex;
          flex-direction: column;
          text-align: center;
          .label {
            padding: 10px 0;
            font-size: $base-font-size-big;
          }
          .count {
            font-size: $base-font-size-max;
            font-weight: bolder;
            color: $base-color-primary;
            &.error {
              color: var(--el-color-danger);
            }
            &.success {
              color: var(--el-color-success);
            }
          }
        }
      }
      .title {
        margin: 0;
      }
      .skill-title {
        padding: 10px 0;
        font-weight: 500;
      }
      .card {
        margin-bottom: 15px;
        &-body {
          /* display: grid;
          grid-template-columns: repeat(4, 4fr); */
          &.mobile {
            grid-template-columns: repeat(1, 1fr);
          }
          .item {
            box-sizing: border-box;
            padding: 10px 20px;
            margin-top: -1px;
            margin-left: -1px;
            overflow: hidden;
            cursor: pointer;
            border: 1px solid black;
            border: 1px solid #eee;
            transition: box-shadow 0.5;
            .lf {
              display: flex;
              align-items: center;
              max-width: 140px;
              .img {
                width: auto;
                max-width: 120px;
                height: auto;
                max-height: 40px;
              }
            }
            &:hover {
              box-shadow: $base-box-shadow;
            }
            .title {
              padding-left: 5px;
              font-size: 18px;
              font-weight: bold;
            }
            .desc {
              padding: 5px 0;
              font-size: 13px;
              line-height: 1.5;
              color: $base-font-color;
            }
          }
        }
      }
    }
  }
</style>
