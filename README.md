[youtube-iframe-playground](https://dirkarnez.github.io/youtube-iframe-playground)
==================================================================================
[YouTube Player API Reference for iframe Embeds  |  YouTube IFrame Player API  |  Google for Developers](https://developers.google.com/youtube/iframe_api_reference)
```html
<svelte:head>
    <script src="https://www.youtube.com/iframe_api"></script>
</svelte:head>

<script>
	import { onMount } from 'svelte';

	let videoId = '';
	export let value = "";
	export let name = "";
	// let videoTitle = '';
	let isReady = false;

	$: if (isReady === true) {
		if(!!value) {
			const matched = value.match(/.*watch\?v=([^&]+).*/);
			if (Array.isArray(matched) && matched.length == 2) {
				videoId = `${matched[1]}`;
				player.loadVideoById(videoId);
			} else {
				value = "";
				videoId = "";
				player.loadVideoById(videoId);
			}
		} else {
			value = "";
			videoId = "";
			if (!!player) {
				player.loadVideoById(videoId);
			}
		}
	} 
	
	let container;
	let player;

	// export let initialVideoId = '';

	function onPlayerReady() {
		isReady = true;
	}

	function onPlayerStateChange(event) {
		// videoTitle = event.target.getVideoData().title;
		videoId = event.target.getVideoData().video_id;
		value = !!videoId ? `https://www.youtube.com/watch?v=${videoId}` : "";
	}

	onMount(() => {
		debugger;
		function load() {
			player = new window.YT.Player(container, {
				height: '100%',
				width: '100%',
				// videoId: videoId,
				playerVars: { autoplay: 1 },
				events: {
					'onReady': onPlayerReady,
					'onStateChange': onPlayerStateChange
				}
			});
		}

		if (window.YT) {
			load();
		} else {
			window.onYouTubeIframeAPIReady = load;
		}
	});
</script>


<style>
	.iframe-container {
		overflow: hidden;
		padding-top: 56.25%;
		position: relative;
	}
</style>

<div class="column is-three-quarters">
	<input
		class="input"
		type="text"
		name={name}
		bind:value={value}
		placeholder="URL of the video"
		required={true}
	/>
	<slot/>
</div>
<div class="column is-one-quarter">
	<div class="iframe-container">
		<div bind:this={container}/>
	</div>
</div>
```
