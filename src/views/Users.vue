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
                :loading-text="loadingText"
                class="elevation-1"
              >
                <template #[`item.actions`]="{ item }">
                  <v-tooltip top>
                    <template v-slot:activator="{ on, attrs }">
                      <v-icon
                        small
                        class="mr-2"
                        color="primary"
                        v-bind="attrs"
                        v-on="on"
                        @click="editCar(item)"
                        :disabled="processing"
                      >
                        mdi-pencil
                      </v-icon>
                    </template>
                    <span>Modificar</span>
                  </v-tooltip>
                  <v-tooltip top>
                    <template v-slot:activator="{ on, attrs }">
                      <v-icon
                        small
                        color="error"
                        v-bind="attrs"
                        v-on="on"
                        @click="deleteCar(item.id)"
                        :disabled="processing"
                      >
                        mdi-delete
                      </v-icon>
                    </template>
                    <span>Eliminar</span>
                  </v-tooltip>
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
      <v-dialog v-model="editDialog" max-width="500px" :persistent="processing">
        <v-card>
          <v-card-title>
            <span class="text-h5">Modificar Vehículo</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.car" label="Marca" :disabled="processing"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.car_model" label="Modelo" :disabled="processing"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.car_model_year" label="Año" :disabled="processing"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.car_color" label="Color" :disabled="processing"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.price" label="Precio" :disabled="processing"></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="closeEdit" :disabled="processing">Cancelar</v-btn>
            <v-btn color="blue darken-1" text @click="saveEdit" :loading="processing">Guardar</v-btn>
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

      <!-- Overlay de carga global -->
      <v-overlay :value="globalLoading" z-index="1000">
        <v-progress-circular indeterminate size="64"></v-progress-circular>
      </v-overlay>
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
      processing: false, // Para acciones específicas
      globalLoading: false, // Para carga general
      loadingText: 'Cargando vehículos...',
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
        { text: 'ID', value: 'id', align: 'start', sortable: true },
        { text: 'Marca', value: 'car', sortable: true },
        { text: 'Modelo', value: 'car_model', sortable: true },
        { text: 'Año', value: 'car_model_year', sortable: true },
        { text: 'Color', value: 'car_color', sortable: false },
        { text: 'Precio', value: 'price', sortable: true },
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
        this.loading = true;
        const response = await fetch('https://myfakeapi.com/api/cars1');
        const data = await response.json();
        this.cars = data.cars;
      } catch (error) {
        // Mensaje personalizado aquí
        this.showSnackbar('¡Oops! No se pudieron cargar los vehículos. Intenta nuevamente más tarde.', 'error');
        console.error('Detalles del error:', error);
      } finally {
        this.loading = false;
      }
    },
    
    editCar(item) {
      this.editedIndex = this.cars.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.editDialog = true;
    },
    
    async deleteCar(id) {
      if (!confirm('¿Estás seguro de que quieres eliminar este vehículo?')) return;
      
      try {
        this.processing = true;
        // Simulamos una llamada API
        await new Promise(resolve => setTimeout(resolve, 1000));
        
        this.cars = this.cars.filter(car => car.id !== id);
        this.showSnackbar('Vehículo eliminado exitosamente', 'success');
      } catch (error) {
        this.showSnackbar('Error al eliminar el vehículo', 'error');
      } finally {
        this.processing = false;
      }
    },
    
    closeEdit() {
      this.editDialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },
    
    async saveEdit() {
      try {
        this.processing = true;
        // Simulamos una llamada API
        await new Promise(resolve => setTimeout(resolve, 1000));
        
        if (this.editedIndex > -1) {
          Object.assign(this.cars[this.editedIndex], this.editedItem);
          this.showSnackbar('Vehículo modificado exitosamente', 'success');
        }
        this.closeEdit();
      } catch (error) {
        this.showSnackbar('Error al guardar los cambios', 'error');
      } finally {
        this.processing = false;
      }
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
.v-data-table {
  margin-top: 20px;
}

.action-icon {
  cursor: pointer;
  transition: opacity 0.3s;
}

.action-icon:hover {
  opacity: 0.7;
}

.action-icon:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>