<script>
    import { onMount } from "svelte";
    import Gradient from "../components/Gradient.svelte";

    const globeEmojis = ["🌍", "🌎", "🌏"];
    let globeEmojiIndex = 0;
    $: currentGlobeEmoji = globeEmojis[globeEmojiIndex];

    const rotateGlobeEmoji = () => {
        if (document.hasFocus()) {
            globeEmojiIndex = globeEmojiIndex < 2 ? globeEmojiIndex + 1 : 0;
        }
    };

    onMount(() => {
        setInterval(rotateGlobeEmoji, 800);
        return () => clearInterval(rotateGlobeEmoji);
    });
</script>

<style lang="scss">
    :global(body) {
        margin: 0;
        position: relative;
        background-color: #eaeaea;
        color: #1f1f1f;
        font-size: 20px;
    }

    :global(:focus) {
        outline: none;
        box-shadow: 0 0 0 3px rgba(21, 156, 228, 0.4);
        transition: box-shadow 120ms ease;

        &:not(.focus-visible) {
            outline: none; // Polyfilled
            box-shadow: none;
        }
        &:not(:focus-visible) {
            outline: none; // Broken when used with the above selector
            box-shadow: none;
        }
    }

    div {
        max-width: 1440px;
        margin: 0 auto;
    }

    @media (max-width: 1480px) {
        div {
            margin: 0 20px;
        }
    }
</style>

<svelte:head>
    <link
        rel="icon"
        href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22
        viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>{currentGlobeEmoji}</text></svg>" />
    <title>personalbrand.club</title>
</svelte:head>

<Gradient />

<div>
    <slot />
</div>

<div class="moveable-container" />
