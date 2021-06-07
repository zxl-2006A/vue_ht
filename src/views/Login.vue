<template>
  <div class="ss">
    <el-row class="cl">

      <div class="avatar_box">
        <img src="../assets/logo.png" alt="avatar" />
      </div>

      <el-col :span="24">
        <el-form
          :model="ruleForm"
          status-icon
          ref="ruleForm"
          label-width="100px"
          class="demo-ruleForm"
        >
          <el-form-item label="账号" prop="user">
            <el-input
              type="text"
              v-model="ruleForm.user"
              autocomplete="off"
            ></el-input>
          </el-form-item>

          <el-form-item label="密码" prop="pass">
            <el-input
              type="password"
              v-model="ruleForm.pass"
              autocomplete="off"
            ></el-input>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="submitForm">登录</el-button>
            <el-button @click="resetForm">重置</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ruleForm: {
        user: "",
        pass: "",
        data: "",
      },
    };
  },
  mounted() {},
  methods: {
    submitForm() {
      // console.log(this.ruleForm.user);
      // console.log(this.ruleForm.pass);
      this.$request("/login", "post", {
        username: this.ruleForm.user,
        password: this.ruleForm.pass,
      }).then((res) => {
        // console.log(res.data.meta);
        if (res.data.meta.status == "200") {
          // ui 弹框
          this.$message({
            message: res.data.meta.msg,
            type: "success",
          });
          //保存 token
          this.$store.commit("settoken", res.data.data.token);
          this.$router.push("/home");
        } else {
          // ui 弹框
          this.$message.error(res.data.meta.msg);
        }
      });
    },
    resetForm() {
      this.ruleForm.user = "";
      this.ruleForm.pass = "";
    },
  },
};
</script>

<style lang="scss" scoped>
.ss {
  width: 100%;
  height: 100vh;
  background: #2B4B6B;
  display: flex;
  justify-content: center;
  align-items: center;
}
.cl {
  text-align:center;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 400px;
  background: #fff;
  padding-right:45px;
  padding-top: 75px;
}
 .avatar_box {
    width: 130px;
    height: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 5px #ddd;
    position: absolute;
    top: -10px;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
</style>
