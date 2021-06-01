<template>
  <div class="login_form">
    <div class="haeder_logo">
		<router-link to="/">
		  <img src="../../assets/images/logo2.png" />
		</router-link>
    </div>
    <div class="login_right">
      <Form v-if="allowRegister" ref="formInline" :model="formInline" :rules="ruleInline" inline>
		<div class="login_title">{{$t('uc.login.register')}}</div>
        <FormItem style="text-align:center; display: none;">
          <ButtonGroup>
            <Button v-for="(list,index) in buttonLists" :key="list.text" :class="{ active:changeActive == index}" @click="actives(index)">{{list.text}}</Button>
          </ButtonGroup>
        </FormItem>
		<label for="login" class="">{{$t('uc.regist.username')}}</label>
        <FormItem prop="username">
          <Input type="text" v-model="formInline.username" placeholder="">
          </Input>
        </FormItem>
		<label for="login" class="">{{key}}</label>
        <FormItem prop="user">
          <Input type="text" v-model="formInline.user" v-if="changeActive==0" style="">
            <Select v-model="country" slot="prepend" style="width: 65px;border-bottom: 1px solid #27313e;margin: 0px;">
              <Option value="中国" label="+86"><span>+86</span><span style="margin-left:10px;color:#ccc">CN</span></Option>
              <Option value="新加坡" label="+65"><span>+65</span><span style="margin-left:10px;color:#ccc">SG</span></Option>
              <Option value="韩国" label="+82"><span>+82</span><span style="margin-left:10px;color:#ccc">KR</span></Option>
              <Option value="日本" label="+81"><span>+81</span><span style="margin-left:10px;color:#ccc">JP</span></Option>
              <Option value="泰国" label="+66"><span>+66</span><span style="margin-left:10px;color:#ccc">TH</span></Option>
              <Option value="俄罗斯" label="+7"><span>+7</span><span style="margin-left:10px;color:#ccc">RU</span></Option>
              <Option value="英国" label="+44"><span>+44</span><span style="margin-left:10px;color:#ccc">UK</span></Option>
              <Option value="越南" label="+84"><span>+84</span><span style="margin-left:10px;color:#ccc">VN</span></Option>
              <Option value="印度" label="+91"><span>+91</span><span style="margin-left:10px;color:#ccc">IN</span></Option>
              <Option value="意大利" label="+39"><span>+39</span><span style="margin-left:10px;color:#ccc">IT</span></Option>
              <Option value="香港" label="+852"><span>+852</span><span style="margin-left:10px;color:#ccc">HK</span></Option>
              <Option value="马来西亚" label="+60"><span>+60</span><span style="margin-left:10px;color:#ccc">MY</span></Option>
              <Option value="台湾省" label="+886"><span>+886</span><span style="margin-left:10px;color:#ccc">TWN</span></Option>
              <Option value="土耳其" label="+90"><span>+90</span><span style="margin-left:10px;color:#ccc">TU</span></Option>
            </Select>
          </Input>
		  
          <Input type="text" v-model="formInline.user" v-if="changeActive==1">
          </Input>
        </FormItem>
		<label for="login" class="">{{$t('uc.regist.smscode')}}</label>
        <FormItem prop="code">
          <Input type="text" v-model="formInline.code">
          </Input>
          <input id="sendCode" @click="sendCode();" type="Button" shape="circle" :value="sendcodeValue" :disabled='codedisabled'>
          </input>
        </FormItem>
		<label for="login" class="">{{$t('uc.regist.pwd')}}</label>
        <FormItem prop="password"  class="password">
          <Input type="password" v-model="formInline.password">
          </Input>
        </FormItem>
		<label for="login" class="">{{$t('uc.regist.repwd')}}</label>
        <FormItem prop="repassword"  class="password">
          <Input type="password" v-model="formInline.repassword">
          </Input>
        </FormItem>
		<label for="login" class="">{{$t('uc.regist.promotion')}}</label>
        <FormItem prop="promotion">
          <Input type="text" v-model="formInline.promotion">
            <!--<span slot="prepend" style="margin-left: 7px;">{{$t('uc.regist.promotion')}} :</span>-->
          </Input>
        </FormItem>
        <div class="check-agree" style="">
          <label>
            <Checkbox v-model="agree">{{$t('uc.regist.agreement')}}</Checkbox>
          </label>
          <a v-if="lang=='繁體中文'" href="/helpdetail?cate=1&id=5&cateTitle=常见问题" target="_blank" style="">《{{$t('uc.regist.userprotocol')}}》</a>
          <a v-if="lang=='English'" href="/helpdetail?cate=1&id=35&cateTitle=Privacy Policy" target="_blank" style="">《{{$t('uc.regist.userprotocol')}}》</a>
        </div>
        <FormItem>
          <Button class="register_btn" @click="handleSubmit('formInline')" :disabled="registing">{{$t('uc.regist.regist')}}</Button>
        </FormItem>
      </Form>
	  
      <Alert v-else type="warning">
        Coming soon!
        <template slot="desc">
          GMEEX.COM will open register soon
        </template>
      </Alert>
    </div>
	<div class='to_register'>
	  {{$t('uc.login.gologinlable')}}<router-link to="/login">{{$t('uc.login.gologin')}}</router-link>
	</div>
  </div>
</template>
<style scoped lang="scss">
.login_form {
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
    .tel-title{
      color: #fff;
    }
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
          .register_btn.ivu-btn {
            font-size: 14px;
            color: #01A781;
            background: transparent;
            border: 2px solid #01A781;
            box-sizing: content-box;
            font-weight: 500;
            border-radius: 2px;
            float: right;
            &:focus {
              -moz-box-shadow: 2px 2px 5px transparent, -2px -2px 4px transparent;
              -webkit-box-shadow: 2px 2px 5px transparent, -2px -2px 4px transparent;
              box-shadow: 2px 2px 5px transparent, -2px -2px 4px transparent;
            }
          }
          .ivu-input-wrapper.ivu-input-type {
            .ivu-input {
              border: 1px solid red;
            }
          }
          #sendCode {
            position: absolute;
            border: 1px solid #01A781;
            background: transparent;
            outline: none;
			top: 2px;
            right: 0;
            padding: 0px 20px;
            color: #ffffff;
            cursor: pointer;
          }
        }
      }
      .check-agree {
        color: #8c938e;
        display: inline-block;
		width: 100%;
		clear: both;
        line-height: 30px;
        font-size: 12px;
        cursor: default;
		float: left;
		text-align: left;
        a {
          color: #01A781;
          margin-left: -10px;
        }
		label{
			color: #ffffff;
			display: initial;
			text-align: left;
		}
        .ivu-checkbox-wrapper.ivu-checkbox-wrapper-checked {
          .ivu-checkbox.ivu-checkbox-checked {
            .ivu-checkbox-inner {
				
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

.login_title {
  text-align: center;
  height: 80px;
  font-size: 25px;
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
.tel-title {
  font-size: 25px;
}
.login_left {
  display: none;
}

</style>
<script>
//   import gtInit from '../../assets/js/gt.js';
import $ from "jquery";
export default {
  data() {
    const validateUser = (rule, value, callback) => {
      if (this.changeActive == 0) {
        var reg = /^[1][3,4,5,6,7,8,9][0-9]{9}$/;
        if (value == "") {
          callback(new Error(this.$t("uc.regist.teltip")));
        } /*else if (!reg.test(this.formInline.user)) {
          callback(new Error(this.$t("uc.regist.telerr")));
        } */else {
          callback();
        }
      } else {
        var reg = /^[a-z0-9]+([._\\-]*[a-z0-9])*@([a-z0-9]+[-a-z0-9]*[a-z0-9]+.){1,63}[a-z0-9]+$/;
        reg = /^(\w)+(\.\w+)*@(\w)+((\.\w{2,3}){1,3})$/;
        if (value == "") {
          callback(new Error(this.$t("uc.regist.emailtip")));
        } else if (!reg.test(this.formInline.user)) {
          callback(new Error(this.$t("uc.regist.emailerr")));
        } else {
          callback();
        }
      }
    };
    const validateRepassword = (rule, value, callback) => {
      if (value === "") {
        callback(new Error(this.$t("uc.regist.confirmpwdtip")));
      } else if (value !== this.formInline.password) {
        callback(new Error(this.$t("uc.regist.confirmpwderr")));
      } else {
        callback();
      }
    };
    return {
      country: "中国",
      codedisabled:false,
      sendcodeValue: this.$t("uc.regist.sendcode"),
      isRegister: false,
      ticket: "",
      randStr: "",
      registing: false,
      captchaObj: null,
      modal1: false,
      _captchaResult: null,
      agree: true,
      allowRegister: true,
      buttonLists: [
        {
          text: this.$t("uc.regist.telregist")
        },
        {
          text: this.$t("uc.regist.emailregist")
        }
      ],
      areas: [],
      changeActive: 0,
      showCode: true,
      countdown: 60,
      formInline: {
        username: "",
        // country: "",
        user: "",
        code: "",
        areaCode: "",
        password: "",
        repassword: "",
        promotion: ""
      },
      ruleInline: {
        user: [{ validator: validateUser, trigger: "blur" }],
        code: [
          {
            required: true,
            message: this.$t("uc.regist.smscodetip"),
            trigger: "blur"
          }
        ],
        password: [
          {
            required: true,
            message: this.$t("uc.regist.pwdtip"),
            trigger: "blur"
          },
          {
            type: "string",
            min: 6,
            message: this.$t("uc.regist.pwdmsg"),
            trigger: "blur"
          }
        ],
        repassword: [{ validator: validateRepassword, trigger: "blur" }]
      },
      key: "",
      code: ""
    };
  },
  watch: {
    changeActive: function(val) {
      this.$refs["formInline"].resetFields();
      // if (val == 0) this.initGtCaptcha();
    },
    lang: function() {
      this.updateLangData();
    }
  },
  computed: {
    lang: function() {
      return this.$store.state.lang;
    },
    isLogin: function() {
      return this.$store.getters.isLogin;
    }
  },
  created: function() {
    window.scrollTo(0, 0);
    // var oDiv = document.getElementById("example-navbar-collapse");
    // oDiv && (oDiv.className = "navbar-collapse collapse");
    this.init();
    this.actives(this.changeActive);
    if(this.$route.query.code != undefined && this.$route.query.code != "" && this.$route.query.code != null){
        this.formInline.promotion = this.$route.query.code;
    }else{
        this.formInline.promotion = "";
    }
  },
  methods: {
    updateLangData() {
      this.buttonLists = [
        {
          text: this.$t("uc.regist.telregist")
        },
        {
          text: this.$t("uc.regist.emailregist")
        }
      ];

      if (this.changeActive == 0) {
        this.key = this.$t("uc.regist.telno");
      } else {
        this.key = this.$t("uc.regist.email");
      }
    },
    init() {
      this.$store.commit("navigate", "nav-other");
      this.$store.state.HeaderActiveName = "0";
      if (this.isLogin) {
        this.$router.push("/");
      }
      // this.getAreas();
      // this.initGtCaptcha();
    },
    initGtCaptcha() {
      // 直接生成一个验证码对象
      var self = this;
      var captcha1 = new TencentCaptcha("2076680797", function(res) {
        res.ret == 0 &&
          (self.isRegister = true) &&
          (self.ticket = res.ticket) &&
          (self.randStr = res.randstr) &&
          self.success();
      });
      captcha1.show(); // 显示验证码
    },
    onAreaChange(value) {
      for (var i = 0; i < this.areas.length; i++) {
        if (this.areas[i].zhName == value) {
          this.formInline.areaCode = this.areas[i].areaCode;
        }
      }
    },
    getAreas() {
      this.$http.post(this.host + this.api.common.area).then(response => {
        var resp = response.body;
        this.areas = resp.data;
        this.formInline.country = this.areas[0].zhName;
        this.formInline.areaCode = this.areas[0].areaCode;
      });
    },
    actives: function(index) {
      this.changeActive = index;
      if (this.changeActive == 0) {
        this.showCode = true;
        this.key = this.$t("uc.regist.telno");
        this.ruleInline.code = [
          {
            required: true,
            message: this.$t("uc.regist.smscodetip"),
            trigger: "blur"
          }
        ];
      } else {
        this.showCode = false;
        this.key = this.$t("uc.regist.email");
        this.ruleInline.code = [];
      }
    },
    handleSubmit(name) {
      this.$refs[name].validate(valid => {
        if (valid) {
          if (this.agree == true) {
            if (this.changeActive == 1) {
              if (this.isRegister) {
                this.registing = true;
                var params = {};
                params["email"] = this.formInline.user;
                params["username"] = this.formInline.username + this.formInline.user;
                params["password"] = this.formInline.password;
                params["promotion"] = this.formInline.promotion; // 邀请码
                params["country"] = this.country;//"中国";
                params["superPartner"] = "";//this.formInline.superType;
                params["code"] = this.formInline.code;
                params["visitCode"] = this.formInline.visitPassword;

                this.$http
                  .post(this.host + "/uc/register/email", params)
                  .then(response => {
                    this.registing = false;
                    var resp = response.body;
                    if (resp.code == 0) {
                      if (
                        this.formInline.superType == "1" ||
                        this.formInline.superType == "2"
                      ) {
                        this.$Notice.success({
                          title: this.$t("common.tip"),
                          desc: "已注册成功!"
                        });
                        var that = this;
                        setTimeout(() => {
                          that.$router.push("/");
                        }, 3000);
                      } else {
                        this.$Notice.success({
                          title: this.$t("common.tip"),
                          desc: resp.message
                        });
                        var that = this;
                        setTimeout(() => {
                          that.$router.push("/login");
                        }, 3000);
                      }
                    } else {
                      this.$Notice.error({
                        title: this.$t("common.tip"),
                        desc: resp.message
                      });
                    }
                  });
              } else {
                this.$Notice.error({
                  title: this.$t("common.tip"),
                  desc: "opps"
                });
              }
            } else {
              if (this.isRegister) {
                this.registing = true;
                var params = {};
                params["phone"] = this.formInline.user;
                params["username"] = this.formInline.username + this.formInline.user;
                params["password"] = this.formInline.password;
                params["promotion"] = this.formInline.promotion; // 邀请码
                params["code"] = this.formInline.code;
                params["country"] = this.country;//"中国";
                params["superPartner"] = "";//this.formInline.superType;
                params["ticket"] = this.ticket;
                params["randStr"] = this.randStr;

                this.$http
                  .post(this.host + "/uc/register/phone", params)
                  .then(response => {
                    this.registing = false;
                    var resp = response.body;
                    if (resp.code == 0) {
                      if (
                        this.formInline.superType == "1" ||
                        this.formInline.superType == "2"
                      ) {
                        this.$Notice.success({
                          title: this.$t("common.tip"),
                          desc: "已注册成功!"
                        });
                        var that = this;
                        setTimeout(() => {
                          that.$router.push("/");
                        }, 3000);
                      } else {
                        this.$Notice.success({
                          title: this.$t("common.tip"),
                          desc: resp.message
                        });
                        var that = this;
                        setTimeout(() => {
                          that.$router.push("/login");
                        }, 3000);
                      }
                    } else {
                      this.$Notice.error({
                        title: this.$t("common.tip"),
                        desc: resp.message
                      });
                    }
                  });
              } else {
                this.$Notice.error({
                  title: this.$t("common.tip"),
                  desc: "请输入正确的验证码"
                });
              }
            }
          } else {
            this.$Notice.error({
              title: this.$t("common.tip"),
              desc: this.$t("uc.regist.agreementtip")
            });
          }
        }
      });
    },
    settime() {
      this.sendcodeValue = this.$t("uc.regist.resendcode") + this.countdown + ")";
      this.codedisabled = true;
      var _this = this;
      let timercode = setInterval(() => {
        _this.countdown--;
        _this.sendcodeValue = _this.$t("uc.regist.resendcode") + _this.countdown + ")";
        if (this.countdown <= 0) {
          clearInterval(timercode);
          _this.codedisabled = false;
          _this.sendcodeValue = _this.$t("uc.regist.sendcode");
          _this.countdown = 60;
        }
      }, 1000);
    },
    sendCode() {
      if(this.changeActive == 0) {
        var mobilePhone = this.formInline.user;
        let reg = /^[1][3,4,5,6,7,8,9][0-9]{9}$/;
        if (mobilePhone == "" ) {
          this.$Message.error(this.$t("uc.regist.teltip"));
          return;
        } else {
          this.initGtCaptcha();
        }
      }else{
        this.initGtCaptcha();
      }
    },
    success() {
      if(this.changeActive == 0){
        var params = {};
        params["phone"] = this.formInline.user;
        params["country"] = this.country;
        var reg = /^[1][3,4,5,6,7,8,9][0-9]{9}$/;
        this.$http.post(this.host + "/uc/mobile/code", params).then(response => {
              var resp = response.body;
              resp.code == 0 && this.$Notice.success({title: this.$t("common.tip"),desc: resp.message});
              resp.code == 0 && this.settime();
              resp.code != 0 && this.$Notice.error({title: this.$t("common.tip"),desc: resp.message});
            });
        // !reg.test(params["phone"]) &&this.$Notice.error({title: this.$t("common.tip"),desc: this.$t("uc.finance.withdraw.telerr")});
      }else{
        var params = {};
        params["email"] = this.formInline.user;
        this.$http.post(this.host + "/uc/reg/email/code", params).then(response => {
              var resp = response.body;
              resp.code == 0 && this.$Notice.success({title: this.$t("common.tip"),desc: resp.message});
              resp.code == 0 && this.settime();
              resp.code != 0 && this.$Notice.error({title: this.$t("common.tip"),desc: resp.message});
            });
      }
    }
  }
};
</script>
<style lang="scss">
.login_form {
  .login_right {
    form.ivu-form.ivu-form-label-right.ivu-form-inline {
      text-align:center;
      .ivu-form-item {
        .ivu-form-item-content {
          .ivu-input-wrapper.ivu-input-type {
            .ivu-input {
              border: none;
              border-bottom: 1px solid #27313e;
              font-size: 14px;
			  height: 40px;
              background:#0e1b25;
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
      .check-agree {
        .ivu-checkbox-wrapper {
          .ivu-checkbox-input {
            &:focus {
              border: none;
              outline: none;
              -moz-box-shadow: 2px 2px 5px transparent, -2px -2px 4px transparent;
              -webkit-box-shadow: 2px 2px 5px transparent, -2px -2px 4px transparent;
              box-shadow: 2px 2px 5px transparent, -2px -2px 4px transparent;
            }
          }
        }
        .ivu-checkbox-wrapper.ivu-checkbox-wrapper-checked {
          .ivu-checkbox.ivu-checkbox-checked {
            .ivu-checkbox-inner {

            }
          }

        }
        .ivu-checkbox-wrapper.ivu-checkbox-default{
          .ivu-checkbox{
            .ivu-checkbox-inner{
              background:transparent;
            }
          }
        }
      }
    }
  }
}
</style>
<style>
  .ivu-select{
	  padding: 4.5px 0px;
  }	
  .ivu-select-single .ivu-select-selection .ivu-select-placeholder, .ivu-select-single .ivu-select-selection .ivu-select-selected-value{
    padding-right: 20px;
  }
  .ivu-select-arrow{
    right: 4px;
  }
  .ivu-form-item-error .ivu-input-group-append, .ivu-form-item-error .ivu-input-group-prepend,.ivu-input-group-append, .ivu-input-group-prepend{
    background-color: #17212e;
    border-bottom: 1px solid #27313e;
    border-top:none;
    border-left: none;
    border-right: none;
  }

  .ivu-select-item span:first-child{
    display: inline-block;
    width: 30px;
    text-align: left;
  }
  .ivu-btn-group>.ivu-btn.ivu-btn-default{
    background: transparent!important;
    color: #828ea1;
  }
  .ivu-btn-group>.ivu-btn.active, .ivu-btn-group>.ivu-btn:active, .ivu-btn-group>.ivu-btn:hover{
    border-color: transparent!important;
    color: #f0ac19!important;
  }
  .ivu-btn-group>.ivu-btn:focus{
    box-shadow: none!important;
  }
  .ivu-btn-group>.ivu-btn{
    font-size: 16px;
  }
  .ivu-input-group-append, .ivu-input-group-prepend{
    padding: 0 0!important;
  }
</style>
