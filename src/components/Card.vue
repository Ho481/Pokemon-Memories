<template>
  <div
    class="card"
    :class="{ diasabled: isDisabled }"
    :style="{
      height: `${(800 - 16 * 4) / Math.sqrt(cardsContext.length) - 8}px`,
      width: `${
        (((800 - 16 * 4) / Math.sqrt(cardsContext.length) - 8) * 3) / 4
      }px`,
    }"
  >
    <div
      class="card_inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard"
    >
      <div class="card_face card_face--frond">
        <div
          class="card_content"
          :style="{
            'background-size': `${
              (((640 - 16 * 4) / Math.sqrt(cardsContext.length) - 8) * 3) /
              4 /
              3
            }px ${
              (((640 - 16 * 4) / Math.sqrt(cardsContext.length) - 8) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card_face card_face--back">
        <div
          class="card_content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: [Array, String, Number, Object],
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onEnableDisabledMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}
.card_inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}
.card.diasabled .card_inner {
  cursor: default;
}
.card_inner.is-flipped {
  transform: rotateY(-180deg);
}
.card_face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card_face--frond .card_content {
  background: url("../assets/image/icon_back.png") no-repeat center center;
  height: 100%;
  width: 100%;
}

.card_face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card_face--back .card_content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}
</style>
