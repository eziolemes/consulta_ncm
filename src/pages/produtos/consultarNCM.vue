<template>
  <q-page class="q-pa-md">
    <q-card class="my-card">
      <q-card-section>
        <q-toolbar class="q-toolbar__title">
          <q-toolbar-title>Consulta de NCMs</q-toolbar-title>
        </q-toolbar>
      </q-card-section>

      <q-separator />

      <q-expansion-item
        expand-icon-class="text-black"
        icon="filter_list"
        label="Filtros"
        caption="Pesquisar"
      >
        <q-card-section class="row q-col-gutter-x-xs q-col-gutter-y-lg">
          <q-input
            autofocus
            type="number"
            v-model="body.ncmsArray"
            class="col-12 col-lg-12 col-md-12"
            outlined
            @input="getNCM"
            label="Código do Produto"
          />
        </q-card-section>
      </q-expansion-item>
      <q-separator />

      <!-- <q-card-actions align="right" class="row">
        <q-btn flat @click="pesquisar" icon="search" color="primary" label="Pesquisar" />
      </q-card-actions>-->

      <q-table
        :loading="loading"
        :dense="$q.screen.lt.md"
        :data="data"
        color="primary"
        :columns="columns"
        row-key="name"
        no-data-label="Nenhum Resultado Encontrado..."
        :rows-per-page-label="'Itens por Página'"
        :pagination-label="getPaginationLabel"
      >
        <!-- <template v-slot:header="props">
          <q-tr :props="props">
            <q-th auto-width />
            <q-th v-for="col in props.cols" :key="col.name" :props="props">{{ col.label }}</q-th>
          </q-tr>
        </template>
        <template v-slot:body="props">
          <q-tr :props="props">
            <q-td auto-width class="q-pa-md q-gutter-sm">
              <q-btn size="sm" color="red" round dense icon="delete">
                <q-tooltip
                  transition-show="rotate"
                  transition-hide="rotate"
                  content-class="bg-accent"
                >Excluir Cadastro</q-tooltip>
              </q-btn>
              <q-btn size="sm" color="warning" round dense icon="create">
                <q-tooltip
                  transition-show="rotate"
                  transition-hide="rotate"
                  content-class="bg-accent"
                >Editar Cadastro</q-tooltip>
              </q-btn>
            </q-td>
            <q-td v-for="col in props.cols" :key="col.name" :props="props">{{ col.value }}</q-td>
          </q-tr>
        </template>-->
      </q-table>
    </q-card>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      data: [],
      form: {},
      selected: [],
      loading: false,
      columns: [
        {
          name: "code",
          required: true,
          label: "Código",
          align: "left",
          field: row => row.code,
          sortable: true
        },
        {
          name: "shortDescription",
          required: true,
          label: "Descrição Breve",
          align: "left",
          field: row => row.shortDescription,
          sortable: true
        },
        {
          name: "active",
          required: true,
          label: "Status",
          align: "left",
          field: row => row.active,
          sortable: true
        },
        {
          name: "expiredDate",
          required: true,
          label: "Data de Expiração",
          align: "left",
          field: row => row.expiredDate,
          sortable: true
        },
        {
          name: "fullDescription",
          required: true,
          label: "Descrição Completa",
          align: "left",
          field: row => row.fullDescription,
          sortable: true
        }
      ],
      body: {
        ncmsArray: [21069029]
      }
    };
  },

  methods: {
    getPaginationLabel(firstRowIndex, endRowIndex, totalRowsNumber) {
      return `${firstRowIndex} - ${endRowIndex} de ${totalRowsNumber}`;
    },

    message(msg, type) {
      this.$q.notify({
        progress: true,
        timeout: 2500,
        message: msg,
        type: type,
        actions: [
          {
            label: "Ok",
            color: "yellow"
          }
        ]
      });
    },

    getNCM() {
      const url = "/ncm";
      this.$axios
        .get(url, this.body)
        .then(res => {
          this.data = res.data;
          this.message("Dados Obtidos", "positive");
        })
        .catch(err => {
          this.message("Erro Ao Obter Dados", 'negative');
        });
    }
  },

  mounted() {}
};
</script>
