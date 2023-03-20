<template>
  <h2>Add new order</h2>
  <p><input type="date" placeholder="Date" v-model="order.date" /></p>
  <p><input type="text" placeholder="Sender" v-model="order.sender" /></p>
  <p><input type="text" placeholder="Destination" v-model="order.destination" /></p>
  <p><input type="number" placeholder="Weight" v-model="order.weight" /></p>
  <p>
    <select v-model="order.status">
      <option>Confirmed</option>
      <option>In delivering</option>
      <option>Delivered</option>
      ¨
    </select>
  </p>
  <p v-if="!isNotEmpty">You must supply all fields</p>
  <p v-if="!isWeightValid">Weight must be greather than 0</p>

  <button @click="add" :disabled="!(isNotEmpty && isWeightValid)">Save</button>
  <router-link to="/"><button>Cancel</button></router-link>
</template>

<script>
import { nanoid } from "nanoid"

export default {
  data() {
    return {
      order: {
        id: "",
        date: "",
        sender: "",
        destination: "",
        status: "",
        weight: ""
      }
    }
  },
  computed: {
    isWeightValid() {
      return this.order.weight > 0
    },
    isNotEmpty() {
      return (
        this.order.date &&
        this.order.sender &&
        this.order.destination &&
        this.order.status &&
        this.order.weight
      )
    }
  },
  methods: {
    add() {
      const orders = JSON.parse(localStorage.getItem("orders")) || []
      this.order.id = this.newId()
      orders.push(this.order)
      localStorage.setItem("orders", JSON.stringify(orders))
      this.$router.push("/")
    },
    newId() {
      return nanoid()
    }
  }
}
</script>
