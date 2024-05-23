<script setup>
import { ref } from 'vue';

const videoElement = ref(null);
const mediaStream = ref(null);
const mediaRecorder = ref(null);
const isRecording = ref(false);
const recordedChunks = ref([]);
const recordedVideoUrl = ref(null);


// 开始录制
const startRecording = async () => {
  recordedChunks.value = []; // 清空之前录制的内容
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
    mediaStream.value = stream;
    videoElement.value.srcObject = stream; // 设置视频元素的源为摄像头流
    mediaRecorder.value = new MediaRecorder(stream);

    // 数据可用事件处理
    mediaRecorder.value.ondataavailable = (event) => {
      if (event.data.size > 0) {
        recordedChunks.value.push(event.data);
      }
    };

    // 停止事件处理
    mediaRecorder.value.onstop = () => {
      const blob = new Blob(recordedChunks.value, { type: 'video/webm' });
      recordedVideoUrl.value = URL.createObjectURL(blob);
      saveVideo(blob); // 保存视频
    };

    mediaRecorder.value.start();
    isRecording.value = true;
  } catch (error) {
    console.error('录制失败：', error);
  }
};

// 停止录制函数
const stopRecording = () => {
  if (mediaRecorder.value && mediaRecorder.value.state !== 'inactive') {
    mediaRecorder.value.stop();
    isRecording.value = false;

    const stream = mediaStream.value;
    if (stream) {
      stream.getTracks().forEach(track => track.stop()); // 停止所有轨道
    }
  }
};

// 保存视频函数
const saveVideo = (blob) => {
  const url = window.URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.style.display = 'none';
  a.href = url;
  a.download = `${new Date().toISOString()}.webm`;
  document.body.appendChild(a);
  a.click();
  window.URL.revokeObjectURL(url);
  document.body.removeChild(a);
};
</script>

<template>
  <div class="main">
    <el-card style="min-width: 480px">
      <el-row>
        <el-col class="left" :span="4">
          <section class="group-btn">
            <el-button class="btn" type="primary" :disabled="isRecording" @click="startRecording" plain>开始录制</el-button>
            <el-button class="btn" type="primary" :disabled="!isRecording" @click="stopRecording" plain>结束录制</el-button>
            <video class="video" ref="videoElement" autoplay></video>
          </section>
        </el-col>
        <el-col class="right" :span="20">
          <section>
            <video controls width="100%">
              <source src="../../public/show.mp4" type="video/mp4" />
            </video>
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

.left .group-btn {
  display: flex;
  flex-direction: column;
}

.left .video {
  margin-top: 32px;
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
