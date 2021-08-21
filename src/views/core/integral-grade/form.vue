<!--  -->
<template>
  <div class="app-container">
    <!-- 输入表单 -->
    <el-form label-width="120px">
      <el-form-item label="借款额度">
        <el-input-number v-model="integralGrade.borrowAmount" :min="0" />
      </el-form-item>
      <el-form-item label="积分区间开始">
        <el-input-number v-model="integralGrade.integralStart" :min="0" />
      </el-form-item>
      <el-form-item label="积分区间结束">
        <el-input-number v-model="integralGrade.integralEnd" :min="0" />
      </el-form-item>
      <el-form-item>
        <el-button
          :disabled="saveBtnDisabled"
          type="primary"
          @click="saveOrUpdate()"
        >
          保存
        </el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
import integralGradeApi from '@/api/core/integral-grade'

export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  data() {
    //这里存放数据
    return {
      saveBtnDisabled: false,
      integralGrade: {} //积分等级对象
    }
  },
  //监听属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    fetchById(id) {
      integralGradeApi.getById(id).then(response => {
        this.integralGrade = response.data.record
      })
    },
    // 保存或修改
    saveOrUpdate() {
      this.saveBtnDisabled = true
      if (!this.integralGrade.id) {
        // 调用新增
        this.saveData()
      } else {
        // 调用更新
        this.updateData()
      }
    },
    saveData() {
      integralGradeApi.save(this.integralGrade).then(response => {
        // this.$message({
        //   type: 'success',
        //   message: response.message
        // })
        this.$message.success(response.message)
        this.$router.push('/core/integral-grade/list')
      })
    },
    updateData() {
      integralGradeApi.updateById(this.integralGrade).then(response => {
        this.$message.success(response.message)
        this.$router.push('/core/integral-grade/list')
      })
    }
  },
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {
    // 当路由中有id时，是回显表单，需要调用回显数据的接口
    if (this.$route.params.id) {
      this.fetchById(this.$route.params.id)
    }
  },
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //生命周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁完成
  activated() {} //如果页面有keep-alive缓存功能，这个函数会触发
}
</script>
<style scoped></style>
