<template>
  <q-layout view="lHh Lpr lFf" class="bg-grey-2">

    <!-- ENCABEZADO -->
    <q-header elevated class="bg-primary">
      <q-toolbar>

        <q-avatar color="white" text-color="primary">
          🔧
        </q-avatar>

        <q-toolbar-title>
          Servicio Técnico Don Efraín
          <div class="text-caption">
            Gestión de celulares y tablets
          </div>
        </q-toolbar-title>

        <q-btn
          color="white"
          text-color="primary"
          icon="add"
          label="Nuevo servicio"
          @click="nuevoServicio"
        />

      </q-toolbar>
    </q-header>


    <q-page-container>
      <q-page class="q-pa-md">


        <div class="row q-col-gutter-md q-mb-lg">

          <div class="col-12 col-sm-4">
            <q-card class="dashboard-card bg-blue-1">
              <q-card-section>
                <div class="text-subtitle2 text-grey-8">
                  Total de servicios
                </div>

                <div class="text-h4 text-primary">
                  {{ servicios.length }}
                </div>
              </q-card-section>
            </q-card>
          </div>

          <div class="col-12 col-sm-4">
            <q-card class="dashboard-card bg-orange-1">
              <q-card-section>
                <div class="text-subtitle2 text-grey-8">
                  Pendientes de entrega
                </div>

                <div class="text-h4 text-orange-9">
                  {{ contarPendientes() }}
                </div>
              </q-card-section>
            </q-card>
          </div>

          <div class="col-12 col-sm-4">
            <q-card class="dashboard-card bg-green-1">
              <q-card-section>
                <div class="text-subtitle2 text-grey-8">
                  Pagados
                </div>

                <div class="text-h4 text-green-9">
                  {{ contarPagados() }}
                </div>
              </q-card-section>
            </q-card>
          </div>

        </div>


        <div class="row items-center justify-between q-mb-md">

          <div>
            <div class="text-h5 text-weight-bold text-grey-9">
              Servicios registrados
            </div>

            <div class="text-grey-7">
              Administra los equipos recibidos en el taller
            </div>
          </div>

          <q-btn
            color="primary"
            icon="add"
            label="Registrar servicio"
            @click="nuevoServicio"
          />

        </div>


        <q-card
          v-if="servicios.length === 0"
          class="q-pa-xl text-center"
        >
          <q-icon
            name="phone_android"
            size="80px"
            color="grey-5"
          />

          <div class="text-h6 q-mt-md text-grey-7">
            No hay servicios registrados
          </div>

          <div class="text-grey-6 q-mb-md">
            Registra el primer equipo del taller.
          </div>

          <q-btn
            color="primary"
            icon="add"
            label="Nuevo servicio"
            @click="nuevoServicio"
          />
        </q-card>


        <div
          v-for="servicio in servicios"
          :key="servicio.id"
          class="q-mb-md"
        >

          <q-card
            class="service-card"
            :class="{
              'payment-pending': servicio.estadoPago === 'Pendiente',
              'payment-abono': servicio.estadoPago === 'Abono',
              'payment-paid': servicio.estadoPago === 'Pagado'
            }"
          >

            <q-card-section>

              <div class="row items-start justify-between">

                <div class="row items-center">

                  <q-avatar
                    color="primary"
                    text-color="white"
                    size="55px"
                  >
                    <q-icon name="phone_android" size="30px" />
                  </q-avatar>

                  <div class="q-ml-md">

                    <div class="text-h6 text-weight-bold">
                      {{ servicio.marca }} {{ servicio.modelo }}
                    </div>

                    <div class="text-grey-7 cliente-line">
                      Cliente: {{ servicio.cliente }}
                    </div>

                  </div>

                </div>

                <div class="column items-end">

                  <q-badge
                    v-if="servicio.estadoPago === 'Pagado'"
                    color="positive"
                    class="q-pa-sm"
                  >
                    <q-icon name="check_circle" class="q-mr-xs" />
                    Pagado
                  </q-badge>

                  <q-badge
                    v-else-if="servicio.estadoPago === 'Abono'"
                    color="warning"
                    text-color="dark"
                    class="q-pa-sm"
                  >
                    <q-icon name="payments" class="q-mr-xs" />
                    Abono
                  </q-badge>

                  <q-badge
                    v-else
                    color="negative"
                    class="q-pa-sm"
                  >
                    <q-icon name="warning" class="q-mr-xs" />
                    Pendiente
                  </q-badge>

                  <q-badge
                    v-if="servicio.estadoEquipo === 'Entregado'"
                    color="grey-7"
                    class="q-pa-sm q-mt-xs"
                  >
                    <q-icon name="lock" class="q-mr-xs" size="14px" />
                    Registro cerrado
                  </q-badge>

                </div>

              </div>

            </q-card-section>

            <q-separator />

            <q-card-section>

              <div class="row q-col-gutter-md">

                <div class="col-12 col-sm-6 col-md-4">
                  <div class="info-label">Marca</div>
                  <div class="info-value">
                    <q-icon name="phone_iphone" color="primary" class="q-mr-xs" />
                    {{ servicio.marca }}
                  </div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <div class="info-label">Modelo</div>
                  <div class="info-value">
                    {{ servicio.modelo }}
                  </div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <div class="info-label">Tipo de reparación</div>
                  <div class="info-value">
                    <q-icon name="build" color="primary" class="q-mr-xs" />
                    {{ servicio.reparacion }}
                  </div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <div class="info-label">Técnico</div>
                  <div class="info-value">
                    <q-icon name="engineering" color="primary" class="q-mr-xs" />
                    {{ servicio.tecnico }}
                  </div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <div class="info-label">Recepción</div>
                  <div class="info-value">
                    <q-icon name="event" color="primary" class="q-mr-xs" />
                    {{ formatearFecha(servicio.fecha) }}
                  </div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <div class="info-label">Precio</div>
                  <div class="info-value text-weight-bold">
                    ${{ formatearPrecio(servicio.precio) }}
                  </div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <div class="info-label">Método de pago</div>
                  <div class="info-value">
                    {{ servicio.metodoPago }}
                  </div>
                </div>

                <div
                  v-if="servicio.estadoPago === 'Abono'"
                  class="col-12 col-sm-6 col-md-4"
                >
                  <div class="info-label">Valor del abono</div>
                  <div class="info-value text-weight-bold">
                    ${{ formatearPrecio(servicio.montoAbono) }}
                  </div>
                </div>

                <div class="col-12 col-sm-6 col-md-4">
                  <div class="info-label">Estado del equipo</div>
                  <div class="info-value">
                    <span v-if="servicio.estadoEquipo === 'Recibido'">Recibido</span>
                    <span v-else-if="servicio.estadoEquipo === 'En reparación'">En reparación</span>
                    <span v-else-if="servicio.estadoEquipo === 'Listo para entregar'">Listo para entregar</span>
                    <span v-else>Entregado</span>
                  </div>
                </div>

              </div>
              <div
                v-if="servicio.estadoEquipo === 'Entregado' && servicio.calificacion > 0"
                class="q-mt-md"
              >
                <div class="info-label">
                  Calificación del cliente
                </div>

                <div>
                  <q-rating
                    :model-value="servicio.calificacion"
                    readonly
                    size="25px"
                    color="orange"
                    icon="star_border"
                    icon-selected="star"
                  />
                </div>
              </div>

              <div
                v-if="servicio.observaciones"
                class="q-mt-md observation-box"
              >
                <div class="info-label">Observaciones</div>
                <div class="observation-text">
                  {{ servicio.observaciones }}
                </div>
              </div>

            </q-card-section>

            <q-separator />

            <q-card-actions align="right">

              <q-btn
                flat
                color="primary"
                icon="edit"
                label="Editar"
                :disable="servicio.estadoEquipo === 'Entregado'"
                @click="cargarServicio(servicio)"
              >
                <q-tooltip v-if="servicio.estadoEquipo === 'Entregado'">
                  Un servicio entregado no se puede editar
                </q-tooltip>
              </q-btn>

              <q-btn
                flat
                color="negative"
                icon="delete"
                label="Eliminar"
                :disable="servicio.estadoEquipo === 'Entregado'"
                @click="confirmarEliminar(servicio.id)"
              >
                <q-tooltip v-if="servicio.estadoEquipo === 'Entregado'">
                  Un servicio entregado no se puede eliminar
                </q-tooltip>
              </q-btn>

            </q-card-actions>

          </q-card>

        </div>

      </q-page>
    </q-page-container>


    <q-dialog v-model="mostrarModal" persistent>

      <q-card class="form-card">

        <q-card-section class="bg-primary text-white">
          <div class="row items-center">
            <q-icon name="phone_android" size="30px" class="q-mr-sm" />
            <div class="text-h6">
              {{ modoEdicion ? 'Editar servicio' : 'Nuevo servicio' }}
            </div>
          </div>
        </q-card-section>

        <q-form ref="formRef" @submit.prevent="guardarServicio" greedy>

          <q-card-section>

            <q-input
              v-model="servicioActual.cliente"
              label="Nombre del cliente *"
              outlined
              class="q-mb-md campo-formulario"
              lazy-rules
              :rules="[
                val => !!val && val.trim().length > 0 || 'El nombre del cliente es obligatorio',
                val => val.trim().length >= 3 || 'Mínimo 3 caracteres'
              ]"
            >
              <template v-slot:prepend>
                <q-icon name="person" />
              </template>
            </q-input>


            <q-select
              v-model="servicioActual.marca"
              label="Marca del equipo *"
              outlined
              class="q-mb-md campo-formulario"
              :options="marcasDisponibles"
              lazy-rules
              :rules="[val => !!val || 'Selecciona la marca del equipo']"
            >
              <template v-slot:prepend>
                <q-icon name="phone_android" />
              </template>
            </q-select>

    
            <q-input
              v-model="servicioActual.modelo"
              label="Modelo del equipo *"
              placeholder="Ej: A15, iPhone 12, Redmi 10"
              outlined
              class="q-mb-md campo-formulario"
              lazy-rules
              :rules="[
                val => !!val && val.trim().length > 0 || 'El modelo es obligatorio'
              ]"
            >
              <template v-slot:prepend>
                <q-icon name="smartphone" />
              </template>
            </q-input>

  
            <q-select
              v-model="servicioActual.reparacion"
              label="Tipo de reparación *"
              outlined
              class="q-mb-md campo-formulario"
              :options="[
                'Cambio de pantalla',
                'Cambio de batería',
                'Cambio de pin de carga',
                'Liberación',
                'Mantenimiento de software',
                'Cambio de flex',
                'Otros'
              ]"
              lazy-rules
              :rules="[val => !!val || 'Selecciona el tipo de reparación']"
            />

            <q-select
              v-model="servicioActual.tecnico"
              label="Técnico que atendió *"
              outlined
              class="q-mb-md campo-formulario"
              :options="['Don Efraín', 'Carlos', 'Andrés']"
              lazy-rules
              :rules="[val => !!val || 'Selecciona el técnico']"
            />

    
            <q-input
              v-model="servicioActual.fecha"
              label="Fecha y hora de recepción (automática)"
              type="datetime-local"
              outlined
              readonly
              disable
              class="q-mb-md campo-formulario"
            >
              <template v-slot:prepend>
                <q-icon name="event" />
              </template>
              <template v-slot:hint>
                Esta fecha se asigna automáticamente y no puede modificarse.
              </template>
            </q-input>

            <q-input
              v-model.number="servicioActual.precio"
              label="Precio cobrado *"
              type="number"
              prefix="$"
              outlined
              class="q-mb-md campo-formulario"
              lazy-rules
              :rules="[
                val => val !== null && val !== '' && val !== undefined || 'El precio es obligatorio',
                val => Number(val) >= 0 || 'El precio no puede ser negativo'
              ]"
            />

            <q-select
              v-model="servicioActual.metodoPago"
              label="Método de pago *"
              outlined
              class="q-mb-md campo-formulario"
              :options="['Efectivo', 'Transferencia', 'Tarjeta']"
              lazy-rules
              :rules="[val => !!val || 'Selecciona el método de pago']"
            />

            <q-select
              v-model="servicioActual.estadoPago"
              label="Estado del pago *"
              outlined
              class="q-mb-md campo-formulario"
              :options="['Pagado', 'Pendiente', 'Abono']"
              lazy-rules
              :rules="[val => !!val || 'Selecciona el estado del pago']"
            />

      
            <q-input
              v-if="servicioActual.estadoPago === 'Abono'"
              v-model.number="servicioActual.montoAbono"
              label="Valor del abono *"
              type="number"
              prefix="$"
              outlined
              class="q-mb-md campo-formulario"
              lazy-rules
              :rules="[
                val => val !== null && val !== '' && val !== undefined || 'Ingresa el valor del abono',
                val => Number(val) > 0 || 'El abono debe ser mayor a 0',
                val => Number(val) <= Number(servicioActual.precio || 0) || 'El abono no puede ser mayor al precio total'
              ]"
            />
            <q-select
              v-model="servicioActual.estadoEquipo"
              label="Estado del equipo *"
              outlined
              class="q-mb-md campo-formulario"
              :options="['Recibido', 'En reparación', 'Listo para entregar', 'Entregado']"
              :disable="!modoEdicion"
              lazy-rules
              :rules="[val => !!val || 'Selecciona el estado del equipo']"
            >
              <template v-slot:hint>
                <span v-if="!modoEdicion">
                  Todo equipo nuevo ingresa como "Recibido".
                </span>
              </template>
            </q-select>

          
            <div
              v-if="modoEdicion && servicioActual.estadoEquipo === 'Entregado'"
              class="q-mb-md calificacion-box"
            >
              <div class="text-subtitle2 q-mb-sm">
                Calificación del cliente *
              </div>

              <q-rating
                v-model="servicioActual.calificacion"
                size="35px"
                color="orange"
                icon="star_border"
                icon-selected="star"
              />

              <div
                v-if="calificacionInvalida"
                class="text-negative text-caption q-mt-xs"
              >
                Debes registrar la calificación del cliente antes de marcar el equipo como entregado.
              </div>

              <div class="text-caption text-grey-6 q-mt-xs">
                El cliente evalúa el servicio en el momento de recoger el equipo.
              </div>
            </div>

            <q-input
              v-model="servicioActual.observaciones"
              label="Observaciones"
              type="textarea"
              outlined
              autogrow
              class="campo-formulario"
              placeholder="Ej: Pantalla partida en la esquina superior..."
            />

          </q-card-section>

          <q-card-actions align="right" class="q-pa-md">
            <q-btn
              flat
              label="Cancelar"
              color="grey-7"
              @click="cerrarFormulario"
            />

            <q-btn
              type="submit"
              color="primary"
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
import { ref } from 'vue'
import { useQuasar } from 'quasar'
import { useLocalStorage } from '@vueuse/core'

const $q = useQuasar()

const servicios = useLocalStorage('servicios-tecnicos-don-efrain', [])

const mostrarModal = ref(false)
const modoEdicion = ref(false)
const formRef = ref(null)
const calificacionInvalida = ref(false)

const marcasDisponibles = [
  'Samsung',
  'Apple (iPhone)',
  'Xiaomi',
  'Motorola',
  'Huawei',
  'Tecno',
  'Realme',
  'Oppo',
  'Vivo',
  'Nokia',
  'ZTE',
  'Otro'
]

function servicioVacio() {
  return {
    id: null,
    cliente: '',
    marca: '',
    modelo: '',
    reparacion: '',
    tecnico: '',
    fecha: '',
    precio: null,
    metodoPago: '',
    estadoPago: '',
    montoAbono: null,
    estadoEquipo: 'Recibido',
    calificacion: 0,
    observaciones: ''
  }
}

const servicioActual = ref(servicioVacio())

function fechaActualISO() {
  const ahora = new Date()
  const anio = ahora.getFullYear()
  const mes = String(ahora.getMonth() + 1).padStart(2, '0')
  const dia = String(ahora.getDate()).padStart(2, '0')
  const hora = String(ahora.getHours()).padStart(2, '0')
  const minutos = String(ahora.getMinutes()).padStart(2, '0')
  return `${anio}-${mes}-${dia}T${hora}:${minutos}`
}

function nuevoServicio() {
  modoEdicion.value = false
  calificacionInvalida.value = false

  servicioActual.value = servicioVacio()
  servicioActual.value.fecha = fechaActualISO()
  servicioActual.value.estadoEquipo = 'Recibido'

  mostrarModal.value = true
}

function cerrarFormulario() {
  mostrarModal.value = false
  if (formRef.value) {
    formRef.value.resetValidation()
  }
}

async function guardarServicio() {
  const valido = await formRef.value.validate()

  if (!valido) {
    $q.notify({
      type: 'negative',
      message: 'Revisa los campos marcados, hay información obligatoria sin completar.'
    })
    return
  }

  // Regla especial: si se marca "Entregado" debe existir una calificación
  if (
    servicioActual.value.estadoEquipo === 'Entregado' &&
    (!servicioActual.value.calificacion || servicioActual.value.calificacion <= 0)
  ) {
    calificacionInvalida.value = true
    $q.notify({
      type: 'negative',
      message: 'Debes calificar el servicio antes de marcarlo como entregado.'
    })
    return
  }

  calificacionInvalida.value = false

  // Si el estado de pago no es "Abono", no debe quedar un monto de abono residual
  if (servicioActual.value.estadoPago !== 'Abono') {
    servicioActual.value.montoAbono = null
  }

  if (modoEdicion.value) {
    editarServicio()
  } else {
    agregarServicio()
  }

  mostrarModal.value = false

  $q.notify({
    type: 'positive',
    message: modoEdicion.value ? 'Servicio actualizado correctamente' : 'Servicio registrado correctamente'
  })
}

function agregarServicio() {
  const nuevo = {
    id: Date.now(),
    cliente: servicioActual.value.cliente.trim(),
    marca: servicioActual.value.marca,
    modelo: servicioActual.value.modelo.trim(),
    reparacion: servicioActual.value.reparacion,
    tecnico: servicioActual.value.tecnico,
    fecha: servicioActual.value.fecha,
    precio: servicioActual.value.precio,
    metodoPago: servicioActual.value.metodoPago,
    estadoPago: servicioActual.value.estadoPago,
    montoAbono: servicioActual.value.montoAbono,
    estadoEquipo: servicioActual.value.estadoEquipo,
    calificacion: 0,
    observaciones: servicioActual.value.observaciones
  }

  servicios.value.push(nuevo)
}

function cargarServicio(servicio) {
  if (servicio.estadoEquipo === 'Entregado') {
    $q.notify({
      type: 'warning',
      message: 'Este servicio ya fue entregado y no se puede editar.'
    })
    return
  }

  modoEdicion.value = true
  calificacionInvalida.value = false

  servicioActual.value = {
    id: servicio.id,
    cliente: servicio.cliente,
    marca: servicio.marca,
    modelo: servicio.modelo,
    reparacion: servicio.reparacion,
    tecnico: servicio.tecnico,
    fecha: servicio.fecha,
    precio: servicio.precio,
    metodoPago: servicio.metodoPago,
    estadoPago: servicio.estadoPago,
    montoAbono: servicio.montoAbono,
    estadoEquipo: servicio.estadoEquipo,
    calificacion: servicio.calificacion,
    observaciones: servicio.observaciones
  }

  mostrarModal.value = true
}

function editarServicio() {
  for (let i = 0; i < servicios.value.length; i++) {
    if (servicios.value[i].id === servicioActual.value.id) {
      servicios.value[i] = {
        id: servicioActual.value.id,
        cliente: servicioActual.value.cliente.trim(),
        marca: servicioActual.value.marca,
        modelo: servicioActual.value.modelo.trim(),
        reparacion: servicioActual.value.reparacion,
        tecnico: servicioActual.value.tecnico,
        fecha: servicioActual.value.fecha,
        precio: servicioActual.value.precio,
        metodoPago: servicioActual.value.metodoPago,
        estadoPago: servicioActual.value.estadoPago,
        montoAbono: servicioActual.value.montoAbono,
        estadoEquipo: servicioActual.value.estadoEquipo,
        calificacion: servicioActual.value.calificacion,
        observaciones: servicioActual.value.observaciones
      }
      break
    }
  }
}

function confirmarEliminar(id) {
  const servicio = servicios.value.find(s => s.id === id)

  if (servicio && servicio.estadoEquipo === 'Entregado') {
    $q.notify({
      type: 'warning',
      message: 'Este servicio ya fue entregado y no se puede eliminar.'
    })
    return
  }

  $q.dialog({
    title: 'Eliminar servicio',
    message: '¿Está seguro de eliminar este servicio? Esta acción no se puede deshacer.',
    persistent: true,
    ok: { label: 'Eliminar', color: 'negative', flat: true },
    cancel: { label: 'Cancelar', color: 'grey-7', flat: true }
  }).onOk(() => {
    eliminarServicio(id)
  })
}

function eliminarServicio(id) {
  for (let i = 0; i < servicios.value.length; i++) {
    if (servicios.value[i].id === id) {
      servicios.value.splice(i, 1)
      break
    }
  }

  $q.notify({
    type: 'positive',
    message: 'Servicio eliminado correctamente'
  })
}

function contarPendientes() {
  let cantidad = 0
  for (let i = 0; i < servicios.value.length; i++) {
    if (servicios.value[i].estadoEquipo !== 'Entregado') {
      cantidad++
    }
  }
  return cantidad
}

function contarPagados() {
  let cantidad = 0
  for (let i = 0; i < servicios.value.length; i++) {
    if (servicios.value[i].estadoPago === 'Pagado') {
      cantidad++
    }
  }
  return cantidad
}

function formatearPrecio(precio) {
  if (!precio) {
    return '0'
  }
  return Number(precio).toLocaleString('es-CO')
}

function formatearFecha(fechaISO) {
  if (!fechaISO) return ''
  const fecha = new Date(fechaISO)
  if (isNaN(fecha.getTime())) return fechaISO
  return fecha.toLocaleString('es-CO', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  })
}
</script>

<style>
body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
}

.dashboard-card {
  border-radius: 14px;
  transition: 0.2s;
}

.dashboard-card:hover {
  transform: translateY(-2px);
}

.service-card {
  border-radius: 16px;
  overflow: hidden;
  border-left: 6px solid #1976d2;
  transition: 0.2s;
}

.service-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 18px rgba(0, 0, 0, 0.12);
}

.payment-pending {
  border-left-color: #f44336;
}

.payment-abono {
  border-left-color: #ff9800;
}

.payment-paid {
  border-left-color: #21ba45;
}

.cliente-line {
  font-size: 15px;
}

.info-label {
  color: #616161;
  font-size: 14px;
  margin-bottom: 4px;
  font-weight: 600;
}

.info-value {
  color: #212121;
  font-size: 17px;
}

.observation-box {
  background: #f5f5f5;
  border-radius: 10px;
  padding: 12px;
}

.observation-text {
  font-size: 16px;
}

.calificacion-box {
  background: #fff8e1;
  border-radius: 10px;
  padding: 14px;
}

.campo-formulario {
  font-size: 16px;
}

.campo-formulario :deep(.q-field__label) {
  font-size: 15px;
}

.form-card {
  width: 650px;
  max-width: 95vw;
  border-radius: 15px;
}

</style>
