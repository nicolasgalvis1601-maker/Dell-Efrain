<template>
  <q-layout view="lHh Lpr lFf" class="app-layout">


    <q-header class="main-header">

      <q-toolbar class="q-px-lg">

        <div class="brand-container">

          <div class="brand-icon">
            <q-icon name="build" size="28px" />
          </div>

          <div>
            <div class="brand-title">
              Taller Don Efraín
            </div>

            <div class="brand-subtitle">
              Control de servicios técnicos
            </div>
          </div>

        </div>

        <q-space />

        <q-btn
          unelevated
          color="amber-7"
          text-color="dark"
          icon="add"
          label="Nuevo servicio"
          class="new-service-btn"
          @click="nuevoServicio"
        />

      </q-toolbar>

    </q-header>


    <q-page-container>

      <q-page class="main-page q-pa-lg">

        <div class="page-heading q-mb-lg">

          <div>

            <div class="page-title">
              Panel de servicios
            </div>

            <div class="page-description">
              Consulta y administra los equipos del taller.
            </div>

          </div>

          <q-btn
            outline
            color="dark"
            icon="add_circle_outline"
            label="Registrar equipo"
            class="desktop-register-btn"
            @click="nuevoServicio"
          />

        </div>


        <div class="row q-col-gutter-lg q-mb-xl">

          <!-- TOTAL -->
          <div class="col-12 col-md-4">

            <q-card class="summary-card summary-total">

              <q-card-section>

                <div class="summary-top">

                  <div class="summary-icon">
                    <q-icon
                      name="inventory_2"
                      size="28px"
                    />
                  </div>

                  <div class="summary-label">
                    Servicios registrados
                  </div>

                </div>

                <div class="summary-number">
                  {{ servicios.length }}
                </div>

              </q-card-section>

            </q-card>

          </div>


         
          <div class="col-12 col-md-4">

            <q-card class="summary-card summary-pending">

              <q-card-section>

                <div class="summary-top">

                  <div class="summary-icon">
                    <q-icon
                      name="pending_actions"
                      size="28px"
                    />
                  </div>

                  <div class="summary-label">
                    Pendientes de entrega
                  </div>

                </div>

                <div class="summary-number">
                  {{ contarPendientes() }}
                </div>

              </q-card-section>

            </q-card>

          </div>


    
          <div class="col-12 col-md-4">

            <q-card class="summary-card summary-paid">

              <q-card-section>

                <div class="summary-top">

                  <div class="summary-icon">
                    <q-icon
                      name="payments"
                      size="28px"
                    />
                  </div>

                  <div class="summary-label">
                    Servicios pagados
                  </div>

                </div>

                <div class="summary-number">
                  {{ contarPagados() }}
                </div>

              </q-card-section>

            </q-card>

          </div>

        </div>


     
        <q-card
          v-if="servicios.length === 0"
          class="empty-card"
        >

          <q-card-section class="text-center q-pa-xl">

            <div class="empty-icon">
              <q-icon
                name="phone_android"
                size="52px"
              />
            </div>

            <div class="empty-title">
              Todavía no hay servicios
            </div>

            <div class="empty-description">
              Registra el primer equipo para comenzar a gestionar
              los servicios del taller.
            </div>

            <q-btn
              unelevated
              color="dark"
              icon="add"
              label="Registrar primer servicio"
              class="q-mt-md"
              @click="nuevoServicio"
            />

          </q-card-section>

        </q-card>


     
        <div
          v-for="servicio in servicios"
          :key="servicio.id"
          class="service-wrapper"
        >

          <q-card class="service-card">

            <!-- BARRA SUPERIOR -->
            <div
              class="service-status-line"
              :class="obtenerClaseEstado(servicio.estadoEquipo)"
            />

            <q-card-section>

              <div class="row items-center justify-between">

                <!-- EQUIPO -->
                <div class="row items-center">

                  <div class="device-icon">

                    <q-icon
                      name="smartphone"
                      size="30px"
                    />

                  </div>

                  <div class="q-ml-md">

                    <div class="device-name">
                      {{ servicio.marca }} {{ servicio.modelo }}
                    </div>

                    <div class="client-name">
                      <q-icon
                        name="person_outline"
                        size="18px"
                        class="q-mr-xs"
                      />

                      {{ servicio.cliente }}
                    </div>

                  </div>

                </div>


       
                <q-chip
                  :color="obtenerColorEstado(servicio.estadoEquipo)"
                  text-color="white"
                  class="status-chip"
                >

                  <q-icon
                    :name="obtenerIconoEstado(servicio.estadoEquipo)"
                    class="q-mr-xs"
                  />

                  {{ servicio.estadoEquipo }}

                </q-chip>

              </div>

            </q-card-section>


            <q-separator />


            <q-card-section>

              <div class="row q-col-gutter-lg">

             
                <div class="col-12 col-md-6">

                  <div class="data-title">
                    Arreglos solicitados
                  </div>

                  <div class="chips-container">

                    <q-chip
                      v-for="arreglo in servicio.arreglos"
                      :key="arreglo"
                      color="blue-grey-1"
                      text-color="blue-grey-10"
                      class="repair-chip"
                    >
                      {{ arreglo }}
                    </q-chip>

                  </div>

                </div>


                <div class="col-12 col-md-3">

                  <div class="data-title">
                    Técnico
                  </div>

                  <div class="data-value">

                    <q-icon
                      name="engineering"
                      color="blue-grey-7"
                      class="q-mr-xs"
                    />

                    {{ servicio.tecnico }}

                  </div>

                </div>


                <!-- FECHA -->
                <div class="col-12 col-md-3">

                  <div class="data-title">
                    Fecha de recepción
                  </div>

                  <div class="data-value">

                    <q-icon
                      name="calendar_today"
                      color="blue-grey-7"
                      class="q-mr-xs"
                    />

                    {{ formatearFecha(servicio.fecha) }}

                  </div>

                </div>


                <!-- PRECIO -->
                <div class="col-12 col-sm-6 col-md-3">

                  <div class="data-title">
                    Valor del servicio
                  </div>

                  <div class="price-value">
                    ${{ formatearPrecio(servicio.precio) }}
                  </div>

                </div>


                <!-- PAGO -->
                <div class="col-12 col-sm-6 col-md-3">

                  <div class="data-title">
                    Pago
                  </div>

                  <div>

                    <q-chip
                      v-if="servicio.estadoPago === 'Pagado'"
                      color="green-1"
                      text-color="green-9"
                    >
                      <q-icon
                        name="check_circle"
                        class="q-mr-xs"
                      />

                      Pagado
                    </q-chip>

                    <q-chip
                      v-else-if="servicio.estadoPago === 'Abono'"
                      color="amber-2"
                      text-color="orange-10"
                    >
                      <q-icon
                        name="payments"
                        class="q-mr-xs"
                      />

                      Abono:
                      ${{ formatearPrecio(servicio.valorAbono) }}
                    </q-chip>

                    <q-chip
                      v-else
                      color="red-1"
                      text-color="red-9"
                    >
                      <q-icon
                        name="schedule"
                        class="q-mr-xs"
                      />

                      Pendiente
                    </q-chip>

                  </div>

                </div>


             
                <div class="col-12 col-sm-6 col-md-3">

                  <div class="data-title">
                    Método de pago
                  </div>

                  <div class="data-value">
                    {{ servicio.metodoPago }}
                  </div>

                </div>


               
                <div class="col-12 col-sm-6 col-md-3">

                  <div class="data-title">
                    Marca
                  </div>

                  <div class="data-value">
                    {{ servicio.marca }}
                  </div>

                </div>


                <!-- MODELO -->
                <div class="col-12 col-sm-6 col-md-3">

                  <div class="data-title">
                    Modelo
                  </div>

                  <div class="data-value">
                    {{ servicio.modelo }}
                  </div>

                </div>

              </div>


              
              <div
                v-if="servicio.observaciones"
                class="observation-container q-mt-lg"
              >

                <div class="data-title">
                  Observaciones
                </div>

                <div class="observation-text">
                  {{ servicio.observaciones }}
                </div>

              </div>


              <div
                v-if="
                  servicio.estadoEquipo === 'Entregado' &&
                  servicio.calificacion > 0
                "
                class="rating-container q-mt-lg"
              >

                <div class="data-title">
                  Calificación del cliente
                </div>

                <q-rating
                  :model-value="servicio.calificacion"
                  readonly
                  size="30px"
                  color="amber-7"
                  icon="star_border"
                  icon-selected="star"
                />

              </div>

            </q-card-section>


            <q-separator />


        
            <q-card-actions
              align="right"
              class="q-pa-md"
            >

              <template
                v-if="servicio.estadoEquipo !== 'Entregado'"
              >

                <q-btn
                  flat
                  color="dark"
                  icon="edit"
                  label="Editar"
                  @click="cargarServicio(servicio)"
                />

                <q-btn
                  flat
                  color="negative"
                  icon="delete_outline"
                  label="Eliminar"
                  @click="confirmarEliminar(servicio.id)"
                />

              </template>

              <div
                v-else
                class="closed-record"
              >

                <q-icon
                  name="lock"
                  size="18px"
                  class="q-mr-xs"
                />

                Registro cerrado

              </div>

            </q-card-actions>

          </q-card>

        </div>

      </q-page>

    </q-page-container>


   
    <q-dialog
      v-model="mostrarModal"
      persistent
    >

      <q-card class="form-card">

        <!-- TITULO -->
        <q-card-section class="form-header">

          <div class="row items-center">

            <div class="form-header-icon">
              <q-icon
                name="build_circle"
                size="28px"
              />
            </div>

            <div class="q-ml-md">

              <div class="form-title">
                {{ modoEdicion ? 'Editar servicio' : 'Registrar servicio' }}
              </div>

              <div class="form-subtitle">
                Información del equipo y del servicio
              </div>

            </div>

          </div>

        </q-card-section>


        <q-form
          @submit.prevent="guardarServicio"
        >

          <q-card-section class="q-pa-lg">

     
            <div class="form-section-title">
              <q-icon
                name="person"
                class="q-mr-sm"
              />

              Datos del cliente
            </div>

            <q-input
              v-model.trim="servicioActual.cliente"
              label="Nombre del cliente *"
              outlined
              class="q-mb-lg"
              :rules="[
                val => !!val || 'El nombre del cliente es obligatorio',
                val => val.length >= 3 || 'Mínimo 3 caracteres'
              ]"
            />


          
            <div class="form-section-title">
              <q-icon
                name="smartphone"
                class="q-mr-sm"
              />

              Datos del equipo
            </div>


            <q-select
              v-model="servicioActual.marca"
              label="Marca *"
              outlined
              class="q-mb-lg"
              :options="marcas"
              emit-value
              map-options
              :rules="[
                val => !!val || 'Selecciona la marca'
              ]"
            />


         
            <q-input
              v-model.trim="servicioActual.modelo"
              label="Modelo *"
              placeholder="Ej: Galaxy A15"
              outlined
              class="q-mb-lg"
              :rules="[
                val => !!val || 'El modelo es obligatorio',
                val => val.length >= 2 || 'Ingresa un modelo válido'
              ]"
            />


        
            <q-select
              v-model="servicioActual.arreglos"
              label="Arreglos por hacer *"
              outlined
              multiple
              use-chips
              class="q-mb-lg"
              :options="opcionesArreglos"
              :rules="[
                val =>
                  Array.isArray(val) && val.length > 0 ||
                  'Selecciona al menos un arreglo'
              ]"
            />


          
            <div class="form-section-title">
              <q-icon
                name="assignment"
                class="q-mr-sm"
              />

              Información del servicio
            </div>


            <q-select
              v-model="servicioActual.tecnico"
              label="Técnico que atendió *"
              outlined
              class="q-mb-lg"
              :options="[
                'Don Efraín',
                'Don Camilo Aguirre',
                'Doña Isabella de Aguirre'
              ]"
              :rules="[
                val => !!val || 'Selecciona el técnico'
              ]"
            />


         
            <q-input
              :model-value="formatearFecha(servicioActual.fecha)"
              label="Fecha y hora de recepción"
              outlined
              readonly
              disable
              class="q-mb-lg"
            >
              <template v-slot:prepend>
                <q-icon name="event" />
              </template>
            </q-input>


            <!-- PRECIO -->
            <q-input
              v-model.number="servicioActual.precio"
              label="Precio del servicio *"
              type="number"
              prefix="$"
              outlined
              class="q-mb-lg"
              :rules="[
                val =>
                  val !== null &&
                  val !== '' ||
                  'El precio es obligatorio',

                val =>
                  Number(val) >= 0 ||
                  'El precio no puede ser negativo'
              ]"
            />


            <div class="form-section-title">
              <q-icon
                name="payments"
                class="q-mr-sm"
              />

              Información del pago
            </div>


          
            <q-select
              v-model="servicioActual.metodoPago"
              label="Método de pago *"
              outlined
              class="q-mb-lg"
              :options="[
                'Efectivo',
                'Transferencia',
                'Tarjeta'
              ]"
              :rules="[
                val =>
                  !!val ||
                  'Selecciona el método de pago'
              ]"
            />


           
            <q-select
              v-model="servicioActual.estadoPago"
              label="Estado del pago *"
              outlined
              class="q-mb-lg"
              :options="[
                'Pagado',
                'Pendiente',
                'Abono'
              ]"
              :rules="[
                val =>
                  !!val ||
                  'Selecciona el estado del pago'
              ]"
              @update:model-value="manejarEstadoPago"
            />


          
            <q-input
              v-if="servicioActual.estadoPago === 'Abono'"
              v-model.number="servicioActual.valorAbono"
              label="Valor del abono *"
              type="number"
              prefix="$"
              outlined
              class="q-mb-lg"
              :rules="[
                val =>
                  val !== null &&
                  val !== '' ||
                  'Ingresa el valor del abono',

                val =>
                  Number(val) > 0 ||
                  'El abono debe ser mayor que $0',

                val =>
                  Number(val) <= Number(servicioActual.precio) ||
                  'El abono no puede superar el precio'
              ]"
            />


           
            <q-select
              v-model="servicioActual.estadoEquipo"
              label="Estado del equipo"
              outlined
              readonly
              disable
              class="q-mb-lg"
              :options="[
                'Recibido',
                'En reparación',
                'Listo para entregar',
                'Entregado'
              ]"
            />


           
            <q-input
              v-model.trim="servicioActual.observaciones"
              label="Observaciones"
              type="textarea"
              outlined
              autogrow
              class="q-mb-md"
              placeholder="Escribe aquí cualquier detalle importante..."
            />

          </q-card-section>


        
          <q-card-actions
            class="form-actions q-pa-lg"
            align="right"
          >

            <q-btn
              flat
              label="Cancelar"
              color="grey-8"
              @click="mostrarModal = false"
            />

            <q-btn
              unelevated
              type="submit"
              color="dark"
              icon="save"
              :label="modoEdicion ? 'Guardar cambios' : 'Registrar servicio'"
            />

          </q-card-actions>

        </q-form>

      </q-card>

    </q-dialog>


   
    <q-dialog
      v-model="mostrarConfirmacion"
      persistent
    >

      <q-card class="confirm-card">

        <q-card-section>

          <div class="row items-center">

            <div class="delete-icon">
              <q-icon
                name="delete_outline"
                size="30px"
              />
            </div>

            <div class="q-ml-md">

              <div class="confirm-title">
                Eliminar servicio
              </div>

              <div class="confirm-text">
                ¿Está seguro de eliminar este registro?
              </div>

            </div>

          </div>

        </q-card-section>


        <q-card-actions
          align="right"
          class="q-pa-md"
        >

          <q-btn
            flat
            label="Cancelar"
            color="grey-8"
            v-close-popup
          />

          <q-btn
            unelevated
            color="negative"
            label="Eliminar"
            icon="delete_outline"
            @click="eliminarServicio"
          />

        </q-card-actions>

      </q-card>

    </q-dialog>

  </q-layout>
</template>


<script setup>
import { ref } from 'vue'
import { useLocalStorage } from '@vueuse/core'




const servicios = useLocalStorage(
  'servicios-tecnicos-don-efrain',
  []
)



const mostrarModal = ref(false)
const modoEdicion = ref(false)
const mostrarConfirmacion = ref(false)

const servicioAEliminar = ref(null)




const marcas = [
  { label: 'Apple', value: 'Apple' },
  { label: 'Samsung', value: 'Samsung' },
  { label: 'Xiaomi', value: 'Xiaomi' },
  { label: 'Motorola', value: 'Motorola' },
  { label: 'Huawei', value: 'Huawei' },
  { label: 'Honor', value: 'Honor' },
  { label: 'Oppo', value: 'Oppo' },
  { label: 'Realme', value: 'Realme' },
  { label: 'Tecno', value: 'Tecno' },
  { label: 'Infinix', value: 'Infinix' },
  { label: 'ZTE', value: 'ZTE' },
  { label: 'Nokia', value: 'Nokia' },
  { label: 'Otra', value: 'Otra' }
]




const opcionesArreglos = [
  'Cambio de pantalla',
  'Cambio de batería',
  'Cambio de pin de carga',
  'Liberación',
  'Mantenimiento de software',
  'Cambio de flex',
  'Cambio de cámara',
  'Cambio de parlante',
  'Cambio de micrófono',
  'Cambio de botones',
  'Diagnóstico',
  'Otros'
]




const servicioActual = ref(
  crearServicioVacio()
)




function crearServicioVacio() {

  return {

    id: null,

    cliente: '',

    marca: '',

    modelo: '',

    arreglos: [],

    tecnico: '',

    fecha: '',

    precio: null,

    metodoPago: '',

    estadoPago: '',

    valorAbono: null,

    // Estado inicial obligatorio
    estadoEquipo: 'Recibido',

    // La calificación empieza en cero
    calificacion: 0,

    observaciones: ''
  }
}




function obtenerFechaActual() {

  const ahora = new Date()

  const anio = ahora.getFullYear()

  const mes = String(
    ahora.getMonth() + 1
  ).padStart(2, '0')

  const dia = String(
    ahora.getDate()
  ).padStart(2, '0')

  const hora = String(
    ahora.getHours()
  ).padStart(2, '0')

  const minutos = String(
    ahora.getMinutes()
  ).padStart(2, '0')

  return `${anio}-${mes}-${dia}T${hora}:${minutos}`
}




function nuevoServicio() {

  modoEdicion.value = false

  servicioActual.value =
    crearServicioVacio()

  servicioActual.value.fecha =
    obtenerFechaActual()

  mostrarModal.value = true
}




function guardarServicio() {

  if (modoEdicion.value) {

    editarServicio()

  } else {

    agregarServicio()

  }

  mostrarModal.value = false
}




function agregarServicio() {

  const nuevoServicio = {

    id: Date.now(),

    cliente:
      servicioActual.value.cliente,

    marca:
      servicioActual.value.marca,

    modelo:
      servicioActual.value.modelo,

    arreglos:
      [...servicioActual.value.arreglos],

    tecnico:
      servicioActual.value.tecnico,

    fecha:
      servicioActual.value.fecha,

    precio:
      Number(servicioActual.value.precio),

    metodoPago:
      servicioActual.value.metodoPago,

    estadoPago:
      servicioActual.value.estadoPago,

    valorAbono:
      servicioActual.value.estadoPago === 'Abono'
        ? Number(servicioActual.value.valorAbono)
        : 0,

    // Todo registro nuevo comienza recibido
    estadoEquipo: 'Recibido',

    // No se califica al registrar
    calificacion: 0,

    observaciones:
      servicioActual.value.observaciones
  }

  servicios.value.push(
    nuevoServicio
  )
}




function cargarServicio(servicio) {

  // Un entregado está cerrado
  if (
    servicio.estadoEquipo === 'Entregado'
  ) {
    return
  }

  modoEdicion.value = true

  servicioActual.value = {

    id: servicio.id,

    cliente:
      servicio.cliente,

    marca:
      servicio.marca,

    modelo:
      servicio.modelo,

    arreglos:
      Array.isArray(servicio.arreglos)
        ? [...servicio.arreglos]
        : [],

    tecnico:
      servicio.tecnico,

    fecha:
      servicio.fecha,

    precio:
      servicio.precio,

    metodoPago:
      servicio.metodoPago,

    estadoPago:
      servicio.estadoPago,

    valorAbono:
      servicio.valorAbono || 0,

    estadoEquipo:
      servicio.estadoEquipo,

    calificacion:
      servicio.calificacion || 0,

    observaciones:
      servicio.observaciones || ''
  }

  mostrarModal.value = true
}



function editarServicio() {

  const indice =
    servicios.value.findIndex(
      servicio =>
        servicio.id ===
        servicioActual.value.id
    )

  if (indice === -1) {
    return
  }

  // Protección adicional
  if (
    servicios.value[indice].estadoEquipo ===
    'Entregado'
  ) {
    return
  }

  servicios.value[indice] = {

    id:
      servicioActual.value.id,

    cliente:
      servicioActual.value.cliente,

    marca:
      servicioActual.value.marca,

    modelo:
      servicioActual.value.modelo,

    arreglos:
      [...servicioActual.value.arreglos],

    tecnico:
      servicioActual.value.tecnico,

    // La fecha original nunca cambia
    fecha:
      servicios.value[indice].fecha,

    precio:
      Number(servicioActual.value.precio),

    metodoPago:
      servicioActual.value.metodoPago,

    estadoPago:
      servicioActual.value.estadoPago,

    valorAbono:
      servicioActual.value.estadoPago === 'Abono'
        ? Number(servicioActual.value.valorAbono)
        : 0,

    estadoEquipo:
      servicioActual.value.estadoEquipo,

    calificacion:
      servicioActual.value.calificacion || 0,

    observaciones:
      servicioActual.value.observaciones
  }
}




function confirmarEliminar(id) {

  const servicio =
    servicios.value.find(
      item => item.id === id
    )

  if (!servicio) {
    return
  }

  // Entregados no se eliminan
  if (
    servicio.estadoEquipo ===
    'Entregado'
  ) {
    return
  }

  servicioAEliminar.value = id

  mostrarConfirmacion.value = true
}




function eliminarServicio() {

  if (!servicioAEliminar.value) {
    return
  }

  const indice =
    servicios.value.findIndex(
      servicio =>
        servicio.id ===
        servicioAEliminar.value
    )

  if (indice !== -1) {

    if (
      servicios.value[indice].estadoEquipo !==
      'Entregado'
    ) {

      servicios.value.splice(
        indice,
        1
      )

    }
  }

  servicioAEliminar.value = null

  mostrarConfirmacion.value = false
}




function manejarEstadoPago(valor) {

  if (valor !== 'Abono') {

    servicioActual.value.valorAbono =
      0

  }
}




function contarPendientes() {

  return servicios.value.filter(
    servicio =>
      servicio.estadoEquipo !==
      'Entregado'
  ).length
}


function contarPagados() {

  return servicios.value.filter(
    servicio =>
      servicio.estadoPago ===
      'Pagado'
  ).length
}



function obtenerColorEstado(estado) {

  switch (estado) {

    case 'Recibido':
      return 'blue-grey-7'

    case 'En reparación':
      return 'blue-8'

    case 'Listo para entregar':
      return 'amber-8'

    case 'Entregado':
      return 'green-7'

    default:
      return 'grey-7'
  }
}


function obtenerClaseEstado(estado) {

  switch (estado) {

    case 'Recibido':
      return 'status-received'

    case 'En reparación':
      return 'status-repair'

    case 'Listo para entregar':
      return 'status-ready'

    case 'Entregado':
      return 'status-delivered'

    default:
      return 'status-default'
  }
}


function obtenerIconoEstado(estado) {

  switch (estado) {

    case 'Recibido':
      return 'move_to_inbox'

    case 'En reparación':
      return 'build'

    case 'Listo para entregar':
      return 'inventory_2'

    case 'Entregado':
      return 'check_circle'

    default:
      return 'info'
  }
}




function formatearPrecio(precio) {

  if (
    precio === null ||
    precio === undefined ||
    precio === ''
  ) {

    return '0'

  }

  return Number(
    precio
  ).toLocaleString(
    'es-CO'
  )
}




function formatearFecha(fecha) {

  if (!fecha) {
    return ''
  }

  const fechaObjeto =
    new Date(fecha)

  if (
    Number.isNaN(
      fechaObjeto.getTime()
    )
  ) {

    return fecha

  }

  return fechaObjeto.toLocaleString(
    'es-CO',
    {
      dateStyle: 'short',
      timeStyle: 'short'
    }
  )
}
</script>


<style>
 */

body {
  margin: 0;
  font-family:
    Arial,
    Helvetica,
    sans-serif;
}

.app-layout {
  background: #eef1f5;
  color: #263238;
}

.main-page {
  max-width: 1400px;
  margin: 0 auto;
}




.main-header {
  background: #17202a;
  box-shadow:
    0 3px 12px rgba(0, 0, 0, 0.18);
}

.brand-container {
  display: flex;
  align-items: center;
}

.brand-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;

  display: flex;
  align-items: center;
  justify-content: center;

  background: #f5b642;
  color: #17202a;
}

.brand-title {
  margin-left: 14px;
  font-size: 21px;
  font-weight: 700;
  color: white;
}

.brand-subtitle {
  margin-left: 14px;
  font-size: 14px;
  color: #b8c2cc;
}

.new-service-btn {
  min-height: 44px;
  border-radius: 10px;
  font-size: 15px;
  font-weight: 600;
}




.page-heading {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.page-title {
  font-size: 28px;
  font-weight: 700;
  color: #17202a;
}

.page-description {
  margin-top: 5px;
  font-size: 16px;
  color: #68737d;
}

.desktop-register-btn {
  min-height: 44px;
  border-radius: 10px;
  font-size: 15px;
}




.summary-card {
  border-radius: 14px;
  border: none;
  overflow: hidden;

  box-shadow:
    0 4px 15px rgba(0, 0, 0, 0.06);

  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease;
}

.summary-card:hover {
  transform: translateY(-3px);

  box-shadow:
    0 8px 22px rgba(0, 0, 0, 0.1);
}

.summary-total {
  background: #ffffff;
  border-top: 5px solid #37474f;
}

.summary-pending {
  background: #fffaf0;
  border-top: 5px solid #f5a623;
}

.summary-paid {
  background: #f1faf4;
  border-top: 5px solid #2e9d59;
}

.summary-top {
  display: flex;
  align-items: center;
}

.summary-icon {
  width: 48px;
  height: 48px;

  border-radius: 12px;

  display: flex;
  align-items: center;
  justify-content: center;

  background: #edf0f2;
  color: #37474f;
}

.summary-pending .summary-icon {
  background: #fff0ce;
  color: #bd7410;
}

.summary-paid .summary-icon {
  background: #d9f1e1;
  color: #237a45;
}

.summary-label {
  margin-left: 13px;
  font-size: 16px;
  font-weight: 600;
  color: #56616b;
}

.summary-number {
  margin-top: 15px;
  font-size: 34px;
  font-weight: 700;
  color: #17202a;
}




.service-wrapper {
  margin-bottom: 20px;
}

.service-card {
  position: relative;

  background: white;

  border-radius: 14px;

  overflow: hidden;

  box-shadow:
    0 3px 15px rgba(0, 0, 0, 0.06);

  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease;
}

.service-card:hover {
  transform: translateY(-2px);

  box-shadow:
    0 7px 22px rgba(0, 0, 0, 0.1);
}

.service-status-line {
  height: 5px;
  width: 100%;
}

.status-received {
  background: #607d8b;
}

.status-repair {
  background: #1976d2;
}

.status-ready {
  background: #f5a623;
}

.status-delivered {
  background: #2e9d59;
}

.status-default {
  background: #9e9e9e;
}



.device-icon {
  width: 58px;
  height: 58px;

  display: flex;
  align-items: center;
  justify-content: center;

  border-radius: 15px;

  background: #edf1f4;
  color: #37474f;
}

.device-name {
  font-size: 20px;
  font-weight: 700;
  color: #17202a;
}

.client-name {
  margin-top: 5px;

  display: flex;
  align-items: center;

  font-size: 15px;
  color: #68737d;
}

.status-chip {
  font-size: 14px;
  font-weight: 600;
}



.data-title {
  margin-bottom: 7px;

  font-size: 14px;
  font-weight: 600;

  color: #7a858f;
}

.data-value {
  display: flex;
  align-items: center;

  font-size: 16px;
  color: #263238;
}

.price-value {
  font-size: 19px;
  font-weight: 700;
  color: #17202a;
}

.chips-container {
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

.repair-chip {
  font-size: 14px;
}

.observation-container {
  padding: 14px;

  border-radius: 10px;

  background: #f4f6f8;
}

.observation-text {
  font-size: 15px;
  line-height: 1.5;
  color: #37474f;
}

.rating-container {
  padding: 14px;

  border-radius: 10px;

  background: #fff9e8;
}

.closed-record {
  display: flex;
  align-items: center;

  padding: 8px 13px;

  border-radius: 8px;

  background: #edf6ef;

  color: #287c47;

  font-size: 14px;
  font-weight: 600;
}



.empty-card {
  border-radius: 15px;
  background: white;

  box-shadow:
    0 4px 15px rgba(0, 0, 0, 0.05);
}

.empty-icon {
  width: 80px;
  height: 80px;

  margin: auto;

  display: flex;
  align-items: center;
  justify-content: center;

  border-radius: 20px;

  background: #edf1f4;
  color: #607d8b;
}

.empty-title {
  margin-top: 18px;

  font-size: 21px;
  font-weight: 700;

  color: #263238;
}

.empty-description {
  max-width: 450px;

  margin: 8px auto 0;

  font-size: 16px;
  line-height: 1.5;

  color: #7a858f;
}



.form-card {
  width: 680px;
  max-width: 96vw;

  border-radius: 16px;

  overflow: hidden;
}

.form-header {
  padding: 20px 24px;

  background: #17202a;
  color: white;
}

.form-header-icon {
  width: 48px;
  height: 48px;

  display: flex;
  align-items: center;
  justify-content: center;

  border-radius: 12px;

  background: #f5b642;
  color: #17202a;
}

.form-title {
  font-size: 20px;
  font-weight: 700;
}

.form-subtitle {
  margin-top: 3px;

  font-size: 14px;

  color: #b8c2cc;
}

.form-section-title {
  display: flex;
  align-items: center;

  margin-bottom: 16px;
  padding-bottom: 8px;

  border-bottom: 1px solid #e1e5e8;

  font-size: 17px;
  font-weight: 700;

  color: #263238;
}

.form-section-title .q-icon {
  color: #546e7a;
}

.form-actions {
  background: #f7f8f9;
}


/* =============================================================
   CONFIRMACIÓN
============================================================= */

.confirm-card {
  width: 440px;
  max-width: 94vw;

  border-radius: 15px;
}

.delete-icon {
  width: 52px;
  height: 52px;

  display: flex;
  align-items: center;
  justify-content: center;

  border-radius: 12px;

  background: #ffebee;
  color: #c62828;
}

.confirm-title {
  font-size: 19px;
  font-weight: 700;
  color: #263238;
}

.confirm-text {
  margin-top: 4px;

  font-size: 15px;
  color: #757575;
}


/* =============================================================
   CAMPOS
============================================================= */

.q-field__label,
.q-field__native,
.q-field__input {
  font-size: 16px;
}

.q-field--outlined .q-field__control {
  border-radius: 9px;
}

.q-btn {
  font-size: 15px;
}


/* =============================================================
   RESPONSIVE
============================================================= */

@media (max-width: 700px) {

  .main-page {
    padding: 16px !important;
  }

  .page-heading {
    display: block;
  }

  .desktop-register-btn {
    display: none;
  }

  .page-title {
    font-size: 24px;
  }

  .brand-title {
    font-size: 18px;
  }

  .brand-subtitle {
    font-size: 13px;
  }

  .new-service-btn {
    min-width: auto;
  }

  .device-name {
    font-size: 18px;
  }

  .service-card .q-card-section {
    padding: 16px;
  }
}
</style>
