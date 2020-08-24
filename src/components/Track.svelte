<script>
    import Moveable from "svelte-moveable";
    let target;
    let frame = {
        matrix: [1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1],
    };

    export let title;
    export let audioFile;
    export let coverImage;
    export let paused = true;

    $: playing = !paused;
</script>

<style>
    :global(.moveable-control-box) {
        opacity: 0.2;
    }

    :global(.moveable-control-box > *) {
        background-color: #1f1f1f !important;
    }

    :global(.moveable-control-box .moveable-line) {
        display: none;
    }

    :global(.moveable-control-box:hover .moveable-line) {
        display: block;
    }

    :global(.moveable-control-box .moveable-control) {
        border: none !important;
        width: 10px !important;
        height: 10px !important;
        margin-top: calc(-5 * var(--zoompx)) !important;
        margin-left: calc(-5 * var(--zoompx)) !important;
    }

    img {
        width: 100%;
    }
</style>

<div bind:this={target} on:click={() => (paused = !paused)}>
    <p>{title}</p>
    <img src={coverImage} alt={title + 'cover image'} />
    <audio bind:paused {title}>
        <source src={audioFile} />
        <track kind="captions" />
    </audio>
</div>
{#if process.browser}
    <Moveable
        {target}
        origin={false}
        warpable={true}
        renderDirections={['nw', 'ne', 'sw', 'se']}
        padding={{ left: 0, top: 0, right: 0, bottom: 0 }}
        on:warpStart={({ detail: { set } }) => {
            set(frame.matrix);
        }}
        on:warp={({ detail: { matrix } }) => {
            frame.matrix = matrix;
            target.style.transform = `matrix3d(${matrix.join(',')})`;
        }} />
{/if}
