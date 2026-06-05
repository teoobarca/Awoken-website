<template>
    <div class="game-wrapper">
        <div class="game-stage" ref="stage">
            <iframe
                class="game-frame"
                ref="frame"
                src="/game/index.html"
                title="Awoken"
                allow="fullscreen; autoplay; gamepad"
                allowfullscreen
            ></iframe>
        </div>
        <button class="red-button fs-button" type="button" @click="toggleFullscreen">
            Fullscreen (F)
        </button>
    </div>
</template>

<script>
export default {
    mounted() {
        window.addEventListener('keydown', this.onKey)
    },
    beforeUnmount() {
        window.removeEventListener('keydown', this.onKey)
    },
    methods: {
        onKey(e) {
            // Ignore if focus is inside an input/textarea
            const tag = (e.target && e.target.tagName) || ''
            if (tag === 'INPUT' || tag === 'TEXTAREA') return
            if (e.key === 'f' || e.key === 'F') {
                e.preventDefault()
                this.toggleFullscreen()
            }
        },
        toggleFullscreen() {
            const el = this.$refs.stage
            if (!document.fullscreenElement) {
                (el.requestFullscreen || el.webkitRequestFullscreen || el.msRequestFullscreen).call(el)
            } else {
                (document.exitFullscreen || document.webkitExitFullscreen || document.msExitFullscreen).call(document)
            }
        }
    }
}
</script>

<style scoped>
    .game-wrapper {
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 20px;
    }

    .game-stage {
        width: 90%;
        max-width: 1280px;
        aspect-ratio: 16 / 9;
        background: #231F20;
        box-shadow: 0 10px 40px -10px rgba(0, 0, 0, 0.6);
    }

    /* When the stage is fullscreen, fill the whole screen */
    .game-stage:fullscreen {
        width: 100vw;
        height: 100vh;
        max-width: none;
        aspect-ratio: auto;
    }

    .game-frame {
        width: 100%;
        height: 100%;
        border: none;
        display: block;
    }

    .fs-button {
        font-size: 18px;
        padding: 12px 22px;
    }

    @media screen and (max-width: 600px) {
        .game-stage {
            width: 100%;
        }
    }
</style>
