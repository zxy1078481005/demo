<template>
  <div class="authorinfo">
    <div class="authorsummary">
      <div class="topbar">作者</div>
      <router-link :to="{
        name:'user_info',
        params:{
          name:userinfo.loginname
        }
      }">
        <img :src="userinfo.avatar_url">
      </router-link>
    </div>
    <div class="recent_topics">
      <div class="topbar">作者最近主题</div>
      <div class="inner">
        <ul>
          <li v-for="list in topicLimitBy5">
            <router-link :to="{
              name:'post_content',
              params:{
                id:list.id,
                name:list.author.loginname
              }
            }">
              {{list.title}}
            </router-link>
          </li>
        </ul>
      </div>
    </div>
    <div class="recent_replies">
      <div class="topbar">作者最近回复</div>
      <div class="inner">
        <ul>
        <li v-for="list in replyLimitBy5">
          <router-link :to="{
              name:'post_content',
              params:{
                id:list.id,
                name:list.author.loginname
              }
            }">
              {{list.title}}
          </router-link>
        </li>
      </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name:"SlideBar",
  data(){
    return {
      userinfo: {}
    }
  },
  computed:{
    topicLimitBy5(){
      if(this.userinfo.recent_topics){
        return this.userinfo.recent_topics.slice(0,5);
      }
    },
    replyLimitBy5(){
      if(this.userinfo.recent_replies){
        return this.userinfo.recent_replies.slice(0,5);
      }
    }
  },
  methods:{
    getData(){
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res=>{
          if(res.data.success == true){
            this.userinfo = res.data.data;
          }
        })
        .catch(err=>{
          console.log(err)
        })
    }
  },
  beforeMount(){
    this.getData();
  }
}
</script>

<style scoped>
.authorsummary, .recent_replies, .recent_topics {
    background-color: #fff;
  }
  .authorinfo {
    width: 328px;
    float: right;
    margin-top: 0;
  }
  li{
    white-space: nowrap;
    max-width: 270px;
    line-height: 30px;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .user_profile{
    padding: 10px;
    font-size: 13px;
  }
  .recent_replies ul, .recent_topics ul {
    margin-top: 0px;
    margin-bottom: 0px;
    list-style: none;
    padding-left: 14px;
  }
  .inner{
    padding: 10px;
  }
  ul a {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    font-size: 12px;
    text-decoration: none;
    color: #778087;
  }
  .topbar{
    padding: 13px;
    background-color: #f6f6f6;
    font-size: 12px;
  }
  .topbar_first{
    margin: auto;
    padding: 13px;
    background-color: #f6f6f6;
    font-size: 12px;
  }
  img {
    height: 48px;
    width: 48px;
    border-radius: 3px;
    margin: 10px;
  }
  .loginname {
    width: 100px;
    float: right;
    margin-top: 22px;
    margin-right: 159px;
    font-size: 14px;
    color: #666;
    text-decoration: none;
  }
  .loginname a {
    text-decoration: none;
    color: #778087;
  }
  .authorsummay .topbar {
    margin-top: 0px;
  }
</style>