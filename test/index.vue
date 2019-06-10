
<template>
    <div>
			<el-button type="" icon="el-icon-plus" size="small"@click="formData.target.push({name:'l',value:'',namevalue:'ly'})">新增</el-button>
			<el-button type="" icon="el-icon-plus" size="small"@click="checktable">校验</el-button>
			<el-button type="" icon="el-icon-plus" size="small"@click="resettable">重置校验</el-button>
      <ruletable
          style="width:50%;margin:auto"
          :formData="formData" 
          target="target"
          :tableBox="true"
          ref="ruleTabe"
          @handle-selectionChange="selectChange"
          @current-change="currentChange"
          :tableHeader="tableHeader"
        >
          <template slot="column" slot-scope="{item, data,index,rowindex}">
              <el-input v-model="item.name"  v-if="index==0" size="mini"/>
              <el-input v-model="item.value"  v-else-if="index==1" size="mini"/>
              <el-button v-model="item.value" @click="deleteItem(rowindex)" v-else-if="index==tableHeader.length-1" size="mini" >删除</el-button>
              <span v-else>默认值:{{data}}</span>
          </template>
      </ruletable>
    </div>

</template>

<script>
import { mapGetters } from "vuex";
import ruletable from './component/index';
export default{
  components:{
    ruletable
  },
  data() {
    return {
      map: null,
      mPoint: null,
      formData:{target:[{name:'su',value:'yan',namevalue:'sy'},{name:'h',value:'w',namevalue:'hw'}]},
      tableHeader:[
            {label:'姓',prop:'name',rule:[{required: true, message: '必填项'}, {max: 30, message: '输入过长'}]},
            {label:'名',prop:'value',rule:[{required: true, message: '必填项'}, {max: 30, message: '输入过长'}]},
            {label:'姓名',prop:'namevalue'},
            {label:'操作',prop:'value',rule:[{required: true, message: '必填项'}, {max: 30, message: '输入过长'}]},
          ]
    }
  },
  props:{
    getpose:{
      type:Boolean,
      default:false
    }
  },
  computed: {
  },
  mounted() {
  },
  methods:{
    deleteItem(index){
      this.formData.target.splice(index,1)
    },
    selectChange(item){
      console.log(item);
    },
    currentChange(item){
      console.log(item);
    },
    async checktable(){
      this.$refs.ruleTabe.validateTable().then(res=>{
        console.log(res);
      }).catch(res=>{
        console.log(res);
      })
      let res = await this.$refs.ruleTabe.validateTable();
    },
    resettable(){
      this.$refs.ruleTabe.resetTable()
    },
  }
}
</script>

<style scoped>
#allmap {
  height: 100%;
}
</style>
