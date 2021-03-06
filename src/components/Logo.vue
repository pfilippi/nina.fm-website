<template>
  <div
    id="logo"
    :class="{ init, animated }"
    @click="toggleAnimation"
    @mouseover="init = false"
  ></div>
</template>

<script>
let cache = new Map()
export default {
  name: 'Logo',
  props: {
    src: { type: String, required: true }
  },
  data() {
    return {
      animated: false,
      init: true
    }
  },
  async mounted() {
    if (!cache.has(this.src)) {
      try {
        cache.set(
          this.src,
          fetch(this.src).then((r) => r.text())
        )
      } catch (e) {
        cache.delete(this.src)
      }
    }
    if (cache.has(this.src)) {
      this.$el.innerHTML = await cache.get(this.src)
    }
  },
  methods: {
    toggleAnimation() {
      this.animated = !this.animated
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~$scss/base.scss';
@keyframes dash {
  from {
    stroke-dashoffset: 1000;
  }
  to {
    stroke-dashoffset: 0;
  }
}
@keyframes dash2 {
  from {
    stroke-dashoffset: 1000;
  }
  to {
    stroke-dashoffset: 0;
  }
}
@keyframes exit {
  to {
    stroke-width: 0;
  }
}
#logo {
  padding-top: 0;
  z-index: 0;
  position: absolute;
  top: 30%;
  left: 50%;
  @include prefix(transform, translate(-50%, -50%));
  @include prefix(transition, $animation);
  @include respond-to(small-height) {
    opacity: 0;
  }
  /deep/ {
    img,
    svg {
      pointer-events: auto;
      transition: all 0.2s ease-out;
      width: 200px;
      @include respond-to(phone) {
        width: 150px;
      }
    }
    $stroke-color: $color-main-bg;
    $main-word-stroke-w: 6.4px;
    $sub-word-stroke-w: 2.5px;
    $delay-before: 1.5s;
    $anim-out: exit 3s 7s linear infinite forwards;
    .line {
      fill: none;
      stroke: $stroke-color;
      stroke-linecap: round;
      stroke-linejoin: round;
      stroke-dasharray: 1000 1000;
      stroke-dashoffset: 0;
      stroke-width: 1px;
      transition: all 3s ease-out, stroke-dasharray 0s, stroke-dashoffset 0s;
    }
    &.init .line {
      stroke-dashoffset: 1000;
      @include prefix(animation, dash 1s 0s linear 1 forwards);
    }
    .letter-n {
      @include prefix(animation-duration, 1.8s !important);
      @include prefix(animation-delay, #{$delay-before + 0ms} !important);
      stroke-width: $main-word-stroke-w;
    }
    .letter-i {
      @include prefix(animation-duration, 2.2s !important);
      @include prefix(animation-delay, #{$delay-before + 400ms} !important);
      stroke-width: $main-word-stroke-w;
    }
    .letter-i-dot {
      @include prefix(animation-duration, 1s !important);
      @include prefix(animation-delay, #{$delay-before + 600ms} !important);
      stroke-width: $main-word-stroke-w * 1.25;
    }
    .letter-n2 {
      @include prefix(animation-duration, 1.8s !important);
      @include prefix(animation-delay, #{$delay-before + 800ms} !important);
      stroke-width: $main-word-stroke-w;
    }
    .letter-a {
      @include prefix(animation-duration, 1.8s !important);
      @include prefix(animation-delay, #{$delay-before + 1200ms} !important);
      stroke-width: $main-word-stroke-w;
    }
    .letter-a-bar {
      @include prefix(animation-duration, 1.5s !important);
      @include prefix(animation-delay, #{$delay-before + 1400ms} !important);
      stroke-width: $main-word-stroke-w;
    }
    .dot-fm {
      @include prefix(animation-duration, 1.8s !important);
      @include prefix(animation-delay, #{$delay-before + 2000ms} !important);
      stroke-width: $sub-word-stroke-w * 1.25;
    }
    .letter-f {
      @include prefix(animation-duration, 1.2s !important);
      @include prefix(animation-delay, #{$delay-before + 2100ms} !important);
      stroke-width: $sub-word-stroke-w;
    }
    .letter-f-bar {
      @include prefix(animation-duration, 1.6s !important);
      @include prefix(animation-delay, #{$delay-before + 2100ms} !important);
      stroke-width: $sub-word-stroke-w;
    }
    .letter-m {
      @include prefix(animation-duration, 1s !important);
      @include prefix(animation-delay, #{$delay-before + 2200ms} !important);
      stroke-width: $sub-word-stroke-w;
    }
    .letter-a-bar,
    .letter-i-dot,
    .sub-word {
      transition: opacity 0.3s;
    }
    &:hover {
      svg {
        opacity: 0.2;
      }
    }
    #app:not(.loading):not(.muted) &:hover {
      .letter-a-bar,
      .letter-i-dot,
      .sub-word {
        opacity: 0;
      }
      .letter-n,
      .letter-i,
      .letter-n2,
      .letter-a {
        transition: stroke-dasharray 0s;
        stroke-dashoffset: 0;
        @include prefix(animation, dash2 5s 0s linear infinite forwards);
        @include prefix(animation-delay, 0s !important);
        stroke-dasharray: 100 100 !important;
      }
    }
  }
  #app.show-posts & {
    left: 25% !important;

    @include respond-to(tablet) {
      top: 150px !important;
      left: 50% !important;
    }
  }
  #app.show-details & {
    left: 75% !important;
  }
}
</style>
