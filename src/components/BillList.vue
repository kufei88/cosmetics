<template>
  <div>
    <div id="emptylist" v-if="list.length==0">
        	<h4>居然还没有订单</h4>
        	<p class="font-size-12">好东西，手慢无</p>
          <router-link to="/" class="tag" style="padding:8px 30px;" >
            去逛逛
          </router-link>

    </div>
    <div v-else id="billlist">
    		<ul class="ubilllist" >
          <li class="bill" v-for="item in list" :key="item.billcode">
            <div>
              <div class="title" >
                <p>{{title}}:{{item.billdate}}</p>
                <div v-if="showMenu">
                  <div v-if="item.audite=='0'">
                    <a class="del" @click.prevent="delBill(item.billcode)">删除</a>
                    <a @click.prevent="edtBill(item.billcode)">修改</a>
                  </div>
                  <div v-else class="signet"></div>
                </div>
              </div>

              <ul class="goodslist" >
                <li v-for="item1 in item.goodslist" :key="item1.goodsName">{{item1.goodsName}}:{{item1.number}}</li>
              </ul>

            </div>
          </li>
    		</ul>
    </div>
  </div>
</template>

<script>
    import {XNumber} from 'vux';
    export default {
        name:'billList',
        components:{
          XNumber
        },

        props:{
          list: Array,
          title:String,
          showMenu:Boolean,
        },

        methods:{
          delBill (item) {
            this.$emit('on-del-item', item)

          },
          edtBill (src) {

            this.$emit('on-edt-item', src)

          },
          findbyindex(currentValue){
            return currentValue.index = this.index;
          }
        }
    }
</script>
<style scoped>




  .clear {
	clear: both;
}

#emptylist {
	margin-bottom: 20px;
	padding-top: 60px;
	font-size: 14px;
	display: block;
	text-align: center;
	padding: 30px 10px;
	color: #999;
	width: 100%;
	padding: 20px 10px;
	vertical-align: middle;
	text-align: center;
	color: #999;
	font-size: 12px;
	line-height: 20px;
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	box-sizing: border-box;
}

#emptylist h4 {
	font-size: 16px;
	margin-bottom: 10px;
	color: #666;
}

.tag {
	color: #f60;
	border-color: #f60;
	font-size: 14px;
	line-height: 18px;
	display: inline-block;
	background-color: transparent;
	border: 1px solid #f60;
	border-radius: 3px;
	text-align: center;
	margin: 0;
	text-decoration: none;
}

#billlist {
	background: #f0f2f5;
	padding-top: 5px;
}

.ubilllist {
	list-style: none;
	margin: 0;
	padding: 0;
	font-size: 0;
}

.bill {
	background: #fff;
	margin-top: 10px;
	border-bottom: 1px solid #ddd;
	border-top: 1px solid #ddd;
	position: relative;
}

.title p {
	margin: 0 10px;
	line-height: 40px;
	height: 40px;
	font-size: 18px;
	float: left;
}

.title a {
	font: 18px/30px "微软雅黑";
	float: right;
	text-decoration: none;
	color: red;
	border: 1px solid red;
	width: 50px;
	text-align: center;
	border-radius: 5px;
	height: 30px;
	margin: 5px;
}

.title {
	height: 40px;
	border-bottom: 1px solid #ddd;
}

.goodslist {
	font-size: 0;
	padding: 0 15px;
}

.goodslist li {
	display: inline-block;
	font-size: 13px;
	width: 45%;
	height: 40px;
	line-height: 40px;
}

.signet {
	z-index: 2;



	right:30px;
	top: 20%;
	width: 64px;
	height: 64px;
	background: url(../assets/index.png) no-repeat;
	background-size: 64px 64px;
}


</style>

