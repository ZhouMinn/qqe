<template>
  <div>
    <header class="header">
      <div class="back iconfont">&#xe624;</div>
      <div class="search">
      <i class="search-con iconfont">&#xe6a4;</i>
      <span class="search-btn">输入城市/景点/游玩主题</span>
      </div>
      <div class="city">北京</div>
    </header>
    
    <swiper-router :swiperInfo="swiperInfo"></swiper-router>
    
    <icon-router :pages="pages"></icon-router>
    
    <ul class="list-item">
      <li class="list-item-con list-border">
        <i class="tit iconfont">&#xe64e;</i>
        定位失败
      </li>
      <li class="list-item-con">    
        <i class="tit iconfont">&#xe654;</i>
        5折泡温泉
      </li>
    </ul>

    <ul class="list-activity">
      <li class="list-activity-con" v-for="item in listInfo" :key="item.id">
        <a href="javascript:;" class="promotion">
          <img :src="item.imgUrl" class="promotion-img">
        </a>
      </li>
    </ul>

    <div class="lazy-load">
      <h2 class="modtitle">热销推荐</h2>
      <div class="mp-hot-con">
        <ul class="hot-list">
          <li class="hot-list-con" v-for="item in hotInfo" :key="item.id">
            <a href="javascript:;" class="hot-list-click">
              <div class="hot-click-con">
                <img :src="item.imgUrl" class="hot-click-img">
              </div>
              <div class="hot-click-title">
                <div class="hot-clickcon">{{item.title}}</div>
                <div class="hot-title-con">{{item.titlepage}}</div>
              </div>
              <div class="hot-list-price">
                ￥
                <em class="hot-list-num">{{item.price}}</em>
                <span class="hot-list-start">起</span>
              </div>
            </a>
          </li>
        </ul>
        <div class="hot-list-more">
          <a href="javascript:;">查看所有产品</a>
        </div>
      </div>
    </div>

    <div class="week">
      <h2 class="modtitle">周末去哪儿</h2>
      <div>
        <div class="week-list" v-for="item in weekInfo" :key="item.id">
          <a href="javascript">
            <div class="week-list-con">
              <img :src="item.imgUrl" alt="" class="week-list-img">
            </div>
            <div class="title">
              <p class="title-name">{{item.title}}</p>
              <p class="title-page">{{item.titlepage}}</p>
            </div>
          </a>
        </div>
      </div>
      <div class="price-title">
        <i class="hint iconfont">&#xe6a5;</i>
        <div class="price-title-con">
          <span class="ticketfee">票面价</span>
          是指通过景区指定窗口售卖的纸质门票上标注的价格
        </div>
      </div> 
    </div>

    <div class="footer">
      <ul class="footer-list">
        <li class="footer-list-con">
          <i class="plane iconfont">&#xe70e;</i>
          <a href="javascript:;" class="footer-list-title">机票</a>
        </li>
        <li class="footer-list-con">
          <i class="plane iconfont">&#xe713;</i>
          <a href="javascript:;" class="footer-list-title">酒店</a>
        </li>
        <li class="footer-list-con">
          <i class="plane iconfont">&#xe6e3;</i>
          <a href="javascript:;" class="footer-list-title">公寓</a>
        </li>
        <li class="footer-list-con">
          |
        </li>
        <li class="footer-list-con more">
          <i class="more-list iconfont">&#xe6e3;</i>
          <a href="javascript:;" class="footer-list-title foot-tit">更多</a>
        </li>
      </ul>

      <ul class="detail">
        <li class="detail-list">
          <a href="javascript:;" class="detail-list-con">
          登录
          </a>
        </li>
        <li class="detail-list">
          <a href="javascript:;" class="detail-list-con">
          我的订单
          </a>
        </li>
        <li class="detail-list">
          <a href="javascript:;" class="detail-list-con">
          最近浏览
          </a>
        </li>
        <li class="detail-list">
          <a href="javascript:;" class="detail-list-con">
          关于我们
          </a>
        </li>
      </ul>

      <ul class="toggle">
        <li class="toggle-list">
          <a href="javascript:;" class="toggle-list-con">触屏版</a>
        </li>
        <li class="toggle-list">
          <a href="javascript:;" class="toggle-list-con1">电脑版</a>
        </li>
      </ul>
      <div class="address">
        <p class="address-con">Qunar 京ICP备05021087</p>
        <p class="address-con address-con1">意见反馈</p>
      </div>
    </div>
  </div>
</template>

<script>

import SwiperRouter from './swiper'
import IconRouter from './iconswiper'

export default {
  name: 'Index',
  components: {
    SwiperRouter,
    IconRouter
  },
  data () {
    return {
      swiperInfo: [],
      iconInfo: [],
      listInfo: [],
      hotInfo: [],
      weekInfo: []
    }
  },
  computed: {
    pages () {
      const pages = []
      this.iconInfo.forEach((value, index) => {
        let page = Math.floor(index / 8)
        if (!pages[page]) {
          pages[page] = []
        }
        pages[page].push(value)
      })
      return pages
    }
  },
  methods: {
    getIndexData () {
      this.$http.get('./static/index.json')
        .then(this.handleGetDataSucc.bind(this))
    },
    handleGetDataSucc (res) {
      const body = res.body
      if (body && body.data && body.data.swiper) {
        this.swiperInfo = body.data.swiper
        this.iconInfo = body.data.icons
        this.listInfo = body.data.list
        this.hotInfo = body.data.hot
        this.weekInfo = body.data.week
      }
    }
  },
  created () {
    this.getIndexData()
  }
}
</script>

<style scoped>
  .header {
    display: flex;
    background: #05bad5;
    color: #fff;
  }
  .back {
    width: .64rem;
    line-height: .86rem;
    padding-left: 0.2rem;
  }
  .search {
    flex: 1;
    margin: .14rem .18rem;
    background: #fff;
    border-radius: .1rem;
  }
  .search-con {
    display: block;
    color:#ebebeb;
    margin-top:.12rem;
    margin-left: .2rem;
    float: left;
  }
  .search-btn {
    float: left;
    font-size: .26rem;
    color:#ebebeb;
    margin-top:.16rem;
    margin-left: .16rem;
  }
  .city {
    width: 1.14rem;
    line-height: .86rem;
    text-align: center;
    padding-right: 0.3rem; 
    font-size:0.28rem;
  }
  .city::after {
    content: "";
    display: block;
    width:0;
    height:0;
    border: 0.1rem solid #fff;
    border-color: white transparent transparent transparent;
    position: absolute;
    right: 0.36rem;
    top: 0.38rem; 
  }
  
 
  .list-item {
    display: flex;
    position: relative;
  }
  .list-item::before {
    content: "";
    width: 100%;
    height: 2px;
    background: #e1e1e1;
    position: absolute;
    bottom: 1rem;
    transform: scaleY(0.5);
    transform-origin: left bottom;
  }
  .list-item-con {
    flex: 1;
    text-align: center;
    line-height: 0.98rem;
    font-size: 0.26rem;
    height:0;
    padding-bottom: .98rem;
  }
  .list-border::before {
    content: "";
    width: 2px;
    height: 100%;
    background: #e1e1e1;
    position: absolute;
    left: 3.7rem;
    transform: scaleX(0.5);
    overflow: hidden;
  }
  .tit {
    color: #0e0e0e;
  }
  .list-activity {
    display: flex;
    padding-top:.2rem;
    background: #f5f5f5;
    position: relative;
    overflow: hidden;
  }
  .list-activity::before {
    content: "";
    width: 100%;
    height: 2px;
    background: #e1e1e1;
    position: absolute;
    transform: scaleY(0.5);
    transform-origin: left bottom;
  }
  .list-activity::after {
    content: "";
    width: 100%;
    height: 2px;
    background: #e1e1e1;
    position: absolute;
    bottom:0rem;
    transform: scaleY(0.5);
    transform-origin: left bottom;
  }
  .list-activity-con:first-child::before {
    content: "";
    width: 2px;
    height: 85%;
    background: #e1e1e1;
    position: absolute;
    left: 3.7rem;
    bottom:0rem;
    transform: scaleX(0.5);
    overflow: hidden;
  }
  .list-activity-con {
    height: 1.4rem;
    flex: 1;
    background: #fff;
  }
  .promotion {
    display: block;
    width:100%;
    height:100%;
  }
  .promotion-img {
    width:100%;
    height:100%;

  }
  .lazy-load {
    
  }
  .modtitle {
    height:.8rem;
    line-height: .8rem;
    font-size:.26rem;
    color: #212121;
    padding-left: 0.26rem;
    background:#f5f5f5;
  }
  .mp-hot-con {
   
  }
  .hot-list {

  }
  .hot-list-con {
    height: 1.4rem;
    padding: .24rem;
    position: relative;
    overflow: hidden;
  }
  .hot-list-con::before {
    content: "";
    width: 100%;
    height: 2px;
    background: #e1e1e1;
    bottom: 0rem;
    left:0rem;
    position: absolute;
    transform: scaleY(0.5);
    transform-origin: left bottom;
  }
  .hot-click-con {
    width:1.4rem;
    height: 1.4rem;
    float: left;
  }
  .hot-click-img {
    width:100%;
    height:100%;
  }
  .hot-click-title {
    margin-left: 1.6rem;
  }
  .hot-clickcon {
    font-size:0.3rem;
    color:#212121;
    margin: .04rem 0 .1rem 0;
  }
  .hot-title-con {
    color: #9e9e9e;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    margin-bottom: 0.1rem;
    height:.4rem;
    line-height: .4rem;
  }
  .hot-list-price {
    margin-left: 1.6rem;
    color:#ff8300;
    font-size: 0.26rem;
  }
  .hot-list-num {
    font-size: 0.36rem;
  }
  .hot-list-start {
    color:#9e9e9e;
    font-size: 0.24rem;
  }
  .hot-list-more {
    height:.88rem;
    color:#00afc7;
    text-align: center;
    line-height: .88rem;
  }
  .week {
    background:#f5f5f5;
  }
  .week-list {
    margin-bottom: .1rem;
    background: #fff;
  }
  .week-list-con {
    height:0;
    overflow: hidden;
    padding-bottom: 37.4375%;
  }
  .week-list-img {
    width:100%;
  }
  .title {
    position: relative;
    padding: .14rem .2rem .2rem .2rem;
  }
  .title-name {
    padding-right: 1.4rem;
    color:#212121;
    font-size:0.28rem;
    line-height: 0.48rem;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .title-page {
    padding-right: 1.4rem;
    color:#616161;
    font-size: .24rem;
    line-height: .42rem;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .price-title {
    margin-top: .1rem;
    padding: .14rem .1rem;
    background: #fff;
  }
  .hint {
    display: block;
    width:.24rem;
    line-height: .32rem;
    float: left;
    font-size: .24rem;
  }
  .price-title-con {
    margin-left:.41rem;
    font-size: .22rem;
    line-height: .32rem;
    color: #777;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .ticketfee {
    color: #535353;
  }
  .footer {
    background: #f3f3f3;
    height: 2.67rem;
    position: relative;
  }
  .footer-list {
    box-sizing: border-box;
    padding:.21rem .42rem;
    height: .94rem;
  }
  .footer-list-con {
    font-size: .26rem;
    float: left;
    line-height: .42rem;
    box-sizing: border-box;
    padding-left:0.48rem;
  }
  .plane {
    font-size: .42rem;
    float: left;
    color: #a3a3a3;
  }
  .footer-list-title {
    color: #969696;
    margin-left: .11rem;
    border-bottom:.01rem solid #969696; 
  }
  .more-list {
    font-size:0.2rem;
  }
  .more {
    box-sizing:border-box;
    padding-left: 0.29rem;
  }
  .foot-tit {
    border-bottom:none; 
  }
  .detail {
    box-sizing:border-box;
    height:.54rem;
    padding-bottom:.3rem;
  }
  .detail::after {
    content: "";
    width: 100%;
    height:2px;
    background: #c4c4c4;
    bottom: 1.2rem;
    left:0rem;
    position: absolute;
    transform: scaleY(0.5);
    transform-origin: left bottom;

  }
  .detail-list {
    float: left;
    padding-left:.42rem; 
    font-size: .26rem;
   
  }
  .detail-list-con {
    line-height: .24rem;
  }
  .toggle {
    height:.69rem;
    box-sizing:border-box;
    padding:.21rem 2.62rem;
  }
  .toggle-list {
    font-size:.26rem;
    float: left;
  }
  .toggle-list-con {
    color:#060606;
  }
  .toggle-list-con1 {
    padding-left: .65rem;
  }
  .address {
    height:.45rem;
    box-sizing:border-box;
    padding-bottom:.22rem;
    padding-left: 1.64rem;
  }
  .address-con {
    float: left;
    font-size: .26rem;
    color: #949494;
    line-height: .45rem;
  }
</style>
