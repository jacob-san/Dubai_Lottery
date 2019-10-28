<template>
  <q-page class="flex flex-center page-container winners">
    <h5 class="page-title">Recent Winners({{winners.length}})</h5>
    <div class="ticket-container">
      <q-item
        outline
        style="color: goldenrod;"
        class="ticket clickable"
        v-for="ticket in winners"
        :key="ticket.ID"
      >
        <div class="ticket-info">
          <div class="series">{{ticket.SERIES}}</div>
          <div class="series">{{ticket.DRAW_DATE}}</div>
          <div class="number">{{ticket.TICKET_NUMBER}}</div>
          <div class="nationality">{{ticket.NATIONALITY}}</div>
          <div class="name">{{ticket.WINNER_NAME}}</div>
        </div>
      </q-item>
    </div>
  </q-page>
</template>

<script>
import data from "../winners.json";
import { log } from "util";
export default {
  name: "PageIndex",
  data() {
    return {
      currentPage: 3,
      winners: data,
      openCart: false,
      picked: []
    };
  },
  mounted() {
    // this.tickets = data.data[0];
    // this.fetchTickets();
  },
  methods: {
    async fetchTickets(page = 1, count = 500) {
      const res = await fetch(
        `https://www.dubaidutyfree.com/ccstorex/custom/v1/getRaffleSKU/MM317/614089798/A/${page}/${count}`,
        {
          // method: "GET",
          // mode: "no-cors",
          // headers: {
          //   "Content-Type": "application/json",
          //   Accept: "application/json"
          //   // 'Content-Type': 'application/x-www-form-urlencoded',
          // }
        }
      );
      console.log({ res });
      const data = await res.json();

      return data;
    },
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

<style lang="scss">
.page-container {
  width: 80%;
  margin: 0 auto;
  margin-top: 42px;
}
.ticket-container {
  display: flex;
  flex-wrap: wrap;
}

.winners {
  .ticket {
    margin-right: 5px;
    margin-top: 5px;
    border: 1px solid;
    border-radius: 4px;
    width: 100px;
    display: flex;
    align-items: center;
    justify-content: center;

    .ticket-info {
      text-align: center;
    }
  }
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

.series {
  font-size: 11px;
  color: #9c9c9c;
}
.name {
  font-size: 11px;
  color: #9c9c9c;
  width: 72px;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}

.number {
  font-size: 22px;
  margin: 4px 0;
  color: #545454;
}

.nationality {
  font-size: 11px;
  text-transform: uppercase;
}
</style>
