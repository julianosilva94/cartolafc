<template>
  <p>Rodada atual: {{ rodada }}</p>
  <table id="table">
    <thead>
      <tr>
        <th>Nome</th>
        <th>Time</th>
        <th>Posição</th>
        <th>Valor Atual</th>
        <th>Ult Rodada</th>
        <th>Variação</th>
        <th>Média</th>
        <th>Para Valorizar</th>
      </tr>
    </thead>
    <tbody>
      <tr v-bind:key="atleta.atleta_id" v-for="atleta in atletas">
        <td>{{ atleta.apelido }}</td>
        <td>{{ clubes[atleta.clube_id].nome }}</td>
        <td>{{ posicoes[atleta.posicao_id].nome }}</td>
        <td>C$ {{ atleta.preco_num }}</td>
        <td>{{ atleta.pontos_num }}</td>
        <td>{{ atleta.variacao_num }}</td>
        <td>{{ atleta.media_num }}</td>
        <td>{{ atleta.para_valorizar }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
// eslint-disable-next-line camelcase
import { rodada, atletas, posicoes, clubes } from '../assets/mercado.json'

export default {
  name: 'Dashboard',
  data: function () {
    return {
      formacao: {
        goleiro: 1,
        laterais: 2,
        zagueiros: 2,
        meias: 3,
        atacantes: 3,
        tecnico: 1
      },
      rodada,
      posicoes,
      clubes,
      atletas: atletas
        // .filter(atleta => atleta.pontos_num > 0)
        .filter(atleta => atleta.status_id === 7)
        .map((atleta) => {
          return {
            ...atleta,
            // eslint-disable-next-line camelcase
            para_valorizar: ((atleta.preco_num * 0.60) - (atleta.media_num / rodada)).toFixed(2)
          }
        })
        .sort((a, b) => b.preco_num - a.preco_num)
        // .sort((a, b) => a.para_valorizar - b.para_valorizar)
    }
  }
  // methods: {
  //   calculaTimeValorizacao: function () {
  //     const escalacao = {
  //       goleiro: null,
  //       laterais: [],
  //       zagueiros: [],
  //       meias: [],
  //       atacantes: [],
  //       tecnico: null
  //     }
  //
  //     const atletas = this.atletas.sort((a, b) => a.para_valorizar - b.para_valorizar)
  //   }
  // }
}
</script>

<style scoped>
  #table {
    border-collapse: collapse;
    width: 100vw;
  }

  #table, #table th, #table tr, #table td {
    border-bottom: 1px solid black;
    padding: 5px;
  }
</style>
