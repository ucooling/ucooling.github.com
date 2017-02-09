---
layout: post
title: "brightcove"
date: 2015-09-23 21:20:52 +0800
comments: true
categories: 
---
## What is brightcove

  
  Brightcove revolutionizes the way organizations deliver video experiences. it change the video transmission ways. It is different the YouTube video, YouTube focus on the video of users homemade. The brightcove hope build a chance, there is everyone can build the video, meantime share the video and get revenue. Brightcove is position itself as an online video paltform, the main work is to provide video and other digital media and publish paltform to manufacturers of video content, totally that is cloud video
    
 ## How to use brightcove
 
 you need to sign up a account of brightcove
 * you can upload a video to media, chose the video that is updated, and click publish button and publish your video
 
 * you also can create a player, when you pushlish your video, you can chose   
   you player
   
 * you also create a playlist, and add some video to the playlist, besides you can publish the playlist.
 
 ## Demo
 
 * publish a video
 
 Use iframe tag
 
   ```
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Dynamically Load Player</title>
</head>
<body>
  <iframe src='http://players.brightcove.net/4446402108001/ee7a42d1-6d38-46f1-a79e-738e08b6ef36_default/index.html?videoId=4446806003001' allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe>
</body>
</html>  
   ```
  
  Use video tag
  
  ```
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Dynamically Load Player</title>
</head>
<body>
  <video
  data-video-id="4448133659001"
  data-account="4446402108001"
  data-player="default"
  data-embed="default"
  class="video-js" controls></video>

<script src="http://players.brightcove.net/1719543778001/vttjs/dist/vtt.min.js"></script>
<script src="http://players.brightcove.net/4446402108001/default_default/index.min.js"></script>

</body>
</html>
```

* publish a playlist
 
 ```
 <html>
  <head>
    <title>Media API Sample</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <!-- Javascript Media API wrapper from http://docs.brightcove.com/en/video-cloud/open-source/index.html -->
    <script type="text/javascript" src="http://files.brightcove.com/bc-mapi.js"></script>
  </head>
  <body>
    <h1>Media API Sample</h1>
    <!-- Start of Brightcove Player -->
    <script language="JavaScript" type="text/javascript" src="http://admin.brightcove.com/js/BrightcoveExperiences.js"></script>
    <object id="myExperience" class="BrightcoveExperience">
      <param name="bgcolor" value="#FFFFFF" />
      <param name="width" value="480" />
      <param name="height" value="270" />
      <param name="playerID" value="921267190001" />
      <param name="playerKey" value="AQ~~,AAAA1oy1bvE~,ALl2ezBj3WG3MLvDx9F9zkV06cNK00ey" />
      <param name="isVid" value="true" />
      <param name="isUI" value="true" />
      <param name="dynamicStreaming" value="true" />
      <!-- params for Universal Player API -->
      <param name="includeAPI" value="true" />
      <param name="templateReadyHandler" value="BCL.onTemplateReady" />
    </object>
    <script type="text/javascript">brightcove.createExperiences();</script>
    <!-- End of Brightcove Player -->
    <fieldset>
      <legend>Videos</legend>
      <div id="results"></div>
    </fieldset><br>
    <!-- This is the script to modify for the exercise -->
    <script type="text/javascript">
      // BCL Media API search maker -- adapted from JS-MAPI on http://docs.brightcove.com/en/video-cloud/open-source/index.html
      // namespace to keep all the "global" vars together
      var BCL = {};
      // placeholder - params for API call
      BCL.params = {};
      // Media API read token
      BCMAPI.token = "WDGO_XdKqXVJRVGtrNuGLxCYDNoR-SvA5yUqX2eE6KjgefOxRzQilw..";
      // set the callback for Media API calls
      BCMAPI.callback = "BCL.onSearchResponse";
      // set the filter
      BCL.params.any = "tag:nature";
      BCMAPI.search(BCL.params);
      BCL.onSearchResponse = function(jsonData) {
        var str = "";
        for (var index in jsonData.items) {
          str += "<a onclick=\"BCL.playVideo(" + jsonData.items[index].id + ")\" style=\"cursor:pointer\"><img src=\"" + jsonData.items[index].thumbnailURL + "\"/><br/><small>" + jsonData.items[index].name + "</<small></a><hr/>";
        }
        document.getElementById("results").innerHTML = str;
      }
      // Player API scripting
      // event listener for the player being ready
      BCL.onTemplateReady = function (event) {
        BCL.player = brightcove.api.getExperience("myExperience");
        // get a reference to the video player
        BCL.videoPlayer = BCL.player.getModule(brightcove.api.modules.APIModules.VIDEO_PLAYER);
      }
      // play video function
      BCL.playVideo = function(videoID) {
        BCL.videoPlayer.loadVideoByID(videoID);
      }
    </script>
  </body>
</html>
 ```
 
 ## reference material
 
 * http://docs.brightcove.com/en/index.html
 * https://studio.brightcove.com/products/videocloud/home
 * https://docs.brightcove.com/en/video-cloud/cms-api/getting-started/overview-cms.html#generalInfo
 
   
 
    

