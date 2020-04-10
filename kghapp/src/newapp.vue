
<template>
  <v-app id="inspire">
    <v-navigation-drawer id="player"
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app
    >
      <v-list dense>
        <template v-for="item in  musicList" >
          <v-row
            v-if="item.heading"
            :key="item.heading"
            align="center"
          >
            <v-col cols="6">
              <v-subheader v-if="item.heading">
               
              </v-subheader>
            </v-col>
            <v-col
              cols="6"
              class="text-center"
            >
              <a
                href="#!"
                class="body-2 black--text"
              >555</a>
            </v-col>
          </v-row>
          
          <v-list-item
            v-else
            :key="item.text"
            link
          >
            <v-list-item-action>
              <v-icon class="ma-2" color="red" dark>mdi-television-play</v-icon>
            </v-list-item-action>
            <v-list-item-content id="player" @click='playMusic(item.id)' >              
              <v-list-item-title @click='playMusic(item.id)'>
                {{ item.name }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="red darken-1"
      dark
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title
        style="width: 300px"
        class="ml-0 pl-5"
      >
        <span class="hidden-sm-and-down">KGH的音乐播放器</span>
      </v-toolbar-title>
      <v-text-field v-model='query' @keyup.enter="searchMusic();" id="player"
        flat
        solo-inverted
        hide-details
        prepend-inner-icon="mdi-magnify"
        label="请输入歌手或歌曲名"
        class="hidden-sm-and-down"
      />
      <v-spacer />
      <v-btn icon>
        <v-icon>mdi-thumb-up</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>mdi-cloud-upload</v-icon>
      </v-btn>
      <v-btn
        icon
        large
      >
        <v-avatar
          size="32px"
          item
        >
          <v-img
            src="https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=891565512,3423296413&fm=26&gp=0.jpg"
            alt="Vuetify"
          /></v-avatar>
      </v-btn>
    </v-app-bar>
    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-row
          align="center"
          justify="center"
        >
        </v-row>
      </v-container>
    </v-content>
    <v-btn
      bottom
      color="pink"
      dark
      fab
      fixed
      right
      @click="dialog = !dialog"
    >
      <v-icon>mdi-plus</v-icon>
    </v-btn>

    <v-dialog
      v-model="dialog"
      width="800px"
    >

      <v-card>
        <v-card-title class="blue darken-1">
          注册界面
        </v-card-title>
        <v-container>
          <v-row class="mx-2">
            <v-col
              class="align-center justify-space-between"
              cols="12"
            >
              <v-row
                align="center"
                class="mr-0"
              >
                <v-avatar
                  size="40px"
                  class="mx-3"
                >
                  <img
                    src="//ssl.gstatic.com/s2/oz/images/sge/grey_silhouette.png"
                    alt=""
                  >
                </v-avatar>
                <v-text-field
                  placeholder="Name"
                />
              </v-row>
            </v-col>
            <v-col cols="6">
              <v-text-field
                prepend-icon="mdi-account-card-details-outline"
                placeholder="Company"
              />
            </v-col>
            <v-col cols="6">
              <v-text-field
                placeholder="Job title"
              />
            </v-col>
            <v-col cols="12">
              <v-text-field
                prepend-icon="mdi-mail"
                placeholder="Email"
              />
            </v-col>
            <v-col cols="12">
              <v-text-field
                type="tel"
                prepend-icon="mdi-phone"
                placeholder="(000) 000 - 0000"
              />
            </v-col>
            <v-col cols="12">
              <v-text-field
                prepend-icon="mdi-text"
                placeholder="Notes"
              />
            </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-btn
            text
            color="primary"
          >More</v-btn>
          <v-spacer />
          <v-btn
            text
            color="primary"
            @click="dialog = false"
          >Cancel</v-btn>
          <v-btn
            text
            @click="dialog = false"
          >保存</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <template>
      <div class="player_con" :class="{playing:isPlay}">
          <img src="./assets/player_bar.png" class="play_bar" style="position: absolute;
  left: 480px;
  top: 180px;
  z-index: 10;
  transform: rotate(-25deg);
  transform-origin: 12px 12px;
  transition: 1s;
   transform: rotate(0);
  " />
          <img src="./assets/disc.png" class="disc autoRotate" style=" position: absolute;
  left: 500px;
  top: 250px;
  width: 300px;
  height: 300px;
  z-index: 9;
  animation-name: Rotate;
  animation-iteration-count: infinite;
  animation-play-state: paused;
  animation-timing-function: linear;
  animation-duration: 5s;
  transform: rotateZ(0-360deg);"
  
  />
          <img :src="coverUrl==''?'./assets/cover.png':coverUrl" class="cover autoRotate" style=" 
          position: absolute;
  left: 550px;
  top: 280px;
  width: 190px;
  height: 230px;
  border-radius: 75px;
  z-index: 8;
          animation-name: Rotate;
  animation-iteration-count: infinite;
  animation-play-state: paused;
  animation-timing-function: linear;
  animation-duration: 5s;" />
        </div>
     
    </template>
    <template>
       <div class="comment_wrapper" ref='comment_wrapper' style=" width: 580px;
  height: 600px;
  list-style: none;
  position: absolute;
  left: 900px;
  top: 50px;
  padding: 25px 10px;
  overflow: auto;" >
      <h5 class='text-center'>热门留言</h5>
    <div class='comment_list' style=" position: absolute;
  top: 0;
  margin-top: 10px;">
            <dl v-for="item in hotComments"  v-bind:key="item" style=" padding-top: 40px;
  padding-left: 55px;
  position: relative;
  margin-bottom: 20px;
  ">
              <dt style="position: absolute;
  left: 4px;
  top: 10px;">
                <img :src="item.user.avatarUrl" alt=""  style="width: 40px;
  height: 40px;
  border-radius: 20px;"/>
              </dt>
              <dd class="name" style=" font-weight: bold;
  color: #333;
  padding-top: 5px;">{{item.user.nickname}}</dd>
              <dd class="detail" style=" color: #666;
  margin-top: 5px;
  line-height: 18px;">
                {{item.content}}
              </dd>
            </dl>
          </div>
          <img src="./assets/line.png" class="right_line" style=" position: absolute;
  left: 0;
  top: 0;">
           </div>
    </template>
    <template> 
       <div class="audio_con" style="color: #333;height:50px;">
        <audio ref='audio' @play="play" @pause="pause" :src="musicUrl" controls autoplay loop style="height:30px;outline: none;margin-left:280px;width:1150px;background-color:red"></audio>
      </div>  
</template>
  </v-app>
</template>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

  <!-- 官网提供的 axios 在线地址 -->
<script>
  export default {
    props: {
      source: String,
     },
     data: () => ({
       dialog: false,
       drawer: null,
      items: [
         { icon: 'mdi-contacts', text: 'Contacts' },
         { icon: 'mdi-history', text: 'Frequently contacted' },
         { icon: 'mdi-content-copy', text: 'Duplicates' },
        { icon: 'mdi-settings', text: 'Settings' },
        { icon: 'mdi-message', text: 'Send feedback' },
        { icon: 'mdi-help-circle', text: 'Help' },
         { icon: 'mdi-cellphone-link', text: 'App downloads' },
         { icon: 'mdi-keyboard', text: 'Go to the old version' },
    ], 
    query: '',
        // 歌曲列表
        musicList: [],
        // 歌曲url
        musicUrl: '',
        // 是否正在播放
        isPlay: false,
        // 歌曲热门评论
        hotComments: [],
        // 歌曲封面地址
        coverUrl: '',
        // 显示视频播放
        showVideo: false,
        // mv地址
        mvUrl: ''
     }),     
     methods: {
        // 搜索歌曲
        searchMusic() {
          if (this.query == 0) {
            return
          }
          axios.get('https://autumnfish.cn/search?keywords=' + this.query).then(response => {
            // 保存内容
            this.musicList = response.data.result.songs;

          })

          // 清空搜索
          this.query = ''
        },
        // 播放歌曲
        playMusic(musicId) {
          // 获取歌曲url
          axios.get('https://autumnfish.cn/song/url?id=' + musicId).then(response => {
            // 保存歌曲url地址
            this.musicUrl = response.data.data[0].url
          })
          // 获取歌曲热门评论
          axios.get('https://autumnfish.cn/comment/hot?type=0&id=' + musicId).then(response => {
            // console.log(response)
            // 保存热门评论
            this.hotComments = response.data.hotComments

          })
          // 获取歌曲封面
          axios.get('https://autumnfish.cn/song/detail?ids=' + musicId).then(response => {
            // console.log(response)
            // 设置封面
            this.coverUrl = response.data.songs[0].al.picUrl
          })

        },
        // audio的play事件
        play() {
          this.isPlay = true
          // 清空mv的信息
          this.mvUrl = ''
        },
        // audio的pause事件
        pause() {
          this.isPlay = false
        },
        // 播放mv
        playMv(vid) {
          if (vid) {
            this.showVideo = true;
            // 获取mv信息
            axios.get('https://autumnfish.cn/mv/url?id=' + vid).then(response => {
              // console.log(response)
              // 暂停歌曲播放
              this.$refs.audio.pause()
              // 获取mv地址
              this.mvUrl = response.data.data.url
            })
          }
        },
        // 关闭mv界面
        closeMv() {
          this.showVideo = false
          this.$refs.video.pause()
        },
        // 搜索历史记录中的歌曲
        historySearch(history) {
          this.query = history
          this.searchMusic()
          this.showHistory = false;
        }
      },

  }
</script>
