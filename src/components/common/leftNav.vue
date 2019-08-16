<template>
    <div class="left-nav">
        <ul>
            <li>
                <i class="icon iconfont icon-wodezichan"></i>
                <div>收银</div>
            </li>
            <li>
                <i class="icon iconfont icon-dianpu"></i>
                <div>店铺</div>
            </li>
            <li>
                <i class="icon iconfont icon-hanbao"></i>
                <div>商品</div>
            </li>
            <li>
                <i class="icon iconfont icon-huiyuanqia"></i>
                <div>会员卡</div>
            </li>
            <li>
                <i class="icon iconfont icon-gongnengjianyi"></i>
                <div>设置</div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name:'leftNav',
    data(){
        return {

        }
    }
}
</script>


<style lang="less" scoped>
@maincolor: #1D8ce0;
    .left-nav{
       color:#fff;
       font-size:10px;
       height:100%;
       background-color:@maincolor;
       float:left;
       width:5%;
    }
    .iconfont{
       font-size:24px;
    }
    .left-nav ul{
        padding:0px;
        margin: 0px;
    }
    .left-nav li{
        cursor: pointer;
        list-style: none;
        text-align: center;
        border-bottom:1px solid #fff;
        padding:10px;
    }
</style>