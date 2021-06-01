<template>
  <div class="content-wraps">
    <div class="containers" id="List">
      <div class="fiat">
        <div class="to_business">
          <h3>GMEEX P2P: BUY/SELL YOUR CRYPTO LOCALLY</h3>
          <span>Trade with Zero Fees</span>
          <a href="javascript:void(0)" @click="goBusiness">Become a merchant</a>
          <!-- <router-link to="/identbusiness">成为商家</router-link> -->
        </div>
      </div>
      <div class="content">
        <Menu ref="navMenu" mode="horizontal" width="auto" :active-name="activeMenuName" @on-select="menuSelected" class='tradelist'>
          <MenuGroup>
            <template v-for="(coin,index) in coins">
              <MenuItem :name="'coin-'+index"> {{coin.unit}}
              </MenuItem>
            </template>
          </MenuGroup>
        </Menu>
        <router-view></router-view>
      </div>
      <div class="advantage">
        <ul>
          <li>
            <div class="image"><img src="../../assets/images/price.png" alt=""></div>
            <div class="title">Low cost transaction fees</div>
            <div class="content1">As P2P exchange is a simple platform, the overhead costs are negligible for buyers and sellers. On GMEEX P2P, you are charged zero trading fees. We do not take any fees from peer-to-peer fiat-crypto transactions or offers posted on our platform.</div>
          </li>
          <li>
            <div class="image"><img src="../../assets/images/poundage.png" alt=""></div>
            <div class="title">Flexible payment methods</div>
            <div class="content1">Peer-to-peer exchanges allow sellers freedom to define how they want to be paid.On GMEEX P2P, you can buy and sell crypto with over 150 payment methods, including bank transfer, cash, PayPal, M-Pesa and multiple e-wallets.</div>
            <li>
              <div class="image"><img src="../../assets/images/instant.png" alt=""></div>
              <div class="title">Trade at your preferred prices</div>
              <div class="content1">Peer-to-peer exchanges bring users freedom to trade crypto at the preferred prices.On GMEEX P2P, you can not just buy or sell crypto from the existing offers, but also create your trade advertisements to set your own prices.</div>
            </li>
            <li>
              <div class="image"><img src="../../assets/images/platedanbao.png" alt=""></div>
              <div class="title">Protection for your privacy</div>
              <div class="content1">Unlike credit card or bank transfers, peer-to-peer exchanges do not collect information about buyers and sellers.So you can buy Bitcoin with cash on GMEEX P2P and don't need to use any bank account or online wallet to make a crypto-fiat transaction.</div>
            </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.content-wraps {
  padding: 0 12%;
  // background-color: #fff;
  padding-top: 60px;
  .containers {
    width: 100%;
    margin: 20px 0;
    .fiat {
      border-radius: 5px;
      height: 250px;
      background: #273640;
      background-size: 100%;
      display: flex; //flex布局
      justify-content: center; //使子项目水平居中
      align-items: center; //使子项目垂直居中
      .to_business {
        color: #fff;
        text-align: center;
        h3 {
          font-size: 46px;
          letter-spacing: 0px;
        }
        span {
          font-size: 20px;
          letter-spacing: 0px;
          display: block;
        }
        a {
          width: 220px;
          height: 45px;
          display: inline-block;
          background: #ffa400;
          border-radius: 5px;
          font-size: 20px;
          line-height: 45px;
          color: #000;
          margin-top: 20px;
        }
      }
    }
    .content {
      width: 100%;
      margin: 20px auto;
      background-color: #192330;
      border-radius: 4px;
    }
    .advantage {
      background-color: #192330;
      border-radius: 4px;
      ul {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 30px;
        li {
          width: 25%;
          list-style-type: none;
          min-height: 190px;
          div {
            text-align: center;
          }
          div.image {
            width: 50px;
            height: 50px;
            margin: 20px auto;
            img {
              width: 80%;
              // height: 80%;
              vertical-align: middle;
            }
          }
          div.title {
            line-height: 30px;
            font-size: 16px;
            color: #fff;
          }
          div.content1 {
            padding: 20px 40px;
            line-height: 20px;
            font-size: 12px;
            color: #999;
          }
        }
      }
    }
  }
}
</style>
<style lang="scss">
.content-wraps {
  .containers {
    .content {
      ul.tradelist.ivu-menu.ivu-menu-light.ivu-menu-horizontal {
        background-color: #192330;
        border-radius: 4px;
        &:after {
          background: none;
        }
        .ivu-menu-item-group {
          li.ivu-menu-item {
            border: none;
            &:hover {
              color: #f0ac19;
              border-bottom: 0;
            }
          }
          li.ivu-menu-item.ivu-menu-item-active.ivu-menu-item-selected {
            color: #f0ac19;
            border-bottom: none;
          }
        }
      }
      .nav-right.tradeCenter .list-content .ivu-tabs .ivu-tabs-tabpane {
        .ivu-table-wrapper {
          .ivu-spin.ivu-spin-large.ivu-spin-fix {
            border-color: #fff;
          }
        }
      }
    }
  }
}
</style>
<script>
export default {
  data() {
    return {
      coins: [],
      activeMenuName: "coin-1"
    };
  },
  computed: {
    isLogin: function() {
      return this.$store.getters.isLogin;
    }
  },
  watch:{
    $route(to, from) {
      this.activeMenu();
    }
  },
  methods: {
    init() {
      this.$store.commit("navigate", "nav-otc");
      this.$http.post(this.host + this.api.otc.coin).then(response => {
        if (response.body.code == 0) {
          this.coins = response.body.data;
          this.activeMenu();
          this.$nextTick(function() {
            this.$refs.navMenu.updateActiveName();
          });
        }
      });
    },
    goBusiness() {
      if (this.isLogin) {
        this.$router.push({
          path: "/identbusiness"
        });
      } else {
        this.$Message.warning("Please login in");
      }
    },
    menuSelected(menuName) {
      if (menuName.startsWith("coin")) {
        var coin = this.coins[menuName.split("-")[1]];
        this.$router.push("/otc/trade/" + coin.unit);
      } else {
        this.$router.push("/otc/" + menuName);
      }
    },
    activeMenu() {
      let coin = this.$route.params[0] || "USDT";
      coin = coin.toUpperCase();
      let index=0;
      this.coins.forEach((v,i)=>{
        if(v.unit===coin){
          index=i;
        }
      })
      this.activeMenuName = `coin-${index}`;
      this.$nextTick(function() {
        this.$refs.navMenu.updateActiveName();
      });
    }
  },
  created: function() {
    this.init();
    // this.activeMenuName = "coin-1";
    // this.$nextTick(function() {
    //   this.$refs.navMenu.updateActiveName();
    // });
  }
};
</script>
