<template>
  <v-container>
        <v-card v-if="tareas =! []">
          <v-card-text class="success">
            Listado de tareas
          </v-card-text>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="tareas"
              :items-per-page="5"
              class="elevation-1"
            ></v-data-table>
          </v-card-text>
        </v-card>
        <v-card>
          <v-card-text class="success">
            Listado de articulos SAP
          </v-card-text>
          <v-card-text>
            <v-data-table
              :headers="headersItems"
              :items="items"
              :items-per-page="10"
              class="elevation-1"
            ></v-data-table>
          </v-card-text>
        </v-card>

        <div class="text-center">
          <v-btn
            class="ma-2"
            :loading="loading"
            :disabled="loading"
            color="success"
            @click="ListarItems"
          >
            Actualizar <v-icon>cached</v-icon>
            <template v-slot:loader>
              <span>Buscando...</span>
            </template>
          </v-btn>
        </div>

  </v-container>
</template>

<script>
  import axios from "axios";
  export default {
    name: 'HelloWorld',

    data: () => ({
      headers: [
        { text: 'ID',value: 'idTarea'},
        { text: 'Nombre', value: 'nombre' },
        { text: 'Descripcion', value: 'descripcion' },
        { text: 'Condicion', value: 'condicion' }
      ],
      tareas: [],
      headersItems: [
        { text: 'Código',value: 'ItemCode'},
        { text: 'Descripción', value: 'ItemName' },
        { text: 'Codigo de barras', value: 'BarCode' },
        { text: 'Unidad de venta', value: 'SalesUnit' }
      ],
      items: [],

      //Loading Buttons
      loading: false,
    }),
    methods:{
      ListarTareas(){
        let objeto = this;

          axios({
            method: 'get',
            url: 'http://localhost:50337/api/tareas',
          }).then(function(response){
              console.log(response);
              objeto.tareas = response.data;
          })
      },
      ListarItems(){
        this.loading = true;
        this.items = [];
        let objeto = this;
        let sendData = { "CompanyDB": "SBODEMOMX",
                          "UserName": "manager",
                          "Password": "1234"
                      };
          axios({
            method: 'post',
            url: 'http://localhost:8086/api/SL/GetAxios?command=Items',
            headers: {
            'content-type': 'application/json'
          },
            data: sendData,
          }).then(function(response){
              console.log(response);
              objeto.loading = false;
              objeto.items = response.data.value;
          })
      },
    },
    mounted(){
      //this.ListarTareas();
      this.ListarItems();
    },
    watch: {
      
    }
  }
</script>
