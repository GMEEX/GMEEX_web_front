<template>
  
  <div class="login_form">
	<div class="haeder_logo">
	  	<router-link to="/">
	  	  <img src="../../assets/images/logo2.png" />
	  	</router-link>
	</div>
    <div class="login_right">
      <Form ref="formInline" :model="formInline" :rules="ruleInline" inline>
        <div class="login_title">{{$t('uc.login.login')}}</div>
		<div class='to_register'>
		  <span>{{$t('uc.login.noaccount')}}</span>
		</div>
		<label for="login" class="">{{$t('uc.login.userlable')}}</label>
        <FormItem prop="user">
          <Input name="user" type="text" v-model="formInline.user" :placeholder="$t('uc.login.usertip')" class="user">
          </Input>
        </FormItem>
		<label for="login" class="">{{$t('uc.login.pwdlable')}}</label>
        <FormItem prop="password" class="password">
          <Input type="password" v-model="formInline.password" :placeholder="$t('uc.login.pwdtip')" @on-keyup="onKeyup">
          </Input>
        </FormItem>
        <p id="notice" class="hide">{{$t('uc.login.validatemsg')}}</p>
        <p style="height:60px; padding-top: 20px;">
          <router-link to="/findPwd" style="color:#03CA9B;font-size:12px;text-align: left;">
            {{$t('uc.login.forget')}}
          </router-link>
        </p>
        <FormItem style="margin-bottom:15px;">
          <Button class="login_btn">{{$t('uc.login.login')}}</Button>
        </FormItem>
        
      </Form>

    </div>
	<div class='to_register'>
	  {{$t('uc.login.goregisterlable')}}<router-link to="/register">{{$t('uc.login.goregister')}}</router-link>
	</div>
  </div>
</template>
<style scoped lang="scss">
/* 验证码 */
.login_form {
  background: #273640r;
  position: relative;
  overflow: hidden;
  margin:0px 5px;
  .haeder_logo{
    width: 100%;
	text-align: center;
    height: 48px;
	margin: 30px auto;
  }
  .login_right {
    padding: 40px 40px 15px 40px;
    background: #172d3e;
    max-width: 525px;
	margin: 0px auto;
    border-radius: 3px;
	label{
		color: #82BAF6;
		height: 30px;
		overflow: hidden;
		display: block;
		text-align: left;
	}
    form.ivu-form.ivu-form-label-right.ivu-form-inline {
      .login_title{
        height: 50px;
        color: #fff;
      }
      .ivu-form-item {
        .ivu-form-item-content {
          .login_btn.ivu-btn {
			font-size: 14px;
            color: #01A781;
            background: transparent;
            border: 2px solid #01A781;
            box-sizing: content-box;
            font-weight: 500;
            border-radius: 2px;
			float: right;
            &:focus {
              -moz-box-shadow: 0px 0px 0px #fff, 0px 0px 0px #fff;
              -webkit-box-shadow: 0px 0px 0px #fff, 0px 0px 0px #fff;
              box-shadow: 0px 0px 0px #fff, 0px 0px 0px #fff;
            }
          }
        }
      }
    }
  }
  
  .to_register {
    overflow: hidden;
    font-size: 12px;
	width: 100%;
	text-align: center;
	padding: 10px 0px;
	margin-bottom: 20px;
    a {
      text-align: center;
      color: #03CA9B;
    }
  }
}
#captcha {
  width: 100%;
  display: inline-block;
}
.show {
  display: block;
}
.hide {
  display: none;
}
#notice {
  color: red;
}
#wait {
  text-align: left;
  color: #666;
  margin: 0;
}
.geetest_wait_dot geetest_dot_1 {
  color: red;
}
.user .ivu-btn,
.ivu-btn:active,
.ivu-btn:focus {
  border-color: #d7dde4;
  box-shadow: none;
}
/*  */
</style>
<script>
import gtInit from "../../assets/js/gt.js";
import $ from "jquery";
export default {
  data() {
    return {
      country: "+86",
      captchaObj: null,
      _captchaResult: null,
      formInline: {
        user: "",
        password: ""
      },
      ruleInline: {
        user: [
          {
            required: true,
            message: this.$t("uc.login.loginvalidate"),
            trigger: "blur"
          }
        ],
        password: [
          {
            required: true,
            message: this.$t("uc.login.pwdvalidate1"),
            trigger: "blur"
          },
          {
            type: "string",
            min: 6,
            message: this.$t("uc.login.pwdvalidate2"),
            trigger: "blur"
          }
        ]
      }
    };
  },
  created: function() {
    this.init();
  },
  computed: {
    isLogin: function() {
      return this.$store.getters.isLogin;
    }
  },
  methods: {
    init() {
      this.$store.commit("navigate", "nav-other");
      this.$store.state.HeaderActiveName = "0";

      if (this.isLogin) {
        this.$router.push("/uc/safe");
      } else {
        this.initGtCaptcha();
      }
    },
    onKeyup(ev) {
      if (ev.keyCode == 13) {
        $(".login_btn").click();
      }
    },
    initGtCaptcha() {
      var that = this;
      this.$http.get(this.host + this.api.uc.captcha).then(function(res) {
        window.initGeetest(
          {
            // 以下配置参数来自服务端 SDK
            gt: res.body.gt,
            challenge: res.body.challenge,
            offline: !res.body.success, //表示用户后台检测极验服务器是否宕机
            new_captcha: res.body.new_captcha, //用于宕机时表示是新验证码的宕机
            product: "bind",
            width: "100%"
          },
          this.handler
        );
      });
    },
    handler(captchaObj) {
      captchaObj.onReady(() => {
          $("#wait").hide();
        }).onSuccess(() => {
          let result = (this._captchaResult = captchaObj.getValidate());
          if (!result) {
            this.$Message.error("Please complete identity verification.");
          } else {
            this.handleSubmit("formInline");
          }
        });
      $(".login_btn").click(() => {
        let reg = /^[1][3,4,5,6,7,8,9][0-9]{9}$/,
          tel = this.formInline.user,
          flagtel = reg.test(tel),
          flagpass = this.formInline.password.length >= 6 ? true : false;
        flagtel && flagpass; // && captchaObj.verify();
        (!flagtel || !flagpass) && this.$Message.error("Please fill in the complete information.");
        this.handleSubmit("formInline"); // 屏蔽了验证
      });
    },
    logout() {
      this.$http.post(this.host + "/uc/logout", {}).then(response => {
        var resp = response.body;
        if (resp.code == 0) {
          localStorage.setItem("MEMBER", JSON.stringify(null));
          localStorage.setItem("TOKEN", null);
          localStorage.removeItem("USERKEY", null);
        } else {
          // this.$Message.error(resp.message);
        }
      });
    },
    handleSubmit(name) {
      // 不带验证
      this.$refs[name].validate(valid => {
        if (valid) {
          var params = {};
          params["username"] = this.formInline.user;
          params["password"] = this.formInline.password;
          this.$http.post(this.host + this.api.uc.login, params).then(response => {
              var resp = response.body;
              if (resp.code == 0) {
                this.$Message.success(this.$t("uc.login.success"));
                this.$store.commit("setMember", response.body.data);
                if (this.$route.query.key != null && this.$route.query.key != "") {
                  localStorage.setItem("USERKEY", this.$route.query.key);
                }
                this.$router.push("/uc/safe");
              } else {
                this.$Message.error(resp.message);
              }
            });
        } else {

        }
      });
      /* 带验证
      var result = this._captchaResult;
      if (!result) {
        $("#notice").show();
        setTimeout(function() {
          $("#notice").hide();
        }, 2000);
      } else {
        this.$refs[name].validate(valid => {
          if (valid) {
            var params = {};
            params["username"] = this.formInline.user;
            params["password"] = this.formInline.password;
            this.$http.post(this.host + this.api.uc.login, params).then(response => {
                var resp = response.body;
                if (resp.code == 0) {
                  this.$Message.success(this.$t("uc.login.success"));
                  this.$store.commit("setMember", response.body.data);
                  if (this.$route.query.key != null && this.$route.query.key != "") {
                    localStorage.setItem("USERKEY", this.$route.query.key);
                  }
                  this.$router.push("/uc/safe");
                } else {
                  this.$Message.error(resp.message);
                }
              });
          } else {

          }
        });
      }
      */
    }
  }
};
</script>
<style lang="scss">
.login_form {
  .login_right {
    form.ivu-form.ivu-form-label-right.ivu-form-inline {
      .ivu-form-item {
        .ivu-form-item-content {
          .ivu-input-wrapper.ivu-input-type {
            .ivu-input {
              background-color:#132532;
              font-size: 14px;
			  height: 40px;
              border: none;
              border-bottom: 1px solid #27313e;
              border-radius:0;
              // color:#fff;
              &:focus {
                border: none;
                border-bottom: 1px solid #27313e;
                -moz-box-shadow: 2px 2px 5px transparent, -2px -2px 4px transparent;
                -webkit-box-shadow: 2px 2px 5px transparent, -2px -2px 4px transparent;
                box-shadow: 2px 2px 5px transparent, -2px -2px 4px transparent;
              }
            }
          }
        }
      }
    }
  }
}

.ivu-select-single .ivu-select-selection .ivu-select-placeholder, .ivu-select-single .ivu-select-selection .ivu-select-selected-value{
  padding-right: 10px;
  padding-left: 3px;
}
.ivu-select-single .ivu-select-selection .ivu-select-arrow{
  right: 2px;
}

.ivu-form-item-error .ivu-input-group-append, .ivu-form-item-error .ivu-input-group-prepend{
  background-color: #17212e;
  border-color: transparent;
}
.ivu-form-item-error .ivu-select-arrow{
  color: #808695;
}

.login_right .ivu-select-dropdown{
  background: #1c2a32;
}
</style>
<style>
  .ivu-select-single .ivu-select-selection .ivu-select-placeholder, .ivu-select-single .ivu-select-selection .ivu-select-selected-value{
    padding-right: 20px;
  }
  .ivu-select-arrow{
    right: 4px;
  }

  .ivu-select-item span:first-child{
    display: inline-block;
    width: 30px;
    text-align: left;
  }
</style>
