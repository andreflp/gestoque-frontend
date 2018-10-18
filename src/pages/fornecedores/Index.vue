<template>
  <v-app>
    <v-card>
      <v-card-title>
        <h2>Fornecedores</h2>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="search"
          label="Pesquisar"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
      :headers="headers"
      :items="list"
      :search="search"
      > 
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.nome }}</td>
        <td class="text-xs-center">{{ props.item.email }}</td>
        <td class="text-xs-center">{{ props.item.cnpj | cnpj }}</td>
        <td class="text-xs-center">{{ props.item.telefone | phone }}</td>
        <td class="text-xs-center">{{ props.item.uf }}</td>
        <td class="text-xs-center">
          <v-tooltip left>
            <v-btn  flat icon color="primary" slot="activator" :to="`fornecedor/${props.item.id}`">
              <v-icon>edit</v-icon>
            </v-btn>
            <span>Editar</span>
          </v-tooltip>
          <v-tooltip right>  
            <v-btn flat icon color="red" slot="activator" @click="deleteFornecedor([props.item, props.item.id])">
              <v-icon>delete</v-icon>
            </v-btn>
            <span>Remover</span>
          </v-tooltip>
        </td>
      </template>
      </v-data-table>
    </v-card>
  </v-app>
</template>

<script>
import jsonFornecedores from "@/data/fornecedores.json";
import { mapState, mapActions, mapGetters } from "vuex";
import axios from "axios";

export default {
  data: () => ({
    search: "",
    dialog: false,
    pagination: {
      sortBy: "name"
    },
    headers: [
      {
        text: "Nome",
        align: "center",
        value: "nome"
      },
      { text: "E-mail", value: "email", align: "center" },
      { text: "CNPJ", value: "cnpj", align: "center" },
      { text: "Telefone ", value: "telefone", align: "center" },
      { text: "UF", value: "uf", align: "center" },
      { text: "Outros", align: "center" }
    ]
  }),

  computed: {
    ...mapState("Fornecedores", ["list"])
  },

  mounted() {
    this.setFornecedores();
  },

  methods: {
    ...mapActions("Fornecedores", ["setFornecedores", "deleteFornecedor"])
  }
};
</script>