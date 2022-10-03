<template>
  <div class="calculator">
    <h1 class="calculator__title">Рассчитайте стоимость автомобиля в лизинг</h1>
    <div class="calculator__fields-elements">
      <div
        class='calculator__element calculator__element_type_field'
      >
        <label for="priceCar" class="calculator__label">
          Стоимость автомобиля
        </label>
        <div class="calculator__input-block">
          <input
            v-model="priceCar"
            class="calculator__input"
            id="priceCar"
            type="text"
            placeholder="Введите стоимость машины"
            autocomplete="off"
          />
          ₽
          <input
            v-model="priceCar"
            type="range"
            class="e-range"
            :min="minPriceCar"
            :max="maxPriceCar"
          />
        </div>
      </div>
      <div
        class='calculator__element calculator__element_type_field'
      >
        <label for="initialPay" class="calculator__label">
          Первоначальный взнос
        </label>
        <div
          class="calculator__input-block calculator__input-block_type_payment"
        >
          <input
            v-model="initialPay"
            class="calculator__input"
            id="initialPay"
            type="text"
            placeholder="Введите процент взноса"
            autocomplete="off"
          />
          <input
            v-model="initialPercent"
            class="e-range"
            type="range"
            :min="minInitialPercent"
            :max="maxInitialPercent"
          />
          <div class="calculator__init-block">{{ initialPercent }}%</div>
        </div>
      </div>
      <div
      class='calculator__element calculator__element_type_field'
      >
        <label for="termLease" class="calculator__label"> Срок лизинга </label>
        <div class="calculator__input-block">
          <input
            v-model="termLease"
            class="calculator__input"
            id="termLease"
            type="number"
            placeholder="Введите срок лизинга"
            autocomplete="off"
          />
          мес.
          <input
            v-model="termLease"
            class="e-range"
            type="range"
            :min="minTermLease"
            :max="maxTermLease"
          />
        </div>
      </div>
      <div class="calculator__element">
        <h5 class="calculator__label">Сумма договора лизинга</h5>
        <div class="calculator__values">
          {{ sumLeasing.toLocaleString() }} ₽
        </div>
      </div>
      <div class="calculator__element">
        <h5 class="calculator__label">Ежемесячный платеж от</h5>
        <div class="calculator__values"> {{ monthPay.toLocaleString() }} ₽ </div>
      </div>
      <div class="calculator__element">
        <button
          :class="[
            disabled ? 'calculator__button disabled' : '',
            'calculator__button ',
          ]"
          @click.prevent="sendCalcData"
        >
          <span v-if="!loading"> Оставить заявку </span>
          <div v-else class="spinner"/>
        </button>
      </div>
    </div>
  </div>
</template>

<script>

import { sendCalcDataFetch } from '../fetch'

export default {
  name: "CalculationCarLising",
  data() {
    return {
      defaultPriceCar: 3300000,
      defaultTermLease: 60,
      defaultInitialPercent: 13,
      percentRate: 3.5,
      priceCar: 0,
      initialPay: 0,
      initialPercent: 0,
      termLease: 0,
      minPriceCar: 1000000,
      maxPriceCar: 6000000,
      minInitialPercent: 10,
      maxInitialPercent: 60,
      minTermLease: 1,
      maxTermLease: 60,
      loading: false,
      disabled: false,
      errorMessage: "",
      successMessage: ""
     
    };
  },
  computed: {
    sumLeasing() {
      if (this.initialPay >= 1000) {
        let sum = this.initialPay + this.termLease * this.monthPay;
        return this.monthPay === 0 ? 0 : sum;
      } else {
        return 0;
      }
    },
    monthPay() {
      let pay =
        (this.priceCar - this.initialPay) *
        (((this.percentRate / 100) *
          Math.pow(1 + this.percentRate / 100, this.termLease)) /
          (Math.pow(1 + this.percentRate / 100, this.termLease) - 1));
      let roundPay = Math.round(pay);
      return roundPay === Infinity || this.initialPercent < 1 ? 0 : roundPay;
    },
  },
  created() {
    this.setDefaultParams();
  },
  methods: {
    setDefaultParams() {
      this.priceCar = this.defaultPriceCar;
      this.termLease = this.defaultTermLease;
      this.initialPercent = this.defaultInitialPercent;
      this.calcInitialPay()
    },
    calcInitialPay () {
        this.initialPay = Math.round(
          (this.priceCar * this.initialPercent) / 100
        );
      },
    async sendCalcData() {
      console.log('sx')
      let data = {
        priceCar: this.priceCar,
        termLease: this.termLease,
        initialPay: this.initialPay,
        sumLeasing: this.sumLeasing,
        monthPay: this.monthPay,
      };
      console.log(data)
      this.loading = true;
      this.disabled = true;
      this.errorMessage = "";

      try {
        const response = await sendCalcDataFetch(data);

        if (response.status === 200) {
          this.successMessage = "Ваши данные успешно отправлены  ";
        } else {
          this.disabled = false;
          this.errorMessage = "Что-то пошло не так, попробуйте позже";
        }
      } catch (e) {
        this.disabled = false;
        this.errorMessage = "Что-то пошло не так, попробуйте позже";
      } finally {
        this.loading = false;
      }
    },
  },

  
};
</script>

<style></style>
