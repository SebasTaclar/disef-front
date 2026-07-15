<template>
  <main class="servicios-page">
    <section class="servicios-hero">
      <h1>Servicios</h1>
      <p>Soluciones eléctricas e industriales integrales</p>
    </section>

    <section class="servicios-grid">
      <div class="servicio-card">
        <div class="servicio-icon">
          <i class="fas fa-headset"></i>
        </div>
        <h3>Asesoría Técnica</h3>
        <p>Contamos con un equipo de expertos que te orienta en la selección de productos y soluciones adaptadas a tus necesidades industriales.</p>
      </div>

      <div class="servicio-card">
        <div class="servicio-icon">
          <i class="fas fa-truck-fast"></i>
        </div>
        <h3>Envíos a Todo el País</h3>
        <p>Realizamos envíos a toda Colombia con cobertura nacional. Recibe tus productos donde los necesites de manera segura y oportuna.</p>
      </div>

      <div class="servicio-card">
        <div class="servicio-icon">
          <i class="fas fa-shield-halved"></i>
        </div>
        <h3>Garantía</h3>
        <p>Todos nuestros productos cuentan con garantía del fabricante. Si tienes algún inconveniente, lo solucionamos de forma rápida y efectiva.</p>
      </div>

      <div class="servicio-card">
        <div class="servicio-icon">
          <i class="fas fa-arrow-right-arrow-left"></i>
        </div>
        <h3>Cambios y Devoluciones</h3>
        <p>Si el producto no cumple con tus expectativas, gestionamos cambios y devoluciones bajo nuestros términos y condiciones.</p>
      </div>

      <div class="servicio-card">
        <div class="servicio-icon">
          <i class="fas fa-file-invoice"></i>
        </div>
        <h3>Cotizaciones Personalizadas</h3>
        <p>Solicita cotizaciones a medida para proyectos grandes o compras al por mayor. Te ofrecemos precios competitivos.</p>
      </div>

      <div class="servicio-card">
        <div class="servicio-icon">
          <i class="fas fa-users"></i>
        </div>
        <h3>Atención Empresarial</h3>
        <p>Programa especial para empresas con condiciones preferenciales, facturación directa y soporte dedicado.</p>
      </div>
    </section>

    <section class="servicios-cta">
      <h2>¿Necesitas ayuda?</h2>
      <p>Contáctanos y te atenderemos lo antes posible</p>
      <a href="/contacto" class="btn-contact">Contactar ahora</a>
    </section>
  </main>
</template>

<script setup lang="ts">
defineOptions({ name: 'ServiciosPage' })
</script>

<style scoped>
.servicios-page {
  background: #f7f8fa;
  min-height: 100vh;
}

.servicios-hero {
  background: linear-gradient(135deg, #0b0b0b, #1a1a1a);
  color: #fff;
  text-align: center;
  padding: 80px 20px 60px;
}

.servicios-hero h1 {
  font-size: 2.5rem;
  font-weight: 800;
  margin: 0 0 0.5rem;
}

.servicios-hero p {
  font-size: 1.1rem;
  color: #ccc;
  margin: 0;
}

.servicios-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 24px;
  max-width: 1200px;
  margin: 0 auto;
  padding: 60px 24px;
}

.servicio-card {
  background: #fff;
  border-radius: 16px;
  padding: 32px 28px;
  border: 1px solid #eee;
  transition: all 0.3s ease;
}

.servicio-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 32px rgba(0, 0, 0, 0.08);
  border-color: #FFC107;
}

.servicio-icon {
  width: 56px;
  height: 56px;
  border-radius: 14px;
  background: linear-gradient(135deg, #FFC107, #FFB300);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}

.servicio-icon i {
  font-size: 1.4rem;
  color: #0b0b0b;
}

.servicio-card h3 {
  margin: 0 0 0.75rem;
  font-size: 1.15rem;
  font-weight: 700;
  color: #111;
}

.servicio-card p {
  margin: 0;
  font-size: 0.92rem;
  line-height: 1.6;
  color: #666;
}

.servicios-cta {
  text-align: center;
  padding: 60px 20px 80px;
}

.servicios-cta h2 {
  margin: 0 0 0.5rem;
  font-size: 1.8rem;
  color: #111;
}

.servicios-cta p {
  margin: 0 0 1.5rem;
  color: #888;
  font-size: 1rem;
}

.btn-contact {
  display: inline-block;
  background: linear-gradient(135deg, #FFC107, #FFB300);
  color: #0b0b0b;
  font-weight: 700;
  padding: 14px 36px;
  border-radius: 12px;
  text-decoration: none;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.btn-contact:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 193, 7, 0.35);
}

@media (max-width: 768px) {
  .servicios-hero { padding: 60px 20px 40px; }
  .servicios-hero h1 { font-size: 1.8rem; }
  .servicios-grid { grid-template-columns: 1fr; padding: 40px 16px; }
}

@media (max-width: 480px) {
  .servicios-hero h1 { font-size: 1.5rem; }
}
</style>
