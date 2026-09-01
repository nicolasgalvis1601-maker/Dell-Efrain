```vue
<template>
  <q-layout view="hHh Lpr lFf" class="bg-grey-2">

    
    <q-drawer v-model="drawer" show-if-above :width="230" bordered>
      <div class="q-pa-md row items-center">
        <q-avatar color="primary" text-color="white">
          <q-icon name="handyman" />
        </q-avatar>
        <div class="q-ml-sm">
          <b>Don Efraín</b>
          <div class="text-caption text-grey">Servicio técnico</div>
        </div>
      </div>

      <q-separator />

      <q-list padding>
        <q-item clickable active active-class="bg-blue-1 text-primary">
          <q-item-section avatar>
            <q-icon name="dashboard" />
          </q-item-section>
          <q-item-section>Dashboard</q-item-section>
        </q-item>

        <q-item clickable @click="nuevoServicio">
          <q-item-section avatar>
            <q-icon name="add_circle" />
          </q-item-section>
          <q-item-section>Nuevo servicio</q-item-section>
        </q-item>

        <q-item>
          <q-item-section avatar>
            <q-icon name="phone_android" />
          </q-item-section>
          <q-item-section>Equipos</q-item-section>
        </q-item>

        <q-item>
          <q-item-section avatar>
            <q-icon name="payments" />
          </q-item-section>
          <q-item-section>Pagos</q-item-section>
        </q-item>
      </q-list>
    </q-drawer>


    <q-page-container>
      <q-page>

        <q-header class="bg-white text-dark">
          <q-toolbar>
            <q-btn
              flat
              round
              icon="menu"
              @click="drawer = !drawer"
            />

            <q-toolbar-title>
              <b>Dashboard</b>
              <div class="text-caption text-grey">
                Gestión del servicio técnico
              </div>
            </q-toolbar-title>

            <q-btn
              color="primary"
              icon="add"
              label="Nuevo servicio"
              @click="nuevoServicio"
            />
          </q-toolbar>
        </q-header>

        <div class="q-pa-lg">

     
          <q-banner
            class="bg-primary text-white rounded-borders q-mb-lg"
          >
            <template v-slot:avatar>
              <q-icon name="build_circle" />
            </template>

            <b class="text-h6">¡Bienvenido al taller! 👋</b>
            <div>
              Administra los servicios técnicos registrados.
            </div>
          </q-banner>

        
          <div class="row q-col-gutter-md q-mb-lg">

            <div
              v-for="dato in estadisticas"
              :key="dato.titulo"
              class="col-12 col-sm-6 col-md-3"
            >
              <q-card flat bordered>
                <q-card-section>
                  <q-icon
                    :name="dato.icono"
                    :color="dato.color"
                    size="30px"
                  />

                  <div class="text-h5 text-weight-bold q-mt-sm">
                    {{ dato.valor }}
                  </div>

                  <div class="text-grey">
                    {{ dato.titulo }}
                  </div>
                </q-card-section>
              </q-card>
            </div>

          </div>

          <!-- TÍTULO -->
          <div class="row items-center justify-between q-mb-md">
            <div>
              <div class="text-h5 text-weight-bold">
                Servicios registrados
              </div>
              <div class="text-grey">
                Equipos recibidos en el taller
              </div>
            </div>

            <q-btn
              flat
              color="primary"
              icon="add"
              label="Registrar"
              @click="nuevoServicio"
            />
          </div>

          
          <q-card
            v-if="!servicios.length"
            flat
            bordered
            class="q-pa-xl text-center"
          >
            <q-icon
              name="phone_android"
              size="70px"
              color="grey-5"
            />

            <div class="text-h6 q-mt-md">
              No hay servicios registrados
            </div>

            <q-btn
              color="primary"
              icon="add"
              label="Nuevo servicio"
              class="q-mt-md"
              @click="nuevoServicio"
            />
          </q-card>

          
          <q-card
            v-for="servicio in servicios"
            :key="servicio.id"
            flat
            bordered
            class="q-mb-md"
          >
            <q-card-section>

              <div class="row items-center justify-between">

                <div class="row items-center">
                  <q-avatar
                    color="blue-1"
                    text-color="primary"
                    size="50px"
                  >
                    <q-icon name="phone_android" />
                  </q-avatar>

                  <div class="q-ml-md">
                    <div class="text-h6 text-weight-bold">
                      {{ servicio.equipo }}
                    </div>

                    <div class="text-grey">
                      {{ servicio.cliente }}
                    </div>
                  </div>
                </div>

                <q-chip
                  :color="
                    servicio.estadoPago === 'Pagado'
                      ? 'positive'
                      : servicio.estadoPago === 'Abono'
                      ? 'warning'
                      : 'negative'
                  "
                  :text-color="
                    servicio.estadoPago === 'Abono'
                      ? 'dark'
                      : 'white'
                  "
                >
                  {{ servicio.estadoPago }}
                </q-chip>

              </div>

            </q-card-section>

            <q-separator />

            <q-card-section>

              <div class="row q-col-gutter-lg">

                <div class="col-12 col-sm-6 col-md-4">
                  <b>Reparación</b>
                  <div>{{ servicio.reparacion }}</div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <b>Técnico</b>
                  <div>{{ servicio.tecnico }}</div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <b>Fecha</b>
                  <div>{{ servicio.fecha }}</div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <b>Precio</b>
                  <div class="text-primary text-weight-bold">
                    ${{ formatearPrecio(servicio.precio) }}
                  </div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <b>Método de pago</b>
                  <div>{{ servicio.metodoPago }}</div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <b>Estado del equipo</b>
                  <div>{{ servicio.estadoEquipo }}</div>
                </div>

              </div>

              <div
                v-if="servicio.calificacion"
                class="q-mt-md"
              >
                <b>Calificación</b>

                <q-rating
                  :model-value="servicio.calificacion"
                  readonly
                  color="orange"
                  icon="star_border"
                  icon-selected="star"
                />
              </div>

              <div
                v-if="servicio.observaciones"
                class="bg-grey-2 q-pa-md q-mt-md rounded-borders"
              >
                <b>Observaciones</b>
                <div>{{ servicio.observaciones }}</div>
              </div>

            </q-card-section>

            <q-separator />

            <q-card-actions align="right">

              <q-btn
                flat
                color="primary"
                icon="edit"
                label="Editar"
                @click="cargarServicio(servicio)"
              />

              <q-btn
                flat
                color="negative"
                icon="delete"
                label="Eliminar"
                @click="eliminarServicio(servicio.id)"
              />

            </q-card-actions>
          </q-card>

        </div>
      </q-page>
    </q-page-container>

    <!-- MODAL -->
    <q-dialog v-model="mostrarModal">
      <q-card style="width: 650px; max-width: 95vw">

        <q-card-section class="bg-primary text-white">
          <div class="text-h6">
            {{ modoEdicion ? 'Editar servicio' : 'Nuevo servicio' }}
          </div>
        </q-card-section>

        <q-form @submit.prevent="guardarServicio">

          <q-card-section>

            <q-input
              v-model="servicioActual.cliente"
              label="Nombre del cliente *"
              outlined
              :rules="[v => !!v || 'Campo obligatorio']"
              class="q-mb-md"
            />

            <q-input
              v-model="servicioActual.equipo"
              label="Marca y modelo *"
              outlined
              :rules="[v => !!v || 'Campo obligatorio']"
              class="q-mb-md"
            />

            <q-select
              v-model="servicioActual.reparacion"
              label="Tipo de reparación *"
              outlined
              :options="reparaciones"
              :rules="[v => !!v || 'Selecciona una opción']"
              class="q-mb-md"
            />

            <q-select
              v-model="servicioActual.tecnico"
              label="Técnico *"
              outlined
              :options="tecnicos"
              :rules="[v => !!v || 'Selecciona una opción']"
              class="q-mb-md"
            />

            <q-input
              v-model="servicioActual.fecha"
              label="Fecha y hora *"
              type="datetime-local"
              outlined
              class="q-mb-md"
            />

            <q-input
              v-model.number="servicioActual.precio"
              label="Precio *"
              type="number"
              prefix="$"
              outlined
              class="q-mb-md"
            />

            <q-select
              v-model="servicioActual.metodoPago"
              label="Método de pago *"
              outlined
              :options="metodosPago"
              class="q-mb-md"
            />

            <q-select
              v-model="servicioActual.estadoPago"
              label="Estado del pago *"
              outlined
              :options="estadosPago"
              class="q-mb-md"
            />

            <q-select
              v-model="servicioActual.estadoEquipo"
              label="Estado del equipo *"
              outlined
              :options="estadosEquipo"
              class="q-mb-md"
            />

            <div class="q-mb-md">
              <div class="text-subtitle2">
                Calificación
              </div>

              <q-rating
                v-model="servicioActual.calificacion"
                color="orange"
                size="30px"
              />
            </div>

            <q-input
              v-model="servicioActual.observaciones"
              label="Observaciones"
              type="textarea"
              outlined
              autogrow
            />

          </q-card-section>

          <q-card-actions align="right">

            <q-btn
              flat
              label="Cancelar"
              @click="mostrarModal = false"
            />

            <q-btn
              color="primary"
              type="submit"
              icon="save"
              :label="modoEdicion ? 'Actualizar' : 'Guardar'"
            />

          </q-card-actions>

        </q-form>
      </q-card>
    </q-dialog>

  </q-layout>
</template>

<script setup>

import { ref, computed } from 'vue'
import { useLocalStorage } from '@vueuse/core'

const servicios = useLocalStorage(
  'servicios-tecnicos-don-efrain',
  []
)

const drawer = ref(true)
const mostrarModal = ref(false)
const modoEdicion = ref(false)

const servicioActual = ref({
  id: null,
  cliente: '',
  equipo: '',
  reparacion: '',
  tecnico: '',
  fecha: '',
  precio: 0,
  metodoPago: '',
  estadoPago: '',
  estadoEquipo: '',
  calificacion: 0,
  observaciones: ''
})

const reparaciones = [
  'Cambio de pantalla',
  'Cambio de batería',
  'Cambio de pin de carga',
  'Liberación',
  'Mantenimiento de software',
  'Cambio de flex',
  'Otros'
]

const tecnicos = [
  'Don Efraín',
  'Carlos',
  'Andrés'
]

const metodosPago = [
  'Efectivo',
  'Transferencia',
  'Tarjeta'
]

const estadosPago = [
  'Pagado',
  'Pendiente',
  'Abono'
]

const estadosEquipo = [
  'Recibido',
  'En reparación',
  'Listo para entregar',
  'Entregado'
]

const estadisticas = computed(() => [
  {
    titulo: 'Servicios',
    valor: servicios.value.length,
    icono: 'assignment',
    color: 'primary'
  },
  {
    titulo: 'Pendientes',
    valor: contarPendientes(),
    icono: 'schedule',
    color: 'orange'
  },
  {
    titulo: 'Pagados',
    valor: contarPagados(),
    icono: 'check_circle',
    color: 'positive'
  },
  {
    titulo: 'Ingresos',
    valor: '$' + formatearPrecio(totalIngresos()),
    icono: 'payments',
    color: 'purple'
  }
])

function limpiarFormulario() {

  servicioActual.value = {
    id: null,
    cliente: '',
    equipo: '',
    reparacion: '',
    tecnico: '',
    fecha: '',
    precio: 0,
    metodoPago: '',
    estadoPago: '',
    estadoEquipo: '',
    calificacion: 0,
    observaciones: ''
  }

}

function nuevoServicio() {

  modoEdicion.value = false
  limpiarFormulario()

  const ahora = new Date()

  const fecha =
    ahora.getFullYear() + '-' +
    String(ahora.getMonth() + 1).padStart(2, '0') + '-' +
    String(ahora.getDate()).padStart(2, '0') + 'T' +
    String(ahora.getHours()).padStart(2, '0') + ':' +
    String(ahora.getMinutes()).padStart(2, '0')

  servicioActual.value.fecha = fecha
  mostrarModal.value = true

}

function guardarServicio() {

  if (modoEdicion.value) {

    const index = servicios.value.findIndex(
      s => s.id === servicioActual.value.id
    )

    if (index !== -1) {
      servicios.value[index] = {
        ...servicioActual.value
      }
    }

  } else {

    servicios.value.push({
      ...servicioActual.value,
      id: Date.now()
    })

  }

  mostrarModal.value = false

}

function cargarServicio(servicio) {

  modoEdicion.value = true

  servicioActual.value = {
    ...servicio
  }

  mostrarModal.value = true

}

function eliminarServicio(id) {

  if (
    window.confirm(
      '¿Está seguro de eliminar este servicio?'
    )
  ) {

    servicios.value =
      servicios.value.filter(
        servicio => servicio.id !== id
      )

  }

}

function contarPendientes() {

  return servicios.value.filter(
    servicio =>
      servicio.estadoEquipo !== 'Entregado'
  ).length

}

function contarPagados() {

  return servicios.value.filter(
    servicio =>
      servicio.estadoPago === 'Pagado'
  ).length

}

function totalIngresos() {

  return servicios.value.reduce(
    (total, servicio) =>
      total + (Number(servicio.precio) || 0),
    0
  )

}

function formatearPrecio(precio) {

  return Number(precio || 0).toLocaleString(
    'es-CO'
  )

}

</script>

<style scoped>

body {
  margin: 0;
}

q-card {
  border-radius: 14px;
}

</style>
```




