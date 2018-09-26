<template>
  <div>
    <div class="detail-top">
    <div class="box">
      <div class="dt-left">
        <img :src='detailData.albumImg'>
      </div>
    <div class="right">
      <div class="dt-right">
        {{detailData.nm}}
      </div>
      <div class="dt-right">
        <p>用户评分:{{detailData.sc}}</p>
      </div>
      <div class="dt-right">
        {{detailData.cat}}
      </div>
      <div class="dt-right">
        {{detailData.src}}{{detailData.dur}}
      </div>
    </div>
    </div>
  </div>
   <div class="div-bottom">
    <div class="detail-center">
         <p>导演:{{detailData.dir}}</p>
    </div>
    <br>
    <div class="detail-bottom">
      <p>剧情简介:{{detailData.dra}}</p>
    </div>
  </div>
  </div>
</template>

<script>
  import axios from 'axios'
  export default({
    created(){
      this.getData()
    },
    methods:{
      getData(){
        axios.get(`${API_PROXY}http://m.maoyan.com/ajax/detailmovie?movieId=${this.$route.params.movieId}`).then((res)=>{
          this.detailData=res.data.detailMovie
          this.isLoading=false;
           if(res.data.detailMovie.length==0){
                this.isEnd=true;
           }
        }).catch((res)=>{
          console.log('error')
        })
      }
    },
    data(){
      return {
        detailData:{
        },
        isLoading:false,
        isEnd:false
      }
    },
    mounted(){
          window.onscroll = ()=> {
            let scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
            let clientHeight = document.documentElement.clientHeight;
            let scrollHeight = document.documentElement.scrollHeight;
            if(scrollTop+clientHeight == scrollHeight && !this.isEnd){
              this.isLoading = true;
              setTimeout(()=>{
                  this.isLoading = false;
                  this.isEnd = true;
              },1000);
            }
          }
        }
  })
</script>

<style scoped>
  .box{
    display: flex;
    flex-wrap: nowrap
  }
  .dt-left{
    margin: 5px 10px;
    width: 200px;
    height: 300px;
  }
  .right{
    margin-top: 40px;
    flex-grow: 2
  }
  .dt-right {
    font-size: 16px;
  }
  .div-bottom{
    margin-top: -80px;
  }
  .detail-bottom{
    font-size: 16px;
    margin-top: -10px;
  }
</style>
