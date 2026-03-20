<template>
  <div class="app-layout">
    <!-- Sidebar -->
    <aside class="sidebar" :class="{ collapsed: sidebarCollapsed, 'force-expanded': sidebarForceExpanded }">
      <div class="sidebar-brand">
        <span class="brand-icon">
          <svg width="28" height="28" viewBox="0 0 28 28" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="2" y="2" width="11" height="11" rx="2" fill="#3b82f6"/>
            <rect x="15" y="2" width="11" height="11" rx="2" fill="#3b82f6" opacity="0.6"/>
            <rect x="2" y="15" width="11" height="11" rx="2" fill="#3b82f6" opacity="0.6"/>
            <rect x="15" y="15" width="11" height="11" rx="2" fill="#3b82f6" opacity="0.4"/>
          </svg>
        </span>
        <div class="brand-text">
          <span class="brand-name">{{ t('nav.companyName') }}</span>
          <span class="brand-subtitle">{{ t('nav.subtitle') }}</span>
        </div>
      </div>

      <nav class="sidebar-nav">
        <router-link to="/" class="nav-item" :class="{ active: $route.path === '/' }" :title="t('nav.overview')" exact @click="closeSidebar">
          <span class="nav-icon">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect x="2" y="2" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
              <rect x="11" y="2" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
              <rect x="2" y="11" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
              <rect x="11" y="11" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.overview') }}</span>
        </router-link>

        <router-link to="/inventory" class="nav-item" :class="{ active: $route.path === '/inventory' }" :title="t('nav.inventory')" @click="closeSidebar">
          <span class="nav-icon">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M10 2L17 5.5V14.5L10 18L3 14.5V5.5L10 2Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
              <path d="M10 2V18" stroke="currentColor" stroke-width="1.5"/>
              <path d="M3 5.5L10 9L17 5.5" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.inventory') }}</span>
        </router-link>

        <router-link to="/orders" class="nav-item" :class="{ active: $route.path === '/orders' }" :title="t('nav.orders')" @click="closeSidebar">
          <span class="nav-icon">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect x="4" y="2" width="12" height="16" rx="2" stroke="currentColor" stroke-width="1.5"/>
              <path d="M7 7H13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              <path d="M7 10H13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              <path d="M7 13H10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.orders') }}</span>
        </router-link>

        <router-link to="/spending" class="nav-item" :class="{ active: $route.path === '/spending' }" :title="t('nav.finance')" @click="closeSidebar">
          <span class="nav-icon">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
              <circle cx="10" cy="10" r="8" stroke="currentColor" stroke-width="1.5"/>
              <path d="M10 5V6.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              <path d="M10 13.5V15" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              <path d="M7.5 8C7.5 7 8.5 6.5 10 6.5C11.5 6.5 12.5 7.25 12.5 8.25C12.5 9.25 11.5 9.75 10 10C8.5 10.25 7.5 10.75 7.5 11.75C7.5 12.75 8.5 13.5 10 13.5C11.5 13.5 12.5 13 12.5 12" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.finance') }}</span>
        </router-link>

        <router-link to="/demand" class="nav-item" :class="{ active: $route.path === '/demand' }" :title="t('nav.demandForecast')" @click="closeSidebar">
          <span class="nav-icon">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M2 14L7 9L11 12L18 5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M14 5H18V9" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.demandForecast') }}</span>
        </router-link>

        <router-link to="/reports" class="nav-item" :class="{ active: $route.path === '/reports' }" title="Reports" @click="closeSidebar">
          <span class="nav-icon">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect x="2" y="12" width="4" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
              <rect x="8" y="8" width="4" height="10" rx="1" stroke="currentColor" stroke-width="1.5"/>
              <rect x="14" y="4" width="4" height="14" rx="1" stroke="currentColor" stroke-width="1.5"/>
            </svg>
          </span>
          <span class="nav-label">Reports</span>
        </router-link>

        <router-link to="/restocking" class="nav-item" :class="{ active: $route.path === '/restocking' }" :title="t('nav.restocking')" @click="closeSidebar">
          <span class="nav-icon">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M4 10C4 6.686 6.686 4 10 4C12.21 4 14.14 5.172 15.2 6.927" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              <path d="M16 10C16 13.314 13.314 16 10 16C7.79 16 5.86 14.828 4.8 13.073" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              <path d="M13 5L15.5 7L18 5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M2 15L4.5 13L7 15" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.restocking') }}</span>
        </router-link>
      </nav>

      <div class="sidebar-filters">
        <FilterBar />
      </div>

      <div class="sidebar-footer">
        <button class="collapse-btn" @click="toggleSidebar" :title="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
          <svg
            width="16"
            height="16"
            viewBox="0 0 16 16"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            class="collapse-icon"
            :class="{ rotated: sidebarCollapsed }"
          >
            <path d="M10 3L5 8L10 13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="collapse-label">Collapse</span>
        </button>
      </div>
    </aside>
    <div v-if="sidebarForceExpanded" class="sidebar-backdrop" @click="closeSidebar"></div>

    <!-- Top Bar -->
    <header class="topbar" :class="{ 'sidebar-collapsed': sidebarCollapsed }">
      <div class="topbar-right">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </header>

    <!-- Main Content -->
    <main class="main-content" :class="{ 'sidebar-collapsed': sidebarCollapsed }">
      <router-view />
    </main>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const apiTasks = ref([])
    const sidebarCollapsed = ref(false)
    const sidebarForceExpanded = ref(false)
    const isSmallScreen = ref(window.innerWidth <= 1024)

    const onResize = () => {
      isSmallScreen.value = window.innerWidth <= 1024
      if (!isSmallScreen.value) {
        sidebarForceExpanded.value = false
      }
    }

    const toggleSidebar = () => {
      if (isSmallScreen.value) {
        sidebarForceExpanded.value = !sidebarForceExpanded.value
      } else {
        sidebarCollapsed.value = !sidebarCollapsed.value
      }
    }

    const closeSidebar = () => {
      sidebarForceExpanded.value = false
    }

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(() => {
      window.addEventListener('resize', onResize)
      loadTasks()
    })

    onUnmounted(() => {
      window.removeEventListener('resize', onResize)
    })

    return {
      t,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask,
      sidebarCollapsed,
      sidebarForceExpanded,
      isSmallScreen,
      toggleSidebar,
      closeSidebar
    }
  }
}
</script>

<style>
:root {
  --sidebar-width: 240px;
  --sidebar-collapsed-width: 64px;
  --topbar-height: 56px;
  --content-padding: 24px;
  --color-bg-primary: #ffffff;
  --color-bg-secondary: #f8fafc;
  --color-bg-sidebar: #0f172a;
  --color-text-primary: #0f172a;
  --color-text-secondary: #64748b;
  --color-text-sidebar: #94a3b8;
  --color-text-sidebar-active: #ffffff;
  --color-border: #e2e8f0;
  --color-accent: #3b82f6;
  --color-accent-light: #dbeafe;
  --color-accent-hover: #2563eb;
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-danger: #ef4444;
  --color-info: #3b82f6;
  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 1px 3px rgba(0, 0, 0, 0.08);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: var(--color-bg-secondary);
  color: var(--color-text-primary);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ---- App Layout ---- */
.app-layout {
  display: flex;
  min-height: 100vh;
}

/* ---- Sidebar ---- */
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: var(--sidebar-width);
  background: var(--color-bg-sidebar);
  z-index: 200;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  transition: width 0.2s ease;
}

.sidebar.collapsed {
  width: var(--sidebar-collapsed-width);
}

/* Brand */
.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 20px 16px 16px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  flex-shrink: 0;
  overflow: hidden;
}

.brand-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 32px;
  height: 32px;
}

.brand-text {
  display: flex;
  flex-direction: column;
  gap: 2px;
  overflow: hidden;
  white-space: nowrap;
  opacity: 1;
  transition: opacity 0.15s ease;
}

.sidebar.collapsed .brand-text {
  opacity: 0;
  pointer-events: none;
}

.brand-name {
  font-size: 0.938rem;
  font-weight: 700;
  color: #f1f5f9;
  letter-spacing: -0.015em;
  line-height: 1.2;
}

.brand-subtitle {
  font-size: 0.75rem;
  color: #94a3b8;
  font-weight: 400;
  line-height: 1.2;
}

/* Nav */
.sidebar-nav {
  display: flex;
  flex-direction: column;
  gap: 2px;
  padding: 12px 8px;
  flex-shrink: 0;
  overflow: hidden;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 12px;
  border-radius: var(--radius-sm);
  color: var(--color-text-sidebar);
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 400;
  transition: background 0.15s ease, color 0.15s ease;
  white-space: nowrap;
  overflow: hidden;
  min-height: 38px;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #e2e8f0;
}

.nav-item.active {
  background: var(--color-accent);
  color: var(--color-text-sidebar-active);
  font-weight: 500;
}

.nav-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 20px;
  height: 20px;
}

.nav-label {
  opacity: 1;
  transition: opacity 0.15s ease;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar.collapsed .nav-label {
  opacity: 0;
  width: 0;
  pointer-events: none;
}

/* Filters section */
.sidebar-filters {
  flex: 1;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  overflow-y: auto;
  overflow-x: hidden;
}

.sidebar.collapsed .sidebar-filters {
  overflow: hidden;
}

/* Footer / collapse toggle */
.sidebar-footer {
  flex-shrink: 0;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  padding: 8px;
}

.collapse-btn {
  display: flex;
  align-items: center;
  gap: 10px;
  width: 100%;
  padding: 8px 12px;
  background: none;
  border: none;
  border-radius: var(--radius-sm);
  color: var(--color-text-sidebar);
  cursor: pointer;
  font-size: 0.875rem;
  transition: background 0.15s ease, color 0.15s ease;
  white-space: nowrap;
  overflow: hidden;
}

.collapse-btn:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #e2e8f0;
}

.collapse-icon {
  flex-shrink: 0;
  transition: transform 0.2s ease;
}

.collapse-icon.rotated {
  transform: rotate(180deg);
}

.collapse-label {
  opacity: 1;
  transition: opacity 0.15s ease;
}

.sidebar.collapsed .collapse-label {
  opacity: 0;
  width: 0;
  pointer-events: none;
}

/* ---- Top Bar ---- */
.topbar {
  position: fixed;
  top: 0;
  left: var(--sidebar-width);
  right: 0;
  height: var(--topbar-height);
  background: var(--color-bg-primary);
  border-bottom: 1px solid var(--color-border);
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding: 0 1.5rem;
  transition: left 0.2s ease;
}

.topbar.sidebar-collapsed {
  left: var(--sidebar-collapsed-width);
}

.topbar-right {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

/* ---- Main Content ---- */
.main-content {
  margin-left: var(--sidebar-width);
  margin-top: var(--topbar-height);
  min-height: calc(100vh - var(--topbar-height));
  background: var(--color-bg-secondary);
  padding: var(--content-padding) 32px;
  transition: margin-left 0.2s ease;
  flex: 1;
}

.main-content.sidebar-collapsed {
  margin-left: var(--sidebar-collapsed-width);
}

/* ---- Page Header ---- */
.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: var(--color-text-primary);
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: var(--color-text-secondary);
  font-size: 0.938rem;
}

/* ---- Stats Grid ---- */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: var(--color-bg-primary);
  padding: 1.25rem;
  border-radius: var(--radius-lg);
  border: 1px solid var(--color-border);
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.stat-label {
  color: var(--color-text-secondary);
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: var(--color-text-primary);
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value {
  color: #ea580c;
}

.stat-card.success .stat-value {
  color: #059669;
}

.stat-card.danger .stat-value {
  color: #dc2626;
}

.stat-card.info .stat-value {
  color: #2563eb;
}

/* ---- Card ---- */
.card {
  background: var(--color-bg-primary);
  border-radius: var(--radius-lg);
  padding: var(--content-padding);
  border: 1px solid var(--color-border);
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid var(--color-border);
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: var(--color-text-primary);
  letter-spacing: -0.025em;
}

/* ---- Table ---- */
.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: var(--color-bg-secondary);
  border-top: 1px solid var(--color-border);
  border-bottom: 1px solid var(--color-border);
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: var(--color-bg-secondary);
}

/* ---- Badges ---- */
.badge {
  display: inline-block;
  padding: 0.25rem 0.625rem;
  border-radius: 9999px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success {
  background: #d1fae5;
  color: #065f46;
}

.badge.warning {
  background: #fed7aa;
  color: #92400e;
}

.badge.danger {
  background: #fecaca;
  color: #991b1b;
}

.badge.info {
  background: var(--color-accent-light);
  color: #1e40af;
}

.badge.increasing {
  background: #d1fae5;
  color: #065f46;
}

.badge.decreasing {
  background: #fecaca;
  color: #991b1b;
}

.badge.stable {
  background: #e0e7ff;
  color: #3730a3;
}

.badge.high {
  background: #fecaca;
  color: #991b1b;
}

.badge.medium {
  background: #fed7aa;
  color: #92400e;
}

.badge.low {
  background: var(--color-accent-light);
  color: #1e40af;
}

/* ---- Loading / Error ---- */
.loading {
  text-align: center;
  padding: 3rem;
  color: var(--color-text-secondary);
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: var(--radius-md);
  margin: 1rem 0;
  font-size: 0.938rem;
}

/* ---- Responsive: Tablet ---- */
@media (max-width: 1024px) {
  .sidebar:not(.force-expanded) {
    width: var(--sidebar-collapsed-width);
  }

  .sidebar:not(.force-expanded) .nav-label,
  .sidebar:not(.force-expanded) .brand-text,
  .sidebar:not(.force-expanded) .collapse-label {
    opacity: 0;
    width: 0;
    pointer-events: none;
  }

  .sidebar:not(.force-expanded) .sidebar-filters {
    overflow: hidden;
  }

  .topbar {
    left: var(--sidebar-collapsed-width);
  }

  .main-content {
    margin-left: var(--sidebar-collapsed-width);
  }

  /* When force-expanded on small screen, overlay */
  .sidebar.force-expanded {
    width: var(--sidebar-width);
    box-shadow: 4px 0 24px rgba(0, 0, 0, 0.3);
    z-index: 300;
  }

  .sidebar.force-expanded .nav-label,
  .sidebar.force-expanded .brand-text,
  .sidebar.force-expanded .collapse-label {
    opacity: 1;
    width: auto;
    pointer-events: auto;
  }

  .sidebar.force-expanded .sidebar-filters {
    overflow-y: auto;
    overflow-x: hidden;
  }
}

/* ---- Responsive: Mobile ---- */
@media (max-width: 768px) {
  .main-content {
    padding: var(--content-padding) 16px;
  }

  .page-header h2 {
    font-size: 1.5rem;
  }

  .stats-grid {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }
}

/* ---- Sidebar Backdrop ---- */
.sidebar-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.4);
  z-index: 250;
  transition: opacity 0.2s ease;
}
</style>
