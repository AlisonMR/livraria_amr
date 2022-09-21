<script>
    import axios from "axios";
    export default {
      data() {
        return {
          autores: [],
          novo_autor: {
            nome: "",
          },
        };
      },
      async created() {
        const autores = await axios.get(
          "https://backend-livrarias-tigre-azul.herokuapp.com/autores"
        );
        this.autores = autores.data;
      },
      methods: {
        async salvar() {
          const autor = {
            nome: this.novo_autor.nome,
          };
          const autor_criado = await axios.post(
            "https://backend-livrarias-tigre-azul.herokuapp.com/autores",
            autor
          );
          this.autores.push(autor_criado.data);
        },
        async excluir(autor) {
          await axios.delete(
            `https://backend-livrarias-tigre-azul.herokuapp.com/autores/${autor.id}`
          );
          const indice = this.autores.indexOf(autor);
          this.autores.splice(indice, 1);
        },
      },
    };
    </script>
    
    <template>
      <main>
        <div class="container">
          <div class="title">
            <h2>Autores</h2>
          </div>
          <div class="form-input">
            <input
              type="text"
              placeholder="Autor"
              @keyup.enter="salvar"
              v-model="novo_autor.nome"
            />
            <button @click="salvar">Salvar</button>
          </div>
          <div class="list-aut">
            <table>
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Autor</th>
                  <th>Excluir</th>
                </tr>
              </thead>
    
              <tbody>
                <tr v-for="autor in autores" :key="autor.id">
                  <td>{{ autor.id }}</td>
                  <td>{{ autor.nome }}</td>
                  <button class="button-excluir" @click="excluir(autor)">
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