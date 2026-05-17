<script setup>
import abstract from '../mds/abstract.mdx';
</script>

<template>
  <div>
    <el-divider />

    <!-- teaser image -->
    <el-row justify="center">
      <el-col :xs="24" :sm="22" :md="20" :lg="18" :xl="16">
        <img class="teaser-img" src="/figs/teaser.png" alt="Teaser" />
      </el-col>
    </el-row>

    <!-- abstract -->
    <el-row justify="center">
      <el-col :xs="24" :sm="20" :md="16" :lg="12" :xl="12">
        <abstract />
      </el-col>
    </el-row>

  </div>
</template>

<style scoped>
.teaser-img {
  width: 100%;
  display: block;
  margin: 20px auto 24px auto;
}
</style>