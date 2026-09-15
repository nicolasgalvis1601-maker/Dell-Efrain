<template>

  <q-layout view="lHh Lpr lFf" class="app-layout">

    <q-header class="app-header">

      <q-toolbar class="q-px-lg">

        <q-avatar size="48px" class="header-avatar">

          <q-icon name="build" size="28px" />

        </q-avatar>

        <q-toolbar-title class="q-ml-sm">

          <div class="header-title">

            Servicio Técnico Don Efraín

          </div>

          <div class="header-subtitle">

            Gestión de celulares y tablets

          </div>

        </q-toolbar-title>

        <q-btn

          unelevated

          color="white"

          text-color="primary"

          icon="person_add"

          label="Nuevo cliente"

          class="new-service-btn"

          @click="nuevoServicio"

        />

      </q-toolbar>

    </q-header>

    <q-page-container>

      <q-page class="main-page">

        <div class="dashboard-grid">

          <q-card class="dashboard-card dashboard-blue">

            <q-card-section>

              <div class="dashboard-icon">

                <q-icon name="assignment" />

              </div>

              <div class="dashboard-content">

                <div class="dashboard-label">

                  Total de servicios

                </div>

                <div class="dashboard-number">

                  {{ servicios.length }}

                </div>

              </div>

            </q-card-section>

          </q-card>

          <q-card class="dashboard-card dashboard-orange">

            <q-card-section>

              <div class="dashboard-icon">

                <q-icon name="pending_actions" />

              </div>

              <div class="dashboard-content">

                <div class="dashboard-label">

                  Pendientes de entrega

                </div>

                <div class="dashboard-number">

                  {{ contarPendientes() }}

                </div>

              </div>

            </q-card-section>

          </q-card>

          <q-card class="dashboard-card dashboard-green">

            <q-card-section>

              <div class="dashboard-icon">

                <q-icon name="payments" />

              </div>

              <div class="dashboard-content">

                <div class="dashboard-label">

                  Servicios pagados

                </div>

                <div class="dashboard-number">

                  {{ contarPagados() }}

                </div>

              </div>

            </q-card-section>

          </q-card>

        </div>

        <div class="section-header">

          <div>

            <div class="section-title">

              Servicios registrados

            </div>

            <div class="section-description">

              Administra los equipos recibidos en el taller.

            </div>

          </div>

        </div>

        <q-card

          v-if="servicios.length === 0"

          class="empty-card"

        >

          <q-icon

            name="phone_android"

            size="90px"

            color="grey-4"

          />

          <div class="empty-title">

            No hay servicios registrados

          </div>

          <div class="empty-description">

            Registra el primer equipo recibido en el taller.

          </div>

        </q-card>

        <div

          v-else

          class="services-grid"

        >

          <div

            v-for="servicio in servicios"

            :key="servicio.id"

            class="service-wrapper"

          >

            <q-card

              class="service-card"

              :class="{

                'payment-pending': servicio.estadoPago === 'Pendiente',

                'payment-abono': servicio.estadoPago === 'Abono',

                'payment-paid': servicio.estadoPago === 'Pagado',

                'service-delivered': servicio.estadoEquipo === 'Entregado'

              }"

            >

              <q-card-section class="service-header">

                <div class="service-main-info">

                  <q-avatar

                    size="62px"

                    class="device-avatar"

                  >

                    <q-icon

                      name="phone_android"

                      size="34px"

                    />

                  </q-avatar>

                  <div class="service-title-area">

                    <div class="service-device">

                      {{ servicio.marca }} {{ servicio.modelo }}

                    </div>

                    <div class="service-client">

                      <q-icon

                        name="person"

                        size="18px"

                        class="q-mr-xs"

                      />

                      {{ servicio.cliente }}

                    </div>

                  </div>

                </div>

                <div class="service-status">

                  <q-badge

                    v-if="servicio.estadoPago === 'Pagado'"

                    color="positive"

                    class="status-badge"

                  >

                    <q-icon

                      name="check_circle"

                      class="q-mr-xs"

                    />

                    Pagado

                  </q-badge>

                  <q-badge

                    v-else-if="servicio.estadoPago === 'Abono'"

                    color="warning"

                    text-color="dark"

                    class="status-badge"

                  >

                    <q-icon

                      name="payments"

                      class="q-mr-xs"

                    />

                    Abono

                  </q-badge>

                  <q-badge

                    v-else

                    color="negative"

                    class="status-badge"

                  >

                    <q-icon

                      name="warning"

                      class="q-mr-xs"

                    />

                    Pendiente

                  </q-badge>

                  <q-badge

                    v-if="servicio.estadoEquipo === 'Entregado'"

                    color="grey-8"

                    class="status-badge q-mt-xs"

                  >

                    <q-icon

                      name="lock"

                      class="q-mr-xs"

                    />

                    Registro cerrado

                  </q-badge>

                </div>

              </q-card-section>

              <q-separator />

              <q-card-section>

                <div class="service-info-grid">

                  <div class="info-item">

                    <div class="info-label">

                      Marca

                    </div>

                    <div class="info-value">

                      <q-icon

                        name="phone_iphone"

                        color="primary"

                      />

                      {{ servicio.marca }}

                    </div>

                  </div>

                  <div class="info-item">

                    <div class="info-label">

                      Modelo

                    </div>

                    <div class="info-value">

                      <q-icon

                        name="smartphone"

                        color="primary"

                      />

                      {{ servicio.modelo }}

                    </div>

                  </div>

                  <div class="info-item">

                    <div class="info-label">

                      Tipo de reparación

                    </div>

                    <div class="info-value">

                      <q-icon

                        name="build"

                        color="primary"

                      />

                      {{

                        servicio.reparacion === 'Otros'

                          ? servicio.reparacionOtro

                          : servicio.reparacion

                      }}

                    </div>

                  </div>

                  <div class="info-item">

                    <div class="info-label">

                      Técnico

                    </div>

                    <div class="info-value">

                      <q-icon

                        name="engineering"

                        color="primary"

                      />

                      {{ servicio.tecnico }}

                    </div>

                  </div>

                  <div class="info-item">

                    <div class="info-label">

                      Fecha de recepción

                    </div>

                    <div class="info-value">

                      <q-icon

                        name="event"

                        color="primary"

                      />

                      {{ formatearFecha(servicio.fecha) }}

                    </div>

                  </div>

                  <div class="info-item">

                    <div class="info-label">

                      Precio total

                    </div>

                    <div class="info-value price">

                      ${{ formatearPrecio(servicio.precio) }}

                    </div>

                  </div>

                  <div class="info-item">

                    <div class="info-label">

                      Método de pago

                    </div>

                    <div class="info-value">

                      <q-icon

                        name="account_balance_wallet"

                        color="primary"

                      />

                      {{ servicio.metodoPago }}

                    </div>

                  </div>

                  <div

                    v-if="servicio.estadoPago === 'Abono'"

                    class="info-item"

                  >

                    <div class="info-label">

                      Valor del abono

                    </div>

                    <div class="info-value price text-orange-9">

                      ${{ formatearPrecio(servicio.montoAbono) }}

                    </div>

                  </div>

                  <div

                    v-if="servicio.estadoPago === 'Abono'"

                    class="info-item"

                  >

                    <div class="info-label">

                      Saldo pendiente

                    </div>

                    <div class="info-value price text-negative">

                      ${{

                        formatearPrecio(

                          Math.max(

                            Number(servicio.precio || 0) -

                            Number(servicio.montoAbono || 0),

                            0

                          )

                        )

                      }}

                    </div>

                  </div>

                  <div class="info-item">

                    <div class="info-label">

                      Estado del equipo

                    </div>

                    <div class="info-value">

                      <q-badge

                        v-if="servicio.estadoEquipo === 'Recibido'"

                        color="blue"

                        class="state-badge"

                      >

                        Recibido

                      </q-badge>

                      <q-badge

                        v-else-if="servicio.estadoEquipo === 'En reparación'"

                        color="orange"

                        text-color="dark"

                        class="state-badge"

                      >

                        En reparación

                      </q-badge>

                      <q-badge

                        v-else-if="servicio.estadoEquipo === 'Listo para entregar'"

                        color="purple"

                        class="state-badge"

                      >

                        Listo para entregar

                      </q-badge>

                      <q-badge

                        v-else

                        color="positive"

                        class="state-badge"

                      >

                        Entregado

                      </q-badge>

                    </div>

                  </div>

                </div>

                <div

                  v-if="servicio.estadoEquipo === 'Entregado'"

                  class="rating-result"

                >

                  <div class="rating-title">

                    <q-icon

                      name="star"

                      color="orange"

                    />

                    Calificación del cliente

                  </div>

                  <div

                    v-if="!servicio.calificacion || servicio.calificacion === 0"

                    class="rating-prompt"

                  >

                    <div class="rating-message">

                      ¿Cómo califica el servicio?

                    </div>

                    <q-rating

                      v-model="servicio.calificacion"

                      :max="5"

                      size="34px"

                      color="orange"

                      icon="star_border"

                      icon-selected="star"

                      icon-half="star_half"

                      @update:model-value="guardarCalificacion(servicio)"

                    />

                  </div>

                  <div

                    v-else

                    class="rating-content"

                  >

                    <q-rating

                      v-model="servicio.calificacion"

                      :max="5"

                      size="34px"

                      color="orange"

                      icon="star_border"

                      icon-selected="star"

                      icon-half="star_half"

                      readonly

                    />

                    <span class="rating-number">

                      {{ servicio.calificacion }}/5

                    </span>

                    <span class="rating-thanks">

                      ¡Gracias por calificar!

                    </span>

                  </div>

                </div>

                <div

                  v-if="servicio.observaciones"

                  class="observation-box"

                >

                  <div class="info-label">

                    Observaciones

                  </div>

                  <div class="observation-text">

                    {{ servicio.observaciones }}

                  </div>

                </div>

              </q-card-section>

              <q-separator />

              <q-card-actions

                align="right"

                class="service-actions"

              >

                <q-btn

                  v-if="servicio.estadoEquipo !== 'Entregado'"

                  flat

                  color="primary"

                  icon="edit"

                  label="Editar"

                  @click="cargarServicio(servicio)"

                />

                <q-btn

                  v-if="servicio.estadoEquipo !== 'Entregado'"

                  flat

                  color="negative"

                  icon="delete"

                  label="Eliminar"

                  @click="confirmarEliminar(servicio)"

                />

                <div

                  v-if="servicio.estadoEquipo === 'Entregado'"

                  class="closed-message"

                >

                  <q-icon

                    name="lock"

                    class="q-mr-xs"

                  />

                  Servicio cerrado

                </div>

              </q-card-actions>

            </q-card>

          </div>

        </div>

      </q-page>

    </q-page-container>

    <q-dialog

      v-model="mostrarModal"

      persistent

      transition-show="scale"

      transition-hide="scale"

    >

      <q-card class="form-card">

        <q-card-section class="form-header">

          <div class="form-header-content">

            <q-avatar class="form-avatar">

              <q-icon

                name="phone_android"

                size="28px"

              />

            </q-avatar>

            <div>

              <div class="form-title">

                {{ modoEdicion ? 'Editar servicio' : 'Nuevo servicio' }}

              </div>

              <div class="form-subtitle">

                {{

                  modoEdicion

                    ? 'Actualiza la información del servicio'

                    : 'Registra un nuevo equipo en el taller'

                }}

              </div>

            </div>

          </div>

        </q-card-section>

        <q-form

          ref="formRef"

          autocomplete="off"

          greedy

          @submit.prevent="guardarServicio"

        >

          <q-card-section class="form-body">

            <q-input

              v-model="servicioActual.cliente"

              label="Nombre del cliente "

              outlined

              clearable

              class="form-field"

              lazy-rules

              :rules="[

                val =>

                  String(val ?? '').trim().length > 0 ||

                  'El nombre del cliente es obligatorio',

                val =>

                  String(val ?? '').trim().length >= 3 ||

                  'El nombre debe tener mínimo 3 caracteres'

              ]"

            >

              <template #prepend>

                <q-icon name="person" />

              </template>

            </q-input>

            <q-select

              v-model="servicioActual.marca"

              label="Marca del equipo "

              outlined

              class="form-field"

              :options="marcasDisponibles"

              lazy-rules

              :rules="[

                val =>

                  !!val ||

                  'Selecciona la marca del equipo'

              ]"

            >

              <template #prepend>

                <q-icon name="phone_android" />

              </template>

            </q-select>

            <q-input

              v-model="servicioActual.modelo"

              label="Modelo del equipo "

              placeholder="Ej: A15, iPhone 12, Redmi Note 10"

              outlined

              clearable

              class="form-field"

              lazy-rules

              :rules="[

                val =>

                  String(val ?? '').trim().length > 0 ||

                  'El modelo es obligatorio',

                val =>

                  String(val ?? '').trim().length >= 2 ||

                  'Ingresa un modelo válido'

              ]"

            >

              <template #prepend>

                <q-icon name="smartphone" />

              </template>

            </q-input>

            <q-select

              v-model="servicioActual.reparacion"

              @update:model-value="cambiarTipoReparacion"

              label="Tipo de reparación "

              outlined

              class="form-field"

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

              :rules="[

                val =>

                  !!val ||

                  'Selecciona el tipo de reparación'

              ]"

            >

              <template #prepend>

                <q-icon name="build" />

              </template>

            </q-select>

            <q-input

              v-if="servicioActual.reparacion === 'Otros'"

              v-model="servicioActual.reparacionOtro"

              label="Especifique la reparación"

              placeholder="Ej: Cambio de cámara, reparación de placa..."

              outlined

              clearable

              class="form-field"

              lazy-rules

              :rules="[

                val =>

                  String(val ?? '').trim().length > 0 ||

                  'Especifica la reparación',

                val =>

                  String(val ?? '').trim().length >= 3 ||

                  'Ingresa mínimo 3 caracteres'

              ]"

            >

              <template #prepend>

                <q-icon name="edit_note" />

              </template>

            </q-input>

            <q-select

              v-model="servicioActual.tecnico"

              label="Técnico que atendió "

              outlined

              class="form-field"

              :options="[

                'Don Efraín',

                'Carlos',

                'Andrés'

              ]"

              lazy-rules

              :rules="[

                val =>

                  !!val ||

                  'Selecciona el técnico'

              ]"

            >

              <template #prepend>

                <q-icon name="engineering" />

              </template>

            </q-select>

            <q-input

              :model-value="formatearFecha(servicioActual.fecha)"

              label="Fecha y hora de recepción"

              outlined

              readonly

              disable

              class="form-field"

            >

              <template #prepend>

                <q-icon name="event" />

              </template>

              <template #hint>

                La fecha se registra automáticamente y no puede modificarse.

              </template>

            </q-input>

            <q-input

              v-model.number="servicioActual.precio"

              label="Precio total "

              type="number"

              prefix="$"

              outlined

              class="form-field"

              lazy-rules

              :rules="[

                val =>

                  val !== null &&

                  val !== '' &&

                  val !== undefined ||

                  'El precio es obligatorio',

                val =>

                  Number(val) > 0 ||

                  'El precio debe ser mayor que $0'

              ]"

            >

              <template #prepend>

                <q-icon name="attach_money" />

              </template>

            </q-input>

            <q-select

              v-model="servicioActual.metodoPago"

              label="Método de pago "

              outlined

              class="form-field"

              :options="[

                'Efectivo',

                'Transferencia',

                'Tarjeta'

              ]"

              lazy-rules

              :rules="[

                val =>

                  !!val ||

                  'Selecciona el método de pago'

              ]"

            >

              <template #prepend>

                <q-icon name="account_balance_wallet" />

              </template>

            </q-select>

            <q-select

              v-model="servicioActual.estadoPago"

              @update:model-value="cambiarEstadoPago"

              label="Estado del pago "

              outlined

              class="form-field"

              :options="[

                'Pagado',

                'Pendiente',

                'Abono'

              ]"

              lazy-rules

              :rules="[

                val =>

                  !!val ||

                  'Selecciona el estado del pago'

              ]"

            >

              <template #prepend>

                <q-icon name="payments" />

              </template>

            </q-select>

            <q-input

              v-if="servicioActual.estadoPago === 'Abono'"

              v-model.number="servicioActual.montoAbono"

              label="Valor del abono "

              type="number"

              prefix="$"

              outlined

              class="form-field abono-field"

              lazy-rules

              :rules="[

                val =>

                  val !== null &&

                  val !== '' &&

                  val !== undefined ||

                  'Ingresa el valor del abono',

                val =>

                  Number(val) > 0 ||

                  'El abono debe ser mayor que $0',

                val =>

                  Number(val) <= Number(servicioActual.precio || 0) ||

                  'El abono no puede superar el precio total'

              ]"








            >

              <template #prepend>

                <q-icon

                  name="payments"

                  color="orange"

                />

              </template>

            </q-input>

            <q-select

              v-model="servicioActual.estadoEquipo"

              label="Estado del equipo "

              outlined

              class="form-field"

              :options="opcionesEstadoEquipo"

              :disable="!modoEdicion"

              emit-value

              map-options

              lazy-rules

              :rules="[

                val =>

                  !!val ||

                  'Selecciona el estado del equipo',

                val =>

                  val !== 'Entregado' ||

                  puedeEntregar ||

                  'No puedes entregar el equipo mientras exista un saldo pendiente'

              ]"

            >

              <template #prepend>

                <q-icon name="assignment" />

              </template>

              <template #hint>

                <span v-if="!modoEdicion">

                  Los equipos nuevos ingresan como "Recibido".

                </span>

                <span

                  v-else-if="!puedeEntregar"

                  class="text-negative"

                >

                  Debes completar el pago antes de entregar el equipo.

                </span>

              </template>

            </q-select>

            <q-input

              v-model="servicioActual.observaciones"

              label="Observaciones"

              type="textarea"

              outlined

              autogrow

              class="form-field"

              placeholder="Ej: Pantalla partida en la esquina superior..."

            >

              <template #prepend>

                <q-icon name="notes" />

              </template>

            </q-input>

          </q-card-section>

          <q-card-actions class="form-actions">

            <q-btn

              flat

              label="Cancelar"

              color="grey-7"

              size="md"

              @click="cerrarFormulario"

            />

            <q-btn

              unelevated

              type="submit"

              color="primary"

              icon="save"

              :label="modoEdicion ? 'Guardar cambios' : 'Registrar servicio'"

              size="md"

            />

          </q-card-actions>

        </q-form>

      </q-card>

    </q-dialog>

    <q-dialog v-model="mostrarConfirmacion">

      <q-card class="confirm-card">

        <q-card-section class="confirm-header">

          <q-avatar

            color="red-1"

            text-color="negative"

            size="54px"

          >

            <q-icon

              name="delete"

              size="28px"

            />

          </q-avatar>

          <div class="q-ml-md">

            <div class="confirm-title">

              Eliminar servicio

            </div>

            <div class="confirm-subtitle">

              Esta acción no se puede deshacer.

            </div>

          </div>

        </q-card-section>

        <q-card-section class="confirm-body">

          ¿Está seguro de que desea eliminar el servicio de

          <strong>{{ servicioParaEliminar?.cliente }}</strong>?

          <div

            v-if="servicioParaEliminar"

            class="confirm-device"

          >

            {{ servicioParaEliminar.marca }}

            {{ servicioParaEliminar.modelo }}

          </div>

        </q-card-section>

        <q-card-actions

          align="right"

          class="q-pa-md"

        >

          <q-btn

            flat

            label="Cancelar"

            color="grey-7"

            @click="cancelarEliminar"

          />

          <q-btn

            unelevated

            label="Eliminar"

            icon="delete"

            color="negative"

            @click="ejecutarEliminar"

          />

        </q-card-actions>

      </q-card>

    </q-dialog>

  </q-layout>

</template>

<script setup>

import { ref, computed } from 'vue'

import { useQuasar } from 'quasar'

import { useLocalStorage } from '@vueuse/core'

const $q = useQuasar()

const servicios = useLocalStorage(

  'servicios-tecnicos-don-efrain',

  []

)

const mostrarModal = ref(false)

const modoEdicion = ref(false)

const formRef = ref(null)

const mostrarConfirmacion = ref(false)

const servicioParaEliminar = ref(null)

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

  'ZTE'

]

const servicioActual = ref(servicioVacio())

function servicioVacio() {

  return {

    id: null,

    cliente: '',

    marca: '',

    modelo: '',

    reparacion: '',

    reparacionOtro: '',

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

const puedeEntregar = computed(() => {

  return servicioActual.value.estadoPago === 'Pagado'

})

const opcionesEstadoEquipo = computed(() => {

  return [

    {

      label: 'Recibido',

      value: 'Recibido'

    },

    {

      label: 'En reparación',

      value: 'En reparación'

    },

    {

      label: 'Listo para entregar',

      value: 'Listo para entregar'

    },

    {

      label: 'Entregado',

      value: 'Entregado',

      disable: !puedeEntregar.value

    }

  ]

})

function fechaActualISO() {

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

  servicioActual.value = servicioVacio()

  servicioActual.value.fecha = fechaActualISO()

  servicioActual.value.estadoEquipo = 'Recibido'

  mostrarModal.value = true

}

function cambiarTipoReparacion(valor) {

  servicioActual.value.reparacion = valor

  if (valor !== 'Otros') {

    servicioActual.value.reparacionOtro = ''

  }

}

function cambiarEstadoPago(valor) {

  servicioActual.value.estadoPago = valor

  if (valor !== 'Pagado') {

    if (

      servicioActual.value.estadoEquipo === 'Entregado'

    ) {

      servicioActual.value.estadoEquipo = 'Listo para entregar'

    }

  }

  if (valor !== 'Abono') {

    servicioActual.value.montoAbono = null

  }

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

      message: 'Revisa los campos obligatorios.',

      icon: 'error'

    })

    return

  }

  if (

    servicioActual.value.estadoEquipo === 'Entregado' &&

    !puedeEntregar.value

  ) {

    $q.notify({

      type: 'negative',

      message: 'No puedes entregar el equipo porque aún tiene un saldo pendiente.',

      caption: 'Completa el pago total antes de marcarlo como entregado.',

      icon: 'payments'

    })

    return

  }

  if (

    servicioActual.value.estadoPago !== 'Abono'

  ) {

    servicioActual.value.montoAbono = null

  }

  if (modoEdicion.value) {

    editarServicio()

  } else {

    agregarServicio()

  }

  const mensaje =

    modoEdicion.value

      ? 'Servicio actualizado correctamente'

      : 'Servicio registrado correctamente'

  mostrarModal.value = false

  $q.notify({

    type: 'positive',

    message: mensaje,

    icon: 'check_circle'

  })

}

function agregarServicio() {

  const nuevo = {

    id: Date.now(),

    cliente:

      servicioActual.value.cliente.trim(),

    marca:

      servicioActual.value.marca,

    modelo:

      servicioActual.value.modelo.trim(),

    reparacion:

      servicioActual.value.reparacion,

    reparacionOtro:

      servicioActual.value.reparacionOtro?.trim() || '',

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

    montoAbono:

      servicioActual.value.montoAbono,

    estadoEquipo:

      'Recibido',

    calificacion:

      0,

    observaciones:

      servicioActual.value.observaciones.trim()

  }

  servicios.value.push(nuevo)

}

function cargarServicio(servicio) {

  if (

    servicio.estadoEquipo === 'Entregado'

  ) {

    $q.notify({

      type: 'warning',

      message:

        'Este servicio ya fue entregado y está cerrado.',

      icon: 'lock'

    })

    return

  }

  modoEdicion.value = true

  servicioActual.value = {

    id: servicio.id,

    cliente: servicio.cliente,

    marca: servicio.marca,

    modelo: servicio.modelo,

    reparacion: servicio.reparacion,

    reparacionOtro:

      servicio.reparacionOtro ?? '',

    tecnico: servicio.tecnico,

    fecha: servicio.fecha,

    precio: servicio.precio,

    metodoPago: servicio.metodoPago,

    estadoPago: servicio.estadoPago,

    montoAbono:

      servicio.montoAbono ?? null,

    estadoEquipo:

      servicio.estadoEquipo,

    calificacion: 0,

    observaciones:

      servicio.observaciones ?? ''

  }

  mostrarModal.value = true

}

function editarServicio() {

  for (

    let i = 0;

    i < servicios.value.length;

    i++

  ) {

    if (

      servicios.value[i].id ===

      servicioActual.value.id

    ) {

      if (

        servicios.value[i].estadoEquipo ===

        'Entregado'

      ) {

        $q.notify({

          type: 'warning',

          message:

            'Este servicio ya está cerrado y no puede modificarse.',

          icon: 'lock'

        })

        return

      }

      servicios.value[i] = {

        id:

          servicioActual.value.id,

        cliente:

          servicioActual.value.cliente.trim(),

        marca:

          servicioActual.value.marca,

        modelo:

          servicioActual.value.modelo.trim(),

        reparacion:

          servicioActual.value.reparacion,

        reparacionOtro:

          servicioActual.value.reparacionOtro?.trim() || '',

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

        montoAbono:

          servicioActual.value.montoAbono,

        estadoEquipo:

          servicioActual.value.estadoEquipo,

        calificacion:

          servicioActual.value.calificacion,

        observaciones:

          servicioActual.value.observaciones.trim()

      }

      servicios.value = [

        ...servicios.value

      ]

      break

    }

  }

}

function guardarCalificacion(servicio) {

  if (servicio.estadoEquipo !== 'Entregado') {

    return

  }

  servicio.calificacion =

    Math.min(5, Math.max(0, Number(servicio.calificacion) || 0))

  servicios.value = [

    ...servicios.value

  ]

  $q.notify({

    type: 'positive',

    message: `Servicio calificado con ${servicio.calificacion} estrellas`,

    icon: 'star'

  })

}

function confirmarEliminar(servicio) {

  if (

    servicio.estadoEquipo ===

    'Entregado'

  ) {

    $q.notify({

      type: 'warning',

      message:

        'Un servicio entregado no se puede eliminar.',

      icon: 'lock'

    })

    return

  }

  servicioParaEliminar.value =

    servicio

  mostrarConfirmacion.value =

    true

}

function cancelarEliminar() {

  mostrarConfirmacion.value =

    false

  servicioParaEliminar.value =

    null

}

function ejecutarEliminar() {

  if (!servicioParaEliminar.value) {

    return

  }

  const id =

    servicioParaEliminar.value.id

  const servicio =

    servicios.value.find(

      s => s.id === id

    )

  if (

    servicio &&

    servicio.estadoEquipo ===

      'Entregado'

  ) {

    mostrarConfirmacion.value =

      false

    servicioParaEliminar.value =

      null

    $q.notify({

      type: 'warning',

      message:

        'Un servicio entregado no se puede eliminar.',

      icon: 'lock'

    })

    return

  }

  for (

    let i = 0;

    i < servicios.value.length;

    i++

  ) {

    if (

      servicios.value[i].id === id

    ) {

      servicios.value.splice(i, 1)

      break

    }

  }

  mostrarConfirmacion.value =

    false

  servicioParaEliminar.value =

    null

  $q.notify({

    type: 'positive',

    message:

      'Servicio eliminado correctamente',

    icon: 'delete'

  })

}

function contarPendientes() {

  let cantidad = 0

  for (

    let i = 0;

    i < servicios.value.length;

    i++

  ) {

    if (

      servicios.value[i].estadoEquipo !==

      'Entregado'

    ) {

      cantidad++

    }

  }

  return cantidad

}

function contarPagados() {

  let cantidad = 0

  for (

    let i = 0;

    i < servicios.value.length;

    i++

  ) {

    if (

      servicios.value[i].estadoPago ===

      'Pagado'

    ) {

      cantidad++

    }

  }

  return cantidad

}

function formatearPrecio(precio) {

  if (

    precio === null ||

    precio === undefined ||

    precio === ''

  ) {

    return '0'

  }

  return Number(precio)

    .toLocaleString('es-CO')

}

function formatearFecha(fechaISO) {

  if (!fechaISO) {

    return ''

  }

  const fecha =

    new Date(fechaISO)

  if (

    isNaN(fecha.getTime())

  ) {

    return fechaISO

  }

  return fecha.toLocaleString(

    'es-CO',

    {

      day: '2-digit',

      month: '2-digit',

      year: 'numeric',

      hour: '2-digit',

      minute: '2-digit'

    }

  )

}

</script>

<style>

body {

  margin: 0;

  font-family:

    Inter,

    Roboto,

    Arial,

    Helvetica,

    sans-serif;

  font-size: 18px;

  color: #263238;

}

.app-layout {

  background: #f4f6f9;

}

.app-header {

  background:

    linear-gradient(

      135deg,

      #1565c0,

      #1976d2

    );

  box-shadow:

    0 3px 15px

    rgba(0, 0, 0, 0.16);

}

.header-avatar {

  background: white;

  color: #1976d2;

}

.header-title {

  font-size: 22px;

  font-weight: 800;

  letter-spacing: 0.2px;

}

.header-subtitle {

  font-size: 14px;

  opacity: 0.85;

  margin-top: 2px;

}

.new-service-btn {

  border-radius: 10px;

  font-weight: 700;

}

.main-page {

  padding: 28px;

  max-width: 1450px;

  margin: 0 auto;

}

.dashboard-grid {

  display: grid;

  grid-template-columns:

    repeat(3, 1fr);

  gap: 20px;

  margin-bottom: 30px;

}

.dashboard-card {

  border-radius: 18px;

  border: 1px solid

    rgba(0, 0, 0, 0.05);

  box-shadow:

    0 4px 16px

    rgba(0, 0, 0, 0.06);

  transition:

    transform 0.2s,

    box-shadow 0.2s;

}

.dashboard-card:hover {

  transform: translateY(-3px);

  box-shadow:

    0 8px 25px

    rgba(0, 0, 0, 0.10);

}

.dashboard-card .q-card__section {

  display: flex;

  align-items: center;

  gap: 18px;

  padding: 22px;

}

.dashboard-blue {

  background: #eaf3ff;

}

.dashboard-orange {

  background: #fff4e5;

}

.dashboard-green {

  background: #eaf9ef;

}

.dashboard-icon {

  width: 55px;

  height: 55px;

  border-radius: 15px;

  display: flex;

  align-items: center;

  justify-content: center;

  font-size: 28px;

}

.dashboard-blue .dashboard-icon {

  background: #d5e8ff;

  color: #1976d2;

}

.dashboard-orange .dashboard-icon {

  background: #ffe4bd;

  color: #ef6c00;

}

.dashboard-green .dashboard-icon {

  background: #d5f1df;

  color: #16833a;

}

.dashboard-label {

  font-size: 16px;

  color: #607080;

  font-weight: 600;

}

.dashboard-number {

  font-size: 34px;

  font-weight: 800;

  line-height: 1.2;

  margin-top: 3px;

}

.dashboard-blue .dashboard-number {

  color: #1565c0;

}

.dashboard-orange .dashboard-number {

  color: #e65100;

}

.dashboard-green .dashboard-number {

  color: #16833a;

}

.section-header {

  display: flex;

  align-items: center;

  justify-content: space-between;

  margin-bottom: 22px;

}

.section-title {

  font-size: 26px;

  font-weight: 800;

  color: #263238;

}

.section-description {

  font-size: 16px;

  color: #607080;

  margin-top: 4px;

}

.empty-card {

  text-align: center;

  padding: 60px 25px;

  border-radius: 20px;

  border: 2px dashed #d7dce1;

  background: white;

}

.empty-title {

  font-size: 22px;

  font-weight: 700;

  color: #455a64;

  margin-top: 12px;

}

.empty-description {

  font-size: 17px;

  color: #78909c;

  margin: 8px 0 22px;

}

.services-grid {

  display: grid;

  grid-template-columns:

    repeat(3, minmax(0, 1fr));

  gap: 20px;

  align-items: stretch;

}

.service-wrapper {

  min-width: 0;

}

.service-card {

  height: 100%;

  border-radius: 18px;

  overflow: hidden;

  border-left: 6px solid #1976d2;

  background: white;

  box-shadow:

    0 4px 16px

    rgba(0, 0, 0, 0.07);

  transition:

    transform 0.2s,

    box-shadow 0.2s;

}

.service-card:hover {

  transform: translateY(-2px);

  box-shadow:

    0 8px 24px

    rgba(0, 0, 0, 0.10);

}

.payment-pending {

  border-left-color: #e53935;

}

.payment-abono {

  border-left-color: #fb8c00;

}

.payment-paid {

  border-left-color: #21a354;

}

.service-delivered {

  opacity: 0.92;

}

.service-header {

  padding: 22px;

}

.service-main-info {

  display: flex;

  align-items: center;

}

.device-avatar {

  background:

    linear-gradient(

      135deg,

      #1976d2,

      #42a5f5

    );

  color: white;

}

.service-title-area {

  margin-left: 16px;

  min-width: 0;

}

.service-device {

  font-size: 22px;

  font-weight: 800;

  color: #263238;

  overflow-wrap: break-word;

}

.service-client {

  display: flex;

  align-items: center;

  font-size: 16px;

  color: #607080;

  margin-top: 5px;

  overflow-wrap: break-word;

}

.service-status {

  display: flex;

  flex-direction: column;

  align-items: flex-end;

  margin-top: 15px;

}

.status-badge {

  font-size: 14px;

  padding: 8px 11px;

  border-radius: 8px;

}

.service-info-grid {

  display: grid;

  grid-template-columns:

    repeat(2, 1fr);

  gap: 20px 15px;

}

.info-item {

  min-width: 0;

}

.info-label {

  color: #607080;

  font-size: 15px;

  font-weight: 700;

  margin-bottom: 7px;

}

.info-value {

  display: flex;

  align-items: center;

  gap: 6px;

  color: #263238;

  font-size: 17px;

  font-weight: 500;

  overflow-wrap: anywhere;

}

.info-value.price {

  font-size: 19px;

  font-weight: 800;

}

.state-badge {

  font-size: 14px;

  padding: 7px 10px;

}

.rating-result {

  margin-top: 25px;

  padding: 18px;

  background: #fff9e6;

  border: 1px solid #ffe082;

  border-radius: 13px;

}

.rating-title {

  display: flex;

  align-items: center;

  gap: 7px;

  font-size: 17px;

  font-weight: 800;

  color: #5d4037;

  margin-bottom: 10px;

}

.rating-prompt {

  display: flex;

  flex-direction: column;

  gap: 10px;

}

.rating-message {

  font-size: 16px;

  color: #5d4037;

  font-weight: 600;

}

.rating-content {

  display: flex;

  align-items: center;

  flex-wrap: wrap;

  gap: 10px;

}

.rating-number {

  font-size: 19px;

  font-weight: 800;

  color: #e65100;

}

.rating-thanks {

  width: 100%;

  font-size: 14px;

  font-weight: 600;

  color: #6d4c41;

}

.observation-box {

  margin-top: 22px;

  background: #f6f8fa;

  border-radius: 12px;

  padding: 15px;

}

.observation-text {

  font-size: 16px;

  line-height: 1.5;

  color: #37474f;

  overflow-wrap: anywhere;

}

.service-actions {

  padding: 12px 18px;

}

.closed-message {

  display: flex;

  align-items: center;

  color: #607080;

  font-size: 15px;

  font-weight: 600;

}

.form-card {

  width: 680px;

  max-width: 96vw;

  border-radius: 20px;

  overflow: hidden;

}

.form-header {

  background:

    linear-gradient(

      135deg,

      #1565c0,

      #1976d2

    );

  color: white;

  padding: 22px;

}

.form-header-content {

  display: flex;

  align-items: center;

}

.form-avatar {

  background: rgba(255, 255, 255, 0.18);

}

.form-title {

  font-size: 23px;

  font-weight: 800;

}

.form-subtitle {

  font-size: 15px;

  opacity: 0.85;

  margin-top: 3px;

}

.form-body {

  padding: 25px;

  max-height: 72vh;

  overflow-y: auto;

}

.form-field {

  font-size: 18px;

  margin-bottom: 20px;

}

.form-field :deep(.q-field__label) {

  font-size: 17px;

}

.form-field :deep(.q-field__native),

.form-field :deep(.q-field__input) {

  font-size: 18px;

}

.form-field :deep(.q-field__bottom) {

  font-size: 15px;

}

.abono-field {

  background: #fffaf1;

  border-radius: 10px;

}

.form-actions {
  display: flex !important;
  justify-content: flex-end !important;
  align-items: center;
  gap: 8px;
  width: 100%;
  box-sizing: border-box;
  padding: 16px 25px;
  background: #fafafa;
  border-top: 1px solid #eeeeee;
}

.confirm-card {

  width: 460px;

  max-width: 92vw;

  border-radius: 18px;

}

.confirm-header {

  display: flex;

  align-items: center;

  padding: 22px;

}

.confirm-title {

  font-size: 22px;

  font-weight: 800;

}

.confirm-subtitle {

  color: #78909c;

  font-size: 14px;

  margin-top: 3px;

}

.confirm-body {

  font-size: 16px;

  line-height: 1.6;

}

.confirm-device {

  margin-top: 12px;

  padding: 10px 12px;

  border-radius: 8px;

  background: #f5f7f9;

  font-weight: 700;

  color: #37474f;

}

@media (max-width: 1100px) {

  .services-grid {

    grid-template-columns:

      repeat(2, minmax(0, 1fr));

  }

  .dashboard-grid {

    grid-template-columns: 1fr;

  }

}

@media (max-width: 650px) {

  .main-page {

    padding: 16px;

  }

  .q-toolbar {

    padding-left: 12px !important;

    padding-right: 12px !important;

  }

  .header-title {

    font-size: 17px;

  }

  .header-subtitle {

    font-size: 12px;

  }

  .new-service-btn {

    min-width: 42px;

  }

  .new-service-btn :deep(.q-btn__content) {

    font-size: 0;

  }

  .new-service-btn :deep(.q-icon) {

    margin-right: 0;

  }

  .section-header {

    align-items: flex-start;

    flex-direction: column;

    gap: 15px;

  }

  .section-title {

    font-size: 23px;

  }

  .services-grid {

    grid-template-columns: 1fr;

    gap: 20px;

  }

  .service-header {

    padding: 17px;

  }

  .service-main-info {

    align-items: flex-start;

  }

  .service-status {

    align-items: flex-start;

  }

  .service-info-grid {

    grid-template-columns: 1fr;

    gap: 18px;

  }

  .service-device {

    font-size: 19px;

  }

  .info-value {

    font-size: 16px;

  }

  .form-card {

    width: 100%;

    max-width: 100%;

    border-radius: 16px;

  }

  .form-body {

    padding: 18px;

    max-height: 70vh;

  }

  .form-title {

    font-size: 20px;

  }

}

</style>
