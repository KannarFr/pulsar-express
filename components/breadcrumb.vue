<template>
  <div class="breadcrumb-container">
    <el-breadcrumb separator=">" class="breadcrumb">
      <img class="el-breadcrumb-img" :src="img + '.png'" width="32"/>
      <el-breadcrumb-item v-for="item in items" :key="item.path" :to="{ path: item.path }">{{ item.label }}</el-breadcrumb-item>
    </el-breadcrumb>
  </div>
</template>

<script>
export default {
  name: 'breadcrumb',

  props: [ 'items' ],

  computed: {
    img() {
      const path = this.$route.path
      const idxSlash = path.indexOf('/', 1)
      return path.substring(0, idxSlash == -1 ? path.length : idxSlash)
    }
  }
}
</script>

<style scoped>
.el-breadcrumb {
  line-height: 2.2;
}

.el-breadcrumb-img {
  float: left;
  margin-right: 20px;
}

.breadcrumb-container {
  /*border-bottom: 1px solid #eaecef;*/
  /*padding-bottom: 10px;*/
}
</style>
