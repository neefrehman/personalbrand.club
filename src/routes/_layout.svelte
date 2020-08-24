<script>
    import { onMount } from "svelte";

    const globeEmojis = ["🌍", "🌎", "🌏"];
    let globeEmojiIndex = 0;
    let currentGlobeEmoji = globeEmojis[globeEmojiIndex];

    const rotateGlobeEmoji = () => {
        if (document.hasFocus()) {
            globeEmojiIndex = globeEmojiIndex < 2 ? globeEmojiIndex + 1 : 0;
            currentGlobeEmoji = globeEmojis[globeEmojiIndex];
        }
    };

    onMount(() => {
        setInterval(rotateGlobeEmoji, 800);
        return () => clearInterval(rotateGlobeEmoji);
    });
</script>

<style>
    :global(body) {
        background-color: #f1f1f1;
        color: #101010;
        max-width: 1180px;
        padding: 2em;
        margin: 0 auto;
    }
</style>

<svelte:head>
    <link
        rel="icon"
        href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22
        viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>{currentGlobeEmoji}</text></svg>" />
    <title>personalbrand.club</title>
</svelte:head>

<slot />
