<template>
  <div style="padding-bottom: 50px">
    <x-header :left-options="{showBack: false}">个人中心</x-header>
    <div class="main">
      <grid :cols="5" class="header" :show-vertical-dividers="false">
        <grid-item>
          <img :src="headImgUrl" class="headImg">
        </grid-item>
        <grid-item style="width:200px">
          <div class="nick">{{nick}}</div>
          <div class="rank">{{memberType}}</div>
        </grid-item>
      </grid>

      <flexbox class="moneyBox" gutter="2">
        <flexbox-item>
          <cell title="积分" :value="money" is-link class="moneyText"></cell>
        </flexbox-item>
      </flexbox>
      <flexbox class="moneyBox" gutter="2">
        <flexbox-item>
          <cell title="赠送积分" :value="givingMoney" is-link class="moneyText"></cell>
        </flexbox-item>
      </flexbox>
      <flexbox class="moneyBox" gutter="2">
        <flexbox-item>
          <cell title="产品积分" :value="rebatesMoney" is-link class="moneyText"></cell>
        </flexbox-item>
      </flexbox>
      <flexbox class="moneyBox" gutter="2">
        <flexbox-item>
          <cell title="报货积分" :value="rechargeRebates" is-link class="moneyText"></cell>
        </flexbox-item>
      </flexbox>
      <flexbox class="moneyBox" gutter="2">
        <flexbox-item>
          <cell title="团队积分" :value="resultsRebates" is-link class="moneyText"></cell>
        </flexbox-item>
      </flexbox>
      <group :gutter="0">
        <cell title="我的订单" is-link link="/order"></cell>
        <cell title="抽奖" is-link @click.native="lottery"></cell>
      </group>
    </div>
  </div>
</template>
<script>
import {
  XHeader,
  Divider,
  XInput,
  Group,
  XButton,
  Toast,
  Cell,
  Qrcode,
  Flexbox,
  FlexboxItem,
  CellBox,
  Grid,
  GridItem
} from "vux";
import HttpUtil from "../util/HttpUtil";
import MyUtil from "../util/MyUtil";
import { mapState } from "vuex";
export default {
  data() {
    return {
      number: "",
      code: "",
      money: 0,
      givingMoney: 0,
      rebatesMoney: 0,
      rechargeRebates: 0,
      resultsRebates: 0,
      memberType: "",
      shopcode:
        "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wxc8423586aa004b0a&redirect_uri=http%3a%2f%2fwww.jishengsoft.com%2fredirect%2fredirectwemall.asp&response_type=code&scope=snsapi_userinfo&state=STATE#wechat_redirect"
    };
  },
  mounted() {
    this.getInformation();
  },
  methods: {
    getInformation() {
      let _this = this;
      this.$httputil.get(
        "client/getClientInformation.do",
        { client: this.$store.state.vux.snsUserInfo.username },
        function(data) {
          console.log(data.money);
          _this.money = data.money;
          _this.givingMoney = data.givingMoney;
          _this.rebatesMoney = data.rebatesMoney;
          _this.memberType = data.memberType;
          _this.rechargeRebates = data.rechargeRebates;
          _this.resultsRebates = data.resultsRebates;
        }
      );
    },
    lottery() {
      console.log(1111);
      let _this = this;
      this.$httputil.get("wheel/getActivityWheels.do", {}, function(data) {
        if (!data) {
          _this.$vux.alert.show({
            title: "提示",
            content: "目前没有抽奖活动"
          });
        } else {
          _this.$router.push("/myLottery");
        }
      });
    }
  },
  computed: {
    ...mapState({
      nick: state => state.vux.snsUserInfo.nickname,
      headImgUrl: state => state.vux.snsUserInfo.headImgUrl,
      username: state => state.vux.snsUserInfo.username
    })
  },

  components: {
    XHeader,
    Divider,
    XInput,
    Group,
    XButton,
    Toast,
    Cell,
    Qrcode,
    Flexbox,
    FlexboxItem,
    CellBox,
    Grid,
    GridItem
  }
};
</script>
<style>
.header {
  background: linear-gradient(90deg, #4954be, #6bc2f7);
  box-shadow: 0 2px 5px rgba(255, 98, 98, 0.4);
}
.headImg {
  width: 50px;
  height: 50px;
  float: left;
}
.nick {
  font-size: 16px;
  color: #fff;
}
.rank {
  font-size: 16px;
  color: #fff;
  background: orange;
  width: 75px;
  border-radius: 5px;
}

.numberform h1 {
  text-align: center;
  color: #a1a1a1;
}
.moneyText {
  font-size: 9px;
  color: #000;
  padding: 10px 5px;
  background: linear-gradient(90deg, #ecf0fa, #f6f8fc);
}

.moneyBox {
  border-radius: 5px;
}
</style>
