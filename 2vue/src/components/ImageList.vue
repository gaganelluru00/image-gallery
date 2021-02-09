<template>
     <h1>Image List Element</h1>

     <div class="page-buttons fixed">
       <button id="prev"  :disabled="pageNumber<=1"   class="btn btn-primary" @click="prev"><b>Previous</b></button>
       
     
        <button id="next" class="btn btn-primary" @click="next"><b>Next</b></button>
     </div>

  <div v-if="errorData==null" class="row">
  <div class="gallery col-md-3" v-for="image in images" :key="image">
    
 <router-link :to="{name:'SelectedImage',params:{id:image.id}}"><img :src=imageSize(image) class="img-thumbnail"/></router-link>
 <br>
 <router-link class="author" :to="{name:'SelectedImage',params:{id:image.id}}"><b>{{image.author}}</b></router-link>
 
 
  </div>
  </div>
  <div class="display" v-else>
    <h1>{{errorData.message}}</h1>
  </div>
</template>
<script>

import {useStore} from 'vuex'
import {computed,watchEffect} from 'vue'
import {GET_IMAGES_DATA, NEXT_PAGE_URL, PREVIOUS_PAGE_URL} from '../store/constants'
export default {
name:"ImageList",
setup(){
  const store=useStore();

  const pageNumber=computed(()=>store.state.pageNumber)

  const pageUrl=computed(()=>store.state.pageUrl);

  const limit=computed(()=>store.state.limit)

  watchEffect(()=>{store.dispatch(GET_IMAGES_DATA,pageUrl)})

  const images=computed(()=>store.state.images)

  const errorData=computed(()=>store.state.error)
 
 
  
 
 
 
 const next=()=>{
  store.commit(NEXT_PAGE_URL)
 }
 const prev=()=>{
  store.commit(PREVIOUS_PAGE_URL)
 }
 
 

  const imageSize=(image)=>{ 
    return image.download_url.substring(0,24)+'/'+image.id+'/300'
    }
  
  return {images,imageSize,pageNumber,errorData,limit,next,prev}
}
}
</script>
<style scoped>
.gallery{
  
  
  padding-left: 80px;
  margin-bottom: 50px;
  

}
.page-buttons{
  padding: 20px;

  
  
 
}
.display{
  text-align: center;
  color: white;
  padding: 50px;
}
.author{
  color: white;
  font-weight: bold;
  text-decoration: none;
  
}
#next{
  right: 10px;
  top:70px;
  float: right;
  width: 100px;
 
}
.limit{
  width: 100px;
  
}
#prev{
 width: 100px;
}
.imageLimit{
  position: absolute;
  left: 45%;
  border-radius: 10px;
}
</style>