<script context="module">
    const audioElements = new Set();
</script>

<script>
    import { onMount } from "svelte";

    import Moveable from "svelte-moveable";

    // props
    export let title;
    export let audioFile;
    export let coverImage;

    // audio controls
    let audio;
    let paused = true;
    $: rawDuration = (audio && audio.duration) || 0;
    $: minutes = Math.floor(rawDuration / 60);
    $: seconds = Math.floor(rawDuration % 60);
    $: secondsTwoDigits = ("0" + seconds).slice(-2);
    $: duration = `${minutes}:${secondsTwoDigits}`;

    // Add track to module context list
    onMount(() => {
        audioElements.add(audio);
        return () => elements.delete(audio);
    });

    const togglePlaying = () => (paused = !paused);
    const pauseOthers = () => {
        audioElements.forEach(element => {
            if (element !== audio) element.pause();
        });
    };

    // Moveable
    let target;
    let frame = {
        matrix: [1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1],
    };
    let isHovered = false;
    let hoverTimeout;

    // Vanilla needed as Moveable dom elements/style arent dynamically accessible otherwise
    let moveableCorners;
    onMount(() => {
        moveableCorners = document.querySelectorAll(
            ".moveable-control-box .moveable-control"
        );
    });

    $: moveableCorners &&
        moveableCorners.forEach(corner => {
            corner.style.opacity = isHovered ? 0.4 : 0;
        });

    const handleMouseEnter = () => {
        clearTimeout(hoverTimeout);
        hoverTimeout = setTimeout(() => (isHovered = true), 700);
    };
    const handleMouseLeave = () => {
        clearTimeout(hoverTimeout);
        hoverTimeout = setTimeout(() => (isHovered = false), 500);
    };
</script>

<style>
    :global(.moveable-control-box .moveable-line) {
        display: none;
    }

    :global(.moveable-control-box .moveable-control) {
        --dot-size: 7px;
        --half-dot-size: -3.5;
        background-color: #fff !important;
        border: none !important;
        width: var(--dot-size) !important;
        height: var(--dot-size) !important;
        margin-top: calc(var(--half-dot-size) * var(--zoompx)) !important;
        margin-left: calc(var(--half-dot-size) * var(--zoompx)) !important;
        transition: opacity 100ms ease-out;
    }

    :global(.moveable-control-box .moveable-control:hover) {
        opacity: 0.4 !important;
    }

    div {
        grid-column: span 3;
        cursor: pointer;
        transition: filter 300ms ease;
    }

    div.playing {
        filter: brightness(0.8);
    }

    img {
        width: 100%;
    }
</style>

<div
    bind:this={target}
    on:click={togglePlaying}
    on:mouseenter={handleMouseEnter}
    on:mouseleave={handleMouseLeave}
    class:playing={!paused}>
    <p>{title}</p>
    <img src={coverImage} alt={title + ' cover image'} />
    <audio bind:this={audio} bind:paused on:play={pauseOthers} {title}>
        <source src={audioFile} />
        <track kind="captions" />
    </audio>
    <!-- TODO: progress -->
    <p>{duration}</p>
</div>

{#if process.browser}
    <Moveable
        {target}
        origin={false}
        warpable={true}
        renderDirections={['nw', 'ne', 'sw', 'se']}
        padding={{ left: 0, top: 0, right: 0, bottom: 0 }}
        on:warpStart={({ detail: { set } }) => set(frame.matrix)}
        on:warp={({ detail: { matrix } }) => {
            frame.matrix = matrix;
            target.style.transform = `matrix3d(${matrix.join(',')})`;
        }} />
{/if}
