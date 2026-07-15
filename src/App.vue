<template>
  <header v-if="showHeader" class="site-header" :class="{ scrolled: isScrolled, 'compact-header': isCategoryPage }">
    <!-- Header normal (home y otras páginas) -->
    <template v-if="!isCategoryPage">
      <div v-if="showUtilityBar" class="utility-bar">
        <div class="utility-left">
          <i class="fas fa-bolt" aria-hidden="true"></i>
          <span>Más de 20 años suministrando soluciones eléctricas e industriales</span>
        </div>
        <div class="utility-right">
          <span><i class="fas fa-headset" aria-hidden="true"></i> Asesoría técnica</span>
          <span><i class="fas fa-truck" aria-hidden="true"></i> Envíos a todo el país</span>
          <span><i class="fas fa-phone" aria-hidden="true"></i> Línea nacional: 322 9118168</span>
        </div>
      </div>

      <nav class="header-main">
        <RouterLink class="brand-container" to="/" @click="closeMobileMenu">
          <img class="brand-logo-image" src="/images/logof.png" alt="DISEF Comercializadora Industrial" />
        </RouterLink>

        <form v-if="showHeaderSearch" class="header-search desktop-only" @submit.prevent="openSearch">
          <button type="button" class="search-category" @click="openSearch">Todas las categorías</button>
          <input
            class="search-input"
            type="search"
            placeholder="Buscar productos, marcas o categorías..."
            aria-label="Buscar productos"
            readonly
            @focus="openSearch"
          />
          <button type="submit" class="search-submit" aria-label="Buscar">
            <i class="fas fa-search" aria-hidden="true"></i>
          </button>
        </form>

        <div class="nav-actions desktop-only">
          <RouterLink v-if="!isLoggedIn" class="account-chip" to="/login">
            <i class="fas fa-user" aria-hidden="true"></i>
            <span>
              <small>Iniciar sesión</small>
              <strong>Mi cuenta</strong>
            </span>
          </RouterLink>

          <RouterLink v-else-if="isAdmin" class="account-chip" to="/admin/products">
            <i class="fas fa-user-shield" aria-hidden="true"></i>
            <span>
              <small>Panel admin</small>
              <strong>Mi cuenta</strong>
            </span>
          </RouterLink>

          <button v-else type="button" class="account-chip" @click="logout">
            <i class="fas fa-user" aria-hidden="true"></i>
            <span>
              <small>{{ username || 'Hola' }}</small>
              <strong>Mi cuenta</strong>
            </span>
          </button>

          <button type="button" class="cart-summary" @click="toggleDrawer">
            <span class="cart-summary-icon">
              <i class="fas fa-clipboard-list" aria-hidden="true"></i>
              <span v-if="totalItems > 0" class="cart-summary-badge">{{ totalItems }}</span>
            </span>
            <span class="cart-summary-text">
              <small>Mi Cotización</small>
            </span>
          </button>
        </div>

        <div class="mobile-header-controls">
          <button
            v-if="showHeaderSearch"
            type="button"
            class="search-toggle mobile-only"
            aria-label="Abrir buscador"
            title="Buscar"
            @click="openSearch"
          >
            <svg class="search-toggle-icon" viewBox="0 0 24 24" width="20" height="20" aria-hidden="true">
              <path
                fill="currentColor"
                d="M10 2a8 8 0 1 1 0 16 8 8 0 0 1 0-16zm8.707 17.293-4.387-4.387a9 9 0 1 0-1.414 1.414l4.387 4.387a1 1 0 0 0 1.414-1.414z"
              />
            </svg>
          </button>

          <button type="button" class="mobile-cart-btn" @click="toggleDrawer" aria-label="Abrir cotización">
            <i class="fas fa-clipboard-list" aria-hidden="true"></i>
            <span v-if="totalItems > 0" class="mobile-cart-badge">{{ totalItems }}</span>
          </button>
        </div>

        <button class="hamburger-menu" @click="toggleMobileMenu" :class="{ active: isMobileMenuOpen }">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </nav>

      <div class="nav-strip desktop-only">
        <div class="nav-menu">
          <RouterLink to="/" class="nav-link" :class="{ active: isCurrentRoute('/') }" @click="closeMobileMenu">Inicio</RouterLink>
          <div class="nav-products-dropdown" @mouseenter="productsDropdownOpen = true" @mouseleave="productsDropdownOpen = false">
            <RouterLink to="/productos" class="nav-link nav-link--products" :class="{ active: isCurrentRoute('/productos') }" @click="closeMobileMenu">
              Productos <i class="fas fa-chevron-down header-caret" aria-hidden="true"></i>
            </RouterLink>
            <div class="products-dropdown" :class="{ open: productsDropdownOpen }">
              <RouterLink to="/productos" class="dropdown-item" @click="productsDropdownOpen = false">
                Todos los productos
              </RouterLink>
              <RouterLink
                v-for="cat in categories"
                :key="cat.id"
                :to="'/' + slugify(cat.name)"
                class="dropdown-item"
                @click="productsDropdownOpen = false"
              >
                {{ cat.name }}
              </RouterLink>
            </div>
          </div>
          <RouterLink to="/servicios" class="nav-link" :class="{ active: isCurrentRoute('/servicios') }" @click="closeMobileMenu">Servicios</RouterLink>
          <RouterLink to="/#marcas" class="nav-link" @click.prevent="goToMarcas">Marcas</RouterLink>
          <RouterLink to="/#products" class="nav-link" @click.prevent="goToSection('products')">Destacados</RouterLink>
          <RouterLink to="/nosotros" class="nav-link" @click.prevent="goToSection('nosotros')">Nosotros</RouterLink>
          <RouterLink to="/contacto" class="nav-link" @click.prevent="goToSection('contacto')">Contacto</RouterLink>
        </div>
      </div>
    </template>

    <!-- Header compacto (páginas de categoría) -->
    <template v-else>
      <nav class="header-main header-main--compact">
        <RouterLink class="brand-container" to="/" @click="closeMobileMenu">
          <img class="brand-logo-image" src="/images/logof.png" alt="DISEF Comercializadora Industrial" />
        </RouterLink>

        <div class="nav-actions desktop-only">
        <form v-if="showHeaderSearch" class="header-search desktop-only" @submit.prevent="openSearch">
          <button type="button" class="search-category" @click="openSearch">Todas las categorías</button>
          <input
            class="search-input"
            type="search"
            placeholder="Buscar productos, marcas o categorías..."
            aria-label="Buscar productos"
            readonly
            @focus="openSearch"
          />
          <button type="submit" class="search-submit" aria-label="Buscar">
            <i class="fas fa-search" aria-hidden="true"></i>
          </button>
        </form>

          <button type="button" class="cart-summary" @click="toggleDrawer">
            <span class="cart-summary-icon">
              <i class="fas fa-clipboard-list" aria-hidden="true"></i>
              <span v-if="totalItems > 0" class="cart-summary-badge">{{ totalItems }}</span>
            </span>
            <span class="cart-summary-text">
              <small>Mi Cotización</small>
            </span>
          </button>
        </div>

        <div class="mobile-header-controls">
          <button type="button" class="search-toggle mobile-only" @click="openSearch" aria-label="Buscar">
            <svg viewBox="0 0 24 24" width="20" height="20" aria-hidden="true">
              <path fill="currentColor" d="M10 2a8 8 0 1 1 0 16 8 8 0 0 1 0-16zm8.707 17.293-4.387-4.387a9 9 0 1 0-1.414 1.414l4.387 4.387a1 1 0 0 0 1.414-1.414z"/>
            </svg>
          </button>
          <button type="button" class="mobile-cart-btn" @click="toggleDrawer" aria-label="Abrir cotización">
            <i class="fas fa-clipboard-list" aria-hidden="true"></i>
            <span v-if="totalItems > 0" class="mobile-cart-badge">{{ totalItems }}</span>
          </button>
        </div>

        <button class="hamburger-menu" @click="toggleMobileMenu" :class="{ active: isMobileMenuOpen }">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </nav>

      <div class="nav-strip desktop-only">
        <div class="nav-menu">
          <RouterLink to="/" class="nav-link" :class="{ active: isCurrentRoute('/') }" @click="closeMobileMenu">Inicio</RouterLink>
          <div class="nav-products-dropdown" @mouseenter="productsDropdownOpen = true" @mouseleave="productsDropdownOpen = false">
            <RouterLink to="/productos" class="nav-link nav-link--products" :class="{ active: isCurrentRoute('/productos') }" @click="closeMobileMenu">
              Productos <i class="fas fa-chevron-down header-caret" aria-hidden="true"></i>
            </RouterLink>
            <div class="products-dropdown" :class="{ open: productsDropdownOpen }">
              <RouterLink to="/productos" class="dropdown-item" @click="productsDropdownOpen = false">
                Todos los productos
              </RouterLink>
              <RouterLink
                v-for="cat in categories"
                :key="cat.id"
                :to="'/' + slugify(cat.name)"
                class="dropdown-item"
                @click="productsDropdownOpen = false"
              >
                {{ cat.name }}
              </RouterLink>
            </div>
          </div>
          <RouterLink to="/servicios" class="nav-link" :class="{ active: isCurrentRoute('/servicios') }" @click="closeMobileMenu">Servicios</RouterLink>
          <RouterLink to="/#marcas" class="nav-link" @click.prevent="goToMarcas">Marcas</RouterLink>
          <RouterLink to="/#products" class="nav-link" @click.prevent="goToSection('products')">Destacados</RouterLink>
          <RouterLink to="/nosotros" class="nav-link" @click.prevent="goToSection('nosotros')">Nosotros</RouterLink>
          <RouterLink to="/contacto" class="nav-link" @click.prevent="goToSection('contacto')">Contacto</RouterLink>
        </div>
      </div>
    </template>

    <!-- Mobile Menu -->
    <div class="mobile-menu" :class="{ active: isMobileMenuOpen }">
      <div class="mobile-menu-content">
        <div class="mobile-nav-links">
          <RouterLink to="/" class="mobile-link" :class="{ active: isCurrentRoute('/') }" @click="closeMobileMenu">Inicio</RouterLink>
          <RouterLink to="/productos" class="mobile-link" :class="{ active: isCurrentRoute('/productos') }" @click="closeMobileMenu">
            Productos <i class="fas fa-chevron-down header-caret" aria-hidden="true"></i>
          </RouterLink>
          <div class="mobile-products-list">
            <RouterLink to="/productos" class="mobile-category-link" @click="closeMobileMenu">
              Todos los productos
            </RouterLink>
            <RouterLink
              v-for="cat in categories"
              :key="cat.id"
              :to="'/' + slugify(cat.name)"
              class="mobile-category-link"
              @click="closeMobileMenu"
            >
              {{ cat.name }}
            </RouterLink>
          </div>
          <RouterLink to="/servicios" class="mobile-link" :class="{ active: isCurrentRoute('/servicios') }" @click="closeMobileMenu">Servicios</RouterLink>
          <RouterLink to="/#marcas" class="mobile-link" @click.prevent="goToMarcas">Marcas</RouterLink>
          <RouterLink to="/#products" class="mobile-link" @click.prevent="goToSection('products')">Destacados</RouterLink>
          <RouterLink to="/nosotros" class="mobile-link" @click.prevent="goToSection('nosotros')">Nosotros</RouterLink>
          <RouterLink to="/contacto" class="mobile-link" @click.prevent="goToSection('contacto')">Contacto</RouterLink>
        </div>

        <div class="mobile-menu-footer">
          <div v-if="isLoggedIn" class="mobile-user-info">
            <i class="fas fa-user"></i>
            <span>{{ username }}</span>
            <button class="mobile-logout-btn" @click="handleMobileLogout">Cerrar sesión</button>
          </div>
          <RouterLink v-else to="/login" class="mobile-login-link" @click="closeMobileMenu">
            <i class="fas fa-sign-in-alt"></i> Iniciar sesión
          </RouterLink>
        </div>
      </div>
    </div>

    <GlobalSearchOverlay v-model:open="isSearchOpen" />
  </header>

  <main class="main-content" :style="{ paddingTop: showHeader ? (isCategoryPage ? (isScrolled ? '85px' : '117px') : (showUtilityBar ? (isScrolled ? '85px' : '117px') : '85px')) : '0' }">
    <RouterView />
  </main>

  <ProductQuickViewModal
    :open="quickViewOpen"
    :product="quickViewProduct"
    @close="closeQuickView"
  />

  <GlobalCart />
  <SocialFloating v-if="showUtilityBar" />
</template>

<script setup lang="ts">
import { RouterLink, RouterView, useRoute } from 'vue-router'
import { authService } from '@/services/api'
import { computed, onMounted, onUnmounted, ref, watch } from 'vue'
import router from './router'
import SocialFloating from '@/components/SocialFloating.vue'
import GlobalCart from '@/components/GlobalCart.vue'
import GlobalSearchOverlay from '@/components/GlobalSearchOverlay.vue'
import ProductQuickViewModal from '@/components/ProductQuickViewModal.vue'
import { useProductQuickView } from '@/composables/useProductQuickView'
import { useQuotation } from '@/composables/useQuotation'
import { useCategories } from '@/composables/useCategories'

const isLoggedIn = ref(false)
const username = ref('')
const isMobileMenuOpen = ref(false)
const isSearchOpen = ref(false)
const isScrolled = ref(false)
const productsDropdownOpen = ref(false)

const { isOpen: quickViewOpen, product: quickViewProduct, close: closeQuickView } = useProductQuickView()
const { totalItems, toggleDrawer } = useQuotation()
const { categories, loadCategories } = useCategories()

const currentRoute = useRoute()
const isAdmin = computed(() => authService.isAdmin())
const isCategoryPage = computed(() => {
  const path = currentRoute.path
  return path === '/productos' || (path !== '/' && path !== '/login' && path !== '/servicios' && path !== '/contacto' && !path.startsWith('/admin') && !path.startsWith('/payment') && !path.startsWith('/ofertas') && !path.startsWith('/terms') && !path.startsWith('/test'))
})
const showHeaderSearch = computed(() => !currentRoute.path.startsWith('/admin') && currentRoute.path !== '/login')
const showHeader = computed(() => !currentRoute.path.startsWith('/admin'))
const showUtilityBar = computed(() => !currentRoute.path.startsWith('/admin') && currentRoute.path !== '/login')

const isCurrentRoute = (path: string): boolean => currentRoute.path === path

const slugify = (value: string): string =>
  value
    .trim()
    .toLowerCase()
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
    .replace(/[^a-z0-9\s-]/g, '')
    .replace(/\s+/g, '-')
    .replace(/-+/g, '-')

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}

const goToMarcas = () => {
  closeMobileMenu()
  if (currentRoute.path === '/') {
    document.getElementById('marcas')?.scrollIntoView({ behavior: 'smooth' })
  } else {
    router.push('/').then(() => {
      setTimeout(() => {
        document.getElementById('marcas')?.scrollIntoView({ behavior: 'smooth' })
      }, 300)
    })
  }
}

const goToSection = (sectionId: string) => {
  closeMobileMenu()
  if (currentRoute.path === '/') {
    document.getElementById(sectionId)?.scrollIntoView({ behavior: 'smooth' })
  } else {
    router.push('/').then(() => {
      setTimeout(() => {
        document.getElementById(sectionId)?.scrollIntoView({ behavior: 'smooth' })
      }, 300)
    })
  }
}

const openSearch = () => {
  isSearchOpen.value = true
}

const checkAuthStatus = () => {
  isLoggedIn.value = authService.isAuthenticated()
  if (isLoggedIn.value) {
    const currentUser = authService.getCurrentUser()
    username.value = currentUser?.name || ''
  } else {
    username.value = ''
  }
}

const logout = () => {
  authService.logout()
  isLoggedIn.value = false
  username.value = ''
  router.replace({ name: 'home' })
}

const handleMobileLogout = () => {
  closeMobileMenu()
  logout()
}

const handleScroll = () => {
  isScrolled.value = window.scrollY > 40
}

onMounted(() => {
  checkAuthStatus()
  window.addEventListener('scroll', handleScroll)
  loadCategories()
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})

watch(currentRoute, () => {
  checkAuthStatus()
})

defineOptions({
  name: 'App'
})
</script>

<style scoped>
.site-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 9999;
  background: #0b0b0b;
  color: #ffffff;
  width: 100%;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
}

.utility-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 18px;
  height: 32px;
  padding: 0 clamp(16px, 4vw, 44px);
  background: #0b0b0b;
  border-bottom: 1px solid rgba(255, 193, 7, 0.18);
  font-size: 13px;
  letter-spacing: 0.1px;
  transition: height 0.3s ease, opacity 0.3s ease, padding 0.3s ease;
  overflow: hidden;
}

.scrolled .utility-bar {
  height: 0;
  opacity: 0;
  padding: 0 clamp(16px, 4vw, 44px);
  border-bottom: none;
}

.utility-left,
.utility-right {
  display: flex;
  align-items: center;
  gap: 14px;
  flex-wrap: wrap;
}

.utility-left {
  color: #ffffff;
  font-weight: 500;
}

.utility-right {
  color: #ffffff;
  justify-content: flex-end;
}

.utility-left i,
.utility-right i {
  color: #ffc107;
  margin-right: 6px;
}

.header-main {
  min-height: 85px;
  height: 85px;
  padding: 0 clamp(16px, 4vw, 44px);
  background: #0b0b0b;
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
  display: flex;
  align-items: center;
  gap: 24px;
}

.header-main--compact {
  min-height: 60px;
  height: 60px;
  justify-content: space-between;
}

.compact-header .utility-bar {
  display: none;
}

.brand-container {
  flex-shrink: 0;
  display: flex;
  align-items: center;
}

.brand-logo-image {
  width: clamp(160px, 18vw, 220px);
  height: 56px;
  object-fit: contain;
  background: transparent;
  padding: 0;
  box-shadow: none;
}

.header-main--compact .brand-logo-image {
  height: 42px;
}

.search-icon-btn {
  width: 42px;
  height: 42px;
  border-radius: 12px;
  border: 1px solid rgba(255, 193, 7, 0.28);
  background: transparent;
  color: #ffc107;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 18px;
  transition: all 0.2s ease;
}

.search-icon-btn:hover {
  background: rgba(255, 193, 7, 0.1);
  border-color: rgba(255, 193, 7, 0.5);
}

.header-search {
  flex: 1;
  max-width: 600px;
  display: flex;
  align-items: center;
  margin: 0 20px;
  border-radius: 12px;
  overflow: hidden;
  border: 1px solid rgba(255, 193, 7, 0.24);
  background: #ffffff;
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.18);
}

.search-category {
  border: 0;
  background: #f5f5f5;
  color: #0b0b0b;
  padding: 0 18px;
  min-height: 50px;
  font-weight: 600;
  white-space: nowrap;
  cursor: pointer;
}

.search-input {
  flex: 1;
  min-width: 0;
  height: 50px;
  border: 0;
  background: #ffffff;
  color: #0b0b0b;
  padding: 0 18px;
  font-size: 14px;
  outline: none;
}

.search-input::placeholder {
  color: #7a7a7a;
}

.search-submit {
  width: 56px;
  height: 50px;
  border: 0;
  background: #ffc107;
  color: #0b0b0b;
  cursor: pointer;
}

.nav-actions {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-left: auto;
}

.account-chip {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 8px 12px;
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  background: transparent;
  color: #ffffff;
  cursor: pointer;
  text-align: left;
  text-decoration: none;
  transition: transform 0.2s ease, border-color 0.2s ease, background 0.2s ease;
}

.account-chip:hover {
  transform: translateY(-1px);
  border-color: rgba(255, 193, 7, 0.35);
  background: rgba(255, 193, 7, 0.06);
}

.account-chip i {
  font-size: 20px;
  color: #ffc107;
}

.account-chip span {
  display: flex;
  flex-direction: column;
  line-height: 1.05;
}

.account-chip small {
  font-size: 11px;
  color: #a0a0a0;
  font-weight: 500;
}

.account-chip strong {
  font-size: 13px;
  font-weight: 600;
}

.cart-summary {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  padding: 8px 12px;
  border-radius: 12px;
  border: 1px solid rgba(255, 193, 7, 0.25);
  background: transparent;
  color: #ffffff;
  cursor: pointer;
}

.cart-summary-icon {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
}

.cart-summary-badge,
.mobile-cart-badge {
  position: absolute;
  top: -8px;
  right: -8px;
  min-width: 20px;
  height: 20px;
  padding: 0 4px;
  border-radius: 999px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  background: #ffc107;
  color: #0b0b0b;
  font-size: 11px;
  font-weight: 800;
}

.cart-summary-text {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  line-height: 1.1;
}

.cart-summary-text small {
  font-size: 11px;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.7px;
  color: #a0a0a0;
}

.cart-summary-text strong {
  font-size: 13px;
  font-weight: 600;
}

.nav-strip {
  display: flex;
  align-items: center;
  width: 100%;
  height: 55px;
  background: #0b0b0b;
  border-bottom: 1px solid #ffc107;
  position: relative;
  z-index: 1;
}

.nav-menu {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 38px;
  width: 100%;
  height: 100%;
}

.nav-link {
  color: #ffffff;
  text-decoration: none;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  font-size: 15px;
  font-weight: 500;
  position: relative;
  height: 100%;
  display: inline-flex;
  align-items: center;
  white-space: nowrap;
  cursor: pointer;
  transition: color 0.3s ease;
}

.nav-link--products {
  gap: 6px;
}

.header-caret {
  font-size: 10px;
  transform: translateY(1px);
}

.nav-link::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  bottom: 8px;
  height: 3px;
  background: #ffc107;
  transform: scaleX(0);
  transform-origin: left center;
  transition: transform 0.3s ease;
}

.nav-link:hover,
.nav-link.active {
  color: #ffc107;
}

.nav-link:hover::after,
.nav-link.active::after {
  transform: scaleX(1);
}

/* Productos Dropdown */
.nav-products-dropdown {
  position: relative;
  height: 100%;
  display: inline-flex;
  align-items: center;
}

.products-dropdown {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%) translateY(4px);
  min-width: 200px;
  background: #1a1a1a;
  border: 1px solid rgba(255, 193, 7, 0.2);
  border-radius: 8px;
  padding: 6px 0;
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  transition: opacity 0.2s ease, transform 0.2s ease, visibility 0.2s ease;
  z-index: 100;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.4);
}

.products-dropdown.open {
  opacity: 1;
  visibility: visible;
  pointer-events: auto;
  transform: translateX(-50%) translateY(0);
}

.dropdown-item {
  display: block;
  padding: 8px 18px;
  color: #e0e0e0;
  text-decoration: none;
  font-size: 14px;
  font-weight: 400;
  transition: background 0.2s ease, color 0.2s ease;
  white-space: nowrap;
}

.dropdown-item:hover {
  background: rgba(255, 193, 7, 0.1);
  color: #ffc107;
}

.mobile-products-list {
  display: flex;
  flex-direction: column;
  padding-left: 16px;
}

.mobile-category-link {
  padding: 6px 0;
  color: #b0b0b0;
  text-decoration: none;
  font-size: 13px;
  transition: color 0.2s ease;
}

.mobile-category-link:hover {
  color: #ffc107;
}

.mobile-header-controls {
  display: none;
  align-items: center;
  gap: 10px;
  margin-left: auto;
}

.search-toggle {
  width: 42px;
  height: 42px;
  border-radius: 999px;
  border: 1px solid rgba(255, 193, 7, 0.28);
  background: transparent;
  color: #ffc107;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.search-toggle.mobile-only {
  display: none;
}

.mobile-cart-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 42px;
  height: 42px;
  border-radius: 999px;
  border: 1px solid rgba(255, 193, 7, 0.28);
  background: transparent;
  color: #ffc107;
}

.hamburger-menu {
  margin-left: 10px;
  display: none;
  flex-direction: column;
  width: 30px;
  height: 30px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  justify-content: space-around;
  align-items: center;
  z-index: 1001;
}

.hamburger-menu span {
  display: block;
  height: 3px;
  width: 100%;
  background-color: #ffffff;
  border-radius: 3px;
  transition: all 0.3s ease;
}

.hamburger-menu.active span:nth-child(1) {
  transform: rotate(45deg) translate(8px, 8px);
}

.hamburger-menu.active span:nth-child(2) {
  opacity: 0;
}

.hamburger-menu.active span:nth-child(3) {
  transform: rotate(-45deg) translate(7px, -6px);
}

.mobile-menu {
  display: none;
  position: fixed;
  top: 117px;
  left: 0;
  width: 100%;
  height: calc(100vh - 117px);
  background: #0b0b0b;
  transform: translateX(-100%);
  transition: transform 0.3s ease;
  z-index: 9998;
  overflow-y: auto;
}

.mobile-menu.active {
  transform: translateX(0);
}

.mobile-menu-content {
  padding: 30px 20px;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.mobile-nav-links {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.mobile-link {
  color: #ffffff;
  text-decoration: none;
  padding: 15px 20px;
  font-size: 18px;
  font-weight: 500;
  border-radius: 12px;
  transition: all 0.3s ease;
  text-align: center;
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.08);
}

.mobile-link.active,
.mobile-link:hover {
  color: #ffc107;
}

.mobile-controls {
  display: flex;
  flex-direction: column;
  gap: 15px;
  padding-top: 20px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.mobile-btn {
  padding: 15px 20px;
  border-radius: 12px;
  text-decoration: none;
  font-weight: 600;
  font-size: 16px;
  text-align: center;
  transition: all 0.3s ease;
  border: none;
  cursor: pointer;
  width: 100%;
  font-family: inherit;
}

.mobile-btn.access-btn,
.mobile-btn.admin-btn,
.mobile-btn.logout-btn {
  background: transparent;
  color: #ffffff;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.mobile-user-greeting {
  color: #e2e8f0;
  text-align: center;
  padding: 15px 20px;
  font-weight: 600;
  font-size: 16px;
  background: transparent;
  border-radius: 12px;
}

@media (max-width: 768px) {
  .utility-bar {
    flex-direction: column;
    align-items: flex-start;
    padding: 8px 16px;
    gap: 6px;
    height: auto;
  }

  .utility-right {
    gap: 10px;
  }

  .header-main {
    height: 85px;
    padding: 12px 16px;
  }

  .desktop-only {
    display: none;
  }

  .mobile-header-controls {
    display: flex;
  }

  .search-toggle.mobile-only {
    display: inline-flex;
  }

  .hamburger-menu {
    display: flex;
  }

  .mobile-menu {
    display: block;
    top: 117px;
    height: calc(100vh - 117px);
  }
}

@media (max-width: 480px) {
  .utility-right {
    display: none;
  }

  .brand-logo-image {
    width: clamp(130px, 42vw, 180px);
    height: 46px;
  }

  .header-main {
    height: 70px;
    padding: 10px 12px;
  }

  .mobile-menu {
    top: 100px;
    height: calc(100vh - 100px);
  }

  .search-bar-container {
    padding: 8px 12px;
  }

  .search-bar-input {
    font-size: 14px;
  }
}
</style>
