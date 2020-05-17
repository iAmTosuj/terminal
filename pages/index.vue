<template>
  <div class="container">
    <div class="monitor">
      <div class="monitor__stats">
         Все купюры: {{money}}
      </div>
      <div class="monitor__content">
        <div>
          <div class="monitor__money-title">
            Имеются купюры номиналом:
          </div>
          <div class="monitor__money-in-stock-list">
            <div class="monitor__money-in-stock-item" v-for="money of getStockMoney">
              {{money}}
            </div>
          </div>
          <div>

          </div>
          <div class="monitor__money-field-container">
            <input type="text" v-model="test">
            <button @click="isHaveMoney(test)">посчитать</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  data() {
    return {
      money: {
        100:10,
        500: 10,
        1000: 10,
        5000: 10
      },
      test: 0
    }
  },
  created() {
  },
  methods: {
    isHaveMoney(giveOut) {
      const moneyArr = this.getMoneyArray;
      const copyMoney = JSON.parse(JSON.stringify(this.money));
      giveOut = +giveOut;

      moneyArr.forEach((value) => {
        if (giveOut === 0) return;

        if (giveOut / value >= 1) {
          let limitValue = Math.floor(giveOut / value);
            if (copyMoney[value] - limitValue <= 0) {
              limitValue = copyMoney[value]
            }
          copyMoney[value] -= limitValue;
          giveOut -= value * limitValue;
        }
      });
      if (giveOut > 0) {
        alert('Невозможно выдать данную сумму')
      } else {
        this.money = copyMoney
      }

    },

    allMoney() {
      return this.money.reduce((sum, item) => sum + this.pointMoney(item.value, item.count), 0)
    },

    pointMoney(value, count) {
      return value * count
    },

  },
  computed: {
    getMoneyArray() {
      return Object.keys(this.money).sort((a, b) => b - a)
    },

    getStockMoney() {
      return this.getMoneyArray.filter((money) => (this.money[money]))
    }
  }
}
</script>

<style>
.monitor {
  width: 500px;
  height: 500px;
  border: 10px solid black;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.monitor__content {

}
.monitor__stats {
  margin-bottom: 50px;
}
.monitor__money-in-stock-list {
  display: flex;
  justify-content: center;
}
.monitor__money-in-stock-item {
  margin: 5px;
}
.monitor__money-field-container {
  margin-top: 20px;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
