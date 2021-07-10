<template>
  <section class="wrapper">
    <button
      class="wrapper_btn"
      :class="{ active: isOpen }"
      @click="isOpen = !isOpen"
    >
      Налоговый вычет
    </button>
    <transition name="open">
      <v-popup-forms v-if="isOpen" @close="close">
        <template #header>
          <h1 class="title-text">Налоговый вычет</h1>
        </template>
        <template #description>
          <p class="description-text">
            Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер
            налогового вычета составляет не более 13% от своего официального
            годового дохода.
          </p>
        </template>
        <template #input>
          <v-calculator />
        </template>
      </v-popup-forms>
    </transition>
  </section>
</template>

<script>
import VPopupForms from "@/components/ThePopup";
import VCalculator from "@/components/TheCalculator";
export default {
  name: "Home",

  components: {
    VPopupForms,
    VCalculator,
  },

  data() {
    return {
      isOpen: false,
      isOption: false,
    };
  },

  methods: {
    close() {
      this.isOpen = false;
    },
  },
};
</script>


<style lang="scss" scoped>
.wrapper {
  width: 100%;
  min-height: 700px;
  height: 100vh;

  background-color: #ff5e56;
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    linear-gradient(0deg, #ff5e56, #ff5e56);

  display: flex;
  align-items: center;

  position: relative;

  &_btn {
    width: 211px;
    height: 56px;

    display: block;
    margin: 0 auto;

    background-color: transparent;
    border: #ffffff solid 1px;
    border-radius: 6px;
    color: white;

    font-size: 16px;
    font-style: normal;
    font-weight: 500;
    line-height: 24px;
    letter-spacing: 0em;
    text-align: center;

    transition: all 0.5s;
  }
  &_btn:hover {
    background-color: #ffffff;
    color: black;
  }
  &_btn:disabled {
    background-color: #bec5cc;
    color: white;
  }
  &_btn.active {
    background-color: #ffffff;
    color: black;
  }
}
.title-text {
  font-size: 28px;
  font-style: normal;
  font-weight: 500;
  line-height: 40px;
  letter-spacing: 0em;
  text-align: left;
  color: black;

  margin-top: 20px;
}
.description-text {
  font-size: 12px;
  font-style: normal;
  font-weight: 400;
  line-height: 16px;
  letter-spacing: 0em;
  text-align: left;
}

@media (max-width: 350px) {
  .title-text {
    font-size: 18px;
    font-style: normal;
    font-weight: 500;
    line-height: 24px;
    letter-spacing: 0em;
    text-align: left;
  }
  .description-text {
    font-size: 12px;
    font-style: normal;
    font-weight: 400;
    line-height: 16px;
    letter-spacing: 0em;
    text-align: left;
  }
  .wrapper {
    min-height: 100vh;
  }
  .wrapper_btn {
    width: 211px;
    height: 56px;
    
    font-size: 12px;
    font-style: normal;
    font-weight: 500;
    line-height: 16px;
    letter-spacing: 0em;
    text-align: center;
  }
}
// transition

.open-enter-active {
  animation: open 0.5s;
}
.open-leave-active {
  animation: open 0.5s reverse;
}
@keyframes open {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>


