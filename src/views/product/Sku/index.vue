<template>
  <div>
    <!-- 表格 -->
    <el-table border stripe style="width: 100%" :data="records">
      <el-table-column type="index" label="序号" width="60px" align="center">
      </el-table-column>
      <el-table-column prop="skuName" label="名称" width="width" align="center">
      </el-table-column>
      <el-table-column prop="skuDesc" label="描述" width="width" align="center">
      </el-table-column>
      <el-table-column label="默认图片" width="110px">
        <template slot-scope="{ row }">
          <img
            :src="row.skuDefaultImg"
            alt=""
            style="width: 80px; height: 80px"
          />
        </template>
      </el-table-column>
      <el-table-column
        prop="weight"
        label="重量(kg)"
        width="100px"
        align="center"
      >
      </el-table-column>
      <el-table-column
        prop="price"
        label="价格(元)"
        width="100px"
        align="center"
      >
      </el-table-column>
      <el-table-column prop="prop" label="操作" width="width" align="center">
        <template slot-scope="{ row }">
          <el-button
            type="success"
            icon="el-icon-top"
            size="mini"
            v-if="row.isSale == 0"
            @click="sale(row)"
          ></el-button>
          <el-button
            type="success"
            icon="el-icon-bottom"
            size="mini"
            v-else
            @click="cancel(row)"
          ></el-button>
          <el-button
            type="primary"
            icon="el-icon-edit"
            size="mini"
            @click="edit()"
          ></el-button>
          <el-button
            type="info"
            icon="el-icon-info"
            size="mini"
            @click="getSkuInfo(row)"
          ></el-button>
          <el-button
            type="danger"
            icon="el-icon-delete"
            size="mini"
          ></el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页器 -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="getSkuList"
      style="text-align: center; margin: top 15px"
      :current-page="page"
      :page-sizes="[3, 5, 7]"
      :page-size="limit"
      layout="prev, pager, next, jumper,->,sizes,total"
      :total="total"
    >
    </el-pagination>
    <!-- 抽屉 -->
    <el-drawer :visible.sync="drawer" :show-close="false" size="50%">
      <el-row>
        <el-col :span="5">名称:</el-col>
        <el-col :span="16"> {{ skuInfo.skuName }}</el-col>
      </el-row>
      <el-row>
        <el-col :span="5">描述:</el-col>
        <el-col :span="16"> {{ skuInfo.skuDesc }}</el-col>
      </el-row>
      <el-row>
        <el-col :span="5">价格:</el-col>
        <el-col :span="16"> {{ skuInfo.price }}元</el-col>
      </el-row>
      <el-row>
        <el-col :span="5">平台属性</el-col>
        <el-col :span="16">
          <template>
            <el-tag
              type="success"
              v-for="attr in skuInfo.skuAttrValueList"
              :key="attr.id"
              style="margin-right: 10px"
              >{{ attr.attrId }}-{{ attr.valueId }}</el-tag
            >
          </template>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="5">商品图片</el-col>
        <el-col :span="16">
          <el-carousel height="450px">
            <el-carousel-item
              v-for="item in skuInfo.skuImageList"
              :key="item.id"
            >
              <img :src="item.imgUrl" />
            </el-carousel-item>
          </el-carousel>
        </el-col>
      </el-row>
    </el-drawer>
  </div>
</template>

<script>
export default {
  name: "Sku",
  data() {
    return {
      page: 1,
      limit: 5,
      total: 20,
      records: [], //存储sku列表的数据
      skuInfo: {},
      //抽屉的显示与隐藏
      drawer: false,
    };
  },
  mounted() {
    //获取sku列表的方法
    this.getSkuList();
  },
  methods: {
    //获取sku列表数据的方法
    async getSkuList(pages = 1) {
      this.page = pages;
      //解构出默认参数
      const { page, limit } = this;
      let result = await this.$API.sku.reqSkuList1(page, limit);
      if (result.code == 200) {
        //存储sku列表的数据
        this.total = result.data.total;
        this.records = result.data.records;
      }
    },
    //分页器改变显示数量的方法
    handleSizeChange(limit) {
      //修改参数
      this.limit = limit;
      //发请求
      this.getSkuList();
    },
    //上架按钮
    async sale(row) {
      let result = await this.$API.sku.reqSale(row.id);
      if (result.code == 200) {
        this.$message({
          type: "success",
          message: "上架成功！！",
          center: true,
        });
        row.isSale = 1;
        this.getSkuList();
      }
    },
    //下架按钮
    async cancel(row) {
      let result = await this.$API.sku.reqCancel(row.id);
      if (result.code == 200) {
        this.$message({
          type: "success",
          message: "下架成功！！",
          center: true,
        });
        row.isSale = 0;
        this.getSkuList();
      }
    },
    //编辑按钮
    edit() {
      this.$message({ message: "正在开发中！！", center: true });
    },
    //获取Sku详情的方法
    async getSkuInfo(sku) {
      //展示数据
      this.drawer = true;
      //发请求获取 数据
      let result = await this.$API.sku.reqSkuById(sku.id);
      if (result.code == 200) {
        this.skuInfo = result.data;
      }
    },
  },
};
</script>
<style>
.el-carousel__item h3 {
  color: #475669;
  font-size: 14px;
  opacity: 0.75;
  line-height: 150px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
</style>
<style scoped>
.el-row .el-col-5 {
  font-size: 18px;
  text-align: right;
}
.el-col {
  margin: 10px 10px;
}

.el-carousel__button {
  width: 10px;
  height: 10px;
  background: red;
  border-radius: 50%;
}
</style>