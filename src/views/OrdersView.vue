<template>
  <div class="content">
    <router-link to="add"><button>Add new order</button></router-link>
    <input type="text" v-model="search" />
    <select v-model="statusFilter">
      <option>All Status</option>
      <option>Confirmed</option>
      <option>In delivering</option>
      <option>Delivered</option>
    </select>

    <table>
      <tr>
        <th @click="sortBy('date')">Date</th>
        <th @click="sortBy('sender')">Sender</th>
        <th @click="sortBy('destination')">Destination</th>
        <th @click="sortBy('status')">Status</th>
        <th @click="sortBy('weight')">Weight</th>
        <th>Actions</th>
      </tr>
      <tr v-for="order in filteredOrders" :key="order.id">
        <td>{{ order.date }}</td>
        <td>{{ order.sender }}</td>
        <td>{{ order.destination }}</td>
        <td>{{ order.status }}</td>
        <td>{{ order.weight }}</td>
        <td><button @click="removeOrder(order)">delete</button></td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      search: "",
      statusFilter: "All Status",
      sortColumn: "date",
      sortOrder: "asc",
      orders: [
        {
          id: 1,
          date: "2023/03/12",
          sender: "Gerlach-Sanford",
          destination: "Cole-Armstrong",
          status: "Delivered",
          weight: 8
        },
        {
          id: 2,
          date: "2023/03/11",
          sender: "Von-Satterfield",
          destination: "Purdy, Reichert and Ritchie",
          status: "Delivered",
          weight: 1
        },
        {
          id: 3,
          date: "2023/03/11",
          sender: "Lowe and Sons",
          destination: "Goodwin and Sons",
          status: "Confirmed",
          weight: 2
        },
        {
          id: 4,
          date: "2023/03/05",
          sender: "Sawayn-Streich",
          destination: "Rohan Group",
          status: "Delivered",
          weight: 1
        },
        {
          id: 5,
          date: "2023/03/05",
          sender: "Durgan-Koch",
          destination: "Rosenbaum, Parker and Willms",
          status: "Delivered",
          weight: 4
        },
        {
          id: 6,
          date: "2023/03/11",
          sender: "Christiansen, Treutel and Kirlin",
          destination: "Barrows, Glover and Dibbert",
          status: "Confirmed",
          weight: 1
        },
        {
          id: 7,
          date: "2023/03/06",
          sender: "Corwin-Kovacek",
          destination: "Johnston Group",
          status: "Delivered",
          weight: 10
        },
        {
          id: 8,
          date: "2023/03/22",
          sender: "Ullrich-Bashirian",
          destination: "Okuneva and Sons",
          status: "Confirmed",
          weight: 1
        },
        {
          id: 9,
          date: "2023/03/09",
          sender: "Hermiston Inc",
          destination: "Ruecker LLC",
          status: "Delivered",
          weight: 1
        },
        {
          id: 10,
          date: "2023/03/30",
          sender: "Mills, Sporer and Kohler",
          destination: "Connelly-Steuber",
          status: "Delivered",
          weight: 4
        }
      ]
    }
  },
  computed: {
    filteredOrders() {
      return this.orders
        .filter((order) => {
          return (
            order.sender.toLowerCase().includes(this.search.toLowerCase()) ||
            order.destination.toLowerCase().includes(this.search.toLowerCase())
          )
        })
        .filter((order) => {
          if (this.statusFilter === "All Status") {
            return true
          } else {
            return order.status === this.statusFilter
          }
        })
        .sort((a, b) => {
          if (this.sortColumn === "weight") {
            return this.sortOrder === "asc"
              ? a[this.sortColumn] - b[this.sortColumn]
              : b[this.sortColumn] - a[this.sortColumn]
          } else {
            return this.sortOrder === "asc"
              ? a[this.sortColumn].localeCompare(b[this.sortColumn])
              : b[this.sortColumn].localeCompare(a[this.sortColumn])
          }
        })
    }
  },
  mounted() {
    const data = localStorage.getItem("orders")
    if (data) {
      this.orders = JSON.parse(data)
    }
    this.save()
  },
  methods: {
    sortBy(column) {
      if (this.sortColumn === column) {
        this.sortOrder = this.sortOrder === "asc" ? "desc" : "asc"
      } else {
        this.sortColumn = column
        this.sortOrder = "asc"
      }
    },
    save() {
      localStorage.setItem("orders", JSON.stringify(this.orders))
    },
    removeOrder(order) {
      this.orders = this.orders.filter((o) => o.id !== order.id)
      this.save()
    }
  }
}
</script>
