<script setup>
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <el-col :xs="24" :sm="24" :md="22" :lg="20" :xl="18">

        <!-- local -->
        <el-container class="video-container">
          <video controls preload playsinline>
            <source src="/video/teaser_video.mp4" type="video/mp4">
          </video>
        </el-container>

      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
.video-container {
  margin: 24px 0 0 0;
}

video {
  aspect-ratio: 16 / 9;
  width: 100%;
  display: block;
  border: none;
}
</style>