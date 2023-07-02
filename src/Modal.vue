<template>
   <div class="black-bg" v-if="모달창열렸니 == true"> 
  <div class="white-bg">
  <img :src="원룸들[누른거].image" class="room-img">
  <h4>{{원룸들[누른거].title}} </h4>
  <p>{{원룸들[누른거].content}}원</p>
  <input v-model.number="month">개월
  <!-- <input type = "range" min="1" max="12"> -->
  <p>{{원룸들[누른거].price*month}}원</p>
  <!-- <button @click="모달창열렸니=false">닫기</button> -->
  <button @click="$emit('closeModal')">닫기</button>

  <Discount/>  
</div>
  
  </div> 
</template>

<script>
export default {
data(){
return {
month:1

}
},
watch :{
 month(a){
if(a >= 13 ){ 
alert('13이상 입력하지 마셈')
 this.month =1 
} else if (isNaN(a) == true ){
  alert('문자를 입력하면 안됩니다.')
  this.month=1 
}
},
},
name:'Modal-page',
props: {
원룸들: Array,
누른거: Number,
모달창열렸니:Boolean, 
}
}
</script>

<style>

</style>