<template>
  <q-page class="flex flex-center page-container">
    <h5 class="page-title">Select Tickets({{tickets.length}})</h5>
    <div class="ticket-container">
      <q-btn
        outline
        style="color: goldenrod;"
        class="ticket clickable"
        v-for="ticket in tickets"
        :key="ticket.ID"
        @click="pick(ticket.TICKET_NUMBER)"
        :color="picked.includes(ticket.TICKET_NUMBER) ? 'red': ''"
      >{{ticket.TICKET_NUMBER}}</q-btn>
    </div>
    <!-- <q-pagination v-model="currentPage" :max="5"></q-pagination> -->
    <q-btn icon="shopping_cart" class="cart-btn" @click="open">Basket</q-btn>
    <q-dialog v-model="openCart" square full-height position="right">
      <q-card class="column full-height picked" style="width: 300px">
        <q-card-section>
          <div class="cart-header">
            <div class="text-h6">You Picked</div>
            <q-space />
            <q-btn icon="close" flat round dense v-close-popup />
          </div>
        </q-card-section>
        <div>
          <q-chip
            color="red"
            text-color="white"
            size="18px"
            icon="bookmark"
            v-for="ticket in picked"
            :key="ticket"
          >{{ticket}}</q-chip>
        </div>
        <q-card-actions align="right" class="bg-white text-teal">
          <q-btn flat label="OK" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import data from "../data.json";
import { log } from "util";
export default {
  name: "PageIndex",
  data() {
    return {
      currentPage: 3,
      tickets: data,
      openCart: false,
      picked: []
    };
  },
  mounted() {
    this.tickets = data.data[0];
  },
  methods: {
    open() {
      this.openCart = true;
    },
    pick(ticket) {
      const index = this.picked.indexOf(ticket);
      if (index > -1) {
        this.picked.splice(index, 1);
        return;
      }
      this.picked = [...this.picked, ticket];
    }
  }
};
</script>

<style lang="stylus">
.page-container {
  width: 80%;
  margin: 0 auto;
  margin-top: 42px;
}

.ticket-container {
  display: flex;
  flex-wrap: wrap;
}

.ticket {
  margin-right: 5px;
  margin-top: 5px;
}

.q-dialog__inner--minimized {
  padding: 0;
}

.cart-btn {
  position: fixed;
  top: 50px;
  right: 0;
  height: 72px;
  cursor: pointer;
}

.picked {
  padding: 0 24px;
}

.cart-header {
  display: flex;
  justify-content: space-between;
}

.page-title {
  margin-top: 24px;
}
</style>
