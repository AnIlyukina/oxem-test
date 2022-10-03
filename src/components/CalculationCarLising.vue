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
            v-model="initialPayment"
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
          />
        </div>
      </div>
      <div class="calculator__element">
        <h5 class="calculator__label">Сумма договора лизинга</h5>
        <div class="calculator__values">
          343423324 
        </div>
      </div>
      <div class="calculator__element">
        <h5 class="calculator__label">Ежемесячный платеж от</h5>
        <div class="calculator__values"> 343432 </div>
      </div>
      <div class="calculator__element">
        <button
          class='calculator__button'
        >
          <span> Оставить заявку </span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>


export default {
  name: "CalculationCarLising",
  data() {
    return {
      defaultPriceCar: 3300000,
      defaultTermLease: 60,
      defaultInitialPercent: 13,
      percentRate: 3.5
     
    };
  },
  created() {
    this.setDefaultParams();
  },
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
  
};
</script>

<style></style>
