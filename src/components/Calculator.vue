<template>
  <div class="calculator-container">
    <h2>Calculadora</h2>
    <div class="inputs">
      <input type="number" v-model.number="a" placeholder="Digite o primeiro número" />
      <input type="number" v-model.number="b" placeholder="Digite o segundo número" />
    </div>
    <div class="buttons">
      <button @click="calcular('somar')">+</button>
      <button @click="calcular('subtrair')">-</button>
      <button @click="calcular('multiplicar')">×</button>
      <button @click="calcular('dividir')">÷</button>
    </div>
    <div v-if="resultado !== null" class="resultado">
      <h3>Resultado: {{ resultado }}</h3>
    </div>
    <div v-if="erro" class="erro">
      <p>{{ erro }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      a: null,
      b: null,
      resultado: null,
      erro: ""
    };
  },
  methods: {
    calcular(operacao) {
      this.resultado = null;
      this.erro = "";
      axios
        .get(`http://localhost:8181/Calculadora/${operacao}/${this.a}/${this.b}`)
        .then(res => {
          this.resultado = res.data;
        })
        .catch(err => {
          this.erro = err.response && err.response.data
            ? err.response.data
            : "Erro ao calcular";
        });
    }
  }
};
</script>

<style scoped>
.calculator-container {
  max-width: 350px;
  margin: 40px auto;
  padding: 24px 20px;
  border-radius: 12px;
  background: #222; /* fundo escuro */
  box-shadow: 0 2px 12px rgba(0,0,0,0.18);
  text-align: center;
}

.inputs {
  display: flex;
  gap: 10px;
  margin-bottom: 18px;
  justify-content: center;
}

.inputs input {
  width: 120px;
  padding: 8px;
  border: 1px solid #444;
  border-radius: 6px;
  font-size: 16px;
  background: #111;
  color: #f7f7f7;
}

.inputs input::placeholder {
  color: #aaa;
}

.buttons {
  display: flex;
  gap: 12px;
  justify-content: center;
  margin-bottom: 18px;
}

.buttons button {
  width: 40px;
  height: 40px;
  font-size: 20px;
  border: none;
  border-radius: 50%;
  background: #42b983;
  color: #fff;
  cursor: pointer;
  transition: background 0.2s;
}

.buttons button:hover {
  background: #369870;
}

.resultado h3 {
  color: #42b983;
  margin-top: 10px;
}

.erro p {
  color: #ff5252;
  margin-top: 10px;
}
</style>
