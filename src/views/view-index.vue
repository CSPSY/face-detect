<script setup>
</script>

<template>
  <div class="main">
    <el-card style="min-width: 480px">
      <el-row>
        <el-col class="left" :span="6">
          <section>
            <el-button class="btn" type="primary" plain>打开摄像头</el-button>
            <el-button class="btn" type="primary" plain>关闭摄像头</el-button>
          </section>
        </el-col>
        <el-col class="right" :span="18">
          <section>
            <audio src="../../public/show.mp4"></audio>
          </section>
        </el-col>
      </el-row>
    </el-card>
  </div>
</template>

<style scoped>
.main {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.left {
  display: flex;
  flex-direction: column;
}

.right {
  padding: 12px;
}

.btn {
  width: 100px;
  margin: 0;
}

.btn:first-child {
  margin-bottom: 12px;
}
</style>
