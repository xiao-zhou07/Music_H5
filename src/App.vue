<template>
  <div id="app" ref="app">
    <keep-alive>
        <router-view></router-view>
    </keep-alive>
    <bot-player v-show="isRoute && isBot" @toPlayer="isRoute = false" @show-sheet="OpenSheet"></bot-player>
    <transition name="slide-top">
      <Player v-show="!isRoute" @toHome="isRoute = true" @show-sheet="OpenSheet" ref="player"></Player>
    </transition>
    <player-lists ref="playerlists" :state="isSheet" @hide-sheet="isSheet = false"></player-lists>
  </div>
</template>

<script>
import BotPlayer from './components/BotPlayer.vue'
import Player from './components/Player.vue'
import PlayerLists from './components/PlayerLists.vue'
import { mapState } from 'vuex'
export default {
  name: 'app',
  data(){
      return{
        isRoute: true,
        isBot: false,
        isSheet: false,
      }
  },
  components:{ BotPlayer  , Player , PlayerLists },
  computed:{
    ...mapState([ 'songList' ]),
  },
  created(){
    this.$store.state.reSize.push(this.setRem);
    this.reSize();
    let resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize';
    window.addEventListener(resizeEvt, this.reSize, false);
  },
  mounted() {
    this.setDefaultTheme();
  },
  watch:{
    songList(val){
      this.isBot = val.length > 0 ? 1 : 0;
    },
  },
  methods:{
    OpenSheet(){
      this.isSheet = true;
    },
    setDefaultTheme() {
      let theme = localStorage.getItem("theme");
      if(!this.$empty(theme))
        document.body.className = theme;
    },
    reSize(){
      let funs = this.$store.state.reSize;
      for(let i = 0; i < funs.length; i++){
          if(typeof funs[i] === 'function')
              funs[i]();
      }
    },
    setRem() {
      let _html = document.getElementsByTagName("html")[0];
      let w = document.body && document.body.clientWidth || _html.offsetWidth;
      let p = Number(w / 720).toFixed(3);
      p = p > 1.067 ? 1.067 : p < 0.444 ? 0.444 : p;
      _html.setAttribute("style", "font-size:"+ p * 100 + "px");
      window.fontsize = p * 100;
    },
  }
}
</script>

<style lang="less">
@import 'assets/css/theme';
body{
  background: #ececec;
  position: relative;
}
body,html,#app{
  height: 100%;
  max-width: 450px;
  margin: 0 auto;
}
#app{
  overflow: hidden;
  background: #fff;
  position: relative;
}
body{
  font-size: .25rem;
}
.lucky{
  .change();
}
.float{
  float: left;
}
a{
  color: #000 !important;
}
.center{
  position: absolute !important;
  margin: auto !important;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
.centerXY{
  position: absolute !important;
  left: 50% !important;
  top: 50% !important;
  transform: translate(-50% , -50%) !important;
}
.centerX{
  position: absolute !important;
  left: 50% !important;
  transform:   translate(-50% , 0%) !important;
}
.centerY{
  position: absolute !important;
  top: 50% !important;
  transform: translate(0% , -50%) !important;
}

.covebg{
  position: absolute;
  width: 100%;
  height: 100%;
  background: url("https://p1.music.126.net/Bt3dFUR9xWCd8wJlmrsgXg==/3429376768564696.jpg?param=500y500");
  top: 0;
  left: 0;
  background-repeat:no-repeat;
  background-size:cover;
  background-position:50%,50%;
  /*-webkit-filter:blur(50px);*/
  /*filter:blur(40px);*/
}





.slide-right-enter-active {
  transition: all .6s ease;
}
.slide-right-leave-active {
  /*transition: all .5s ease;*/
}
.slide-right-enter, .slide-right-leave-active {
  transform: translateX(60px);
  opacity: 0;
}
.slide-left-enter-active {
  transition: all .6s ease;
}
.slide-left-leave-active {
  /*transition: all .5s ease;*/
}
.slide-left-enter, .slide-fade-leave-active {
  transform: translateX(-60px);
  opacity: 0;
}

.fade-enter-active {
  transition: all .3s ease;
}
.fade-leave-active {
}
.fade-enter, .fade-leave-active {
  opacity: 0;
}
.slide-left-enter-active {
  transition: all .3s ease;
}
.slide-left-leave-active {
  transition: all .2s ease-out;
}
.slide-left-enter, .slide-left-leave-active {
  transform: translateX(-150px);
  opacity: 0;
}

.slide-top-enter-active {
  transition: all .2s ease;
}
.slide-top-leave-active {
  transition: all .2s ease-out;
}
.slide-top-enter, .slide-top-leave-active {
  transform: translateY(200px);
  opacity: 0;
}
</style>
