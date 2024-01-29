<!-- src/components/EyeTracking.vue -->
<template>
    <div>
      <h1>Web Page Analysis with Eye Tracking</h1>
      <div ref="eyeTrackingContainer"></div>
    </div>
  </template>
  
  <script>
  import webgazer from 'webgazer';
  
  export default {
    mounted() {
      this.initializeWebGazer();
    },
    methods: {
      initializeWebGazer() {
        webgazer
          .setGazeListener((data, elapsedTime) => {
            if (data && data.x !== null && data.y !== null) {
              // Handle gaze data (e.g., send it to your server for analysis)
              console.log('Gaze Data:', data);
              console.log('Elapsed Time:', elapsedTime)
            }
          })
          .begin()
          .showPredictionPoints(true);
      },
    },
  };
  </script>
  
  <style scoped>
  /* Add your styles here */
  </style>
  