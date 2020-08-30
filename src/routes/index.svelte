<script>
    import GridWrapper from "../components/GridWrapper.svelte";
    import Header from "../components/Header.svelte";
    import Track from "../components/Track/index.svelte";

    import trackData from "../data/tracks.json";

    const tracks = trackData.tracks;
</script>

<style>
    img {
        width: 100%;
    }
</style>

<Header />

<main>
    <GridWrapper>
        <p>dj</p>
        <p>+</p>
        <p>producer</p>
    </GridWrapper>

    <p>tracks (click them)</p>

    <GridWrapper>
        {#each tracks as { title, file, image }}
            <Track {title} audioFile={file} coverImage={image} />
        {/each}
    </GridWrapper>
</main>

<footer>
    <GridWrapper>
        <p>footer</p>
        <p>a short bio?</p>
        <p>link to souncloud socials etc?</p>
    </GridWrapper>

    <img src="/assets/club.jpeg" alt="personalbrad DJing at Final Taipei" />
</footer>
