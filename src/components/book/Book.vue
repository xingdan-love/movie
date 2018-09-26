<template>
  <div class="book">
    <swipe class="my-swipe">
      <swipe-item class="slide1"></swipe-item>
      <swipe-item class="slide2"></swipe-item>
      <swipe-item class="slide3"></swipe-item>
    </swipe>
    <ul class="book_img">
      <li v-for="book in bookData">
          <img :src='book.src'>
          <p class="p_font">{{book.title}}</p>
      </li>
    </ul>
  </div>
</template>
<script>
  require('vue-swipe/dist/vue-swipe.css');
  import { Swipe, SwipeItem } from 'vue-swipe';
  import store from '@/vuex/store'
  import axios from 'axios'
  export default({
     data(){
          return{
            bookData:[]
          }
        },
    store,
    components: {
      'swipe': Swipe,
      'swipe-item': SwipeItem
    },
    created(){
      this.$store.commit('routerLink',{
        color:'rgb(121,85,72)',
        title:'书籍'
      }),
      this.getData()
    },
    methods:{
      getData(){
         axios.get(`./static/data/bookdata.json`).then((res)=>{
              this.bookData=res.data.bookdatas;
              this.isLoading=false;
               if(res.data.bookdatas.length==0){
                    this.isEnd=true;
               }
            }).catch((res)=>{
              console.log('error')
            })
          }
        }
  })
</script>
<style scoped>
  .my-swipe {
    height: 200px;
    color: #fff;
    font-size: 30px;
    text-align: center;
  }

  .slide1 {
    background-image: url("../../../static/img/shuji3.jpg");
    color: #fff;
  }

  .slide2 {
    background-image: url('../../../static/img/shuji1.jpg');
    color: #000;
  }

  .slide3 {
    background-image: url("../../../static/img/shuji2.jpg");
    color: #fff;
  }
  .book{
    margin-bottom: 10px;
  }
   ul{
      display: flex;
      flex-wrap: wrap;
    }
    ul li {
      width: 50%;
    }
    ul li img{
      width: 80%;
      padding: 5px 5px;
    }
    .p_font{
      color: gray;
    }
</style>
