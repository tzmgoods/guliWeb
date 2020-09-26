/* eslint-disable no-undef */
/* eslint-disable no-tabs */
/* eslint-disable no-tabs */
/* eslint-disable no-tabs */
/* eslint-disable no-tabs */
<template>
  <div>
   <el-tree :data="menus" 
    :props="defaultProps"
    :expand-on-click-node="false" 
	  show-checkbox 
	  node-key="catId" 
	  :default-expanded-keys = "expandedKey">
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
          <el-button v-if="node.level <=2" type="text"  size="mini" @click="() => append(data)"> Append </el-button>
          <el-button v-if="node.childNodes==0" type="text" size="mini" @click="() => remove(node, data)"> Delete</el-button>
        </span>
      </span>
   </el-tree>
	 
   <el-dialog title="提示" :visible.sync="dialogVisible" width="30%">
		  <el-form :model="category">
    		<el-form-item label="分类名称" >
      		<el-input v-model="form.name" autocomplete="off"></el-input>
    		</el-form-item>
    		<el-form-item label="活动区域">
      		<el-select v-model="form.region" placeholder="请选择活动区域">
						<el-option label="区域一" value="shanghai"></el-option>
						<el-option label="区域二" value="beijing"></el-option>
					</el-select>
				</el-form-item>
  		</el-form>
			<span slot="footer" class="dialog-footer">
				<el-button @click="dialogVisible = false">取 消</el-button>
				<el-button type="primary" @click="addCategory = false">确 定</el-button>
			</span>
   </el-dialog>
  </div>
</template>

<script>
// 这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
// 例如：import 《组件名称》 from '《组件路径》';

export default {
  // import引入的组件需要注入到对象中才能使用
  components: {name: ''},
  props: {},
  data () {
    return {
      category: {name: '', parentCid: 0, catLevel: 0, showStatus: 1, sort: 0},
      dialogVisible: false,
      menus: [],
      expandedKey: [],
      defaultProps: {
        children: 'children',
        label: 'name'
      }
    }
  },

  // 计算属性 类似于data概念
  computed: {},
  // 监控data中的数据变化
  watch: {},
  // 方法集合
  methods: {
    getMenus () {
      this.$http({
        url: this.$http.adornUrl('/product/category/list/tree'),
        method: 'get'
      }).then(({data}) => {
        console.log('成功获取分类数据...', data.data)
        this.menus = data.data
      })
    },

    // 添加三级分类菜单
    addCategory () {
      // eslint-disable-next-line no-undef
      console.log('获取表单数据', data.data)
      // eslint-disable-next-line no-undef
      this.menus = data.data
    },

    append (node, data) {
      console.log('append', node, data)
      this.dialogVisible = true
    },
    // 逻辑删除分类
    remove (node, data) {
      var ids = [data.catId]
      this.$confirm(`是否删除[${data.name}]菜单?`, '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$http({
          url: this.$http.adornUrl('/product/category/delete'),
          method: 'post',
          data: this.$http.adornData(ids, false)
        }).then(({data}) => {
          this.$message({
            message: '菜单删除成功',
            type: 'success'
          })
          // 修改后刷新数据
          this.getMenus()
          // 修改后默认展开的菜单
          this.expandedKey = [node.parent.data.catId]
        })
      }).catch(() => {

      })

      console.log('remove', node, data)
    }

  },

  // 生命周期 - 创建完成（可以访问当前this实例）
  created () {
    this.getMenus()
  },
  // 生命周期 - 挂载完成（可以访问DOM元素）
  mounted () {

  },
  beforeCreate () {}, // 生命周期 - 创建之前
  beforeMount () {}, // 生命周期 - 挂载之前
  beforeUpdate () {}, // 生命周期 - 更新之前
  updated () {}, // 生命周期 - 更新之后
  beforeDestroy () {}, // 生命周期 - 销毁之前
  destroyed () {}, // 生命周期 - 销毁完成
  activated () {} // 如果页面有keep-alive缓存功能，这个函数会触发
}
</script>
<style scoped>


</style>