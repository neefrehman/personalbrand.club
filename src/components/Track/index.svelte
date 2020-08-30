<script context="module">
    const audioElements = new Set();
</script>

<script>
    import { onMount } from "svelte";

    import Moveable from "./Moveable.svelte";

    // props
    export let title;
    export let audioFile;
    export let coverImage;

    // audio controls
    let audioElement;
    let paused = true;
    let currentTime = 0;
    let duration;

    const formatTime = seconds => {
        if (isNaN(seconds)) return "...";

        const minutes = Math.floor(seconds / 60);
        let remainingSeconds = Math.floor(seconds % 60);
        if (remainingSeconds < 10) remainingSeconds = "0" + remainingSeconds;

        return `${minutes}:${remainingSeconds}`;
    };

    // Add track to module context list
    onMount(() => {
        audioElements.add(audioElement);
        return () => elements.delete(audioElement);
    });

    const togglePlaying = () => (paused = !paused);
    const pauseOthers = () => {
        audioElements.forEach(element => {
            if (element !== audioElement) element.pause();
        });
    };

    // Moveable interfacing logic
    let target;
</script>

<style>
    div {
        grid-column: span 3;
        cursor: pointer;
        transition: filter 300ms ease;
    }

    div.playing {
        filter: brightness(0.8);
    }

    picture {
        position: relative;
        overflow: hidden;
    }

    img {
        width: 100%;
    }

    progress {
        width: 100%;
        position: absolute;
        bottom: 0;
        left: 0;

        -webkit-appearance: none;
        appearance: none;
        opacity: 0;
        transition: opacity 200ms ease;
    }

    div.playing progress {
        opacity: 1;
    }
</style>

<div bind:this={target} on:click={togglePlaying} class:playing={!paused}>
    <p>{title}</p>
    <picture>
        <img src={coverImage} alt={title + ' cover image'} />
        <progress value={currentTime / duration || 0} />
    </picture>
    <audio
        bind:this={audioElement}
        bind:duration
        bind:currentTime
        bind:paused
        on:play={pauseOthers}
        {title}>
        <source src={audioFile} />
        <track kind="captions" />
    </audio>
    <p>{formatTime(currentTime)} / {formatTime(duration)}</p>
</div>

{#if process.browser}
    <Moveable {target} />
{/if}
