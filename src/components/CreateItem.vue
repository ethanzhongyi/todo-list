<template>
  <div class="warpper">
    <div class="warpper-create">
      <el-form :model="form" label-width="120px" style="width: 900px">
        <el-form-item label="题目ID号">
          <el-input v-model="form.id" placeholder="请输入题目ID号"/>
        </el-form-item>
        <el-form-item label="题目类型">
          <el-input v-model="form.type" placeholder="请输入题目类型"/>
        </el-form-item>
        <el-form-item label="题目描述">
          <el-input v-model="form.title" placeholder="请输入题目描述" type="textarea" :rows="2"/>
        </el-form-item>
        <el-form-item label="选型">
          <ul>
            <li v-for="(item,index) in form.options" :key="item.id">
              <el-button >分值</el-button>
              <el-input v-model="item.score" class="margin-10" placeholder="请输入分值"/>
              <el-input v-model="item.name" class="margin-10" placeholder="请输入选项内容"/>
              <el-button type="danger" @click="del(index)" class="margin-10">删除</el-button>
            </li>
          </ul>
          <el-button type="success" @click="add" class="add">添加选项</el-button>
        </el-form-item>
        <el-form-item class="btn">
          <el-button type="primary" @click="save">保存</el-button>
          <el-button @click="cancel">取消</el-button>
        </el-form-item>
      </el-form>
    </div>

    <el-divider>以下是创建的题目</el-divider>
    <div class="warpper-list">
      <item-list :localList="localList"/>
    </div>
  </div>
</template>

<script>
import ItemList from './ItemList.vue';

export default {
  components: { ItemList },
  data () {
    return {
      form: {
        id: '',
        type: '',
        title:'',
        options:[
          {
            id: Date.now(),
            score:'',
            name: '',
          }
        ],
      },
      localList: []
    }
  },
  created() {
    this.getList()
  },
  methods: {
    del(index) {
      this.form.options.splice(index,1);
    },
    add() {
      this.form.options.push({
        id: Date.now(),
        score:'',
        name: '',
      })
    },
    getList() {
      let topicList = localStorage.getItem('topicList');
      if(topicList) {
        topicList = JSON.parse(topicList)
        this.localList = topicList
      } else {
        topicList = [];
      } 
    },
    save() {
      if(this.form.id) {
        this.localList.push(this.form)
        localStorage.setItem('topicList',JSON.stringify(this.localList))
      }
      this.form = {
        id: '',
        type: '',
        title:'',
        options:[
          {
            id: Date.now(),
            score:'',
            name: '',
          }
        ],
      }
    },
    cancel() {
    }
  }
}

</script>

<style lang="less" scoped>
.warpper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding:  30px 20px;
}
.warpper-list {
  margin: 20px 0;
}
.btn {
  display: flex;
  justify-content: center;
}
 ul,li{
  margin:0;
  padding:0;
  list-style: none;
  width:100%;
 }
 li{
  margin-top: 6px;
  display: flex;
 }
 .margin-10{
  margin-left:10px;
  align-items: center;
 }
 .add{
  width:100%;
  margin-top:10px;
 }
 /deep/ .el-form-item__label {
  font-weight: 800;
 }
</style>
