<script>
    import axios from "axios";
    export default {
      data() {
        return {
          editoras: [],
          nova_editora: {
            nome: "",
            site: "",
          },
        };
      },
      async created() {
        const editoras = await axios.get(
          "https://backend-livrarias-tigre-azul.herokuapp.com/editoras"
        );
        this.editoras = editoras.data;
      },
      methods: {
        async salvar() {
          const editora = {
            nome: this.nova_editora.nome,
            site: this.nova_editora.site,
          };
          const editora_criada = await axios.post(
            "https://backend-livrarias-tigre-azul.herokuapp.com/editoras",
            editora
          );
          this.editoras.push(editora_criada.data);
        },
        async excluir(editora) {
          await axios.delete(
            `https://backend-livrarias-tigre-azul.herokuapp.com/editoras/${editora.id}`
          );
          const indice = this.editoras.indexOf(editora);
          this.editoras.splice(indice, 1);
        },
      },
    };
    </script>
    
    <template>
      <main>
        <div class="container">
          <div class="title">
            <h2>Cadastro de Editora</h2>
          </div>
          <div class="form-input">
            <input
              type="text"
              placeholder="Nome"
              @keyup.enter="salvar"
              v-model="nova_editora.nome"
            />
            <input
              type="text"
              placeholder="Site"
              @keyup.enter="salvar"
              v-model="nova_editora.site"
            />
            <button @click="salvar">Salvar</button>
          </div>
          <div class="list-edit">
            <table>
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Nome</th>
                  <th>Site</th>
                  <th>Excluir</th>
                </tr>
              </thead>
    
              <tbody>
                <tr v-for="editora in editoras" :key="editora.id">
                  <td>{{ editora.id }}</td>
                  <td>{{ editora.nome }}</td>
                  <td>{{ editora.site }}</td>
                  <button class="button-excluir" @click="excluir(editora)">
                    Excluir
                  </button>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </main>
    </template>