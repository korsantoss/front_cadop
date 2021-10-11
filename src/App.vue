<template>
  <div class="container">
    <!-- sessão que contem o form para o usuario realizar a pesquisa desejada -->
    <section class="search">
      <img src="https://static.wixstatic.com/media/b91e09_95ec298e6ed645d197af640983985c6c~mv2.png/v1/fill/w_234,h_56,al_c,q_85,usm_0.66_1.00_0.01/logo_ic_v5_20210604_a4_logo_hQ.webp" alt="logo_ic_v5_20210604_a4_logo_hQ.png" style="width:187px;height:45px;object-fit:cover;margin-bottom:28px">
      <h3>Relação de Operadoras Ativas ANS</h3>
      <form @submit="handleSearch($event)">
        <input  type="text" placeholder="Pesquise o que deseja" v-model="search">
        <button type="submit">Pesquisar</button>
      </form>
    </section>

    <!-- msg caso ocorrar algum erro -->
    <section class="errored" v-if="errored">
      <p>Ocorreu um erro, não conseguimos exibir os resultados!</p>
    </section>

    <section class="noResults" v-if="noResults">
      <p>Nenhum resultado encontrado!</p>
    </section>

    <!-- sessão que mostrara os resultados encontrados na pesquisa caso não ocorrar erro -->
    <section v-else class="results">
      <div v-if="loading">Carregando...</div>
      <!-- uso das diretivas para renderizar os resultados encontrados-->
      <div v-else class="card-result" v-for="(result, index) in results" :key="index"> <!-- card da operadora encontrada na pesquisa -->
        <div><span>Registro ANS: </span>{{result['Registro ANS']}}</div>
        <div><span>CNPJ: </span>{{result['CNPJ']}}</div>
        <div><span>Razão Social: </span>{{result['Razão Social']}}</div>
        <div><span>Nome Fantasia: </span>{{result['Nome Fantasia']}}</div>
        <div><span>Modalidade: </span>{{result['Modalidade']}}</div>
        <div><span>Endereço: </span>{{result['Logradouro']}}, {{result['Número']}}, {{result['Complemento']}}, {{result['Bairro']}}, {{result['Cidade']}} - {{result['UF']}} - {{result['CEP']}}</div>
        <div><span>Tel: </span>{{result['DDD']}} {{result['Telefone']}}</div>
        <div><span>Fax: </span>{{result['Fax']}}</div>
        <div><span>E-mail: </span>{{result['Endereço eletrônico']}}</div>
        <div><span>Representante: </span>{{result['Representante']}}</div>
        <div><span>Cargo Representante: </span>{{result['Cargo Representante']}}</div>
        <div><span>Data Registro ANS: </span>{{result['Data Registro ANS']}}</div>
      </div>
    </section>
  </div>
</template>


<script>
import axios from 'axios';
export default {
  data() {
    return {
      search: '', // search conectado no input de pesquisa
      results: null, // variavel que recebera o resultado da pesquisa
      loading: false, // variavel de carregamento
      errored: false, // variavel de controle para caso ocorra erro na requisição
      noResults: false // variavel de controle caso o resultado da pesquisa seja 0
    }
  },
  methods: {
    // metodo ligado ao click do botao "pesquisar"
    async handleSearch(e) {
      e.preventDefault(); // evitar o recarregamento da pagina
      try {
        this.loading = true;
        this.noResults = false;
        const url = `http://localhost:5000/search?q=${this.search}`; // criacao da url de requisicao com o valor digitado pelo usuario
        const response = await axios.get(url); // realizacao da requisicao utilizando o axios
        this.results = response.data; // atribuir resposta ao results
        this.search = ''; // limpar o input de pesquisa
        if (this.results.length == 0) {
          this.noResults = true;
        }
      } catch (error) {
        this.errored = true;
      } finally {
        this.loading = false;
      }
    }
  }
}
</script>


<style>

@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

  /* estilizações da interface */
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Roboto', sans-serif;
  }

  #app{
    display: flex;
    justify-content: center;
  }

  .container {
    min-height: 100vh;
    width: 100%;
    padding: 15px 24px;
    background: #f6f6f6;
  }

  .search {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .search form {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    margin-top: 15px;
    gap: 3px;
  }

  .search form input {
    height: 28px;
    width: 280px;
    padding: 0 5px;
    border: 1px solid rgb(165, 161, 161);
    border-radius: 3px;
  }

  .search form button {
    height: 28px;
    padding: 0 10px;
    background: #367bff;
    border: none;
    border-radius: 3px;
    color: #ffff;
  }

  .search form button:hover {
    background: #2666dd;
  }

  .errored {
    font-size: 1.1rem;
    color: rgb(231, 36, 36);
    margin-top: 30px;
  }

  .noResults {
    font-size: 1.1rem;
    margin-top: 30px;
  }

  .results {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin-top: 30px;
  }

  .results .card-result {
    padding: 10px 15px;
    background:#ffff;
    border-radius: 5px;
    font-size: 0.95rem;
  }

  .results .card-result span {
    font-weight: 700;
  }

  @media (max-width: 575.98px) { 
    .search form {
      gap: 8px;
    }

    .search form button{
      padding: 0 28px;
    }
  }

</style>
