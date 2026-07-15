<template>
  <section class="featured-section">
    <div class="featured-container">
      <header class="featured-header">
        <span class="featured-label">LO MÁS SOLICITADO</span>
        <h2 class="featured-title">PRODUCTOS <span class="highlight">DESTACADOS</span></h2>
        <span class="featured-title-line"></span>
        <p class="featured-subtitle">Equipos y herramientas industriales de alta calidad, seleccionados por su rendimiento, seguridad y confiabilidad.</p>
        <button class="featured-view-all" @click="goToProducts">
          Ver todo el catálogo de destacados <i class="fas fa-arrow-right"></i>
        </button>
      </header>

      <div class="featured-carousel">
        <button class="featured-arrow featured-arrow--left" @click="prevSlide" aria-label="Anterior">
          <i class="fas fa-chevron-left"></i>
        </button>

        <div class="featured-track" ref="trackRef">
          <div
            v-for="product in products"
            :key="product.id"
            class="featured-card"
          >
            <!-- Imagen - 65% de la tarjeta -->
            <div class="featured-card-image">
              <span class="featured-badge" :class="`featured-badge--${product.badgeType}`">
                <i v-if="product.badgeType === 'offer'" class="fas fa-tag"></i>
                <i v-else class="fas fa-award"></i>
                {{ product.badge }}
              </span>
              <img :src="product.image" :alt="product.name" loading="lazy" />
            </div>

            <!-- Información -->
            <div class="featured-card-info">
              <span class="featured-brand">{{ product.brand }}</span>
              <h3 class="featured-name">{{ product.name }}</h3>


              <!-- Disponibilidad -->
              <div class="featured-availability" :class="{ 'out-of-stock': !product.inStock }">
                <span class="availability-dot"></span>
                {{ product.inStock ? 'Disponible' : 'Bajo pedido' }}
              </div>

              <!-- Botones -->
              <div class="featured-actions">
                <button class="featured-btn featured-btn--details" @click.stop="goToProduct(product)">
                  Ver detalles
                </button>
                <button class="featured-btn featured-btn--quote" @click.stop="addToQuote(product)">
                  <i class="fas fa-clipboard-list"></i>
                  Agregar a cotización
                </button>
              </div>
            </div>
          </div>
        </div>

        <button class="featured-arrow featured-arrow--right" @click="nextSlide" aria-label="Siguiente">
          <i class="fas fa-chevron-right"></i>
        </button>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'
import { useRouter } from 'vue-router'
import { useProducts } from '@/composables/useProducts'
import { useQuotation } from '@/composables/useQuotation'
import { useBrands } from '@/composables/useBrands'

const router = useRouter()
const trackRef = ref<HTMLElement | null>(null)
const { products: allProducts, loadProducts } = useProducts()
const { addToQuotation, openDrawer } = useQuotation()
const { brands, loadBrands } = useBrands()

const getBrandName = (brandId?: number): string => {
  if (!brandId) return 'DISEF'
  const found = brands.value.find(b => Number(b.id) === brandId)
  return found?.name || 'DISEF'
}

interface FeaturedProduct {
  id: string
  name: string
  brand: string
  sku: string
  image: string
  badge: string
  badgeType: 'new' | 'offer' | 'bestseller' | 'featured'
  inStock: boolean
  price: number
  stars: number
  slug?: string
}

const products = computed<FeaturedProduct[]>(() => {
  return allProducts.value
    .filter(p => p.isShowcase || p.isFeatured)
    .slice(0, 8)
    .map(p => {
      const hasDiscount = p.originalPrice && p.originalPrice > p.price
      const hasColor = p.colors && p.colors.length > 0
      let badge = hasDiscount ? 'CON DESCUENTO' : 'PRECIO NORMAL'
      let badgeType: FeaturedProduct['badgeType'] = hasDiscount ? 'offer' : 'featured'

      return {
        id: p.id,
        name: p.name,
        brand: getBrandName(p.brandId),
        sku: p.sku || 'N/A',
        image: p.images?.[0] || 'https://placehold.co/400x400/1a1a1a/ffffff?text=Producto',
        badge,
        badgeType,
        inStock: p.status === 'available',
        price: p.price,
        stars: 5
      }
    })
})

onMounted(async () => {
  await loadProducts()
  await loadBrands()
})

function goToProducts() {
  router.push('/productos')
}

function goToProduct(product: FeaturedProduct) {
  router.push(`/producto/${product.slug || product.id}`)
}

function addToQuote(product: FeaturedProduct) {
  addToQuotation({
    id: product.id,
    name: product.name,
    sku: product.sku,
    brand: product.brand,
    price: product.price,
    image: product.image,
    category: '',
    description: '',
    inStock: product.inStock
    }, 1)
  }

function prevSlide() {
  if (trackRef.value) {
    trackRef.value.scrollBy({ left: -320, behavior: 'smooth' })
  }
}

function nextSlide() {
  if (trackRef.value) {
    trackRef.value.scrollBy({ left: 320, behavior: 'smooth' })
  }
}
</script>

<style scoped>
.featured-section {
  width: 100%;
  padding: 50px 0;
  background: #020202;
}

.featured-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 clamp(16px, 4vw, 44px);
}

/* Header */
.featured-header {
  text-align: center;
  margin-bottom: 48px;
}

.featured-label {
  display: block;
  font-size: 12px;
  font-weight: 700;
  color: #FFC107;
  letter-spacing: 4px;
  margin-bottom: 16px;
  text-transform: uppercase;
}

.featured-title {
  margin: 0;
  font-size: clamp(32px, 4vw, 48px);
  font-weight: 900;
  color: #FFFFFF;
  letter-spacing: -0.5px;
  font-style: italic;
}

.featured-title .highlight {
  color: #FFC107;
}

.featured-title-line {
  display: block;
  width: 60px;
  height: 4px;
  background: #F4B400;
  border-radius: 2px;
  margin: 1px auto 0;
}

.featured-subtitle {
  margin: 16px auto 0;
  max-width: 600px;
  color: #888888;
  font-size: 16px;
  line-height: 1.7;
}

.featured-view-all {
  margin-top: 24px;
  padding: 12px 28px;
  background: transparent;
  border: 2px solid #FFC107;
  color: #FFC107;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: inline-flex;
  align-items: center;
  gap: 8px;
}

.featured-view-all:hover {
  background: #FFC107;
  color: #0a0a0a;
}

.featured-view-all i {
  font-size: 12px;
  transition: transform 0.3s ease;
}

.featured-view-all:hover i {
  transform: translateX(4px);
}

/* Carousel */
.featured-carousel {
  position: relative;
}

.featured-track {
  display: flex;
  gap: 24px;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  scrollbar-width: none;
  -ms-overflow-style: none;
  padding: 20px 0;
  scroll-behavior: smooth;
}

.featured-track::-webkit-scrollbar {
  display: none;
}

/* ============================================
   TARJETA - Estilo Industrial Premium
   ============================================ */
.featured-card {
  flex: 0 0 260px;
  background: #1B1B1B;
  border-radius: 18px;
  overflow: hidden;
  border: 1px solid #2a2a2a;
  transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
  scroll-snap-align: start;
  display: flex;
  flex-direction: column;
}

.featured-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4), 0 0 0 1px rgba(244, 180, 0, 0.15);
  border-color: rgba(244, 180, 0, 0.3);
}

/* ============================================
   ÁREA DE IMAGEN
   ============================================ */
.featured-card-image {
  position: relative;
  width: 100%;
  height: 200px;
  background: linear-gradient(180deg, #FFFFFF 0%, #F5F5F5 100%);
  border-radius: 18px 18px 0 0;
  padding: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.featured-card-image img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  object-position: center;
  transition: transform 0.35s cubic-bezier(0.4, 0, 0.2, 1);
}

.featured-card:hover .featured-card-image img {
  transform: scale(1.08);
}

/* Badge */
.featured-badge {
  position: absolute;
  top: 16px;
  left: 16px;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 10px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  display: flex;
  align-items: center;
  gap: 5px;
  z-index: 2;
}

.featured-badge i {
  font-size: 10px;
}

.featured-badge--bestseller {
  background: #F97316;
  color: #FFFFFF;
}

.featured-badge--new {
  background: #22C55E;
  color: #FFFFFF;
}

.featured-badge--offer {
  background: #FFC107;
  color: #0a0a0a;
}

.featured-badge--featured {
  background: #3B82F6;
  color: #FFFFFF;
}

/* ============================================
   INFORMACIÓN
   ============================================ */
.featured-card-info {
  padding: 16px 18px 18px;
  display: flex;
  flex-direction: column;
  gap: 6px;
  flex: 1;
}

/* Marca */
.featured-brand {
  font-size: 12px;
  font-weight: 600;
  color: #888888;
  letter-spacing: 0.5px;
}

/* Nombre */
.featured-name {
  margin: 0;
  font-size: 17px;
  font-weight: 700;
  color: #FFFFFF;
  line-height: 1.3;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  overflow: hidden;
  min-height: 44px;
}



/* Disponibilidad */
.featured-availability {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  font-size: 11px;
  font-weight: 500;
  color: #22C55E;
  background: rgba(34, 197, 94, 0.1);
  padding: 4px 10px;
  border-radius: 16px;
  width: fit-content;
}

.featured-availability.out-of-stock {
  color: #EF4444;
  background: rgba(239, 68, 68, 0.1);
}

.availability-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #22C55E;
  animation: pulse 2s infinite;
}

.featured-availability.out-of-stock .availability-dot {
  background: #EF4444;
  animation: none;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

/* ============================================
   ESPECIFICACIONES
   ============================================ */
.featured-specs {
  display: flex;
  gap: 8px;
  margin-top: 8px;
  padding-top: 8px;
  border-top: 1px solid #2a2a2a;
}

.spec-item {
  display: flex;
  align-items: center;
  gap: 6px;
  flex: 1;
}

.spec-item i {
  font-size: 12px;
  color: #FFC107;
  width: 16px;
  text-align: center;
}

.spec-content {
  display: flex;
  flex-direction: column;
}

.spec-title {
  font-size: 9px;
  color: #666666;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.spec-value {
  font-size: 11px;
  color: #FFFFFF;
  font-weight: 600;
}

/* ============================================
   BOTONES
   ============================================ */
.featured-actions {
  display: flex;
  gap: 8px;
  margin-top: 12px;
}

.featured-btn {
  padding: 10px 12px;
  border-radius: 8px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.25s ease;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
}

/* Botón secundario */
.featured-btn--details {
  flex: 1;
  background: transparent;
  color: #FFFFFF;
  border: 1px solid #444444;
}

.featured-btn--details:hover {
  border-color: #FFFFFF;
  background: rgba(255, 255, 255, 0.05);
}

/* Botón principal - doble ancho */
.featured-btn--quote {
  flex: 2;
  background: #F4B400;
  color: #000000;
  font-weight: 700;
}

.featured-btn--quote:hover {
  background: #FFD54F;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(244, 180, 0, 0.3);
}

.featured-btn i {
  font-size: 11px;
}

/* ============================================
   FLECHAS
   ============================================ */
.featured-arrow {
  position: absolute;
  top: -30px;
  transform: translateY(-50%);
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: #FFFFFF;
  border: none;
  color: #0a0a0a;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  z-index: 10;
}

.featured-arrow--left {
  left: -24px;
}

.featured-arrow--right {
  right: -24px;
}

.featured-arrow:hover {
  background: #FFC107;
  transform: translateY(-50%) scale(1.1);
}

.featured-arrow i {
  font-size: 16px;
}

/* ============================================
   RESPONSIVE
   ============================================ */
@media (max-width: 1280px) {
  .featured-track {
    gap: 16px;
  }

  .featured-card {
    flex: 0 0 240px;
  }
}

@media (max-width: 1024px) {
  .featured-card {
    flex: 0 0 220px;
  }

  .featured-card-image {
    height: 180px;
  }

  .featured-name {
    font-size: 15px;
  }
}

@media (max-width: 768px) {
  .featured-section {
    padding: 60px 0;
  }

  .featured-header {
    margin-bottom: 32px;
  }

  .featured-arrow {
    display: none;
  }

  .featured-card {
    flex: 0 0 85vw;
    max-width: 260px;
  }

  .featured-title {
    font-style: normal;
  }
}

@media (max-width: 480px) {
  .featured-card {
    flex: 0 0 90vw;
    max-width: 240px;
  }

  .featured-card-image {
    height: 160px;
    padding: 12px;
  }

  .featured-card-info {
    padding: 12px 14px 14px;
  }

  .featured-name {
    font-size: 14px;
    min-height: 36px;
  }

  .featured-specs {
    flex-wrap: wrap;
  }

  .spec-item {
    flex: 0 0 calc(50% - 4px);
  }

  .featured-actions {
    flex-direction: column;
  }

  .featured-btn {
    width: 100%;
  }
}
</style>
