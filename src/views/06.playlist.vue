<template>
  <div class="playlist-container">
    <div class="top-wrap">
      <div class="img-wrap">
        <img :src="playlist.coverImgUrl" alt="" />
      </div>
      <div class="info-wrap">
        <p class="title">{{ playlist.name }}</p>
        <div class="author-wrap">
          <img class="avatar" :src="playlist.creator.avatarUrl" alt="" />
          <span class="name">{{playlist.creator.nickname}}</span>
          <span class="time">{{playlist.createTime}} 创建</span>
        </div>
        <div class="play-wrap">
          <span class="iconfont icon-circle-play"></span>
          <span class="text">播放全部</span>
        </div>
        <div class="tag-wrap">
          <span class="title">标签:</span>
          <ul>
            <li v-for="(item,index) in playList.tags" :key="index">{{iteam}}</li>
          </ul>
        </div>
        <div class="desc-wrap">
          <span class="title">简介:</span>
          <span class="desc">
            {{ playlist.description }}
            </span
          >
        </div>
      </div>
    </div>
    <el-tabs v-model="activeIndex">
      <el-tab-pane label="歌曲列表" name="1">
        <table class="el-table playlit-table">
          <thead>
            <th></th>
            <th></th>
            <th>音乐标题</th>
            <th>歌手</th>
            <th>专辑</th>
            <th>时长</th>
          </thead>
          <tbody>
            <tr class="el-table__row">
              <td>1</td>
              <td>
                <div class="img-wrap">
                  <img src="../assets/songCover.jpg" alt="" />
                  <span class="iconfont icon-play"></span>
                </div>
              </td>
              <td>
                <div class="song-wrap">
                  <div class="name-wrap">
                    <span>你要相信这不是最后一天</span>
                    <span class="iconfont icon-mv"></span>
                  </div>
                  <span>电视剧加油练习生插曲</span>
                </div>
              </td>
              <td>华晨宇</td>
              <td>你要相信这不是最后一天</td>
              <td>06:03</td>
            </tr>
            <tr class="el-table__row">
              <td>2</td>
               <td>
                <div class="img-wrap">
                  <img src="../assets/songCover.jpg" alt="" />
                  <span class="iconfont icon-play"></span>
                </div>
              </td>
              <td>
                <div class="song-wrap">
                  <div class="name-wrap">
                    <span>你要相信这不是最后一天</span>
                    <span class="iconfont icon-mv"></span>
                  </div>
                </div>
              </td>
              <td>华晨宇</td>
              <td>你要相信这不是最后一天</td>
              <td>06:03</td>
            </tr>
          </tbody>
        </table>
      </el-tab-pane>
      <el-tab-pane label="评论(66)" name="2">
        <!-- 精彩评论 -->
        <div class="comment-wrap">
          <p class="title">精彩评论<span class="number">({{hotCount}})</span></p>
          <div class="comments-wrap">
            <div v-for="(item,index) in hotComment" :key="index" class="item">
              <div class="icon-wrap">
                <img :src="item.user.avatarUrl" alt="" />
              </div>
              <div class="content-wrap">
                <div class="content">
                  <span class="name">{{item.user.nickname}}</span>
                  <span class="comment">{{item.content}}</span>
                </div>
                <div class="re-content" v-if="item.beReplied.length!=0">
                  <span class="name">{{item.beReplied[0].user.nickname}}</span>
                  <span class="comment">{{item.beReplied[0].content}}</span>
                </div>
                <div class="date">2020-02-12 17:26:11</div>
              </div>
            </div>
          </div>
        </div>
        <!-- 最新评论 -->
        <div class="comment-wrap">
          <p class="title">最新评论<span class="number">({{total}})</span></p>
          <div class="comments-wrap">
            <div class="item" v-for="(item,index) in comments " :key="index">
              <div class="icon-wrap">
                <img :src="item.user.avatarUrl" alt="" />
              </div>
              <div class="content-wrap">
                <div class="content">
                  <span class="name">{{item.user.nickname}}</span>
                  <span class="comment">{{item.content}}</span>
                </div>
                <div class="re-content" v-if="item.beReplied.length!=0">
                  <span class="name">{{ item.beReplied[0].user.nickname }}</span>
                  <span class="comment">{{ item.beReplied[0].content }}</span>
                </div>
                <div class="date">2020-02-12 17:26:11</div>
              </div>
            </div>
          </div>
        </div>
        <!-- 分页器 -->
        <el-pagination
          @current-change="handleCurrentChange"
          background
          layout="prev, pager, next"
          :total="total"
          :current-page="page"
          :page-size="5"
        >
        </el-pagination>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'playlist',
  data() {
    return {
      activeIndex: '1',
      // 总条数
      total: 0,
      // 页码
      page: 1,
      playlist:{},
      hotComment:[],
      hotCount:0,
      comments:[]
    };
  },
  created(){
     axios({
          url:'https://autumnfish.cn/playlist/detail',
          method:'get',
          params:{
            id:this.$route.query.q
          }
      }).then(res=>{
        this.playlist = res.data.playlist
      })
     axios({
          url:'https://autumnfish.cn/comment/hot',
          method:'get',
          params:{
            id:this.$route.query.q,
            type:2
          }
      }).then(res=>{
        this.hotComment = res.data.hotComments
        this.hotCount = res.data.total
      })
     axios({
          url:'https://autumnfish.cn/comment/playlist',
          method:'get',
          params:{
            id:this.$route.query.q,
            limit:10,
            offset:(this.page-1)*10
          }
      }).then(res=>{
        this.total = res.data.total
        this.comments = res.data.comments
      })
  },
  methods: {
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    }
  }
};
</script>

<style >

</style>
