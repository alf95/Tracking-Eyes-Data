<!-- eslint-disable no-unused-vars -->
<!-- src/components/EyeTracking.vue -->
<template>
    <div>
      <h1>Web Page Analysis with Eye Tracking</h1>
      <div ref="eyeTrackingContainer"></div>
    </div>
  </template>
  
  <script>
  import { v4 as uuidv4 } from 'uuid';

  var eyePageData = {
    'name_page':'test_vue',
    'elements': {
      'titles':{

      },
      'paragraphs':{

      },
      'links':{

      },
      'images':{

      },
      'videos':{

      }
    }

  };

  export default {
    mounted() {
      this.initializeWebGazer();
    },
    methods: {
      initializeWebGazer() {
        //webgazer.showPredictionPoints(true);
        webgazer.setRegression("ridge");
        webgazer
          .setGazeListener((data, elapsedTime) => {
            if (data && data.x !== null && data.y !== null) {
              // Handle gaze data (e.g., send it to your server for analysis)
              console.log('Gaze Data:', data);
              console.log('Elapsed Time:', elapsedTime)

              var elements = getElementsByPosition(data.x, data.y);
              for (let i = 0; i < elements.length; i++) {
                  const tagName = elements[i].tagName.toLowerCase();
                  let keyList = "";
                  if(tagName === "p"){
                    keyList = 'paragraphs';
                  }else if(tagName === "a"){
                    keyList = 'links';
                  }else if(tagName === "img"){
                    keyList = 'images';
                  }else if(tagName === "video"){
                    keyList = 'videos';
                  }else if(/^h[1-6]$/.test(tagName)){
                    keyList = 'titles';
                  }

                  if(keyList){
                    let currentElement = elements[i];  
                    currentElement.classList.add("highlighted");
                    let uniqueId = currentElement.getAttribute("data-eyeTracking-id");
                    if(uniqueId != null){
                      uniqueId = uuidv4();
                    }
                    if(eyePageData.elements[keyList].hasOwnProperty(uniqueId)){
                        //exist
                    }else{
                        //new
                        
                    }
                  }

                  //if (tagName === "p" || /^h[1-6]$/.test(tagName) || tagName === "a" || tagName === "img" || tagName === "video") {}
                  
              }
              //console.log("Elements at mouse position:", elementsAtMousePosition);

              //per creare il json si possono generare delle classi univoche e utlizzarle come chiavi del json
            }
          })
          .begin()
      },
    },
  };

  function findNearestElement(x, y) {
      var elements = document.elementsFromPoint(x, y);
      var nearestElement = null;
      var minDistance = Number.MAX_VALUE;
      
      elements.forEach(function(element) {
          var rect = element.getBoundingClientRect();
          var distance = Math.sqrt(Math.pow(x - rect.left - rect.width / 2, 2) + Math.pow(y - rect.top - rect.height / 2, 2));
          if (distance < minDistance) {
              minDistance = distance;
              nearestElement = element;
          }
      });
      
      return nearestElement;
  }


  function getElementsByPosition(x, y) {
    var elements = document.elementsFromPoint(x, y);
    return elements;
  }
  </script>
  
  <style scoped>
  /* Add your styles here */
  </style>
  