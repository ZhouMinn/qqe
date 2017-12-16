<template>
  <div>
    <header class="header">
      <div class="back iconfont">&#xe624;</div>
      <div class="search"></div>
      <div class="city">北京</div>
    </header>

    <swiper :options="swiperOption">
      <swiper-slide v-for="item in swiperInfo" :key="item.id">
        <div class="swiper-img-con">
          <img  class="swiper-img" :src="item.imgUrl"/>
        </div>
      </swiper-slide>
      <div class="swiper-pagination"  slot="pagination"></div>
    </swiper>

    <swiper :options="iconOption">
      <swiper-slide v-for="(pageInfo, index) in pages" :key="index">
        <div class="icon-wrapper">
          <div class="icon-item" v-for="item in pageInfo" :key="item.id">
            <div class="icon-img-con">
              <img  class="icon-img" :src="item.imgUrl"/>
            </div>
            <div class="icon-title">{{item.title}}</div>
          </div>
        </div>
      </swiper-slide>
      <div class="icon-pagination"  slot="pagination"></div>
    </swiper>
    
    <ul class="list-item">
      <li class="list-item-con list-border">
        <span class="tit iconfont">&#xe64e;</span>
        定位失败
      </li>
      <li class="list-item-con">    
        <span class="tit iconfont">&#xe654;</span>
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
        <span class="iconfont"></span>
        <div class="price-title-con">
          <span class="ticketfee">票面价</span>
          是指通过景区指定窗口售卖的纸质门票上标注的价格
        </div>
      </div> 
    </div>
  </div>
</template>

<script>
export default {
  name: 'Index',
  data () {
    return {
      swiperInfo: [],
      iconInfo: [],
      listInfo: [],
      hotInfo: [],
      weekInfo: [],
      swiperOption: {
        autoplay: 10000,
        direction: 'horizontal',
        pagination: '.swiper-pagination',
        loop: true
      },
      iconOption: {
        pagination: '.icon-pagination'
      }
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
  .swiper-img-con {
    overflow: hidden;
    width: 100%;
    height:0;
    padding-bottom: 31.25%; 

  }
  .swiper-img {
    width: 100%;
  }
  .icon-wrapper {
    position: relative;
  }
  .icon-item {
    box-sizing: border-box;
    float: left;
    width: 25%;
    padding-top: .4rem;
    text-align: center;
  }
  .icon-img-con {
    width:.66rem;
    height: .66rem;
    display: inline-block;
  }
  .icon-img {
    width: 100%;
  }
  .icon-title {
    box-sizing: border-box;
    font-size: 0.26rem;
    color: #1b1b1b;
    text-align: center;
    padding-top:0.21rem;
  }
  .icon-pagination {
    padding:0.2rem 0;
    text-align: center;
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
    height: 0.98rem;
    flex: 1;
    text-align: center;
    line-height: 0.98rem;
    font-size: 0.26rem;
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
    
  }
</style>
