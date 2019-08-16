<template>
    <div class="pos">
        <el-row>
            <el-col :span="7" class="pos-order" id="order-list">
                <el-tabs type="border-card">
                    <el-tab-pane label="点餐">
                        <el-table :data="tableData">
                            <!-- <el-table-column type="selection" width="42"></el-table-column> -->
                            <el-table-column prop="goodsName" label="商品名称" width="150"></el-table-column>
                            <el-table-column prop="count" label="数量" width="70"></el-table-column>
                            <el-table-column prop="price" label="金额" width="70"></el-table-column>
                            <el-table-column label="操作" width="150">
                                <template slot-scope="scope">
                                    <el-button type="primary" plain size="mini" @click="shoppingCart(scope.row)">增加</el-button>
                                    <el-button type="danger" plain size="mini" @click="removeGoods(scope.row)">删除</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div class="totalgoods">
                             数量 : {{totalNum}} &nbsp;&nbsp;&nbsp;总价 : {{totalPrice}}
                        </div>
                        <div class="tab-content-button">
                            <el-button type="warning" size="small">挂单</el-button>
                            <el-button type="danger" size="small" @click="removeAllGoods()">删除</el-button>
                            <el-button type="success" size="small" @click="checkOut">结算</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">挂单</el-tab-pane>
                    <el-tab-pane label="外卖">外卖</el-tab-pane>
                </el-tabs>
            </el-col>
            <el-col :span="17">
                <div class="often-goods">
                    <div class="titlt">常用商品</div>
                    <div class="often-goods-list">
                        <ul>
                            <li v-for="good in oftenGoods" @click="shoppingCart(good)">
                                <span>{{good.goodsName}}</span>
                                <span class="goods-price">￥{{good.price}}元</span>
                            </li>
                        </ul>
                    </div>
                </div>
                <div class="goods-type">
                    <el-tabs type="border-card">
                        <el-tab-pane label="汉堡">
                            <ul class='cookList'>
                                <li v-for="type in type0Goods" @click="shoppingCart(type)">
                                    <span class="foodImg"><img src="../../../static/images/1.png" width="100%"></span>
                                    <span class="foodName">{{type.goodsName}}</span>
                                    <span class="foodPrice">￥{{type.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="小食">
                            <ul class='cookList'>
                                <li v-for="type in type1Goods" @click="shoppingCart(type)">
                                    <span class="foodImg"><img src="../../../static/images/1.png" width="100%"></span>
                                    <span class="foodName">{{type.goodsName}}</span>
                                    <span class="foodPrice">￥{{type.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="饮料">
                            <ul class='cookList'>
                                <li v-for="type in type2Goods" @click="shoppingCart(type)">
                                    <span class="foodImg"><img src="../../../static/images/1.png" width="100%"></span>
                                    <span class="foodName">{{type.goodsName}}</span>
                                    <span class="foodPrice">￥{{type.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="套餐">
                            <ul class='cookList'>
                                <li v-for="type in type3Goods" @click="shoppingCart(type)">
                                    <span class="foodImg"><img src="../../../static/images/1.png" width="100%"></span>
                                    <span class="foodName">{{type.goodsName}}</span>
                                    <span class="foodPrice">￥{{type.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                    </el-tabs>
                </div>
            </el-col>
        </el-row>
    </div>
</template>

<script>
import axios from "axios"
export default {
    name:'pos',
    data() {
        return {
            tableData: [],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            totalNum:0,
            totalPrice:0,
        }
    },
    methods: {
        setHeight(){
            var orderHeight = document.body.clientHeight;
            document.getElementById('order-list').style.height = orderHeight + 'px';
        },
        //汇总数量和金额
        getAllMoney(){
            this.totalNum=0;
            this.totalPrice=0;
            if(this.tableData){
                this.tableData.forEach((element) => {
                    this.totalNum += element.count;
                    this.totalPrice += (element.price*element.count);   
                });
            }
        },
        shoppingCart(goods){
            let flag = false;
            this.tableData.forEach(element => {
                //判断该商品是否存在
                if(element.goodsId == goods.goodsId){
                        flag = true;
                }
                
            });
            //不能放在this.tableData.forEach里面，因为开始是没有数据的，不会进入循环
            if(flag == true){
                //存在就进行数量添加
                 let arr = this.tableData.filter(item =>item.goodsId == goods.goodsId);
                 arr[0].count++;
           }
            else{
                let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
                this.tableData.push(newGoods);
                
            }
            this.getAllMoney();  //更新商品总数和总价格
        },
        removeGoods(goods){
            //过滤
            this.tableData = this.tableData.filter(item =>item.goodsId != goods.goodsId);
            this.getAllMoney();//更新商品总数和总价格
        },
        removeAllGoods(){
            
            this.tableData = [];
            this.totalCount = 0;
            this.totalMoney = 0;   //商品总数和总价格归零
        },
        checkOut(){
            if(this.totalNum != 0){
                this.tableData = [];
                this.totalNum = 0;
                this.totalPrice = 0;
                this.$message({
                    message:'结账成功，感谢你又为店里出了一份力!',
                    type:'success'
                })
            }
            else{
                this.$message.error('不能空结。老板了解你急切的心情！');
            }
        }
    },
    created() {
        axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
        .then(response => {
            //console.log(response);
            this.oftenGoods = response.data;
        }).catch(error => {
            console.log(error);
            alert('网络错误，不能访问');
        });
        axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
        .then(response => {
            //console.log(response);
            this.type0Goods=response.data[0];
            this.type1Goods=response.data[1];
            this.type2Goods=response.data[2];
            this.type3Goods=response.data[3];

        }).catch(error => {
            console.log(error);
            alert('网络错误，不能访问');
        });
    },
    mounted() {
        this.setHeight();
        window.onresize = ()=>{
            this.setHeight();            
        }
        
    }
}
</script>

<style lang="less" scoped>
    .tab-first{
        padding-left: 20px;
    }
    .pos-order{
        background-color: #f9fafc;
        border-right: 1px solid #c0ccda;
    }
    .tab-content-button{
        padding-top: 20px;
    }
     .title{
       height: 20px;
       border-bottom:1px solid #D3DCE6;
       background-color: #F9FAFC;
       padding:10px;
   }
   .often-goods-list ul li{
      cursor: pointer;
      list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:5px;
      background-color:#fff;
   }
  .goods-price{
      color:#58B7FF; 
   }
   .goods-type{
       clear: both;
       padding-top:20px;
   }
   .cookList li{
       cursor: pointer;
       list-style: none;
       width:25%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
 
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
       font-size: 14px;
       padding-left: 10px;
       padding-top:10px;
   }
   .totalgoods{
       line-height: 40px;
       font-size: 14px;
       text-align: center;
       border-bottom: 1px solid #eee;
   }
</style>