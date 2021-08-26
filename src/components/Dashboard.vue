<template>
  <div id="burger-table">
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>

    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.name }}</div>
        <div>{{ burger.bread }}</div>
        <div>{{ burger.meat }}</div>
        <div>
          <ul>
            <li v-for="(option, i) in burger.options" :key="i">{{ option }}</li>
          </ul>
        </div>
        <div>
          <select
            name="status"
            id="status"
            class="status"
            @change="updateBurger($event, burger.id)"
          >
            <option
              v-for="s in status"
              :key="s.id"
              :value="s.tipo"
              :selected="burger.status === s.tipo"
            >
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteBurger(burger.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Dashboard",
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
    };
  },
  methods: {
    async getOrders() {
      const req = await axios.get("http://localhost:3000/burgers");
      this.burgers = req.data;
      this.getStatus();
      console.log('chegou aqui')
    },
    async getStatus() {
      const req = await axios.get("http://localhost:3000/status");
      this.status = req.data;
    },
    async deleteBurger(id) {
      const vue = this;
      this.$swal({
        title: "Deseja cancelar esse pedido?",
        text: "Esta ação não poderá ser desfeita.",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#c0392b",
        confirmButtonText: "Sim",
        cancelButtonText: "Voltar",
        reverseButtons: true,
      }).then((result) => {
        if (result.isConfirmed) {
          vue.$swal
            .fire(
              "Cancelado!",
              `Pedido Nº ${id} foi cancelado com sucesso!`,
              "success"
            )
            .then(async (res) => {
              await axios.delete(`http://localhost:3000/burgers/${id}`);
              await vue.getOrders();
            });
        }
      });
    },
    async updateBurger(evt, id) {
      const vue = this;
      const status = evt.target.value;
      await axios.patch(`http://localhost:3000/burgers/${id}`, { status });
      vue.toastMessage(
        `Pedido Nº ${id} foi atualizado para ${status}!`,
        "success"
      );
      this.getOrders();
    },
    toastMessage(text, status) {
      this.$moshaToast(text.toString(), {
        position: "top-center",
        type: status.toString(),
        transition: "bounce",
        showIcon: true,
        timeout: 2500,
        hideProgressBar: false,
      });
    },
  },
  mounted() {
    this.getOrders();
  },
};
</script>

<style scoped>
#burger-table {
  max-width: 1200px;
  margin: 0 auto;
}
#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}
#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}
.burger-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}
#burger-table-heading div,
.burger-table-row div {
  width: 19%;
}
#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}
select {
  padding: 12px 6px;
  margin-right: 12px;
}
.delete-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>