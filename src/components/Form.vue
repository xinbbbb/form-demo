<template>
  <div class="main">
    <el-form
      v-loading="loading"
      ref="ruleForm"
      element-loading-text="Form uploading..."
      :inline="true"
      :rules="rules"
      label-position="right"
      label-width="120px"
      style="max-width: 780px"
      :model="ruleForm" 
      class="demo-form-inline"
      >
      <div>
        <el-form-item label="First Name" prop="firstName">
          <el-input v-model="ruleForm.firstName" placeholder="First Name" />
        </el-form-item>
        
        <el-form-item el-form-item label="Last Name" prop="lastName">
          <el-input v-model="ruleForm.lastName" placeholder="Last Name" />
        </el-form-item>
      </div>

      <el-form-item label="Description" prop="desc">
        <el-input v-model="ruleForm.desc" type="textarea" rows="4"/>
      </el-form-item>

      <el-form-item label="Email" prop="email">
        <el-input v-model="ruleForm.email"/>
      </el-form-item>

      <el-upload action="#" list-type="picture-card" :auto-upload="false">
        <el-icon><Plus /></el-icon>

        <template #file="{ file }">
          <div>
            <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" />
            <span class="el-upload-list__item-actions">
              <span
                class="el-upload-list__item-preview"
                @click="handlePictureCardPreview(file)"
              >
                <el-icon><zoom-in /></el-icon>
              </span>
              <span
                v-if="!disabled"
                class="el-upload-list__item-delete"
                @click="handleDownload(file)"
              >
                <el-icon><Download /></el-icon>
              </span>
              <span
                v-if="!disabled"
                class="el-upload-list__item-delete"
                @click="handleRemove(file)"
              >
                <el-icon><Delete /></el-icon>
              </span>
            </span>
          </div>
        </template>
      </el-upload>

      <el-dialog v-model="dialogVisible">
        <img w-full :src="dialogImageUrl" alt="Preview Image" />
      </el-dialog>

      <el-button type="primary" style="margin: 12px auto" @click="onSubmit('ruleForm')" :disabled="!IsVaild">Submit</el-button>

    </el-form>
  </div>
</template>

<script >
import { Delete, Download, Plus, ZoomIn } from '@element-plus/icons-vue'

export default {
  name: 'Form',
  props: {
    msg: String
  },
  components: {
    Delete,
    Download,
    Plus,
    ZoomIn
  },
  data() {
    const ValidateEmail = (mail) => {
      return /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(mail)
    }
    const validatePass = (rule, value, callback)=> {
      if(Object.keys(this.ruleForm).every(key => !!this.ruleForm[key])) {
        if(rule.type === 'email') {
          if(ValidateEmail(value)) {
            this.IsVaild = true
          } else {
            this.IsVaild = false
            callback(new Error('Please input correct email'))
          }
        } 
        callback()
      } else {
        this.IsVaild = false
      }
    }
    return {
      loading: false,
      IsVaild: false,
      ruleForm: {
        firstName: '',
        lastName: '',
        desc: '',
        email:'',
      },
      rules: {
        firstName: [
          { required: true, message: 'Please input firstName', trigger: 'blur' },
          { validator: validatePass, trigger: 'change' }
        ],
        lastName: [
          { required: true, message: 'Please input lastName', trigger: 'blur' },
          { validator: validatePass, trigger: 'change' }
        ],
        desc: [
          { required: true, message: 'Please input Description', trigger: 'blur' },
          { validator: validatePass, trigger: 'change' }
        ],
        email: [
          { required: true, message: 'Please input email', trigger: 'blur' },
          { type: 'email', validator: validatePass, trigger: ['blur', 'change'] }
        ]
      },
      fileList: [],
      dialogImageUrl: '',
      dialogVisible: false,
      disabled: false,
    }
  },
  
  methods: {

    handleRemove: (uploadFile, uploadFiles) => {
      // TODO
      console.log(uploadFile, uploadFiles)
    },
    handlePictureCardPreview: (uploadFile) => {
      // TODO
      console.log(uploadFile)
      this.dialogImageUrl.value = uploadFile.url
      this.dialogVisible.value = true
    },
    onSubmit(formName){
       this.$refs[formName].validate((valid) => {
        if (valid) {
          console.log('submit!')
          this.loading = true
          setTimeout(()=> {
            this.$message({
              message: 'Email is successfully sent!!',
              type: 'success'
            })
            this.loading = false
          }, 3000)
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    }
  }
}
</script>

<style scoped>
.main {
  padding: 20px;
}
.demo-form-inline {
  display: inline-grid;
}
</style>
