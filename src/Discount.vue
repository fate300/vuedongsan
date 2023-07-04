<template>
   <div class="discount"> 
<h4>지금 결제하면 {{percent}}% 할인</h4>
    </div>
</template>

<script>
export default {
name:'Discount-page',
 data(){
          return{
            percent: 30, 
          }
        },
mounted(){
setInterval(()=>{
this.percent -= 1;
}, 1000);
},
}

</script>

<style>

</style>