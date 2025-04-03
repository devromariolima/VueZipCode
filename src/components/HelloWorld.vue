<template>
  <div class="cep-form">
    <h1>Buscar Endereço pelo CEP</h1>

  
    <div>
      <label for="cep">CEP:</label>
      <input type="text" v-model="cep" placeholder="Digite o CEP" />
      <button @click="buscarEndereco">Buscar</button>
    </div>

    <div v-if="carregando" class="loader"></div>

    <div v-if="endereco && !carregando">
      <h2>Informações do Endereço:</h2>
      <p><strong>Logradouro:</strong> {{ endereco.logradouro }}</p>
      <p><strong>Complemento:</strong> {{ endereco.complemento }}</p>
      <p><strong>Bairro:</strong> {{ endereco.bairro }}</p>
      <p><strong>Localidade:</strong> {{ endereco.localidade }}</p>
      <p><strong>UF:</strong> {{ endereco.uf }}</p>
      <p><strong>Estado:</strong> {{ endereco.estado }}</p>
      <p><strong>Região:</strong> {{ endereco.regiao }}</p>
      <p><strong>IBGE:</strong> {{ endereco.ibge }}</p>
      <p><strong>GIA:</strong> {{ endereco.gia }}</p>
      <p><strong>DDD:</strong> {{ endereco.ddd }}</p>
      <p><strong>SIAFI:</strong> {{ endereco.siafi }}</p>
    </div>

    <div v-if="erro && !carregando">
      <p>{{ erro }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cep: '',
      endereco: null,
      erro: '',
      carregando: false
    };
  },
  methods: {
    async buscarEndereco() {
      this.carregando = true;
      this.endereco = null;
      this.erro = '';

      try {
        const response = await fetch(`https://viacep.com.br/ws/${this.cep}/json/`);
        const data = await response.json();

        if (data.erro) {
          this.erro = 'CEP não encontrado!';
          this.endereco = null;
        } else {
          this.endereco = {
            logradouro: data.logradouro,
            complemento: data.complemento || 'N/A',
            bairro: data.bairro,
            localidade: data.localidade,
            uf: data.uf,
            estado: data.estado,
            regiao: data.regiao,
            ibge: data.ibge,
            gia: data.gia || 'N/A',
            ddd: data.ddd,
            siafi: data.siafi
          };
        }
      } catch (error) {
        this.erro = 'Erro ao buscar o endereço. Verifique o CEP e tente novamente.';
      } finally {
        this.carregando = false;
      }
    }
  }
};
</script>

<style scoped>
.cep-form {
  max-width: 400px;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

div {
  margin-bottom: 15px;
}

input {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
  margin-bottom: 10px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

button {
  padding: 10px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #339a6e;
}

p {
  font-size: 16px;
}

/* Loader */
.loader {
  border: 8px solid #f3f3f3;
  border-top: 8px solid #3498db;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
  margin: 20px auto;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>
