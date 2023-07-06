<template>
    <div class="p-2">
      <div class="progress-bar" :style="progressBarStyle"></div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      expectedSeconds: {
        type: Number,
        required: true
      },
      completed: {
        type: Boolean,
        required: true
      }
    },
    data() {
      return {
        progressBarStyle: {
          width: '0%'
        }
      };
    },
    mounted() {
      this.startProgressBar();
    },
    methods: {
      startProgressBar() {
        const intervalTime = this.expectedSeconds * 1000 / 100; // Calculate interval time for each 1% progress
  
        let progress = 0;
        const progressBarInterval = setInterval(() => {
          progress++;
          this.progressBarStyle.width = `${progress}%`;
  
          if (progress === 100) {
            clearInterval(progressBarInterval);
          }
        }, intervalTime);
      }
    },
    watch: {
      completed(newVal) {
        if (newVal) {
          this.progressBarStyle.width = '100%';
        }
      }
    }
  };
  </script>
  
  <style>
  .progress-bar {
    height: 20px;
    background-color: #ccc;
    transition: width 0.3s;
    border-radius: 30px;
  }
  </style>
  