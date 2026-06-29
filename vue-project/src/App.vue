<script setup>
import { ref, computed } from 'vue'

const tipoCambio = 3.7
const direccion = ref('usd-a-pen')
const monto = ref('')

const direccionTexto = computed(() =>
  direccion.value === 'usd-a-pen'
    ? 'De dólares (USD) a soles (PEN)'
    : 'De soles (PEN) a dólares (USD)'
)

const resultado = computed(() => {
  const valor = parseFloat(monto.value)
  if (monto.value === '') return ''
  if (Number.isNaN(valor) || valor <= 0) return ''

  if (direccion.value === 'usd-a-pen') {
    return `${(valor * tipoCambio).toFixed(2)} PEN`
  }

  return `${(valor / tipoCambio).toFixed(2)} USD`
})

const errorMensaje = computed(() => {
  if (monto.value === '') return ''
  const valor = parseFloat(monto.value)
  if (Number.isNaN(valor)) return 'Ingresa un número válido.'
  if (valor <= 0) return 'El valor debe ser mayor que cero.'
  return ''
})
</script>

<template>
  <div class="container mt-5">
    <h2 class="text-center mb-4">Conversor de Monedas</h2>

    <div class="card shadow mb-4">
      <div class="card-header bg-primary text-white">
        <h5 class="mb-0">Direccion de Conversion</h5>
      </div>
      <div class="card-body">
        <div class="row g-3">
          <div class="col-md-6">
            <label class="form-label fw-bold">Convertir de:</label>
            <select class="form-select" v-model="direccion">
              <option value="usd-a-pen">Dolares (USD) a Soles (PEN)</option>
              <option value="pen-a-usd">Soles (PEN) a Dolares (USD)</option>
            </select>
          </div>
          <div class="col-md-6">
            <label class="form-label fw-bold">Monto</label>
            <input
              type="number"
              class="form-control"
              v-model="monto"
              placeholder="Ingresa cantidad"
              min="0"
              step="0.1"
            />
          </div>
        </div>
      </div>
    </div>

    <div v-if="errorMensaje" class="alert alert-danger" role="alert">
      {{ errorMensaje }}
    </div>

    <div class="row">
      <div class="col-md-8 mx-auto">
        <div class="card text-center shadow">
          <div class="card-body p-5">
            <h5 class="card-title text-muted">{{ direccionTexto }}</h5>
            <p class="display-5 fw-bold text-primary">{{ resultado || '---' }}</p>
            <small class="text-muted">Tasa fija: 1 USD = 3.70 PEN</small>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 900px;
}

h2 {
  color: #1e293b;
  font-weight: 600;
  letter-spacing: -0.5px;
}

.card {
  border: none;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.12) !important;
}

.card-header {
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.card-header h5 {
  font-size: 1rem;
  font-weight: 600;
  letter-spacing: 0.5px;
}

.form-label {
  color: #334155;
  font-size: 0.95rem;
  margin-bottom: 0.5rem;
}

.form-control,
.form-select {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 0.75rem 1rem;
  font-size: 0.95rem;
  transition: all 0.2s ease;
}

.form-control:focus,
.form-select:focus {
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.alert {
  border: none;
  border-radius: 8px;
  padding: 1rem;
  font-size: 0.95rem;
}

.alert-danger {
  background-color: #fee2e2;
  color: #991b1b;
}

.text-center.shadow {
  border-radius: 12px;
  overflow: hidden;
}

.card-body {
  padding: 2.5rem 2rem;
}

.card-title {
  font-size: 0.9rem;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 1rem;
}

.display-5 {
  font-size: 2.5rem;
  color: #0f172a;
  font-weight: 700;
  margin: 1rem 0;
}

.text-muted {
  color: #94a3b8 !important;
  font-size: 0.9rem;
}

@media (max-width: 768px) {
  .card-body {
    padding: 1.5rem 1rem;
  }

  .display-5 {
    font-size: 1.8rem;
  }

  h2 {
    font-size: 1.5rem;
  }
}
</style>