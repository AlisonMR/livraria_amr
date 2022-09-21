<script>
    import axios from "axios";
    export default {
      data() {
        return {
          livros: [],
          novo_livro: {
            nome: "",
            editora: "",
            autor: "",
            categoria: "",
          },
        };
      },
      async created() {
        const livros = await axios.get(
          "https://backend-livrarias-tigre-azul.herokuapp.com/livros"
        );
        this.livros = livros.data;
      },
      methods: {
        async salvar() {
          const livro = {
            nome: this.novo_livro.nome,
            editora: this.novo_livro.editora,
            autor: this.novo_livro.autor,
            categoria: this.novo_livro.categoria,
          };
          const livro_criado = await axios.post(
            "https://backend-livrarias-tigre-azul.herokuapp.com/livros",
            livro
          );
          this.livros.push(livro_criado.data);
        },
        async excluir(livro) {
          await axios.delete(
            `https://backend-livrarias-tigre-azul.herokuapp.com/livros/${livro.id}`
          );
          const indice = this.livros.indexOf(livro);
          this.livros.splice(indice, 1);
        },
      },
    };
    </script>
    
    <template>
      <main>
        <div class="container">
          <div class="title">
            <h2>Cadastro de Livro</h2>
          </div>
          <div class="form-input">
            <input type="text" placeholder="Livro" v-model="novo_livro.nome" />
            <input type="text" placeholder="Autor" v-model="novo_livro.autor" />
            <select class="add" v-model="novo_livro.categoria">
              <option value="" disabled>Categoria</option>
              <option value="Ficção">Ficção</option>
              <option value="Romance">Romance</option>
              <option value="Comédia">Comédia</option>
              <option value="Terror">Terror</option>
              <option value="Aventura">Aventura</option>
              <option value="Suspense">Suspense</option>
              <option value="Didático">Didático</option>
              <option value="Geek">Geek</option>
              <option value="Mangás">Mangás</option>
              <option value="Mais Vendidos">Mais Vendidos</option>
              <option value="Em Outras Línguas">Em Outras Línguas</option>
            </select>
            <input type="text" placeholder="Editora" v-model="novo_livro.editora" />
            <button @click="salvar">Salvar</button>
          </div>
          <div class="list-liv">
            <table>
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Livros</th>
                  <th>Autor</th>
                  <th>Categoria</th>
                  <th>Editora</th>
                  <th>Excluir</th>
                </tr>
              </thead>
    
              <tbody>
                <tr v-for="livro in livros" :key="livro.id">
                  <td>{{ livro.id }}</td>
                  <td>{{ livro.nome }}</td>
                  <td>{{ livro.autor }}</td>
                  <td>{{ livro.categoria }}</td>
                  <td>{{ livro.editora }}</td>
                  <button @click="excluir(livro)">Excluir</button>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </main>
    </template>
    
    <style></style>