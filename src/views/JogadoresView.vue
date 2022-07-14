<script>
import {v4 as uuid} from "uuid";
import axios from "axios";

export default {
  data() {
    return { 
      jogadores:[],
      times: [],
      jogador: {},
    };
  },
  async created() {
    await this.buscarTodosOsJogadores();
    await this.buscarTodosOsTimes();
  },

  methods: {
    async buscarTodosOsTimes(){
      const times = await axios.get("http://localhost:4000/times");
      this.times = times.data;
    },

    async buscarTodosOsJogadores() {
      const jogadores = await axios.get("http://localhost:4000/jogadores?expand=time");
      this.jogadores = jogadores.data;
    },
    async salvar() {
      await axios.post("http://localhoste:400/jogadores" , this.jogador);
      await this.buscarTodosOsJogadores();
    },
    excluir(jogador) {
      const indice = this.jogadores.indexOf(jogador);
      this.jogadores.splice(indice, 1);
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
      <h2>Histórico jogadores</h2>
    </div>
    <div class="forme-input">
      <input type="text" placeholder="Nome" v-model="jogador.nome" />
      <input type="text" placeholder="Ano Nascimento" v-model="jogador.anoNascimento" />
      <input type="text" v-model="jogador.posicaoJogo" placeholder="Possição de Jogo" />
      <input type="text" v-model="jogador.altura" placeholder="Altura" />
      <input type="text" v-model="jogador.peso" placeholder="Peso" />

      <select v-model="jogador.timeId">
        <option v-for="time in times" key="time.id" :value="time.id">
          {{time.nome}}
        </option>
      </select>
      <button @click="salvar">Salvar</button>
    </div>
    <div class="list-jogadores">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Altura</th>
            <th>Peso</th>
            <th>Posição Jogo</th>
            <th>Idade</th>
            <th>Time</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="jogador in jogadores" :key="jogador.id">
            <td>{{jogador.id}}</td>
            <td>{{jogador.name}}</td>
            <td>{{jogador.altura}}</td>
            <td>{{jogador.peso}}</td>
            <td>{{jogador.posicaoJogo}}</td>
            <td>{{2022 - jogador.anoNascimento}} anos</td>
            <td>{{jogador.time.nome}}</td>
            <td>???</td>
            
              <button @click="alerta">Editar</button>
              <button @click="excluir">excluir</button>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>

</style>