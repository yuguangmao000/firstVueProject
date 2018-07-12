<template>
    <div class="pos">
        <el-row>
            <el-col :span="7" class="pos-order" id="order-list">
                <el-tabs>
                    <el-tab-pane label="点餐" >
                        <el-table :data="tableData" boder style="width:100%">
                            <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                            <el-table-column prop="count" label="数量" width="50%"></el-table-column>
                            <el-table-column prop="price" label="金额" width="50%"></el-table-column>
                            <el-table-column prop="domany" label="操作" width="100" fixed="right">
                                <template scope="scope">
                                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div class="totalDiv">
                            <small>数量：{{totalCount}}</small>
                            <small>金额：{{totalMoney}}元 </small>
                               
                        </div>
                        <div class="div-btn">
                            <el-button type="warning">挂单</el-button>
                            <el-button type="danger" @click="delallGoods()">删除</el-button>
                            <el-button type="success" @click="checkout">结账</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">

                    </el-tab-pane>
                    <el-tab-pane label="外卖">

                    </el-tab-pane>
                </el-tabs>
            </el-col>
            <!--商品展示-->
            <el-col :span="17">
            <div class="title">我是产品栏</div>
            <div class="often-goods-list" >
                <ul>
                    <li v-for="(goods, index) in oftenGoods" :key="index" class="oftenGoods"  @click="addOrderList(goods)">
                        <span>{{goods.goodsName}}</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                        <span class="o-price">￥{{goods.price}} </span>
                    </li>
                </ul>
            </div>
            <div class="goods-type">
                <el-tabs>
                    <el-tab-pane label="汉堡">
                        <div>
                            <ul class='cookList'>
                                <li v-for="(goods,index) in type0Goods" :key="index"  @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                    
                                </li>
                            </ul>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="小食">
                        <div>
                            <ul class='cookList'>
                                <li v-for="(goods,index) in type1Goods" :key="index" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </div>
                    </el-tab-pane><el-tab-pane label="饮料">
                        <div>
                            <ul class='cookList'>
                                <li v-for="(goods,index) in type2Goods" :key="index" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </div>
                    </el-tab-pane><el-tab-pane label="套餐">
                        <div>
                            <ul class='cookList'>
                                <li v-for="(goods,index) in type3Goods" :key="index" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </div>
                    </el-tab-pane>
                </el-tabs>
            </div>
            </el-col>
        </el-row> 
    </div>
</template>
<script>
import axios from 'axios';
// import { error } from 'util';
// import { triggerAsyncId } from 'async_hooks';
export default{
    data(){
        return {
            tableData: [{
                    goodsName: '可口可乐',
                    price: 8,
                    count:1
                    }, {
                    
                    goodsName: '香辣鸡腿堡',
                    price: 15,
                    count:1
                    }, {
                    
                    goodsName: '爱心薯条',
                    price: 8,
                    count:1
                    }, {
                    
                    goodsName: '甜筒',
                    price: 8,
                    count:1
                }
            ],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            totalCount:0,
            totalMoney:0,
        }
    },
    mounted:function(){
      var orderHeight=document.body.clientHeight;
      document.getElementById("order-list").style.height=orderHeight+'px';
  },
  created:function(){
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response=>{
        //   console.log(response);
          this.oftenGoods=response.data;
      })
      .catch(error=>{
          console.log(error);
          alert('wangluo错误，不能访问');
      });
       //读取分类商品列表
      axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(response=>{
        //  console.log(response);
         //this.oftenGoods=response.data;
         this.type0Goods=response.data[0];
         this.type1Goods=response.data[1];
         this.type2Goods=response.data[2];
         this.type3Goods=response.data[3];

      })
      .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
      })
  },
  methods:{
        //添加订单列表的方法
        addOrderList(goods){
					let isHave = false;
					this.totalCount=0; //汇总数量清0
					this.totalMoney=0;
					//商品是否已经添加在订单列表里          
					for (let i = 0,ii=this.tableData.length; i < ii; i++) {
							if (this.tableData[i].goodsId==goods.goodsId) {
									isHave=true;
							}
					}
					//根据isHave的值判断订单列表中是否已经有此商品
					if(isHave){
							//存在就进行数量添加
								let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
								arr[0].count++;
								//console.log(arr);
					}else{
							//不存在就推入数组
							let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
								this.tableData.push(newGoods);

					}
					this.getAllMoney(); 
      },
      //删除单个商品
      delSingleGoods(goods){
          this.tableData=this.tableData.filter(o=>o.goodsId != goods.goodsId);
					this.getAllMoney();
			},
      //进行数量和价格的汇总计算
      getAllMoney(){
          this.totalCount=0;
          this.totalCount=0;
          if (this.tableData) {
            this.tableData.forEach((element) => {
                this.totalCount+=element.count;
                this.totalMoney=this.totalMoney+(element.price*element.count);   
            });
          }
          
			},
			//全部删除
			delallGoods(){
				this.tableData=[];
				this.totalCount=0;
				this.totalMoney=0;

			},
			//模拟结账
			checkout(){
				if (this.totalCount !=0) {
					this.tableData=[];
					this.totalCount=0;
					this.totalMoney=0;
					this.$message({
						message:'结账成功,你又为店里出了一份力',
						type:'success'
					})
				}else{
					this.$message.error('不能空结账，老板了解你急切的心情');
				}
			}
  }
}
</script>
<style scoped>
.oftenGoods{
    cursor: pointer;
}
.totalDiv{
    background-color: #fff;
    padding: 10px;
    border-bottom: 1px solid #E5E9F2;
}
.cookList li{
       list-style: none;
       width:35%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
    cursor: pointer;
   }
   .cookList li span{
       
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 16px;
       padding-left: 10px;
       color:brown;
 
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
.goods-type{
    clear: both;
}
.o-price{
    color: #58B7FF;
    
}
.often-goods-list ul li{
    list-style: none;
    float: left;
    border: 1px solid #e5e9F2;
    padding: 10px;
    margin: 10px;
    background-color: #FFF;
    /* background-color: white; */
}
.title{
    height: 20px;
    border-bottom: 1px solid #D3dce6;
    background-color: #F9FAFC;
    padding: 10px;
    text-align: left;
}
.div-btn{
    margin-top: 10px;
}
.pos-order{
    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
    /* height: 100%; */
    /* 在页面中使用了Element组件，这样他会自动给我们生产虚拟DOM，我们无法设置高度100%； */
}
</style>
