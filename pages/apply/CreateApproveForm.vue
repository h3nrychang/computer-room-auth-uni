<template>
  <el-form :model="form" :rules="rules" ref="form" label-width="100px">
    <!-- 自动完成机房搜索 -->
    <el-form-item label="机房名" prop="room_id">
      <el-autocomplete
        v-model="searchText"
        :fetch-suggestions="querySearch"
        placeholder="请输入机房名"
        @select="handleSelect"
      />
    </el-form-item>

    <el-form-item v-if="selectedRoom">
      <p><strong>地址:</strong> {{ selectedRoom.address }}</p>
      <p><strong>系统:</strong> {{ selectedRoom.sys_name }}</p>
      <p><strong>管理员:</strong> {{ selectedRoom.manager_name }}（{{ selectedRoom.manager_telephone }}）</p>
    </el-form-item>

    <!-- 理由输入 -->
    <el-form-item label="备注" prop="notes">
      <el-input type="textarea" v-model="form.notes" :rows="3" />
    </el-form-item>

    <!-- 提交按钮 -->
    <el-form-item>
      <el-button type="primary" @click="submitForm">提交</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import { get, post } from '@/util/request';
import { Message } from 'element-ui';

export default {
  name: 'CreateApproveForm',
  data() {
    return {
      searchText: '',
      selectedRoom: null,
      roomOptions: [],
      form: {
        room_id: null,
        notes: ''
      },
      rules: {
        room_id: [{ required: true, message: '请选择机房', trigger: 'blur' }]
      }
    };
  },
  methods: {
    async querySearch(queryString, cb) {
      try {
        const res = await get('/room/get', { name: queryString });
        this.roomOptions = res.data;
        const results = res.data.map(room => ({
          value: room.name,
          room
        }));
        cb(results);
      } catch (e) {
        Message.error('查询机房失败');
        cb([]);
      }
    },
    handleSelect(item) {
      this.selectedRoom = item.room;
      this.form.room_id = item.room.id;
    },
    submitForm() {
      this.$refs.form.validate(async (valid) => {
        if (!valid) return;
        try {
          const res = await post('/approve/open', this.form);
          Message.success(res.data.detail || '提交成功');
          this.$emit('success');
        } catch (e) {
          Message.error('提交失败');
        }
      });
    }
  }
};
</script>
