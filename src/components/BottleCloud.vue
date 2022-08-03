<template>
  <div class="body">
    <el-upload drag :action="action" multiple name="multipartFile" :on-success="success">
      <el-icon class="el-icon--upload">
        <upload-filled/>
      </el-icon>
      <div class="el-upload__text">
        拖拽文件或 <em>点击上传</em>
      </div>
    </el-upload>

    <el-card class="box-card" v-for="doc in docs" :key="doc">
      <template #header>
        <div class="card-header">
          <span style="cursor: pointer">{{ doc.name }}</span>
          <span style="position: absolute;right: 0">
                <el-button :icon="Delete" circle @click="deleteFile(doc.uuid)"/>
                <el-button :icon="Download" circle @click="download(doc.uuid)"/>
              </span>
        </div>
      </template>
      <div>{{ doc.time }}</div>
    </el-card>

  </div>
</template>

<script>
import {UploadFilled, Download, Delete} from "@element-plus/icons-vue";
import axios from "axios";
import {onMounted, ref} from "vue";
import {ElMessage} from "element-plus";

export default {
  name: "File",
  components: {UploadFilled},
  setup() {
    const docs = ref([])
    const action = ref("/api/doc/upload.do")
    const success = (res) => {
      if (res.code === '0') {
        getDocs()
        ElMessage.success("上传成功！")
      } else {
        ElMessage.error('上传失败！')
      }
    }
    const getDocs = () => {
      axios.get('/api/doc/list.do?', {
        params: {
          userId: '1'
        }
      }).then(res => {
        docs.value = res.data.data;
      })
    }
    onMounted(() => {
      getDocs()
    })
    const download = (uuid) => {
      const a = document.createElement("a"); // 生成一个a元素
      const event = new MouseEvent("click"); // 创建一个单击事件
      a.href = '/api/doc/download.do?uuid=' + uuid; // 将生成的URL设置为a.href属性
      a.dispatchEvent(event); // 触发a的单击事件
    }
    const deleteFile = (uuid) => {
      axios.get('/api/doc/delete.do?', {
        params: {
          uuid: uuid
        }
      }).then(res => {
        if (res.data.code === '0') {
          getDocs()
          ElMessage.success("删除成功！")
        } else {
          ElMessage.error(res.data.msg)
        }
      })
    }
    return {
      docs,
      Download,
      download,
      getDocs,
      Delete,
      deleteFile,
      action,
      success,
    }
  }
}
</script>

<style scoped>
.body {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #242333;
  min-height: 100vh;
}

:deep(.el-upload) {
  width: 500px;
}

:deep(.el-upload-dragger) {
  border: 0;
  background-color: #fff;
  box-shadow: 0 3px 10px rgba(255, 255, 255, 0.7);

}

:deep(.el-card) {
  font-size: 110%;
  font-weight: bold;
  color: white;
  width: 480px;
  background-color: rgb(186, 85, 211);
  border: 0;
  box-shadow: 0 3px 10px rgba(255, 255, 255, 0.7);
  margin-bottom: 5px;
}

:deep(.el-card.is-always-shadow) {
  box-shadow: 0 3px 10px rgba(186, 85, 211, 0.7);
}

:deep(.el-card__header) {
  border-bottom: 1px gold dashed;
}

/*.box-card {*/
/*  color: white;*/
/*  width: 480px;*/
/*  background-color: rgb(186,85,211);*/
/*  border: 0;*/
/*  box-shadow: 0 3px 10px rgba(255, 255, 255, 0.7);*/
/*  !*box-shadow: 0 30px 100px rgba(186,85,211, 0.7);*!*/
/*}*/

.card-header {
  position: relative;
  border: 0;
}
</style>