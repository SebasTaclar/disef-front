<template>
  <div class="terms-page">
    <div class="terms-container">
      <header class="terms-header">
        <button class="back-button" @click="goBack">
          <i class="fas fa-arrow-left"></i>
        </button>
        <h1 class="terms-title">Políticas y Términos</h1>
      </header>

      <nav class="terms-tabs">
        <button
          v-for="(tab, idx) in tabs"
          :key="idx"
          :class="['tab-btn', { active: activeTab === idx }]"
          @click="activeTab = idx"
        >
          <i :class="tab.icon"></i>
          {{ tab.label }}
        </button>
      </nav>

      <main class="terms-content">
        <!-- Política de Envíos -->
        <article v-if="activeTab === 0" class="tab-panel">
          <h2 class="section-title">Política de Envíos</h2>
          <p class="intro-text">En DISEF Comercializadora Industrial trabajamos para que tus productos lleguen de forma segura y en el menor tiempo posible.</p>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-map-marker-alt"></i> Cobertura</h3>
            <p class="block-text">Realizamos envíos a todo el territorio colombiano a través de transportadoras aliadas.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-clock"></i> Tiempo de entrega</h3>
            <p class="block-text">Los tiempos de entrega pueden variar según la ciudad de destino y la disponibilidad del producto.</p>
            <ul class="block-list">
              <li><strong>Bogotá:</strong> 1 a 2 días hábiles.</li>
              <li><strong>Principales ciudades:</strong> 2 a 5 días hábiles.</li>
              <li><strong>Municipios intermedios o zonas especiales:</strong> 3 a 8 días hábiles.</li>
            </ul>
            <p class="block-note">Los tiempos comienzan a contar una vez se confirme el pago o la aceptación de la cotización.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-dollar-sign"></i> Costo del envío</h3>
            <p class="block-text">El valor del envío será informado antes de confirmar el despacho y dependerá de:</p>
            <ul class="block-list">
              <li>Ciudad de destino.</li>
              <li>Peso y dimensiones del pedido.</li>
              <li>Tipo de producto.</li>
              <li>Transportadora seleccionada.</li>
            </ul>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-truck"></i> Seguimiento</h3>
            <p class="block-text">Una vez despachado el pedido, el cliente recibirá la información necesaria para realizar el seguimiento del envío.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-home"></i> Entrega</h3>
            <p class="block-text">Es responsabilidad del cliente verificar que la información de entrega suministrada sea correcta.</p>
            <p class="block-text">En caso de ausencia en la dirección registrada, la transportadora aplicará sus propias políticas de reintento o devolución.</p>
          </div>
        </article>

        <!-- Términos y Condiciones -->
        <article v-if="activeTab === 1" class="tab-panel">
          <h2 class="section-title">Términos y Condiciones</h2>
          <p class="intro-text">Bienvenido a DISEF Comercializadora Industrial.</p>
          <p class="intro-text">Al utilizar este sitio web aceptas los siguientes términos y condiciones.</p>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-globe"></i> Uso del sitio</h3>
            <p class="block-text">Este sitio tiene como finalidad presentar nuestro catálogo de productos y permitir la solicitud de cotizaciones.</p>
            <p class="block-text">La información publicada podrá modificarse en cualquier momento sin previo aviso.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-box"></i> Disponibilidad</h3>
            <p class="block-text">Los productos publicados están sujetos a disponibilidad de inventario.</p>
            <p class="block-text">La publicación de un producto no garantiza su existencia inmediata.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-tags"></i> Precios</h3>
            <p class="block-text">Los precios publicados podrán cambiar sin previo aviso.</p>
            <p class="block-text">En algunos productos el valor podrá mostrarse únicamente mediante cotización.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-file-invoice"></i> Cotizaciones</h3>
            <p class="block-text">Las solicitudes enviadas mediante WhatsApp o cualquier otro canal no constituyen una venta definitiva.</p>
            <p class="block-text">Toda cotización estará sujeta a:</p>
            <ul class="block-list">
              <li>Disponibilidad.</li>
              <li>Confirmación de precios.</li>
              <li>Condiciones comerciales.</li>
              <li>Tiempo de entrega.</li>
            </ul>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-copyright"></i> Propiedad intelectual</h3>
            <p class="block-text">Todo el contenido del sitio, incluyendo logotipos, fotografías, diseño, textos e imágenes, es propiedad de DISEF Comercializadora Industrial o de sus respectivos fabricantes y distribuidores.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-shield-alt"></i> Responsabilidad</h3>
            <p class="block-text">DISEF no será responsable por daños derivados del uso inadecuado de los productos comercializados.</p>
            <p class="block-text">El cliente deberá seguir las recomendaciones técnicas y normas de seguridad correspondientes.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-lock"></i> Protección de datos</h3>
            <p class="block-text">La información suministrada por nuestros clientes será tratada conforme a la legislación colombiana sobre protección de datos personales.</p>
          </div>
        </article>

        <!-- Cambios y Devoluciones -->
        <article v-if="activeTab === 2" class="tab-panel">
          <h2 class="section-title">Cambios y Devoluciones</h2>
          <p class="intro-text">En DISEF buscamos garantizar la satisfacción de nuestros clientes.</p>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-exchange-alt"></i> Cambios</h3>
            <p class="block-text">Se aceptarán solicitudes de cambio cuando:</p>
            <ul class="block-list">
              <li>El producto presente defectos de fabricación.</li>
              <li>El producto entregado no corresponda al solicitado.</li>
              <li>Exista un error atribuible a DISEF.</li>
            </ul>
            <p class="block-note">La solicitud deberá realizarse dentro de los cinco (5) días hábiles siguientes a la entrega.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-check-circle"></i> Condiciones</h3>
            <p class="block-text">El producto deberá entregarse sin uso, en perfecto estado, con su empaque original, con todos sus accesorios y con factura o documento de compra.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-times-circle"></i> No aplican cambios para</h3>
            <ul class="block-list">
              <li>Productos usados.</li>
              <li>Productos personalizados.</li>
              <li>Productos fabricados bajo pedido.</li>
              <li>Productos con daños ocasionados por mal uso.</li>
              <li>Elementos eléctricos instalados o manipulados.</li>
            </ul>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-award"></i> Garantías</h3>
            <p class="block-text">Las garantías serán atendidas conforme a las políticas del fabricante.</p>
            <p class="block-text">Cada producto podrá tener condiciones específicas de garantía.</p>
          </div>

          <div class="content-block">
            <h3 class="block-title"><i class="fas fa-undo"></i> Devoluciones de dinero</h3>
            <p class="block-text">Cuando aplique una devolución, el reembolso se realizará utilizando el mismo medio de pago empleado por el cliente o mediante transferencia bancaria, según corresponda.</p>
            <p class="block-text">Los tiempos de devolución dependerán de la entidad financiera y del método de pago utilizado.</p>
          </div>
        </article>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const activeTab = ref(0)

const tabs = [
  { label: 'Política de Envíos', icon: 'fas fa-truck' },
  { label: 'Términos y Condiciones', icon: 'fas fa-file-contract' },
  { label: 'Cambios y Devoluciones', icon: 'fas fa-exchange-alt' }
]

const goBack = () => {
  if (window.history.length > 1) {
    router.back()
    return
  }
  router.push('/')
}
</script>

<style scoped>
.terms-page {
  min-height: 100vh;
  background: #FFFFFF;
  padding: 100px 1rem 3rem;
}

.terms-container {
  max-width: 900px;
  margin: 0 auto;
}

.terms-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 32px;
}

.back-button {
  width: 44px;
  height: 44px;
  border-radius: 12px;
  border: 1px solid #E5E5E5;
  background: #F7F8FA;
  color: #111;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
  flex-shrink: 0;
}

.back-button:hover {
  background: #111;
  color: #fff;
  border-color: #111;
}

.terms-title {
  margin: 0;
  font-size: clamp(24px, 3.5vw, 32px);
  font-weight: 900;
  color: #111;
  letter-spacing: -0.5px;
}

.terms-tabs {
  display: flex;
  gap: 8px;
  margin-bottom: 40px;
  border-bottom: 1px solid #E5E5E5;
  padding-bottom: 0;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}

.tab-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 14px 24px;
  background: transparent;
  border: none;
  border-bottom: 3px solid transparent;
  color: #777;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.tab-btn i {
  font-size: 14px;
}

.tab-btn:hover {
  color: #111;
}

.tab-btn.active {
  color: #FFC107;
  border-bottom-color: #FFC107;
}

.tab-panel {
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}

.section-title {
  margin: 0 0 12px;
  font-size: clamp(22px, 3vw, 28px);
  font-weight: 900;
  color: #111;
  letter-spacing: -0.5px;
  font-style: italic;
}

.intro-text {
  font-size: 16px;
  color: #555;
  line-height: 1.7;
  margin: 0 0 24px;
}

.content-block {
  margin-bottom: 28px;
}

.block-title {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 0 0 12px;
  font-size: 16px;
  font-weight: 700;
  color: #111;
}

.block-title i {
  color: #FFC107;
  font-size: 16px;
  width: 20px;
  text-align: center;
}

.block-text {
  font-size: 15px;
  color: #555;
  line-height: 1.7;
  margin: 0 0 8px;
}

.block-note {
  font-size: 14px;
  color: #777;
  font-style: italic;
  margin: 8px 0 0;
}

.block-list {
  margin: 8px 0 0;
  padding-left: 20px;
}

.block-list li {
  font-size: 15px;
  color: #555;
  line-height: 1.7;
  margin: 4px 0;
}

.block-list li strong {
  color: #111;
}

@media (max-width: 768px) {
  .terms-page {
    padding: 80px 1rem 2rem;
  }

  .terms-tabs {
    gap: 0;
    margin-bottom: 24px;
  }

  .tab-btn {
    flex: 1;
    justify-content: center;
    padding: 12px 8px;
    font-size: 12px;
    gap: 6px;
  }

  .tab-btn i {
    display: none;
  }
}

@media (max-width: 480px) {
  .terms-header {
    margin-bottom: 20px;
  }

  .terms-title {
    font-size: 22px;
  }

  .tab-btn {
    font-size: 11px;
    padding: 10px 6px;
  }
}
</style>
