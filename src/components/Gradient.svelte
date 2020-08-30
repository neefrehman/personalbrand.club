<style>
    div.gradient-wrapper {
        width: 100vw;
        height: 90vh;
        position: absolute;
        z-index: -5;
        background: linear-gradient(to bottom, #9d59bd 0, transparent 100%);
        animation: GrowNShrink 8s ease infinite;
        will-change: height;
    }

    svg {
        width: 100%;
        height: 100%;
    }

    div.fade-out {
        width: 100%;
        height: 100px;
        position: absolute;
        bottom: 0;
        background: linear-gradient(to bottom, transparent 0, #eaeaea 100%);
    }

    @keyframes GrowNShrink {
        0% {
            height: 80vh;
        }
        50% {
            height: 110vh;
        }
        100% {
            height: 80vh;
        }
    }
</style>

<div class="gradient-wrapper">
    <svg>
        <filter id="noise" x="0" y="0">
            <feTurbulence
                type="fractalNoise"
                baseFrequency="0.8"
                seed="10"
                stitchTiles="stitch">
                <animate
                    attributeName="seed"
                    dur="3s"
                    values="10;0;10"
                    repeatCount="indefinite" />
            </feTurbulence>
        </filter>
        <rect width="100%" height="100%" filter="url(#noise)" opacity="0.33" />
    </svg>
    <div class="fade-out" />
</div>
