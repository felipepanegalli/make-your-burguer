<template>
  <div>
    <div>
      <form id="burger-form" @submit="createBurger">
        <div class="input-container">
          <label for="name">Nome do cliente:</label>
          <input
            type="text"
            id="name"
            name="name"
            v-model="name"
            placeholder="Digite seu nome..."
          />
        </div>

        <div class="input-container">
          <label for="bread">Selecione o pão:</label>
          <select name="bread" id="bread" v-model="bread">
            <option value="" selected disabled>Selecione o pão</option>
            <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">
              {{ bread.tipo }}
            </option>
          </select>
        </div>

        <div class="input-container">
          <label for="meat">Selecione a carne:</label>
          <select name="meat" id="meat" v-model="meat">
            <option value="" selected disabled>Selecione a carne</option>
            <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">
              {{ meat.tipo }}
            </option>
          </select>
        </div>

        <div class="input-container">
          <label id="option-title">Selecione os opcionais:</label>
          <div
            class="checkbox-container"
            v-for="option in options"
            :key="option.id"
          >
            <input type="checkbox" v-model="optionData" :value="option.tipo" />
            <span :for="option.id">{{ option.tipo }}</span>
          </div>
        </div>

        <div class="input-container">
          <input type="submit" class="submit-btn" value="Criar meu Burguer!" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "BurgerForm",
  data() {
    return {
      breads: null,
      meats: null,
      options: null,
      name: null,
      meat: "",
      bread: "",
      optionData: [],
    };
  },
  methods: {
    async getIngredients() {
      const req = await axios.get("http://localhost:3000/ingredientes");
      const data = req.data;
      this.breads = data.paes;
      this.meats = data.carnes;
      this.options = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault();
      const vue = this;
      const data = {
        name: this.name,
        meat: this.meat,
        bread: this.bread,
        options: Array.from(this.optionData),
        status: "Solicitado",
      };

      await axios
        .post("http://localhost:3000/burgers", data)
        .then(function (response) {
          vue.toastMessage(`Pedido Nº ${response.data.id} realizado com sucesso!`, 'success');
          vue.clearFields();
          //response.data
        })
        .catch(function (error) {
          vue.toastMessage('Houve um erro ao criar o pedido! Por favor tente mais tarde.', 'danger');
          console.error(error);
        });
    },
    clearFields() {
      this.name = null;
      this.meat = "";
      this.bread = "";
      this.optionData = [];
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
    this.getIngredients();
  },
};
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  widows: 300px;
}

#option-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#option-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: center;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container inpu {
  width: auto;
}

.checkbox-container span {
  margin-left: 10px;
  font-weight: bold;
}

.submit-btn {
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

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
