<template>
	<view class="page">
  <div class="my-approvals-container">
	  <view class="custom-header">申请</view>
    <el-space direction="vertical" class="header">
      <el-text size="large">我发起的工单</el-text>
    </el-space>

    <div class="card-list">
      <el-card
        v-for="(row, index) in visibleApprovals"
        :key="row.id"
        class="card"
      >
        <div class="card-content">
          <div class="card-header">
            <span class="card-title">{{ row.room_name }}</span>
            <span class="card-status">{{ formatStatus(row) }}</span>
          </div>
          <div class="card-body">
            <p><strong>机房名:</strong> {{ row.sys_name }}</p>
            <p><strong>管理员:</strong> {{ row.manager_name }}（{{ row.manager_telephone }}）</p>
            <p><strong>申请时间:</strong> {{ row.create_time }}</p>
            <p><strong>申请事由:</strong> {{ row.notes }}</p>
          </div>
        </div>
      </el-card>
    </div>

    <!-- 显示隐藏切换 -->
    <div class="toggle-expired" v-if="expiredApprovals.length > 0">
      <el-button type="text" @click="showExpired = !showExpired">
        {{ showExpired ? '隐藏过期工单' : '显示过期工单' }}
      </el-button>
    </div>
	
	<!-- 新建审批工单按钮 -->
	<div class="create-button-wrapper">
	  <el-button type="primary" icon="el-icon-plus" @click="dialogVisible = true">
	    新建审批工单
	  </el-button>
	</div>
	
	<!-- 新建审批工单 Dialog -->
	<el-dialog
	  title="新建审批工单"
	  :visible.sync="dialogVisible"
	  width="100%"
	  :close-on-click-modal="false"
	>
	  <CreateApproveForm @success="onCreateSuccess" />
	</el-dialog>
	
  </div>
  </view>
</template>

<script>
import CreateApproveForm from './CreateApproveForm.vue';
export default {
  name: 'MyApprovals',
  components: { CreateApproveForm },
  data() {
    return {
      allApprovals: [],       // 全部工单
      showExpired: false,     // 是否展开过期工单
	  dialogVisible: false
    };
  },
  computed: {
    today() {
      const today = new Date();
      return today.toISOString().slice(0, 10); // 格式：yyyy-mm-dd
    },
    todayApprovals() {
      return this.allApprovals.filter(a =>
        a.create_time.startsWith(this.today)
      );
    },
    expiredApprovals() {
      return this.allApprovals.filter(a =>
        !a.create_time.startsWith(this.today)
      );
    },
    visibleApprovals() {
      return this.showExpired
        ? this.allApprovals
        : this.todayApprovals;
    }
  },
  methods: {
    formatStatus(row) {
      if (row.app_status === true) return '✅ 已通过';
      if (row.app_status === false) return '❌ 已拒绝';
      return '⏳ 待审批';
    },
    // 模拟加载 demo 数据
    loadDemoData() {
      this.allApprovals = [
        {
          id: 8,
          room_name: '绵阳涪城六八局站综合机房',
          sys_name: '3301_绵阳六八局站G-GSM900M基站',
          manager_name: '张三',
          manager_telephone: '18888888888',
          user_name: 'admin',
          create_time: new Date().toISOString().slice(0, 19).replace('T', ' '),
          app_status: true,
          notes: '日常巡检'
        },
        {
          id: 1,
          room_name: '绵阳涪城六八局站综合机房',
          sys_name: '3301_绵阳六八局站G-GSM900M基站',
          manager_name: '张三',
          manager_telephone: '18888888888',
          user_name: 'admin',
          create_time: '2024-11-19 01:53:00',
          app_status: false,
          notes: '安全检查'
        },
        {
          id: 1,
          room_name: '绵阳涪城六八局站综合机房',
          sys_name: '3301_绵阳六八局站G-GSM900M基站',
          manager_name: '张三',
          manager_telephone: '18888888888',
          user_name: 'admin',
          create_time: '2024-11-19 01:53:00',
          app_status: true,
          notes: '安全检查'
        }
      ];
    },
	onCreateSuccess() {
	  this.dialogVisible = false;
	  this.loadDemoData(); // 重新加载审批列表
	}
  },
  created() {
    this.loadDemoData();
  }
};
</script>

<style scoped>
	.page {
	  padding-top: 80rpx; /* 留出固定标题栏的高度 */
	  height: 100vh;
	  box-sizing: border-box;
	}
	
	.custom-header {
	  position: fixed;
	  top: 0;
	  left: 0;
	  width: 100%;
	  height: 70rpx;
	  padding-top: 20rpx;
	  background-color: #007aff;
	  color: #fff;
	  text-align: center;
	  font-size: 32rpx;
	  line-height: 50rpx;
	  font-weight: bold;
	  z-index: 1000;
	}
	
	.main-content {
	  padding: 30rpx;
	}
	
.my-approvals-container {
  padding: 20px;
}

.header {
  margin-bottom: 20px;
  text-align: center;
  padding-bottom: 8rpx;
}

.card-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 20px;
}

.card {
  border-radius: 12px;
  overflow: hidden;
}

.card-content {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.card-header {
  display: flex;
  justify-content: space-between;
  font-weight: bold;
  font-size: 16px;
}

.card-status {
  font-size: 14px;
  color: #666;
}

.card-body {
  font-size: 14px;
  color: #666;
}

.toggle-expired {
  margin-top: 20px;
  text-align: center;
}

.create-button-wrapper {
  position: fixed;
  bottom: 140rpx;
  left: 50%;
  transform: translateX(-50%);
  z-index: 999;
}

</style>
