<template>
  <div :style="{backgroundColor: skinColor}" class="search-header">
    <div class="search-head-icon" @click="goback">
      <icon name="goback"></icon>
    </div>
    <form action="" @onsubmit="">
      <input type="text" class="searchtext" v-model="songname" :style="{backgroundColor: skinColor}" placeholder="search" @focus="getfocus" @blur="losefocus" ref="inputext" @keydown.enter="pushSearchHis">
    </form>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'
import { mapState } from 'vuex'

export default {
  name: 'Search',
  activated() {
    this.songname = "";
    //this.$refs.inputext.focus();
  },
  computed: {
    ...mapState([
      'skinColor'
    ])
  },
  data() {
    return {
      songname:'',
      backicon:`url('/static/goback.svg')`,
      searchicon:`url('/static/search.svg')`
    }
  },
  methods: {
    ...mapMutations([
      ]),
    goback() {
      this.$router.push({ path: '/' });
    },
    searchSong() {
      if (this.songname.length !== 0) {
        if (!this.isStrAllBlank(this.songname)) {
          this.$store.dispatch('getSuggest', this.songname);
          /*this.$store.commit('getSongSearchedName', this.songname);
          this.$store.commit('isShowSearchList', true);
          let codeSongName = escape(this.songname);
          let reqStr = `/netApi/search/suggest?keywords=${codeSongName}`;
          this.$axios.get(reqStr, {timeout: 1000}).then(res => {
            console.log(res);
            if (res.data.result.songs) {
              console.log(res.data.result.songs[0].id);
              let songName = [];
              for (let i = 0;i <= res.data.result.songs.length - 1;i++) {
                songName.push({name: res.data.result.songs[i].name, id: res.data.result.songs[i].id});
              }
              console.log(songName);
              let songNameRes = this.getNoRepeatName(songName);
              console.log(songNameRes);
              this.$store.commit('getSuggestSearchList', songNameRes);
            } else {
              let str = [];
              this.$store.commit('getSuggestSearchList', str);
            }
          }).catch(error => {
            console.log(error);
          });
            /*let reqStrSuggest = `/netApi/search/suggest?keywords=${codeSongName}`;
            this.$axios.get(reqStrSuggest).then(res => {
              console.log(res);
              if (res.data.result.songs) {
                console.log("suggest" + res.data.result.songs[0].id);
                this.$store.commit('musicIdChange', res.data.result.songs[0].id);
              }
            });
            let reqStrs = `/netApi/album?id=35150843`;
            this.$axios.get(reqStrs).then(res => {
              console.log(res);
            });
            let reqStrt = `/netApi/testapi?keywords=${codeSongName}`;
            this.$axios.get(reqStrt).then(res => {
              console.log(res);
            });
            let reqStrtNoLimit = `/netApi/music/url?id=504425722`;
            this.$axios.get(reqStrtNoLimit).then(res => {
              console.log(res);
            });
            /*this.$axios.get('/netApi/music/url?id=460043703').then(res => {
              console.log(res);
            });*/
        } else {
          this.$store.commit('isShowSearchList', false);
        }
      } else {
        this.$store.commit('isShowSearchList', false);
      }
    },
    getfocus() {
      let body = document.body.clientHeight;
      console.log(body);
      document.getElementsByTagName('body')[0].style.height = body + "px";
      this.$store.commit('isShowSearchList', !this.isStrAllBlank(this.songname));
    },
    losefocus() {
      setTimeout(() => {
        document.getElementsByTagName('body')[0].style.height = "100%";
        this.$store.commit('isShowSearchList', false);
      }, 100);//防止移动web上出现点击不响应问题
    },
    getNoRepeatName(arr) {
      let low = [];
      let res = [];
      res.push({name: arr[0].name, id: arr[0].id});
      low.push(arr[0].name.toLowerCase());
      if (arr.length > 1) {
        for (let i = 1;i <= arr.length - 1;i++) {
          if (low.indexOf(arr[i].name.toLowerCase()) === -1) {
            res.push({name: arr[i].name, id: arr[i].id});
            low.push(arr[i].name.toLowerCase());
          }
        }
      }
      return res;
    },
    isStrAllBlank(str) {
      let bool = true;
      for (let i of str) {
        if(i !== " ") {
          bool = false;
          break;
        }
      }
      return bool;
    },
    pushSearchHis() {
      console.log("pushpush")
      if (!this.isStrAllBlank(this.songname)) {
        this.$store.dispatch('getSearchSong', this.songname);
        this.$store.commit('searchHistoryDataChange', {add: true, name: this.songname});
        this.$router.push({ path: '/SearchRes' });
        setTimeout(() => {
          document.getElementsByTagName('body')[0].style.height = "100%";
          this.$store.commit('isShowSearchList', false);
        }, 100);
      }
    }
  },
  watch: {
    songname: {
      handler(now, old) {
        this.searchSong();
      }
    }
  }
}
</script>


<style lang="scss">
::-webkit-input-placeholder{color: silver;}
.search-header {
  .search-head-icon {
    position: absolute;
    width: 20px;
    height: 20px;
    top: 35px;
    left: 20px;
    cursor: pointer;
    display: inline-block;

    & > svg {
      pointer-events: none;
      width: 20px;
      height: 20px;
    }
  }
  .searchtext {
    outline: none;
    position: absolute;
    width: 295px;
    left: 55px;
    bottom: 11px;
    padding-bottom: 3px;
    font-size: 20px;
    color: white;
    border-top-width: 0;
    border-left-width: 0;
    border-right-width: 0;
    border-bottom-width: 1px;
    border-bottom-color: white;
    display: inline-block;
  }
}
</style>