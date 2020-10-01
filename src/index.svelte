<script context="module">
  let YouTubeIframeAPIReady = false;
</script>

<script>
  let player;
  import { onMount } from "svelte";
  let divId = "player_" + parseInt(Math.random() * 100000).toString();
  export let videoId;
  export let height = "390";
  export let width = "640";

  onMount(() => {
    let ytTagUrl = "https://www.youtube.com/iframe_api";
    if (!isMyScriptLoaded(ytTagUrl)) {
      // 2. This code loads the IFrame Player API code asynchronously.
      var tag = document.createElement("script");
      tag.src = ytTagUrl;
      var firstScriptTag = document.getElementsByTagName("script")[0];
      firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
    }

    window.onYouTubeIframeAPIReady = function() {
      //console.log('hello')
      window.dispatchEvent(new Event("YouTubeIframeAPIReady"));
    };

    window.addEventListener("YouTubeIframeAPIReady", function() {
      if (YouTubeIframeAPIReady == false) {
        // first load of an YT Video on this project
        YouTubeIframeAPIReady = true; // now the Player can be created
        createPlayer();
      }
    });
    function createPlayer() {
      player = new YT.Player(divId, {
        height,
        width,
        videoId: videoId,
        events: {
          //'onReady': onPlayerReady,
          onStateChange: onPlayerStateChange
        }
      });
    }
    if (YouTubeIframeAPIReady) {
      createPlayer(); // if the YT Script is ready, we can create our player
    }
  });

  function isMyScriptLoaded(url = "") {
    var scripts = document.getElementsByTagName("script");
    for (var i = scripts.length; i--; ) {
      if (scripts[i].src == url) return true;
    }
    return false;
  }

  export function position() { player.getCurrentTime() }
  export function play() { player.playVideo() }
  export function jumpTo(seconds) { player.seekTo(seconds) }
  export function pause() { player.pauseVideo() }
  export function paused() { return [5, 2, -1].includes(player.getPlayerState()) }

</script>

<span class="yt-component" style="float: left">
  <div id={divId} />
</span>
