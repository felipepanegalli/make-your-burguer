<template>
  <div>
    <p>componente de mensagem</p>
    <div>
      <form id="burger-form">
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
            <option v-for="bread in breads" :key="bread.id" :value="bread.id">
              {{ bread.tipo }}
            </option>
          </select>
        </div>

        <div class="input-container">
          <label for="meat">Selecione a carne:</label>
          <select name="meat" id="meat" v-model="meat">
            <option value="" selected disabled>Selecione a carne</option>
            <option v-for="meat in meats" :key="meat.id" :value="meat.id">
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
            <input type="checkbox" v-model="optionData" :value="option.id" />
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
      status: "Solicitado",
      msg: null,
    };
  },
  methods: {
    async getIngredients() {
      const req = await axios.get("http://localhost:3000/ingredientes");
      const data = req.data;
      console.log(data);

      this.breads = data.paes;
      this.meats = data.carnes;
      this.options = data.opcionais;
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