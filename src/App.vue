<template lang="">
  <div style="align:center">
      <!-- <h1>Summify</h1> -->

      <div class="container p-3">
        <div class="text-center">
          <img src="@/assets/logo.png" class="logo mt-5"/>
          <br/>
          <h2 class="mt-4">Summarize videos, time is precious</h2>
        </div>
        
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


@import url('https://fonts.googleapis.com/css2?family=Merriweather&family=Roboto+Mono:ital,wght@0,300;0,400;0,700;1,300&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz@10..48&display=swap');
@import url("assets/mdb.min.css");
@import url("assets/custom.css");

body{
  /* font-family: 'Merriweather', serif; */
font-family: 'Roboto Mono', monospace;
}
  
  .logo{
    max-height: 200px;
  }

  h3, h2, h1{
    font-family: 'Bricolage Grotesque', sans-serif;
  }



  .particle-light {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 50px;
    height: 50px;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 50%;
    animation: particleAnimation 6s linear infinite;
    content: '';
  }

  @keyframes particleAnimation {
    0% {
      transform: scale(1);
      opacity: 1;
    }
    50% {
      transform: scale(0.5);
      opacity: 0.5;
    }
    100% {
      transform: scale(1);
      opacity: 1;
    }
  }
</style>