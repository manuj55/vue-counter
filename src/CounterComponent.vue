<template>
  <div class="counter-app-container">
    <h1 class="counter-heading">Advanced Counter</h1>
    <p id="counter" :class="counterStyleValues">{{ count }}</p>

    <!--Button for incerment and decrement-->
    <div class="button-container">
      <button id="decrease" aria-label="decrease button" @click="decreaseCount"
        :class="{ disabled: this.count <= 0 }">{{ decrease }}</button>
      <button id="increase" aria-label="increase button" @click="increaseCount">{{ increase }}</button>
    </div>

    <!-- Form for adjusting count -->
    <!-- v-model usage -->
    <!-- @submit.prevent we can use this to not reset -->
    <form @submit.prevent="adjustCount" class="form-container">
      <input v-model.number="amount" type="number" placeholder="Enter number" />
      <select v-model="operationType">
        <option value="Add">Add</option>
        <option value="Subtract">Subtract</option>
      </select>
      <button type="submit" id="adjust-button"> Apply {{ operationType }}</button>
    </form>
  </div>
</template>
<script>


export default {
  name: 'CounterComponent',
  data() {
    return {
      count: 0,
      amount: 0,
      logs: [],
      operationType: "Add",
      increase: "Increase Count",
      decrease: "Decrease Count",
    };
  },

  watch: {
    //Allowed to uppdte external values
    //Allowed to make api calls
    //allowed to make console logs or logging values

    count(newValue, oldValue) {
      this.addLogs(`Count changed from ${oldValue} to ${newValue}`);
    }
  },

  methods:
  {
    //side effects in methods
    decreaseCount() {
      if (this.count > 0) {
        this.count--;
      }
    },

    increaseCount() {
      this.count++;
    },
    //error should be handled in starting of the function so that we can save time and resources (lines)
    adjustCount() {
      //js way of handelinf prevent default
      // event.preventDefault();

      const parsedAmount = parseInt(this.amount, 10);
      if (isNaN(parsedAmount) || parsedAmount <= 0) {
        alert("Please enter a valid number");
        return;
      }
      if (this.operationType === "Add") {
        this.count += this.amount;
      }
      else if (this.operationType === "Subtract" && this.count >= this.amount) {
        this.count -= this.amount;
      }

      this.amount = 0;
    },


    addLogs(message) {
      const logEntry = {
        message,
        id: new Date().toLocaleString(),
      };
      this.logs.unshift(logEntry); // add to the start of the array
    }
  },

  computed:
  {
    //no side effects in computed properties
    counterStyleValues() {
      let output = {
        positive: this.count > 0,
        zero: this.count === 0,
      };
      return output;
    }
  }
}
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.counter-app-container {
  width: 100%;
  height: 100vh;
  background-color: #0093e9;
  background-image: linear-gradient(160deg, #0093e9 0%, #80d0c7 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  font-family: "Poppins", sans-serif;
  flex-direction: column;
}

.counter-heading {
  font-size: 50px;
  letter-spacing: 1.2px;
}

#counter {
  font-size: 75px;
  margin: 35px 0;
  transition: all 0.3s;
}

#counter.positive {
  color: #1bea1e;
}

#counter.zero {
  color: #ffc107;
}

.button-container {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  flex-wrap: wrap;
}

button {
  font-size: 20px;
  padding: 10px 30px;
  color: #fff;
  border: 1px solid #fff;
  cursor: pointer;
  background: #44b3d7;
  border-radius: 8px;
  margin: 10px;
}

button.disabled {
  cursor: not-allowed;
  opacity: 0.6;
}

.form-container {
  margin-top: 25px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

form input,
form select {
  padding: 10px;
  font-size: 20px;
  border-radius: 5px;
  border: none;
  outline: none;
}

form button {
  font-size: 20px;
  padding: 10px 20px;
  background: #44b3d7;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  border: 1px solid #fff;
}

form button:hover {
  background-color: #6ca5b6;
}
</style>