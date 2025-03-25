<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col cols="12">
            <h1 class="text-h4 mb-6">Registro de Vehículos</h1>
          </v-col>
          
          <!-- Tabla de vehículos -->
          <v-col cols="12">
            <v-card>
              <v-data-table
                :headers="headers"
                :items="cars"
                :loading="loading"
                loading-text="Cargando vehículos..."
                class="elevation-1"
              >
                <template #[`item.actions`]="{ item }">
                  <v-icon
                    small
                    class="mr-2 action-icon"
                    color="black"
                    v-bind="attrs"
                    v-on="on"
                    @click="editCar(item)"
                  >
                    mdi-pencil
                  </v-icon>
                  <v-icon
                    small
                    class="action-icon"
                    color="black"
                    v-bind="attrs"
                    v-on="on"
                    @click="deleteCar(item.id)"
                  >
                    mdi-delete
                  </v-icon>
                </template>
                
                <template #[`item.car_model_year`]="{ item }">
                  <v-chip :color="getYearColor(item.car_model_year)">
                    {{ item.car_model_year }}
                  </v-chip>
                </template>
              </v-data-table>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
      
      <!-- Diálogo para editar -->
      <v-dialog v-model="editDialog" max-width="500px">
        <v-card>
          <v-card-title>
            <span class="text-h5">Modificar Vehículo</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.car" label="Marca"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.car_model" label="Modelo"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.car_model_year" label="Año"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.car_color" label="Color"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.price" label="Precio"></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="closeEdit">Cancelar</v-btn>
            <v-btn color="blue darken-1" text @click="saveEdit">Guardar</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      
      <!-- Snackbar para notificaciones -->
      <v-snackbar v-model="snackbar.show" :color="snackbar.color" timeout="3000">
        {{ snackbar.message }}
        <template v-slot:action="{ attrs }">
          <v-btn text v-bind="attrs" @click="snackbar.show = false">
            Cerrar
          </v-btn>
        </template>
      </v-snackbar>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'CarRegistry',
  data() {
    return {
      cars: [],
      loading: true,
      editDialog: false,
      editedIndex: -1,
      editedItem: {
        id: '',
        car: '',
        car_model: '',
        car_model_year: '',
        car_color: '',
        price: ''
      },
      defaultItem: {
        id: '',
        car: '',
        car_model: '',
        car_model_year: '',
        car_color: '',
        price: ''
      },
      snackbar: {
        show: false,
        message: '',
        color: 'success'
      },
      headers: [
        { text: 'ID', value: 'id' },
        { text: 'Marca', value: 'car' },
        { text: 'Modelo', value: 'car_model' },
        { text: 'Año', value: 'car_model_year' },
        { text: 'Color', value: 'car_color' },
        { text: 'Precio', value: 'price' },
        { text: 'Acciones', value: 'actions', sortable: false }
      ]
    }
  },
  created() {
    this.fetchCars();
  },
  methods: {
    async fetchCars() {
      try {
        const response = await fetch('https://myfakeapi.com/api/cars');
        const data = await response.json();
        this.cars = data.cars;
        this.loading = false;
      } catch (error) {
        console.error('Error fetching cars:', error);
        this.showSnackbar('Error al cargar los vehículos', 'error');
        this.loading = false;
      }
    },
    editCar(item) {
      this.editedIndex = this.cars.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.editDialog = true;
    },
    deleteCar(id) {
      if (confirm('¿Estás seguro de que quieres eliminar este vehículo?')) {
        this.cars = this.cars.filter(car => car.id !== id);
        this.showSnackbar('Vehículo eliminado exitosamente');
      }
    },
    closeEdit() {
      this.editDialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },
    saveEdit() {
      if (this.editedIndex > -1) {
        Object.assign(this.cars[this.editedIndex], this.editedItem);
        this.showSnackbar('Vehículo modificado exitosamente');
      }
      this.closeEdit();
    },
    showSnackbar(message, color = 'success') {
      this.snackbar.message = message;
      this.snackbar.color = color;
      this.snackbar.show = true;
    },
    getYearColor(year) {
      const currentYear = new Date().getFullYear();
      if (year >= currentYear - 2) return 'green';
      if (year >= currentYear - 5) return 'orange';
      return 'red';
    }
  }
}
</script>

<style>
.modify-btn {
  background-color: #000000 !important;
  color: white !important;
  border-radius: 50%;
  width: 28px;
  height: 28px;
  min-width: 28px;
}

.delete-btn {
  background-color: #000000 !important;
  color: white !important;
  border-radius: 50%;
  width: 28px;
  height: 28px;
  min-width: 28px;
}

.v-icon {
  font-size: 16px;
}
.v-data-table {
  margin-top: 20px;
}
</style>