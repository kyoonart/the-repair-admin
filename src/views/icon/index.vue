<template>
  <div class="icon-container">
    <Descrition title="用户管理" style="margin-bottom: 10px">
      <template #descrition> 用户信息及权管理 </template>
    </Descrition>
    <el-table :data="data" border style="width: 100%">
      <el-table-column prop="id" label="ID"> </el-table-column>
      <el-table-column prop="name" label="姓名"> </el-table-column>
      <el-table-column prop="identity" label="身份"> </el-table-column>
      <el-table-column label="操作">
        <template #default="scope">
          <el-tag style="margin-right: 10px" @click="handleEdit(scope.$index, scope.row)"
            >编辑</el-tag
          >
          <el-tag class="ml-2" type="danger" @click="handleDelete(scope.$index, scope.row)"
            >删除</el-tag
          >
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
  import { getIcons } from '@/api/icon';
  import { reactive, toRefs, onBeforeMount } from 'vue';
  import Descrition from '@/components/Descrition/index.vue';
  import { useI18n } from 'vue-i18n';

  export default {
    components: { Descrition },
    setup() {
      const { t } = useI18n();
      const state = reactive({
        icon: {},
      });
      const data = [
        {
          id: 1,
          name: '张三',
          identity: '管理员',
        },
        {
          id: 2,
          name: '李老师',
          identity: '用户',
        },
        {
          id: 3,
          name: '兰师傅',
          identity: '维修师傅',
        },
      ];
      onBeforeMount(async () => {
        await handleGetIcons();
      });

      const handleGetIcons = async () => {
        getIcons().then((res) => {
          state.icon = res.data;
        });
      };

      return {
        ...toRefs(state),
        t,
        data,
      };
    },
  };
</script>
<style lang="scss" scoped>
  .icon-container {
    padding: $base-main-padding;
    background-color: $base-color-white;
    .title {
      &.reset {
        margin-top: 40px;
      }
    }
    .icon-centent {
      display: flex;
      flex-wrap: wrap;
      .icon-item {
        display: flex;
        align-items: center;
        padding: 5px 15px;
        margin: 5px;
        cursor: pointer;
        border-radius: 6px;
        box-shadow: $base-box-shadow;
        .icon-name {
          width: 30px;
          width: 30px;
        }
      }
    }
  }
</style>
