<template>
  <section id="contact" class="contact-section">
    <div class="contact-container">
      <div class="contact-main">

        <!-- ── IZQUIERDA ── -->
        <div class="col-left">
          <header class="contact-header">
            <span class="contact-label">— CONTÁCTANOS —</span>
            <h2 class="contact-title">¿HABLAMOS DE <br/><span class="highlight">TU PROYECTO</span>?</h2>
            <div class="title-line"></div>
          </header>

          <p class="contact-text">Estamos listos para ayudarte a encontrar la mejor solución eléctrica e industrial. <strong>¡Hablemos!</strong></p>

          <div class="contact-whatsapp">
            <div class="whatsapp-icon">
              <i class="fab fa-whatsapp"></i>
            </div>
            <div class="whatsapp-info">
              <span class="whatsapp-title">¿Necesitas ayuda ahora?</span>
              <span class="whatsapp-text">Escríbenos por WhatsApp y te respondemos rápido.</span>
            </div>
          </div>

          <a href="https://wa.me/573229118168?text=Hola%2C%20me%20interesa%20obtener%20m%C3%A1s%20informaci%C3%B3n.%20%C2%BFPodr%C3%ADan%20atenderme%3F" target="_blank" class="btn-whatsapp">
            <i class="fab fa-whatsapp"></i> ESCRÍBENOS POR WHATSAPP <i class="fas fa-arrow-right"></i>
          </a>
        </div>

        <!-- ── CENTRO + DERECHA ── -->
        <div class="col-right">
          <div class="contact-info-list">
            <div class="info-row">
              <div class="info-icon"><i class="fas fa-map-marker-alt"></i></div>
              <div class="info-content">
                <span class="info-label">Dirección</span>
                <span class="info-value">Calle 19 # 10 - 50 <br> Oficina 202,  Bogotá</span>
              </div>
            </div>
            <div class="info-divider"></div>
            <div class="info-row">
              <div class="info-icon whatsapp"><i class="fab fa-whatsapp"></i></div>
              <div class="info-content">
                <span class="info-label">WhatsApp</span>
                <span class="info-value">+57 322 9118168</span>
              </div>
            </div>
            <div class="info-divider"></div>
            <div class="info-row">
              <div class="info-icon"><i class="fas fa-envelope"></i></div>
              <div class="info-content">
                <span class="info-label">Correo electrónico</span>
                <span class="info-value">cidisef@gmail.com</span>
              </div>
            </div>
            <div class="info-divider"></div>
            <div class="info-row">
              <div class="info-icon"><i class="fas fa-clock"></i></div>
              <div class="info-content">
                <span class="info-label">Horario de atención</span>
                <span class="info-value">Lunes a Viernes: <br/> 8:00 AM - 6:00 PM<br/>Sábados: 8:00 AM - 2:00 PM<br/>Domingos: Cerrado</span>
              </div>
            </div>
          </div>

          <div class="contact-image">
            <img src="https://res.cloudinary.com/dlwzazojt/image/upload/v1784082692/ChatGPT_Image_14_jul_2026__19_56_36-removebg-preview_whi0ju.png" alt="Productos DISEF" />
          </div>
        </div>

      </div>

      <!-- Mapa -->
      <div class="contact-map">
        <iframe
          src="https://maps.google.com/maps?q=Av.%20Ciudad%20de%20Lima%20%23%2010-50%2C%20Santa%20F%C3%A9%2C%20Bogot%C3%A1&t=&z=17&ie=UTF8&iwloc=&output=embed"
          width="100%"
          height="250"
          style="border:0; border-radius: 12px;"
          :allowfullscreen="true"
          loading="lazy"
          referrerpolicy="no-referrer-when-downgrade"
          title="Ubicación DISEF">
        </iframe>
      </div>

      <!-- Trust -->
      <div class="contact-trust">
        <div class="trust-item">
          <i class="fas fa-certificate"></i>
          <span class="trust-title">Productos certificados</span>
          <span class="trust-text">Normas nacionales e internacionales.</span>
        </div>
        <div class="trust-separator"></div>
        <div class="trust-item">
          <i class="fas fa-users"></i>
          <span class="trust-title">Atención personalizada</span>
          <span class="trust-text">Soluciones a la medida de tu empresa y proyecto.</span>
        </div>
        <div class="trust-separator"></div>
        <div class="trust-item">
          <i class="fas fa-award"></i>
          <span class="trust-title">Marcas líderes</span>
          <span class="trust-text">Distribuidores de fabricantes reconocidos a nivel mundial.</span>
        </div>
        <div class="trust-separator"></div>
        <div class="trust-item">
          <i class="fas fa-check-circle"></i>
          <span class="trust-title">Calidad garantizada</span>
          <span class="trust-text">Equipos confiables para máxima seguridad y rendimiento.</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
defineOptions({ name: 'ContactSection' })
</script>

<style scoped>
.contact-section {
  padding: 80px 0;
  background: #fff;
  position: relative;
  overflow: hidden;
}

.contact-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 clamp(16px, 4vw, 44px);
  position: relative;
  z-index: 1;
}

/* ── MAIN GRID ── */
.contact-main {
  display: flex;
  align-items: center;
  gap: 40px;
  margin-bottom: 50px;
}

/* ── COLUMNA IZQUIERDA ── */
.col-left {
  flex: 0 0 35%;
  display: flex;
  flex-direction: column;
  border-right: 1px solid #E5E5E5;
  padding-right: 40px;
}

.contact-label {
  display: block;
  font-size: 13px;
  font-weight: 600;
  color: #FFC107;
  letter-spacing: 4px;
  text-transform: uppercase;
  margin-bottom: 8px;
}

.contact-title {
  margin: 0;
  font-size: clamp(32px, 4vw, 48px);
  font-weight: 900;
  color: #111;
  letter-spacing: -1px;
  font-style: italic;
  line-height: 1;
}

.contact-title .highlight {
  color: #FFC107;
}

.title-line {
  width: 80px;
  height: 4px;
  background: #FFC107;
  margin-top: 16px;
}

.contact-text {
  font-size: 16px;
  color: #555;
  line-height: 1.7;
  margin: 24px 0 0;
}

.contact-text strong {
  color: #111;
}

.contact-whatsapp {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px 20px;
  background: #F7F8FA;
  border-radius: 12px;
  margin-top: 32px;
}

.whatsapp-icon {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: #25D366;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.whatsapp-icon i {
  color: #fff;
  font-size: 22px;
}

.whatsapp-title {
  font-size: 14px;
  font-weight: 700;
  color: #111;
  display: block;
}

.whatsapp-text {
  font-size: 13px;
  color: #777;
  display: block;
}

.btn-whatsapp {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 14px 28px;
  background: #25D366;
  color: #fff;
  border-radius: 10px;
  font-size: 13px;
  font-weight: 700;
  text-decoration: none;
  letter-spacing: 0.5px;
  transition: all 0.3s ease;
  width: fit-content;
  margin-top: 24px;
}

.btn-whatsapp:hover {
  background: #1DA851;
  transform: translateY(-2px);
}

/* ── COLUMNA CENTRO + DERECHA ── */
.col-right {
  flex: 1;
  display: flex;
  align-items: center;
  gap: 20px;
  position: relative;
}

/* ── CENTRO: INFO ── */
.contact-info-list {
  display: flex;
  flex-direction: column;
  flex-shrink: 0;
  z-index: 2;
}

.info-row {
  display: flex;
  align-items: flex-start;
  gap: 16px;
  padding: 14px 0;
}

.info-icon {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: rgba(255,193,7,0.12);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.info-icon i {
  font-size: 18px;
  color: #FFC107;
}

.info-icon.whatsapp {
  background: rgba(37,211,102,0.12);
}

.info-icon.whatsapp i {
  color: #25D366;
}

.info-label {
  font-size: 13px;
  font-weight: 600;
  color: #FFC107;
  display: block;
}

.info-value {
  font-size: 14px;
  color: #555;
  line-height: 1.5;
  display: block;
}

.info-divider {
  width: 100%;
  height: 1px;
  background: #E5E5E5;
  margin-left: 60px;
  width: calc(100% - 60px);
}

/* ── DERECHA: IMAGEN ── */
.contact-image {
  position: relative;
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-left: -10px;
  margin-top: 10px;
  z-index: 1;
}

.contact-image img {
  position: relative;
  width: 600px;
  top: -50px;
  height: auto;
  z-index: 1;
}

/* ── FEATURES ── */
.contact-features {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  margin-bottom: 40px;
}

.feature-box {
  padding: 20px;
  background: #F7F8FA;
  border-radius: 12px;
  border: 1px solid #E5E5E5;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.feature-box i {
  font-size: 20px;
  color: #FFC107;
}

.feature-title {
  font-size: 12px;
  font-weight: 700;
  color: #111;
  letter-spacing: 0.5px;
}

.feature-text {
  font-size: 12px;
  color: #777;
  line-height: 1.5;
}

/* ── MAPA ── */
.contact-map {
  margin-bottom: 40px;
  border-radius: 12px;
  overflow: hidden;
}

/* ── BOTTOM CTA ── */
.contact-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 24px 32px;
  background: #F7F8FA;
  border-radius: 16px;
  border: 1px solid #E5E5E5;
  margin-bottom: 40px;
}

.bottom-left {
  display: flex;
  align-items: center;
  gap: 16px;
}

.bottom-icon {
  font-size: 28px;
  color: #FFC107;
}

.bottom-left h4 {
  margin: 0 0 4px;
  font-size: 16px;
  font-weight: 700;
  color: #111;
}

.bottom-left p {
  margin: 0;
  font-size: 13px;
  color: #777;
}

.bottom-buttons {
  display: flex;
  gap: 12px;
  flex-shrink: 0;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 20px;
  border-radius: 8px;
  font-size: 12px;
  font-weight: 700;
  text-decoration: none;
  transition: all 0.3s ease;
  cursor: pointer;
  border: none;
  white-space: nowrap;
  letter-spacing: 0.5px;
}

.btn-yellow {
  background: #FFC107;
  color: #111;
}

.btn-yellow:hover {
  background: #E5AC00;
  transform: translateY(-2px);
}

.btn-outline {
  background: transparent;
  border: 1.5px solid #111;
  color: #111;
}

.btn-outline:hover {
  background: #111;
  color: #fff;
  transform: translateY(-2px);
}

/* ── TRUST ── */
.contact-trust {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0;
  padding: 24px 40px;
  background: #F7F8FA;
  border: 1px solid #E5E5E5;
  border-radius: 12px;
}

.trust-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  padding: 0 32px;
  text-align: center;
}

.trust-item i {
  font-size: 20px;
  color: #FFC107;
}

.trust-title {
  font-size: 13px;
  font-weight: 700;
  color: #111;
}

.trust-text {
  font-size: 11px;
  color: #777;
}

.trust-separator {
  width: 1px;
  height: 50px;
  background: #E5E5E5;
  flex-shrink: 0;
}

/* ── RESPONSIVE ── */
@media (max-width: 1200px) {
  .contact-image img {
    width: 450px;
  }
  .image-halo {
    width: 600px;
    height: 600px;
  }
}

@media (max-width: 1024px) {
  .contact-main {
    flex-direction: column;
    gap: 40px;
  }
  .col-left {
    flex: 0 0 auto;
    width: 100%;
    border-right: none;
    padding-right: 0;
    border-bottom: 1px solid #E5E5E5;
    padding-bottom: 40px;
  }
  .col-right {
    flex: 0 0 auto;
    width: 100%;
    gap: 30px;
  }
  .contact-image {
    margin-left: 0;
    margin-top: 0;
  }
  .contact-image img {
    width: 100%;
    max-width: 500px;
    margin: 0 auto;
    display: block;
  }
  .contact-bottom {
    flex-direction: column;
    gap: 20px;
    text-align: center;
  }
  .bottom-left {
    flex-direction: column;
  }
  .contact-trust {
    flex-wrap: wrap;
    gap: 20px;
  }
  .trust-separator {
    display: none;
  }
}

@media (max-width: 768px) {
  .contact-section {
    padding: 60px 0;
  }
  .contact-main {
    gap: 30px;
  }
  .contact-title {
    font-size: clamp(28px, 6vw, 36px);
  }
  .contact-text {
    font-size: 15px;
  }
  .info-row {
    padding: 12px 0;
  }
  .info-icon {
    width: 40px;
    height: 40px;
  }
  .info-icon i {
    font-size: 16px;
  }
  .contact-whatsapp {
    padding: 14px 16px;
  }
  .whatsapp-icon {
    width: 44px;
    height: 44px;
  }
  .bottom-buttons {
    flex-direction: column;
    width: 100%;
  }
  .btn {
    justify-content: center;
    width: 100%;
  }
  .contact-map iframe {
    height: 200px;
  }
}

@media (max-width: 480px) {
  .contact-section {
    padding: 40px 0;
  }
  .contact-label {
    font-size: 11px;
    letter-spacing: 3px;
  }
  .contact-title {
    font-size: clamp(24px, 6vw, 32px);
  }
  .contact-text {
    font-size: 14px;
    margin-top: 16px;
  }
  .col-left {
    padding-bottom: 30px;
  }
  .info-divider {
    margin-left: 0;
    width: 100%;
  }
  .info-row {
    gap: 12px;
  }
  .info-icon {
    width: 36px;
    height: 36px;
  }
  .info-icon i {
    font-size: 14px;
  }
  .info-label {
    font-size: 12px;
  }
  .info-value {
    font-size: 13px;
  }
  .contact-bottom {
    padding: 20px;
  }
  .bottom-left h4 {
    font-size: 14px;
  }
  .bottom-left p {
    font-size: 12px;
  }
  .contact-trust {
    flex-direction: column;
    align-items: flex-start;
    padding: 20px;
  }
  .trust-item {
    align-items: flex-start;
    text-align: left;
    padding: 8px 0;
  }
  .contact-map iframe {
    height: 180px;
  }
}
</style>
