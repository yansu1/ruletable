
<template>

       		<el-form :model="formData"  ref="ruleTable" label-width="0px" class="ruletable" >
					<!-- 站点table -->
					<el-table
						:data="formData[target]"
						border
						ref="stationtable"
                        @current-change="currentChange"
                        @selection-change="handleSelectionChange"
						class="stationsTable"
					>
                        <!-- 是否有多选框 -->
                        <el-table-column
                            v-if="tableBox"
                            type="selection"
                            width="55"
                        >
						</el-table-column>
                        <!-- 单元格填充 -->
						<el-table-column
                            v-for="(item,index) in tableHeader"
                            :key="index"
							:label="item.label"
							:width="item.width"
                            :formatter="item.formatter"
							:prop="item.prop">
								<template slot-scope="scope">
                                    <el-form-item :prop="`${target}.${scope.$index}.${item.prop}`" :rules="item.rule">
                                            <slot name="column" :item="scope.row" :data="scope.row[item.prop]" :index="index" :rowindex="scope.$index">{{scope.row[item.prop]}}</slot>
                                    </el-form-item>
								</template>	
						</el-table-column>
					</el-table>
        </el-form>
</template>

<script>
import { mapGetters } from "vuex";
export default{
  data() {
    return {
      map: null,
      mPoint: null
    }
  },
  props:{
    formData:{
        type:Object,
        default:function(){
            return {}
        }
    },
    target:{
        type:String,
        default:''
    },
    tableHeader:{
        type:Array,
        default:function(){
            return [];
        }
    },
    tableBox:{
        type:Boolean,
        default:false
    }

  },
  methods:{
    currentChange(item) {
        this.$emit('current-change', item)
    },
    handleSelectionChange(item) {
        this.$emit('handle-selectionChange', item)
    },
    //校验table
    validateTable(){
        return  this.$refs.ruleTable.validate();
    },
    //重置table
    resetTable(){
        this.$refs.ruleTable.resetFields();
    }
  }
}
</script>

<style lang="scss" >
    .ruletable{
        .el-table td {
            padding: 6px 0 0 0;
        }
        .el-form-item {
            margin-bottom: 6px;
        }
        .el-form-item__error {
            color: #f56c6c;
            font-size: 8px;
            line-height: 1;
            padding-top: 0px;
            position: absolute;
            top: 88%;
            left: 0;
        }
    }
</style>
