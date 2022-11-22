<template>
  <div>
    <div class="top-filter">
      <el-form :inline="true" :model="form">
        <el-form-item label="题目类型">
          <el-input v-model="form.type"/>
        </el-form-item>
        <el-form-item label="题目描述">
          <el-input v-model="form.title"/>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="search">查询</el-button>
          <el-button type="primary" @click="create">创建题目</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="content">
      <div v-for="(item,index) in list" :key="item.id || Date.now">
        <h3>{{index+1}}、{{item.type}}</h3>
        <div class="content-block">
          <p>{{item.title}}</p>
          <el-radio-group v-model="item.model" style="flex-direction: column;">
            <el-radio :label="option.id" v-for="option in item.options" :key="option.id || Date.now">{{option.name}}</el-radio>
          </el-radio-group>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    localList: {
      type: Array,
      default: () => {}
    }
  },
  data () {
    return {
      form: {
        type: '',
        title: '',
      },
      topicList : [],
      list: []
    }
  },
  watch: {
    localList: {
      deep: true,
      immediate: true,
      handler() {
        this.list = this.localList
      }
    }
  },
  methods: {
    search() {
      if(!this.form.title.trim() && !this.form.type.trim()) { 
        this.list = this.localList;
        return
      }
      let temp = '';
      if(this.form.type) {
        temp = this.list.filter(item => item.type.includes(this.form.type));
      } else if(this.form.title) {
        temp = this.list.filter(item => item.title.includes(this.form.title));
      }
      if(temp) {
        this.list = temp;
      }
    },
    create () {}
  }
}
</script>

<style lang="less" scoped>
.top-filter{
  display: flex;
  justify-content: center;
}
.input-con{
  display: flex;
  width: 180x;
}
.content {
  &-block {
    margin-left: 20px;
    /deep/ .el-radio-group {
      margin-left: 10px;
    }
  }
}
.el-radio:last-child {
  margin-right: 32px!important;
}
/deep/ .el-radio {
  display: block;
  margin-bottom: 10px;
}
</style>
