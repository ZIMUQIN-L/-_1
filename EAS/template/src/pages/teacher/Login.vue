<template>
  <q-layout>
    <q-page-container>
      <q-page class="flex bg-image flex-center">
        <q-btn
              class="q-mt-md q-ml-md absolute-top-left"
              size="15px"
              round
              push
              glossy
              color="blue-8"
              icon="arrow_back"
              @click="back"
            ></q-btn>
        <q-card v-bind:style="$q.screen.lt.sm?{'width': '80%'}:{'width':'30%'}">
          <q-card-section>
            <q-avatar size="103px" class="absolute-center shadow-10">
              <img src="../../statics/logo_big.png">
            </q-avatar>
          </q-card-section>
          <q-card-section>
            <div class="text-center q-pt-lg">
              <div class="col text-h6 ellipsis">
                教务管理系统
              </div>
            </div>
          </q-card-section>
          <q-card-section>
            <q-form
              class="q-gutter-md"
            >
              <q-input 
                v-model="teacherId" 
                filled :type="number" 
                hint="请输入教工号"
                bottom-slots label="教工号"
                error-message="请输入 8 位数字"
                :error="!isIdValid">
                <template v-slot:before>
                  <q-icon name="person"></q-icon>
                </template>
              </q-input>

              <q-input 
                v-model="teacherPwd" 
                filled :type="isPwd ? 'password' : 'text'" 
                pattern="[a-zA-Z0-9_]{6,10}"
                hint="请输入密码"
                bottom-slots label="密码">
                <template v-slot:before>
                  <q-icon name="lock"></q-icon>
                </template>
                <template v-slot:append>
                  <q-icon
                    :name="isPwd ? 'visibility_off' : 'visibility'"
                    class="cursor-pointer"
                    @click="isPwd = !isPwd"
                  ></q-icon>
                </template>
              </q-input>

              <div>
                <q-btn 
                  class="full-width" 
                  label="登录" 
                  @click="onClick"
                  type="submit" 
                  color="primary"/>
              </div>
              <q-card-section class="text-center q-pa-none">
            <p class="text-grey-6">没有账号？<a href="http://localhost:8080/?#/teacher/signup"> 注册 </a> </p>
            
          </q-card-section>
            </q-form>
          </q-card-section>
        </q-card>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      teacherId: '',
      teacherPwd: '',
      isPwd: true,
      isIdValid: true,
    }
  },

  methods :{
    onClick:function() {
      // this.$router.push({
      //       path: '/teacher/lesson/' + this.teacherId,
      //     });
      axios({
        method: 'GET',
        url: 'http://localhost:8000/teacher/login/',
        params: {
            "teacherId": this.teacherId,
            "teacherPwd": this.teacherPwd
        }
      }).then(function (response) {
          // handle success
          // this.$router.push('/teacher/lesson/');
          this.$router.push({
            path: '/teacher/lesson/' + this.teacherId,
          })
          console.log(response);
        })
        .catch(function (error) {
          // handle error
          this.$q.notify({
            type: 'negative',
            message: '请输入正确的用户名 / 密码'
          })
          console.log(error);
        })
        .then(function () {
          // always executed
        });
    },
    back:function() {
      this.$router.push('/')
    }
  }
}
</script>

<style>
  .bg-image {
   background-image: url("../../statics/blue-trianglify.jpg");
   background-repeat: no-repeat; /* Do not repeat the image */
   background-size: cover; /* Resize the background image to cover the entire container */
  }
</style>