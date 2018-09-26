<template>
  <div>
    <ul>
      <li @click="toDetail(val.id)" v-for="val in datas" :key="val.id" class="items">
        <div class="item-img">
          <img :src="val.img">
        </div>
        <div class="item-info">
          <h4>{{val.nm}}</h4>
          <p >观众评:{{val.sc}}</p>
          <p >{{val.ver}}</p>
          <p>主演:{{val.star}}</p>
          <div class="btn" v-if="val.preSale==0">购票</div>
          <div class="btn1" v-else>预售</div>
          <p class="showInfo">{{val.showInfo}}</p>
        </div>
      </li>
    </ul>
    <div>
      <img src="@/assets/img/loading.gif" alt="" v-show="isLoading">
    </div>
    <div>
      <h3 v-show="isEnd">到底了</h3>
    </div>
  </div>
</template>
<script>
  import store from '@/vuex/store'
  import axios from 'axios'
  export default({
    data(){
      return{
        datas:[],
        isLoading:false,
        isEnd:false
      }
    },
    store,
    methods:{
      getData(){
        axios.get('./static/data/moviedata.json')
          .then( (response)=> {
            this.datas=this.datas.concat(response.data.data.movies);
            this.isLoading=false;
            if(response.data.data.movies.length==0){
              this.isEnd=true;
            }
          })
          .catch( (error)=> {
            console.log(error);
          });
      },
      toDetail(id){
            this.$router.push({name:'MovieDetail',params:{movieId:id}})
          }
    },
    created(){
      this.$store.commit('routerLink',{
        color:'rgb(33,150,243)',
        title:'电影'
      });
      this.getData();
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
  .items{
    display: flex;
    border-bottom: 1px solid #333;
    margin-bottom: .1rem;
    padding-bottom: .1rem;
  }
  .item-img{
    flex-grow: 1;
    width: 0;
  }
  .item-info{
    flex-grow: 2;
    width: 0;
    padding-left:0.1rem ;
  }
  .showInfo{
    margin: 10px;
  }
.btn{
  width: 35px;
  height: 25px;
  background-color: rgba(255,0,0,.8);
  float: right;
  margin-right: 10px;
  color: white;
}
.btn1{
  width: 35px;
  height: 25px;
  background-color: deepskyblue;
  float: right;
  margin-right: 10px;
  color: white;
}
  .one{
   font-size: 17px;
   color: #333;
   font-weight: 700;
   padding-right: 5px;
   flex-shrink: 1;
 }

</style>
