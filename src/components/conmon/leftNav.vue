<template>
    <div class="left-nav">
        <ul>
            <li>
                <i class="icon iconfont icon-zhengque"></i>
                <div>收银</div>
            </li>
            <li>
                <i class="icon iconfont icon-xiaoxizhongxin"></i>
                <div>店铺</div>
            </li>
            <li>
                <i class="icon iconfont icon-suan"></i>
                <div>商品</div>
            </li>
            <li>
                <i class="icon iconfont icon-shouhuodizhi"></i>
                <div>会员</div>
            </li>
            <li>
                <i class="icon iconfont icon-shezhi"></i>
                <div>设置</div>
            </li>
        </ul>
    </div>
</template>
<script>
export default{
    name:'leftNav',
    data(){
        return {
            name:'ygm'
        }
    }
}
</script>
<style scoped>
.left-nav{
    color:#fff;
    font-size: 10px;
    height: 100%;
    background-color: #1D8CE0;
    float: left;
    width: 5%;
}
.iconfont{
    font-size: 24px;
}
.left-nav ul{
    padding: 0;
    margin: 0;
}
.left-nav li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding: 10px;
}
</style>
