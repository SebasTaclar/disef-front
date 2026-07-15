<template>
  <div class="category-page">
    <div class="layout">
      <!-- SIDEBAR -->
      <aside class="sidebar">
        <div class="sidebar-header">
          <h3 class="sidebar-title">FILTRAR POR</h3>
        </div>

        <!-- Disponibilidad -->
        <div class="filter-group">
          <button class="filter-toggle" @click="toggleFilter('disponibilidad')">
            Disponibilidad
            <i class="fas" :class="openFilters.disponibilidad ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
          </button>
          <div v-if="openFilters.disponibilidad" class="filter-options">
            <label class="checkbox-item">
              <input type="checkbox" value="available" v-model="selectedStatus" />
              <span class="label-text">Disponible</span>
              <span class="count">({{ countByStatus('available') }})</span>
            </label>
            <label class="checkbox-item">
              <input type="checkbox" value="coming-soon" v-model="selectedStatus" />
              <span class="label-text">Bajo pedido</span>
              <span class="count">({{ countByStatus('coming-soon') }})</span>
            </label>
            <label class="checkbox-item">
              <input type="checkbox" value="out-of-stock" v-model="selectedStatus" />
              <span class="label-text">Agotado</span>
              <span class="count">({{ countByStatus('out-of-stock') }})</span>
            </label>
          </div>
        </div>

        <!-- Marcas -->
        <div class="filter-group">
          <button class="filter-toggle" @click="toggleFilter('marcas')">
            Marcas
            <i class="fas" :class="openFilters.marcas ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
          </button>
          <div v-if="openFilters.marcas" class="filter-options">
            <label
              v-for="brand in visibleBrands"
              :key="brand.name"
              class="checkbox-item"
            >
              <input type="checkbox" :value="brand.name" v-model="selectedBrands" />
              <span class="label-text">{{ brand.name }}</span>
              <span class="count">({{ brand.count }})</span>
            </label>
            <button v-if="availableBrands.length > 5" class="ver-mas-btn" @click="showAllBrands = !showAllBrands">
              {{ showAllBrands ? 'Ver menos' : 'Ver más' }}
            </button>
          </div>
        </div>

        <!-- Subcategorías -->
        <div class="filter-group">
          <button class="filter-toggle" @click="toggleFilter('subcategorias')">
            Subcategorías
            <i class="fas" :class="openFilters.subcategorias ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
          </button>
          <div v-if="openFilters.subcategorias" class="filter-options">
            <label
              v-for="sub in visibleSubcategories"
              :key="sub.name"
              class="checkbox-item"
            >
              <input type="checkbox" :value="sub.name" v-model="selectedSubcategories" />
              <span class="label-text">{{ sub.name }}</span>
              <span class="count">({{ sub.count }})</span>
            </label>
            <button v-if="availableSubcategories.length > 5" class="ver-mas-btn" @click="showAllSubcategories = !showAllSubcategories">
              {{ showAllSubcategories ? 'Ver menos' : 'Ver más' }}
            </button>
          </div>
        </div>

        <button class="clear-btn" type="button" @click="clearFilters">
          <i class="fas fa-times"></i> Limpiar Filtros
        </button>
      </aside>

      <!-- CONTENT -->
      <main class="content">
        <!-- Hero Banner -->
        <header class="category-hero">
          <div class="hero-text">
            <span class="hero-label">LÍNEA ELÉCTRICA E INDUSTRIAL</span>
            <h1 class="hero-title">{{ pageTitle }}</h1>
            <p class="hero-description">{{ pageDescription || 'Equipos y herramientas diseñados para brindar máxima seguridad y rendimiento en trabajos con y sin tensión.' }}</p>
            <button class="hero-btn" @click="scrollToProducts">
              Explorar productos <i class="fas fa-arrow-right"></i>
            </button>
          </div>
          <div class="hero-image">
            <img
              :src="categoryImageSrc"
              :alt="pageTitle"
              loading="lazy"
              @error="onCategoryImageError"
            />
          </div>
        </header>

        <!-- Top Bar -->
        <div class="topbar" ref="productsSection">
          <div class="topbar-left">
            <span class="product-count">Mostrando <strong>{{ filteredProducts.length }}</strong> productos</span>
          </div>
          <div class="topbar-right">
            <div class="sort">
              <label class="sort-label" for="sort">Ordenar por:</label>
              <select id="sort" v-model="sortBy" class="sort-select">
                <option value="featured">Más relevantes</option>
                <option value="price-asc">Precio: menor a mayor</option>
                <option value="price-desc">Precio: mayor a menor</option>
                <option value="name">Nombre A-Z</option>
              </select>
            </div>
            <div class="view-toggle">
              <button :class="['view-btn', { active: viewMode === 'grid' }]" @click="viewMode = 'grid'">
                <i class="fas fa-th"></i>
              </button>
              <button :class="['view-btn', { active: viewMode === 'list' }]" @click="viewMode = 'list'">
                <i class="fas fa-list"></i>
              </button>
            </div>
          </div>
        </div>

        <!-- Loading -->
        <div v-if="loading" class="loading-state">
          <div class="spinner"></div>
          <p>Cargando productos...</p>
        </div>

        <!-- Error -->
        <div v-else-if="error" class="error">
          <i class="fas fa-exclamation-triangle"></i>
          {{ error }}
        </div>

        <!-- Products Grid -->
        <div v-else-if="filteredProducts.length > 0" :class="['products-grid', viewMode]">
          <article
            v-for="p in filteredProducts"
            :key="p.id"
            class="product-card"
          >
            <button class="wishlist-btn" @click.stop>
              <i class="far fa-heart"></i>
            </button>
            <div class="card-image">
              <img :src="p.images?.[0] || 'https://placehold.co/400x300/1a1a1a/ffffff?text=Producto'" :alt="p.name" loading="lazy" />
              <span v-if="p.isOffer" class="badge badge--offer">
                <i class="fas fa-tag"></i> OFERTA
              </span>
              <span v-else-if="p.isNew" class="badge badge--new">
                <i class="fas fa-bolt"></i> NUEVO
              </span>
              <span v-else-if="p.isFeatured" class="badge badge--featured">
                <i class="fas fa-award"></i> DESTACADO
              </span>
            </div>

            <div class="card-body">
              <span v-if="getBrandName(p.brandId)" class="card-brand">{{ getBrandName(p.brandId) }}</span>
              <h3 class="card-name">{{ p.name }}</h3>
              <span v-if="p.sku" class="card-sku">
                SKU: {{ p.sku }}
              </span>

              <div class="card-status">
                <span class="status-dot" :class="{ 'out-of-stock': p.status !== 'available' }"></span>
                {{ p.status === 'available' ? 'Disponible' : p.status === 'coming-soon' ? 'Bajo pedido' : 'Agotado' }}
              </div>

              <div class="card-actions">
                <button class="btn-details" @click.stop="openQuickView(p)">
                  Ver detalles
                </button>
                <button
                  v-if="p.status === 'available'"
                  class="btn-quote"
                  @click.stop="addProductToCart(p)"
                >
                  <i class="fas fa-shopping-cart"></i> Agregar a <br> cotización
                </button>
              </div>
            </div>
          </article>
        </div>

        <!-- Empty State -->
        <div v-else class="empty-state">
          <i class="fas fa-box-open"></i>
          <p>No se encontraron productos con los filtros seleccionados.</p>
          <button class="clear-btn" @click="clearFilters">Limpiar Filtros</button>
        </div>

        <!-- WhatsApp Help -->
        <div class="whatsapp-help">
          <div class="whatsapp-icon">
            <i class="fab fa-whatsapp"></i>
          </div>
          <div class="whatsapp-text">
            <strong>¿Necesitas ayuda para encontrar el producto adecuado?</strong>
            <p>Habla con un asesor especializado y recibe orientación personalizada.</p>
          </div>
          <div class="whatsapp-actions">
            <a href="https://wa.me/573229118168" target="_blank" class="btn-whatsapp">
              <i class="fab fa-whatsapp"></i> Hablar por WhatsApp
            </a>
            <a href="tel:+573229118168" class="btn-call">
              <i class="fas fa-phone"></i> Llamar ahora
            </a>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref, reactive, watch } from 'vue'
import { useRoute } from 'vue-router'
import { useProducts } from '@/composables/useProducts'
import { useQuotation } from '@/composables/useQuotation'
import { useProductQuickView } from '@/composables/useProductQuickView'
import { useBrands } from '@/composables/useBrands'
import type { Product } from '@/types/ProductType'

const props = defineProps<{ slug: string; title?: string }>()

const route = useRoute()
const productsSection = ref<HTMLElement | null>(null)

const { products: allProducts, categories, loadProducts, loadCategories } = useProducts()
const { addToQuotation, openDrawer } = useQuotation()
const quickView = useProductQuickView()
const { brands, loadBrands } = useBrands()

const getBrandName = (brandId?: number): string | undefined => {
  if (!brandId) return undefined
  const found = brands.value.find(b => Number(b.id) === brandId)
  return found?.name
}

const loading = ref(false)
const error = ref<string | null>(null)
const viewMode = ref<'grid' | 'list'>('grid')

const searchTerm = ref('')
const sortBy = ref<'featured' | 'price-asc' | 'price-desc' | 'name'>('featured')
const selectedStatus = ref<string[]>([])
const selectedBrands = ref<string[]>([])
const selectedSubcategories = ref<string[]>([])

const openFilters = reactive({
  disponibilidad: true,
  marcas: true,
  subcategorias: true,
})

const showAllBrands = ref(false)
const showAllSubcategories = ref(false)

function toggleFilter(key: keyof typeof openFilters) {
  openFilters[key] = !openFilters[key]
}

watch(
  () => route.query.q,
  q => {
    searchTerm.value = typeof q === 'string' ? q : ''
  },
  { immediate: true }
)

const categoryImageIndex = ref(0)

function slugify(value: string): string {
  return value
    .trim()
    .toLowerCase()
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
    .replace(/[^a-z0-9\s-]/g, '')
    .replace(/\s+/g, '-')
    .replace(/-+/g, '-')
}

const currentCategoryId = computed(() => {
  const slug = props.slug
  const direct = categories.value.find(c => slugify(c.name) === slug)
  if (direct) return direct.id

  const relaxed = categories.value.find(c => {
    const s = slugify(c.name)
    return s.includes(slug) || slug.includes(s)
  })

  return relaxed?.id
})

const currentCategory = computed(() => {
  const id = currentCategoryId.value
  if (id) {
    const byId = categories.value.find(c => c.id === id)
    if (byId) return byId
  }
  return categories.value.find(c => slugify(c.name) === props.slug)
})

const pageTitle = computed(() => props.title || currentCategory.value?.name || props.slug)
const pageDescription = computed(() => currentCategory.value?.description || '')

const categoryImageCandidates = computed(() => {
  const fromBackend = currentCategory.value?.imageUrls || []
  const local = `/images/${props.slug}`
  const allProductsImage = 'https://res.cloudinary.com/dlwzazojt/image/upload/v1784077087/ChatGPT_Image_14_jul_2026_19_56_36_me9ffw.png'
  return [
    ...(props.slug === 'todos' ? [allProductsImage] : []),
    ...fromBackend,
    `${local}.jpg`,
    `${local}.jpeg`,
    `${local}.png`,
    `${local}.webp`,
    'https://placehold.co/900x300/1a1a1a/ffc107?text=' + encodeURIComponent(pageTitle.value)
  ]
})

const categoryImageSrc = computed(() => {
  return categoryImageCandidates.value[Math.min(categoryImageIndex.value, categoryImageCandidates.value.length - 1)]
})

function onCategoryImageError() {
  const next = categoryImageIndex.value + 1
  categoryImageIndex.value = Math.min(next, categoryImageCandidates.value.length - 1)
}

watch(
  () => props.slug,
  () => {
    categoryImageIndex.value = 0
    clearFilters()
    error.value = null
  }
)

function categoryName(categoryId: string): string {
  return categories.value.find(c => c.id === categoryId)?.name?.toUpperCase() || ''
}

const baseProductsForCategory = computed(() => {
  const id = currentCategoryId.value
  if (props.slug === 'todos' || !id) return allProducts.value
  return allProducts.value.filter(p => p.category === id)
})

const availableBrands = computed(() => {
  const brandMap = new Map<number, number>()
  baseProductsForCategory.value.forEach(p => {
    if (p.brandId) {
      brandMap.set(p.brandId, (brandMap.get(p.brandId) || 0) + 1)
    }
  })
  return Array.from(brandMap.entries())
    .map(([id, count]) => ({ name: getBrandName(id) || String(id), count }))
    .sort((a, b) => b.count - a.count)
})

const visibleBrands = computed(() =>
  showAllBrands.value ? availableBrands.value : availableBrands.value.slice(0, 5)
)

function extractKeywords(description: string): string[] {
  if (!description) return []
  const keywords: string[] = []
  const patterns = [
    /guantes?\s+(diel[eé]ctricos?|de\s+seguridad|isolados?|dielectricos?)/gi,
    /detectores?\s+de\s+tensi[oó]n/gi,
    /pertigas?\s+telescopicas?/gi,
    /cascos?\s+(diel[eé]ctricos?|de\s+seguridad|isolados?|dielectricos?)/gi,
    /kits?\s+de\s+seguridad/gi,
    /osciloscopios?/gi,
    /mult[ií]metros?/gi,
    /pinzas?\s+amperim[eé]tricas?/gi,
    /mangueras?\s+(diel[eé]ctricas?|dielectricas?)/gi,
    /tapetes?\s+diel[eé]ctricos?/gi,
    /escaleras?\s+(diel[eé]ctricas?|aislantes?)/gi,
  ]
  patterns.forEach(regex => {
    const match = description.match(regex)
    if (match) keywords.push(...match.map(m => m.trim()))
  })
  return [...new Set(keywords.map(k => k.charAt(0).toUpperCase() + k.slice(1).toLowerCase()))]
}

const availableSubcategories = computed(() => {
  const subMap = new Map<string, number>()
  baseProductsForCategory.value.forEach(p => {
    const keywords = extractKeywords(p.name + ' ' + (p.description || ''))
    keywords.forEach(kw => {
      subMap.set(kw, (subMap.get(kw) || 0) + 1)
    })
  })
  return Array.from(subMap.entries())
    .map(([name, count]) => ({ name, count }))
    .sort((a, b) => b.count - a.count)
})

const visibleSubcategories = computed(() =>
  showAllSubcategories.value ? availableSubcategories.value : availableSubcategories.value.slice(0, 5)
)

function countByStatus(status: string): number {
  return baseProductsForCategory.value.filter(p => p.status === status).length
}

const filteredProducts = computed(() => {
  const q = searchTerm.value.trim().toLowerCase()
  let list = baseProductsForCategory.value

  if (q) {
    list = list.filter(p => {
      const name = p.name.toLowerCase()
      const desc = (p.description || '').toLowerCase()
      const brand = (getBrandName(p.brandId) || '').toLowerCase()
      const sku = (p.sku || '').toLowerCase()
      return name.includes(q) || desc.includes(q) || brand.includes(q) || sku.includes(q)
    })
  }

  if (selectedStatus.value.length > 0) {
    list = list.filter(p => selectedStatus.value.includes(p.status))
  }

  if (selectedBrands.value.length > 0) {
    list = list.filter(p => {
      const brandName = getBrandName(p.brandId)
      return brandName && selectedBrands.value.includes(brandName)
    })
  }

  if (selectedSubcategories.value.length > 0) {
    list = list.filter(p => {
      const text = (p.name + ' ' + (p.description || '')).toLowerCase()
      return selectedSubcategories.value.some(sub => text.includes(sub.toLowerCase()))
    })
  }

  if (sortBy.value === 'price-asc') return [...list].sort((a, b) => a.price - b.price)
  if (sortBy.value === 'price-desc') return [...list].sort((a, b) => b.price - a.price)
  if (sortBy.value === 'name') return [...list].sort((a, b) => a.name.localeCompare(b.name))

  return list
})

function addProductToCart(p: Product) {
  addToQuotation({
    id: p.id,
    name: p.name,
    sku: p.sku || 'N/A',
    brand: getBrandName(p.brandId) || 'N/A',
    price: p.price,
    image: p.images?.[0] || '',
    category: categoryName(p.category),
    description: p.description,
    inStock: p.status === 'available',
    originalPrice: p.originalPrice
  }, 1)
  openDrawer()
}

function openQuickView(p: Product) {
  quickView.open(p)
}

function scrollToProducts() {
  productsSection.value?.scrollIntoView({ behavior: 'smooth' })
}

function clearFilters() {
  searchTerm.value = ''
  sortBy.value = 'featured'
  selectedStatus.value = []
  selectedBrands.value = []
  selectedSubcategories.value = []
}

onMounted(async () => {
  loading.value = true
  error.value = null

  try {
    if (categories.value.length === 0) {
      await loadCategories()
    }
    await loadProducts()
    await loadBrands()
  } catch (e) {
    error.value = e instanceof Error ? e.message : 'Error al cargar productos'
  } finally {
    loading.value = false
  }
})
</script>

<style scoped>
.category-page {
  width: 100%;
  padding-top: 1px;
  background: #f5f7fb;
  min-height: 100vh;
}

.layout {
  max-width: 1360px;
  margin: 0 auto;
  padding: 1.5rem 2rem 3rem;
  display: grid;
  grid-template-columns: 280px 1fr;
  gap: 1.5rem;
}

@media (max-width: 1024px) {
  .layout {
    grid-template-columns: 1fr;
    padding: 1rem;
  }
}

/* ========== SIDEBAR ========== */
.sidebar {
  background: white;
  border-radius: 16px;
  padding: 1.25rem;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
  height: fit-content;
  position: sticky;
  top: 130px;
  overflow: hidden;
}

.sidebar-header {
  margin-bottom: 1rem;
  padding-bottom: 0.75rem;
  border-bottom: 1px solid #eee;
}

.sidebar-title {
  margin: 0;
  font-size: 0.85rem;
  font-weight: 800;
  color: #1a1a1a;
  letter-spacing: 0.5px;
}

.filter-group {
  border-bottom: 1px solid #f0f0f0;
  padding: 0.75rem 0;
  display: flex;
  flex-direction: column;
}

.filter-toggle {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: none;
  border: none;
  padding: 0;
  font-size: 0.9rem;
  font-weight: 700;
  color: #1a1a1a;
  cursor: pointer;
  font-family: inherit;
}

.filter-toggle i {
  font-size: 0.7rem;
  color: #999;
  transition: transform 0.2s;
}

.filter-options {
  margin-top: 0.75rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  width: 100%;
}

.checkbox-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
  font-size: 0.85rem;
  color: #444;
}

.checkbox-item input[type="checkbox"] {
  width: 16px;
  height: 16px;
  accent-color: #d4ac43;
  cursor: pointer;
}

.checkmark {
  display: none;
}

.label-text {
  flex: 1;
}

.count {
  color: #999;
  font-size: 0.8rem;
}

.ver-mas-btn {
  background: none;
  border: none;
  color: #d4ac43;
  font-size: 0.8rem;
  font-weight: 600;
  cursor: pointer;
  padding: 4px 0;
  text-decoration: underline;
  font-family: inherit;
}

.ver-mas-btn:hover {
  color: #b8952f;
}

.radio-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
  font-size: 0.85rem;
  color: #444;
  white-space: nowrap;
}

.radio-item input[type="radio"] {
  width: 16px;
  height: 16px;
  accent-color: #d4ac43;
  cursor: pointer;
  flex-shrink: 0;
}

.radio-mark {
  display: none;
}

.clear-btn {
  margin-top: 1rem;
  width: 100%;
  height: 40px;
  border-radius: 8px;
  background: #f5f5f5;
  border: 1px solid #e0e0e0;
  color: #666;
  font-weight: 600;
  font-size: 0.85rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.4rem;
  transition: all 0.2s;
  font-family: inherit;
}

.clear-btn:hover {
  background: #eee;
  color: #333;
}

/* ========== CONTENT ========== */
.content {
  min-width: 0;
}

/* Hero Banner */
.category-hero {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  align-items: center;
  background: linear-gradient(135deg, #1a1a1a 0%, #2a2a2a 100%);
  border-radius: 16px;
  padding: 2rem 2.5rem;
  margin-bottom: 1.5rem;
  overflow: hidden;
}

@media (max-width: 768px) {
  .category-hero {
    grid-template-columns: 1fr;
    padding: 1.5rem;
  }
}

.hero-label {
  font-size: 0.75rem;
  font-weight: 600;
  color: #ffc107;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.hero-title {
  margin: 0.5rem 0;
  font-size: 1.8rem;
  font-weight: 900;
  color: #ffffff;
  line-height: 1.2;
}

.hero-description {
  margin: 0 0 1.25rem;
  font-size: 0.95rem;
  color: #b0b0b0;
  line-height: 1.6;
}

.hero-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  background: #ffc107;
  color: #1a1a1a;
  border: none;
  border-radius: 8px;
  font-size: 0.9rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s;
  font-family: inherit;
}

.hero-btn:hover {
  background: #e6ac00;
  transform: translateY(-1px);
}

.hero-image {
  display: flex;
  justify-content: flex-end;
}

.hero-image img {
  max-width: 100%;
  max-height: 200px;
  object-fit: contain;
  border-radius: 12px;
}

/* Top Bar */
.topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 1.25rem;
  flex-wrap: wrap;
}

.product-count {
  font-size: 0.9rem;
  color: #666;
}

.product-count strong {
  color: #1a1a1a;
}

.topbar-right {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.sort {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.sort-label {
  font-size: 0.85rem;
  color: #666;
}

.sort-select {
  height: 38px;
  border-radius: 8px;
  border: 1px solid #e0e0e0;
  padding: 0 0.75rem;
  background: white;
  color: #1a1a1a;
  font-size: 0.85rem;
  font-family: inherit;
  cursor: pointer;
}

.view-toggle {
  display: flex;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
}

.view-btn {
  width: 38px;
  height: 38px;
  border: none;
  background: white;
  color: #999;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.view-btn.active {
  background: #1a1a1a;
  color: white;
}

.view-btn:not(.active):hover {
  background: #f5f5f5;
}

/* Loading */
.loading-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 4rem 2rem;
  color: #666;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 3px solid #eee;
  border-top-color: #ffc107;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
  margin-bottom: 1rem;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Error */
.error {
  background: rgba(220, 38, 38, 0.08);
  border: 1px solid rgba(220, 38, 38, 0.2);
  color: rgba(220, 38, 38, 0.9);
  padding: 1rem;
  border-radius: 12px;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

/* Products Grid */
.products-grid.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.25rem;
}

.products-grid.list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

@media (max-width: 1200px) {
  .products-grid.grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 900px) {
  .products-grid.grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 600px) {
  .products-grid.grid {
    grid-template-columns: 1fr;
  }
}

/* Product Card */
.product-card {
  background: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.06);
  border: 1px solid #f0f0f0;
  transition: transform 0.2s, box-shadow 0.2s;
  position: relative;
}

.product-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
}

.products-grid.list .product-card {
  display: grid;
  grid-template-columns: 200px 1fr;
}

.card-image {
  position: relative;
  width: 100%;
  aspect-ratio: 4 / 3;
  background: #ffffff;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1.25rem;
}

.products-grid.list .card-image {
  aspect-ratio: auto;
  height: 100%;
}

.card-image img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  display: block;
}

.badge {
  position: absolute;
  top: 0.75rem;
  left: 0.75rem;
  display: inline-flex;
  align-items: center;
  gap: 0.3rem;
  padding: 0.3rem 0.6rem;
  border-radius: 6px;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.3px;
}

.badge--offer {
  background: #ffc107;
  color: #1a1a1a;
}

.badge--new {
  background: #22c55e;
  color: white;
}

.badge--featured {
  background: #3b82f6;
  color: white;
}

.wishlist-btn {
  position: absolute;
  top: 0.75rem;
  right: 0.75rem;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: white;
  border: 1px solid #eee;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #ccc;
  transition: all 0.2s;
  z-index: 2;
}

.wishlist-btn:hover {
  color: #ef4444;
  border-color: #ef4444;
}

.card-body {
  padding: 1rem 1.25rem 1.25rem;
}

.card-brand {
  display: inline-block;
  font-size: 0.75rem;
  font-weight: 900;
  color: #1a1a1a;
  letter-spacing: 0.5px;
  text-transform: uppercase;
  background: #ffc107;
  padding: 0.2rem 0.5rem;
  border-radius: 4px;
}

.card-name {
  margin: 0.6rem 0 0.4rem;
  font-size: 1rem;
  font-weight: 700;
  color: #1a1a1a;
  line-height: 1.35;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.card-sku {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.8rem;
  color: #999;
  margin-bottom: 0.6rem;
}

.card-status {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.85rem;
  color: #22c55e;
  margin-bottom: 1rem;
}

.status-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #22c55e;
}

.status-dot.out-of-stock {
  background: #ef4444;
}

.card-actions {
  display: flex;
  gap: 0.5rem;
  flex-wrap: nowrap;
}

.btn-details {
  flex: 0 0 auto;
  min-width: 0;
  height: 38px;
  border-radius: 8px;
  border: 1.5px solid #1a1a1a;
  background: white;
  color: #1a1a1a;
  font-size: 0.75rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
  font-family: inherit;
  white-space: nowrap;
  padding: 0 0.9rem;
}

.btn-details:hover {
  background: #1a1a1a;
  color: white;
}

.btn-quote {
  flex: 1;
  min-width: 0;
  height: 38px;
  border-radius: 8px;
  border: none;
  background: #ffc107;
  color: #1a1a1a;
  font-size: 0.7rem;
  font-weight: 700;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.4rem;
  transition: all 0.2s;
  font-family: inherit;
  white-space: nowrap;
  padding: 0 1rem;
}

.btn-quote:hover {
  background: #e6ac00;
}

/* Empty State */
.empty-state {
  text-align: center;
  padding: 4rem 2rem;
  color: #999;
}

.empty-state i {
  font-size: 3rem;
  margin-bottom: 1rem;
  display: block;
}

.empty-state p {
  margin-bottom: 1rem;
}

/* WhatsApp Help */
.whatsapp-help {
  margin-top: 2rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  background: white;
  border-radius: 12px;
  padding: 1.25rem 1.5rem;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}

@media (max-width: 768px) {
  .whatsapp-help {
    flex-direction: column;
    text-align: center;
  }
}

.whatsapp-icon {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: #22c55e;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.4rem;
  flex-shrink: 0;
}

.whatsapp-text {
  flex: 1;
}

.whatsapp-text strong {
  display: block;
  font-size: 0.9rem;
  color: #1a1a1a;
  margin-bottom: 0.2rem;
}

.whatsapp-text p {
  margin: 0;
  font-size: 0.8rem;
  color: #999;
}

.whatsapp-actions {
  display: flex;
  gap: 0.5rem;
  flex-shrink: 0;
}

.btn-whatsapp {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.6rem 1rem;
  background: #22c55e;
  color: white;
  border-radius: 8px;
  text-decoration: none;
  font-size: 0.85rem;
  font-weight: 600;
  transition: background 0.2s;
}

.btn-whatsapp:hover {
  background: #16a34a;
}

.btn-call {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.6rem 1rem;
  background: #f5f5f5;
  color: #1a1a1a;
  border-radius: 8px;
  text-decoration: none;
  font-size: 0.85rem;
  font-weight: 600;
  transition: background 0.2s;
}

.btn-call:hover {
  background: #eee;
}
</style>
