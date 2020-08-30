<script>
    import Moveable from "svelte-moveable";

    export let target;

    let frame = {
        matrix: [1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1],
    };
</script>

<style>
    :global(.moveable-control-box .moveable-line) {
        display: none;
    }

    :global(.moveable-control-box .moveable-control) {
        --dot-size: 36px;
        --half-dot-size: -18;
        border: none !important;
        background-color: transparent !important;
        width: var(--dot-size) !important;
        height: var(--dot-size) !important;
        margin-top: calc(var(--half-dot-size) * var(--zoompx)) !important;
        margin-left: calc(var(--half-dot-size) * var(--zoompx)) !important;
    }

    /* pseudoelement used to ensure larger hitbox than visible element */
    :global(.moveable-control-box .moveable-control::after) {
        content: "";
        position: absolute;
        left: 50%;
        top: 50%;
        z-index: 10;
        will-change: transform;
        border-radius: 50%;
        background-color: #ffffff;
        opacity: 1;
        width: 15px;
        height: 15px;
        transform: translate(-50%, -50%) scale(0);
        transition: transform 400ms cubic-bezier(1, 0.1, 0.1, 1);
    }

    :global(.moveable-control-box:hover .moveable-control::after) {
        transform: translate(-50%, -50%);
    }
</style>

<Moveable
    {target}
    container={document.querySelector('.moveable-container')}
    origin={false}
    warpable={true}
    renderDirections={['nw', 'ne', 'sw', 'se']}
    padding={{ left: 0, top: 0, right: 0, bottom: 0 }}
    on:warpStart={({ detail: { set } }) => set(frame.matrix)}
    on:warp={({ detail: { matrix } }) => {
        frame.matrix = matrix;
        target.style.transform = `matrix3d(${matrix.join(',')})`;
    }} />
