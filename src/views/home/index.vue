<template>
  <div>
    用品管理
  </div>

  <!-- <div class="search-container">

  </div> -->

  <div class="table-container">
    <el-table :data="tableData" style="width: 100%" height="250">
      <el-table-column fixed prop="brand" label="品牌" width="150" />
      <el-table-column prop="product" label="产品" width="120" />
      <el-table-column prop="category" label="品类" width="120" />
      <el-table-column prop="city" label="发货地" width="320" />
      <el-table-column prop="stock" label="库存" width="600" />
      <el-table-column prop="price" label="价格" />
      <el-table-column prop="standardPrice" label="对标价" />
      <el-table-column prop="afterSale" label="售后" />
    </el-table>
  </div>
</template>

<script setup lang="ts" name="Home">
import { ref } from 'vue';

const tableData = ref([
  {
    brand: '品牌',
    product: '产品',
    category: '品类',
    city: '发货地',
    stock: '库存',
    price: '价格',
    standardPrice: '对标价',
    afterSale: '售后',
  },
]); 
</script>
