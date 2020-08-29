<script>
    // import { each } from "svelte/internal";

    import GridWrapper from "../components/GridWrapper.svelte";
    import Header from "../components/Header.svelte";
    import Track from "../components/Track.svelte";
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

    <p>tracks</p>

    <GridWrapper>
        <Track
            title="track 1 (click me)"
            audioFile="uploads/PinkPanther60.wav"
            coverImage="uploads/Digifritids_Illustrations.png" />
    </GridWrapper>
</main>

<footer>
    <GridWrapper>
        <p>footer</p>
        <p>a short bio?</p>
        <p>link to souncloud socials etc?</p>
    </GridWrapper>

    <!-- {each } -->

    <img src="/assets/club.jpeg" alt="personalbrad DJing at Final Taipei" />
</footer>
