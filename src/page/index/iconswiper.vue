<template>
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
</template>

<script>
	export default {
		props: ['pages'],
		data () {
			return {
				iconOption: {
        	pagination: '.icon-pagination'
      	}
			}
		}
	}

</script>

<style>
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
    height:.66rem;
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
</style>