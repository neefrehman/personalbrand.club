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
        if (isNaN(seconds)) return "???";

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

    const handleKeyDown = e => {
        if (e.key === " " || e.key === "Enter") {
            e.preventDefault();
            togglePlaying();
        }
    };

    // Moveable interfacing logic
    let target;
</script>

<style lang="scss">
    div.container {
        grid-column: span 3;
        transition: filter 300ms ease;

        &.playing {
            filter: brightness(0.8);
        }
    }

    div.main {
        cursor: pointer;

        picture {
            position: relative;
            overflow: hidden;

            img {
                width: 100%;
            }

            progress {
                width: 100%;
                height: 12px;
                position: absolute;
                bottom: 0;
                left: 0;

                appearance: none;
                opacity: 0;
                transition: opacity 200ms ease;

                &::-webkit-progress-bar {
                    background-color: transparent;
                }

                &::-webkit-progress-value {
                    background-color: #eee;
                }

                &:not([value="0"]) {
                    opacity: 1;
                }
            }
        }
    }

    div.info {
        margin-top: 0.4em;
        display: flex;
        flex-direction: row;
        justify-content: space-between;

        p {
            margin: 0;
        }
    }
</style>

<div class="container" bind:this={target} class:playing={!paused}>
    <div
        class="main"
        tabindex="0"
        role="button"
        aria-pressed={!paused}
        on:click={togglePlaying}
        on:keydown={handleKeyDown}>
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
    </div>
    <div class="info">
        <p>{formatTime(currentTime)} / {formatTime(duration)}</p>
        <a href={audioFile} download role="button">download</a>
    </div>
</div>

{#if process.browser}
    <Moveable {target} />
{/if}
