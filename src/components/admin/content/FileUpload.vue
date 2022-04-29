<template>
    <el-upload
    class="upload-demo"
    ref="upload"
    action="fileapi"
    :auto-upload="false"
    :on-preview="handlePreview"
    :on-remove="handleRemove"
    :before-remove="beforeRemove"
    :on-success="handleSuccess"
    multiple
    :limit="3"
    :on-exceed="handleExceed"
    :file-list="fileList">
    <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
    <el-button style="margin-left: 10px;" type="success" @click="submitUpload">点击上传</el-button>
    <div slot="tip" class="el-upload__tip">上传的文件不超过3m</div>
    </el-upload>
</template>

<script>
  export default {
    name: 'FileUpload',
    data(){
      return {
        fileList: []
      };
    },
    methods: {
    submitUpload(){
        
        //this.$refs.upload.submit();
    },
      handleRemove(file, fileList) {
        console.log(file, fileList);
      },
      handlePreview(file) {
        console.log(file);
      },
      handleExceed( files, fileList) {
        this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
      },
      beforeRemove( file, fileList) {
        return this.$confirm(`确定移除 ${ file.name }？`);
      },
      handleSuccess (response) {
        this.url = response
        // console.log(this.url)
        this.$emit('onUpload')
        this.$message.warning('上传成功')
      },
      clear () {
        this.$refs.upload.clearFiles()
      }
    }
  }
</script>
