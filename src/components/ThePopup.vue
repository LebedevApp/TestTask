<template>
  <section class="modalWindow" @click.self="close">
    <div class="card">
      <div class="container">
        <div class="card_title">
          <slot name="header"></slot>
          <button class="exit" @click="close">X</button>
        </div>
        <div class="card_content">
          <slot name="description"></slot>
          <slot name="input"></slot>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "v-popup-forms",

  data() {
    return {};
  },

  methods: {
    close() {
      console.log("sdasd");
      this.$emit("close");
    },
  },
};
</script>

<style lang="scss" scoped>
.modalWindow {
  width: 100%;
  height: 100vh;
  min-height: 700px;

  background-color: rgba(0, 0, 0, 0.5);

  display: flex;
  align-items: center;
  justify-content: center;

  position: absolute;
  top: 0;
  left: 0;
  z-index: 5;
}
.container {
  margin: 32px;
}
.card {
  width: 100%;
  max-width: 552px;

  border-radius: 30px;

  background-color: white;

  position: relative;
  &_title {
    width: 100%;
    height: 100%;
  }

  &_content {
    margin: 10px 0;
  }

  &_action {
  }
}

.exit {
  color: #ea0029;
  font-size: 18px;
  font-weight: 600;

  background-color: transparent;
  border: none;
  border-radius: 50%;

  padding: 5px 8px 3px 8px;

  position: absolute;
  top: 25px;
  right: 25px;
}
.exit:hover {
  background-color: rgba(0, 0, 0, 0.1);
}

@media (max-width: 1024px) {
    .card {
        width: 453px;
    }
}

@media (max-width: 455px) {
    .card {
        width: 100%;
        min-height: 100vh;
        height: 100%;
        border-radius: 0;
    }
   
}
</style> 