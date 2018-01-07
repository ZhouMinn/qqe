<template>
  <div class="all-cities">
    <div class="city-group" v-for="(cityGroup, cityTitle) in allCities" :key="cityGroup.id" >
      <div class="city-list-title border-bottom" ref="citiesTitle">{{cityTitle}}</div>
      <div class="city-list" v-for="(cityListItem, index) in cityGroup.group" :key="index">
        <div class="city-list-item border-bottom">
          <router-link to="/">
            <div class="city-group-item" @click="changeCity(cityListItem)">{{cityListItem}}</div>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import { mapMutations } from 'vuex'
  export default {
    name: 'city',
    props: ['allCities', 'headTop', 'currentWord'],
    data () {
      return {
        citiesTitle: []
      }
    },
    methods: {
      getCititesTitleTop () {
        const citiesTitles = this.$refs.citiesTitle
        for (let i = 0; i < citiesTitles.length; i++) {
          let city = {
            title: citiesTitles[i].textContent,
            top: citiesTitles[i].offsetTop - this.headTop
          }
          this.citiesTitle.push(city)
        }
        console.log(this.currentWord)
      },

      handleScroll (e) {
        let scrollTop = document.documentElement.scrollTop
        for (let i = 0; i < this.citiesTitle.length; i++) {
          if (scrollTop >= this.citiesTitle[i].top && scrollTop <= this.citiesTitle[i + 1].top) {
            this.$refs.citiesTitle[i].style.position = 'fixed'
            this.$refs.citiesTitle[i].style.zIndex = '100'
            this.$refs.citiesTitle[i].style.top = '.88rem'
            this.$refs.citiesTitle[i].style.background = '#ccc'
          } else {
            this.$refs.citiesTitle[i].style.position = 'relative'
            this.$refs.citiesTitle[i].style.top = '0'
            this.$refs.citiesTitle[i].style.background = ''
          }
        }
      },

      moveToCurrentWord (word) {
        console.log(word)
        for (let i = 0; i < this.citiesTitle.length; i++) {
          if (this.citiesTitle[i].title === word) {
            console.log(this.citiesTitle[i].top)
          }
        }
      },

      ...mapMutations(['changeCity'])
    },
    updated () {
      this.getCititesTitleTop()
      window.addEventListener('scroll', this.handleScroll)
      if (this.currentWord) {
        console.log(this.currentWord)
        this.moveToCurrentWord(this.currentWord)
      }
    },
    destroyed () {
      window.removeEventListener('scroll', this.handleScroll)
    }
  }
</script>

<style scoped>
  .all-cities {
    width: 100%;
  }
  .city-list-title {
    width: 100%;
    line-height: .54rem;
    padding-left: .3rem;
    color: #616161;
    font-size: .26rem;
  }
  .current-title-fixed {
    position: fixed;
    top: .88rem;
    left: 0;
  }
  .city-list {
    background-color: #fff;
    display: flex;
    flex-direction: column;
  }
  .city-group-item {
    line-height: .76rem;
    padding-left: .2rem;
    font-size: .28rem;
    color: #212121;
  }
</style>