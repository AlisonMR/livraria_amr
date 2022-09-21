<script>
  import axios from "axios";
  export default {
    data() {
      return {
        categorias: [],
        nova_categoria: "",
      };
    },
    async created() {
      const categorias = await axios.get(
        "https://backend-livrarias-tigre-azul.herokuapp.com/categorias"
      );
      this.categorias = categorias.data;
    },
    methods: {
      async salvar() {
        const categoria = {
          nome: this.nova_categoria,
        };
        const categoria_criada = await axios.post(
          "https://backend-livrarias-tigre-azul.herokuapp.com/categorias",
          categoria
        );
        this.categorias.push(categoria_criada.data);
      },
      async excluir(categoria) {
        await axios.delete(
          `https://backend-livrarias-tigre-azul.herokuapp.com/categorias/${categoria.id}`
        );
        const indice = this.categorias.indexOf(categoria);
        this.categorias.splice(indice, 1);
      },
    },
  };
  </script>
  <template>
    <main>
      <div class="container">
        <div class="title">
          <h2>Categorias</h2>
        </div>
        <div class="form-input">
          <input
            type="text"
            placeholder="Categoria"
            @keyup.enter="salvar"
            v-model="nova_categoria"
          />
          <button @click="salvar">Salvar</button>
        </div>
        <div class="list-cat">
          <table>
            <thead>
              <tr>
                <th>ID</th>
                <th>Categoria</th>
                <th>Excluir</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="categoria in categorias" :key="categoria.id">
                <td>{{ categoria.id }}</td>
                <td>{{ categoria.nome }}</td>
                <button class="button-excluir" @click="excluir(categoria)">
                  Excluir
                </button>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </main>
  </template>
  
  <style></style>  