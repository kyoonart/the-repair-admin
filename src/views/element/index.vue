<template>
  <div>
    <Descrition title="报修信息管理" style="margin-bottom: 10px">
      <template #descrition> 维修情况</template>
    </Descrition>
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="id" label="ID" width="50" />
      <el-table-column prop="name" label="报修人" />
      <el-table-column prop="tel" label="电话" />
      <el-table-column prop="address" label="报修地点" />
      <el-table-column prop="descrition" label="问题描述" />
      <el-table-column prop="tel" label="报修时间" />
      <el-table-column prop="status" label="状态">
        <template #default="scope">
          <el-tag type="info" v-if="scope.row.status == 0" @click="handleEdit1(scope, scope.row)"
            >待开始</el-tag
          >
          <el-tag v-if="scope.row.status == 1" @click="handleEdit(scope.$index, scope.row)"
            >维修中</el-tag
          >
          <el-tag
            v-if="scope.row.status == 2"
            type="success"
            @click="handleEdit(scope.$index, scope.row)"
            >已完成</el-tag
          >
        </template>
      </el-table-column>
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
  import Descrition from '@/components/Descrition/index.vue';
  import { reactive, toRefs } from 'vue';
  export default {
    components: { Descrition },
    setup() {
      const state = reactive({
        icon: {},
      });
      const tableData = reactive([
        {
          id: '1',
          name: '王老师',
          tel: '15709255482',
          descrition: '投影仪坏了',
          status: 1,
          address: '长安校区基础教学楼A302',
        },
        {
          id: '2',
          name: '章三',
          tel: '15709255482',
          descrition: '宿舍水龙头坏了',
          status: 2,
          address: '7号宿舍楼',
        },
        {
          id: '3',
          name: '里斯',
          tel: '15709255482',
          status: 0,
          descrition: '宿舍水龙头坏了',
          address: '安悦2-302',
        },
        {
          id: '3',
          name: '里斯',
          tel: '15709255482',
          status: 2,
          descrition: '宿舍暖气不热',
          address: '安悦2-303',
        },
      ]);
      const handleEdit1 = (a, b) => {
        console.log(a, b);
      };
      return {
        ...toRefs(state),
        tableData,
        handleEdit1,
      };
    },
  };
</script>

<style lang="scss" scoped>
  .element-container {
    padding: $base-main-padding;
    background-color: $base-color-white;
    .row {
      margin: 20px 0;
    }
    .group {
      margin: 0 20px;
    }
  }
</style>
