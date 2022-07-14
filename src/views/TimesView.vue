<script>
import {v4 as uuid} from "uuid";
import axios from "axios"
export default {
  data() {
    return { 
      novo_time: "",
      categories: [],
    };
  },
  async created(){
    const times = await axios.get("http://localhost:4000/times");  
  },

  methods: {
    async salvar() {
      const time = {
        nome: this.novo_time,
      };
      const time_criado = await axios.post("http://localhost:4000/times", time);
      this.times.push (time_criado.data);
      this.novo_time = "";
    },

    async excluir(time) {
      await axios.delete('http://localhost:4000/times/$ (time.id)')
      const indice = this.times.indexOf(time);
      this.times.splice(indice, 1);
    },
    alerta() {
      alert("ok");
    },
  },
};

</script>

<template>
  <div class="container">
    <div class="title">
      <h2>Gerenciamento de Times</h2>
    </div>
    <div class="forme-input">
      <input type="text" placeholder="Time" v-model="novo_time" />
      <button @click="salvar">Salvar</button>
    </div>
    <div class="list-times">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>J | V | D | E</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="time in times" :key="time.id">
            <td>{{time.id}}</td>
            <td>{{time.nome}}</td>
            <td>{{time.acao}}</td>
            <td>
              <button @click="alerta">Editar</button>
              <button @click="excluir">excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style>

</style>