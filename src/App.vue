<template lang="">
  <div style="align:center">
      <!-- <h1>Summify</h1> -->

      <div class="container p-3 text-center">
        <img src="@/assets/logo.png" class="logo mt-5"/>
        <h3>Summarize videos, time is precious</h3>
        
        <div class="p-4">
          <URLFetcher/>
        </div>
      </div>
  </div>
</template>
<script>
import URLFetcher from '@/components/URLFetcher.vue';
export default {
    components:{
      URLFetcher
    }
}
</script>
<style>

@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap');
@import url("assets/mdb.min.css");
@import url("assets/custom.css");
  
  .logo{
    max-height: 250px;
  }

</style>