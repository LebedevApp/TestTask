<template>
  <form class="form">
    <label class="form_label">Ваша зарплата в месяц</label>
    <input
      class="form_input"
      type="number"
      placeholder="Введите данные"
      :class="{
        error:
          (!$v.salaries.required || !$v.salaries.numeric) && $v.salaries.$dirty,
      }"
      v-model.lazy="salaries"
      @change="setMany($event.target.value)"
    />
    <span v-if="!$v.salaries.required && $v.salaries.$dirty" class="message"
      >Введите данные</span
    >
    <span v-if="!$v.salaries.numeric && $v.salaries.$dirty" class="message"
      >Только цифры</span
    >
    <button
      class="form_button"
      :class="{ active: isResult }"
      @click.prevent="isResult = !isResult"
    >
      Рассчитать
    </button>

    <div class="form_result" v-show="isResult && calculate">
      <p>Итого можете внести в качестве досрочных:</p>
      <div class="form_item" v-for="(year, i) in calculate" :key="i">
        <input
          class="form_check"
          type="checkbox"
          name="a"
          :id="'check_ ' + i"
          value="1417"
        /><label class="label" :for="'check_ ' + i"
          >{{ year }} рублей <span> {{ i + 1 }}-й год</span></label
        >
      </div>
    </div>

    <div class="form_options">
      <p>Что уменьшаем?</p>
      <button @click.prevent="isMany = !isMany" :class="{ active: isMany }">
        Платеж
      </button>
      <button @click.prevent="isTime = !isTime" :class="{ active: isTime }">
        Срок
      </button>
    </div>
    <button class="form_submit" :disabled="$v.$invalid || !$v.salaries.$dirty">
      Добавить
    </button>
  </form>
</template>

<script>
import { required, numeric } from "vuelidate/lib/validators";

export default {
  name: "calculator",

  validations: {
    salaries: {
      required,
      numeric,
    },
  },

  watch: {
    salaries() {
      this.isResult = false
      this.calculate =null
      if (this.salaries.length > 0) {
        let total = this.salaries * 12 * 0.13;
        const res = [];
        for (let i = 0; i < (Math.floor(this.appartmentSaleTax / total)); i++) {
          res.push(total);
        }
        if (this.appartmentSaleTax % total > 0) {
          res.push(this.appartmentSaleTax % total);
        }
        this.calculate = res;
      } else if (this.salaries.length == 0) {
        this.isResult = false;
        this.calculate = null;
      }
    },
  },

  data() {
    return {
      salaries: "",
      isResult: false,
      isMany: true,
      isTime: false,
      error: null,
      calculate: null,
      appartmentSaleTax: 2000000 * 0.13
    };
  },

  methods: {
    setMany(e) {
      this.salaries = e;
      this.$v.salaries.$touch();
    },
  },
};
</script>

<style lang="scss" scoped>
.message {
  color: #ea0029;
  display: block;

  margin-top: 4px;

  font-size: 10px;
  font-style: normal;
  font-weight: 400;
  line-height: 12px;
  letter-spacing: 0em;
  text-align: left;
}
.form {
  margin-top: 24px;

  &_label {
    font-style: normal;
    font-weight: 500;
    font-size: 14px;
    line-height: 24px;
  }

  &_input {
    appearance: none;

    font-size: 14px;
    font-style: normal;
    font-weight: 400;
    line-height: 24px;
    letter-spacing: 0em;
    text-align: left;

    width: 100%;

    outline: none;

    padding: 5px;
    margin-top: 8px;

    border: 1px solid #dfe3e6;
    box-sizing: border-box;
    border-radius: 3px;

    transition: all 0.3s;
  }

  &_input::-webkit-outer-spin-button,
  &_input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  &_input:hover {
    border: 1px solid #000000;
  }

  &_input:disabled {
    border: 1px solid #808080;
  }
  &_input.error {
    border: 1px solid #ea0029;
  }
  &_button {
    font-size: 14px;
    font-style: normal;
    font-weight: 500;
    line-height: 24px;
    letter-spacing: 0em;
    text-align: left;
    color: #ea0029;
    border: none;
    background: none;

    margin-top: 8px;

    cursor: pointer;
  }
  &_button.active {
    color: #ea0029 !important;
  }
  &_button:hover {
    color: #f53a31;
  }
  &_options {
    margin-top: 24px;
  }
  &_options p {
    font-size: 14px;
    font-style: normal;
    font-weight: 500;
    line-height: 24px;
    letter-spacing: 0em;
    text-align: left;

    margin-right: 24px;
  }
  &_options button,
  &_options p {
    display: inline;
  }
  &_options button {
    margin: 0 8px;

    border: none;
    border-radius: 50px;
    padding: 6px 12px 6px 12px;
    color: black;

    transition: all 0.5s;
  }
  &_options button.active {
    background: linear-gradient(
        255.35deg,
        #dc3131 0.83%,
        rgba(255, 79, 79, 0) 108.93%
      ),
      linear-gradient(0deg, #ff5e56, #ff5e56);

    color: white;

    transition: all 0.5s;
  }
  &_submit {
    display: block;
    width: 100%;

    margin-top: 40px;
    padding: 16px 0;

    font-size: 16px;
    font-style: normal;
    font-weight: 500;
    line-height: 24px;
    letter-spacing: 0em;
    text-align: center;
    color: white;

    background: linear-gradient(
        255.35deg,
        #dc3131 0.83%,
        rgba(255, 79, 79, 0) 108.93%
      ),
      linear-gradient(0deg, #ff5e56, #ff5e56);
    border-radius: 6px;
    border: none;
  }
  &_submit:hover {
    background: linear-gradient(255.35deg, #ea0029, #ea0029),
      linear-gradient(0deg, #ea0029, #ea0029);
    //background: #ea0029;
  }
  &_submit:disabled {
    background: #bec5cc;
  }
  &_result {
    overflow: hidden;
    min-height: 92px;
    max-height: 250px;
    overflow-y: scroll;
  }
  &_result p {
    font-size: 14px;
    font-style: normal;
    font-weight: 500;
    line-height: 24px;
    letter-spacing: 0em;
    text-align: left;

    margin: 16px 0 0 0;
  }
  &_result div::after {
    content: "";
    display: block;
    width: 100%;
    height: 1px;
    background: #dfe3e6;
  }
  &_result input {
    margin: 16px 0;
  }
  &_item {
    position: relative;
  }
  &_check {
    -moz-appearance: none;
    -webkit-appearance: none;
    appearance: none;
    position: absolute;
  }
  &_item label {
    margin: 16px 0;
    display: inline-block;
    margin-left: 34px;
  }
  &_item label::before {
    content: "";
    display: block;
    width: 20px;
    height: 20px;

    background-color: white;

    border: 1px solid #dfe3e6;
    border-radius: 6px;

    position: absolute;
    top: 14px;
    left: 0;
    z-index: 5;
  }
  &_item label::after {
    content: "";
    display: block;
    width: 20px;
    height: 20px;

    background: url("../assets/check.png");
    background-size: cover;

    position: absolute;
    top: 15px;
    left: 1px;
    z-index: 6;
  }
  &_check:checked + &_item label::before {
    background: linear-gradient(
        255.35deg,
        #dc3131 0.83%,
        rgba(255, 79, 79, 0) 108.93%
      ),
      linear-gradient(0deg, #ff5e56, #ff5e56);
    border-radius: 6px;
  }
}

.form_check:checked + .label::before {
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    linear-gradient(0deg, #ff5e56, #ff5e56);
  border-radius: 6px;
}
.label span {
  opacity: 0.7;
}

@media (max-width: 350px) {
  .form_options button {
    display: block;
    margin-top: 10px;
  }
  .form_options p {
    width: 100%;
  }
}

//---scroll
.scroll::-webkit-scrollbar {
  width: 0 !important;
}
.scroll {
  padding-right: 4px;
}

::-webkit-scrollbar-button {
  width: 5px;
  height: 0px;
}
::-webkit-scrollbar-track {
  background-color: rgba(0, 0, 0, 0);
}

::-webkit-scrollbar-thumb {
  -webkit-border-radius: 50px;
  border-radius: 50px;
  background-color: rgba(0, 0, 0, 0.3);
}

::-webkit-scrollbar-thumb:hover {
  background-color: rgba(0, 0, 0, 0.6);
}

::-webkit-resizer {
  width: 4px;
  height: 0px;
}

::-webkit-scrollbar {
  width: 4px;
}
</style>