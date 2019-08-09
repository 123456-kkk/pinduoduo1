<template>
    <div class="wrap">
        <div class="main" ref="main">
            <shopList :shopList="recommendedList"></shopList>
            <div class="top" :style="{display:isShow?'block':'none'}" @click="Click">TOP</div>
      </div> 
       <Footer></Footer>
    </div>
</template>

<script>
import {mapState,mapActions} from 'vuex'
import shopList from './shopList'
export default {
    data(){
        return{
              isShow:false
        }
    },
    computed:{
        ...mapState(['recommendedList'])
    },
    methods:{
        ...mapActions(['_getrecommendedList']),
        Scroll(e){
   
      if(e.target.scrollTop>300){
        this.isShow=true
      }else{
        this.isShow=false
      }
    },
    Click(){
       this.$refs.main.scrollTop=0
    }
    },
    mounted(){
        this._getrecommendedList()
         this.$refs.main.addEventListener('scroll',this.Scroll,false)
    },
    components:{
      shopList
    }
}
</script>

<style>

</style>
