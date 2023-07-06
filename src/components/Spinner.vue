<template>
    <div class="cell">
        <div class="wrapper">
            <div class="spinner spinner4"></div>
        </div>
    </div>
</template>
<script>
export default {

}
</script>
<style scoped>
.cell {
    /* width: 25%; */
}

.cell .wrapper {
    position: relative;
    display: flex;
    justify-content: center;
    height: 10rem;
    margin: 1rem 1rem 0;
    padding-top: 2.2rem;
    background: rgba(255, 255, 255, 0.2);
}

.cell .wrapper::after {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0.2rem;
    text-align: center;
    background: rgba(255, 255, 255, 0.25);
}

.spinner {
    width: 4rem;
    height: 4rem;
}

.spinner.spinner4 {
    background: rgba(0, 0, 0, 0.5);
    -webkit-animation: flip 1.2s ease infinite;
    animation: flip 1.2s ease infinite;
}

@-webkit-keyframes rotation {
    from {
        -webkit-transform: rotate(0deg);
        transform: rotate(0deg);
    }

    to {
        -webkit-transform: rotate(360deg);
        transform: rotate(360deg);
    }
}

@keyframes rotation {
    from {
        -webkit-transform: rotate(0deg);
        transform: rotate(0deg);
    }

    to {
        -webkit-transform: rotate(360deg);
        transform: rotate(360deg);
    }
}

@-webkit-keyframes flip {
    0% {
        -webkit-transform: perspective(120px) rotateX(0deg) rotateY(0deg);
        transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    }

    50% {
        -webkit-transform: perspective(120px) rotateX(-180deg) rotateY(0deg);
        transform: perspective(120px) rotateX(-180deg) rotateY(0deg);
    }

    100% {
        -webkit-transform: perspective(120px) rotateX(-180deg) rotateY(-180deg);
        transform: perspective(120px) rotateX(-180deg) rotateY(-180deg);
    }
}

@keyframes flip {
    0% {
        -webkit-transform: perspective(120px) rotateX(0deg) rotateY(0deg);
        transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    }

    50% {
        -webkit-transform: perspective(120px) rotateX(-180deg) rotateY(0deg);
        transform: perspective(120px) rotateX(-180deg) rotateY(0deg);
    }

    100% {
        -webkit-transform: perspective(120px) rotateX(-180deg) rotateY(-180deg);
        transform: perspective(120px) rotateX(-180deg) rotateY(-180deg);
    }
}

@-webkit-keyframes progress {
    0% {
        left: -130%;
        background: rgba(0, 0, 0, 0.5);
    }

    50% {
        left: 130%;
        background: rgba(0, 0, 0, 0.5);
    }

    51% {
        background: rgba(255, 255, 255, 0.5);
    }

    100% {
        background: rgba(255, 255, 255, 0.5);
    }
}

@keyframes progress {
    0% {
        left: -130%;
        background: rgba(0, 0, 0, 0.5);
    }

    50% {
        left: 130%;
        background: rgba(0, 0, 0, 0.5);
    }

    51% {
        background: rgba(255, 255, 255, 0.5);
    }

    100% {
        background: rgba(255, 255, 255, 0.5);
    }
}

@-webkit-keyframes pulse {
    0% {
        -webkit-transform: translate(-50%, -50%) scale(0);
        transform: translate(-50%, -50%) scale(0);
        opacity: 1;
    }

    100% {
        -webkit-transform: translate(-50%, -50%) scale(1);
        transform: translate(-50%, -50%) scale(1);
        opacity: 0;
    }
}

@keyframes pulse {
    0% {
        -webkit-transform: translate(-50%, -50%) scale(0);
        transform: translate(-50%, -50%) scale(0);
        opacity: 1;
    }

    100% {
        -webkit-transform: translate(-50%, -50%) scale(1);
        transform: translate(-50%, -50%) scale(1);
        opacity: 0;
    }
}

@-webkit-keyframes pulse2 {

    0%,
    100% {
        -webkit-transform: translate(-50%, -50%) scale(0);
        transform: translate(-50%, -50%) scale(0);
        opacity: 1;
    }

    50% {
        -webkit-transform: translate(-50%, -50%) scale(1);
        transform: translate(-50%, -50%) scale(1);
        opacity: 0;
    }
}

@keyframes pulse2 {

    0%,
    100% {
        -webkit-transform: translate(-50%, -50%) scale(0);
        transform: translate(-50%, -50%) scale(0);
        opacity: 1;
    }

    50% {
        -webkit-transform: translate(-50%, -50%) scale(1);
        transform: translate(-50%, -50%) scale(1);
        opacity: 0;
    }
}

@-webkit-keyframes flipWalker {
    0% {
        -webkit-transform: translate(0, 0) rotateX(0) rotateY(0);
        transform: translate(0, 0) rotateX(0) rotateY(0);
    }

    25% {
        -webkit-transform: translate(100%, 0) rotateX(0) rotateY(180deg);
        transform: translate(100%, 0) rotateX(0) rotateY(180deg);
    }

    50% {
        -webkit-transform: translate(100%, 100%) rotateX(-180deg) rotateY(180deg);
        transform: translate(100%, 100%) rotateX(-180deg) rotateY(180deg);
    }

    75% {
        -webkit-transform: translate(0, 100%) rotateX(-180deg) rotateY(360deg);
        transform: translate(0, 100%) rotateX(-180deg) rotateY(360deg);
    }

    100% {
        -webkit-transform: translate(0, 0) rotateX(0) rotateY(360deg);
        transform: translate(0, 0) rotateX(0) rotateY(360deg);
    }
}

@keyframes flipWalker {
    0% {
        -webkit-transform: translate(0, 0) rotateX(0) rotateY(0);
        transform: translate(0, 0) rotateX(0) rotateY(0);
    }

    25% {
        -webkit-transform: translate(100%, 0) rotateX(0) rotateY(180deg);
        transform: translate(100%, 0) rotateX(0) rotateY(180deg);
    }

    50% {
        -webkit-transform: translate(100%, 100%) rotateX(-180deg) rotateY(180deg);
        transform: translate(100%, 100%) rotateX(-180deg) rotateY(180deg);
    }

    75% {
        -webkit-transform: translate(0, 100%) rotateX(-180deg) rotateY(360deg);
        transform: translate(0, 100%) rotateX(-180deg) rotateY(360deg);
    }

    100% {
        -webkit-transform: translate(0, 0) rotateX(0) rotateY(360deg);
        transform: translate(0, 0) rotateX(0) rotateY(360deg);
    }
}</style>