<template>
  <div class="app">
    <!-- Scrim behind the off-canvas sidebar on narrow viewports -->
    <div
      v-if="sidebarOpen"
      class="sidebar-scrim"
      @click="sidebarOpen = false"
    ></div>

    <aside class="sidebar" :class="{ open: sidebarOpen, collapsed: sidebarCollapsed }">
      <div class="brand">
        <!-- Full name/subtitle when expanded; compact monogram when icons-only -->
        <div class="brand-full">
          <h1>{{ t('nav.companyName') }}</h1>
          <span class="subtitle">{{ t('nav.subtitle') }}</span>
        </div>
        <div class="brand-monogram" aria-hidden="true">{{ brandInitial }}</div>
      </div>

      <div class="nav-section-label">Navigation</div>
      <!-- Clicking any link closes the mobile overlay; harmless no-op on desktop.
           Each link carries :title so hover tooltips identify it when only the
           icon is visible (collapsed / tablet icons-only modes). -->
      <nav class="side-nav" @click="sidebarOpen = false">
        <router-link to="/" :class="{ active: $route.path === '/' }" :title="t('nav.overview')">
          <span class="nav-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="3" y="3" width="7" height="7"/>
              <rect x="14" y="3" width="7" height="7"/>
              <rect x="14" y="14" width="7" height="7"/>
              <rect x="3" y="14" width="7" height="7"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.overview') }}</span>
        </router-link>
        <router-link to="/inventory" :class="{ active: $route.path === '/inventory' }" :title="t('nav.inventory')">
          <span class="nav-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"/>
              <polyline points="3.27 6.96 12 12.01 20.73 6.96"/>
              <line x1="12" y1="22.08" x2="12" y2="12"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.inventory') }}</span>
        </router-link>
        <router-link to="/orders" :class="{ active: $route.path === '/orders' }" :title="t('nav.orders')">
          <span class="nav-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M16 4h2a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h2"/>
              <rect x="8" y="2" width="8" height="4" rx="1" ry="1"/>
              <line x1="9" y1="12" x2="15" y2="12"/>
              <line x1="9" y1="16" x2="15" y2="16"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.orders') }}</span>
        </router-link>
        <router-link to="/spending" :class="{ active: $route.path === '/spending' }" :title="t('nav.finance')">
          <span class="nav-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="10"/>
              <line x1="12" y1="6" x2="12" y2="18"/>
              <path d="M15 9.5A2.5 2.5 0 0 0 12.5 7h-1a2.5 2.5 0 0 0 0 5h1a2.5 2.5 0 0 1 0 5h-1A2.5 2.5 0 0 1 9 14.5"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.finance') }}</span>
        </router-link>
        <router-link to="/demand" :class="{ active: $route.path === '/demand' }" :title="t('nav.demandForecast')">
          <span class="nav-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="23 6 13.5 15.5 8.5 10.5 1 18"/>
              <polyline points="17 6 23 6 23 12"/>
            </svg>
          </span>
          <span class="nav-label">{{ t('nav.demandForecast') }}</span>
        </router-link>
        <router-link to="/reports" :class="{ active: $route.path === '/reports' }" title="Reports">
          <span class="nav-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <line x1="18" y1="20" x2="18" y2="10"/>
              <line x1="12" y1="20" x2="12" y2="4"/>
              <line x1="6" y1="20" x2="6" y2="14"/>
            </svg>
          </span>
          <span class="nav-label">Reports</span>
        </router-link>
      </nav>

      <!-- Collapse toggle, pinned to the sidebar bottom via margin-top: auto.
           Hidden on tablet (icons-only is forced there, so the toggle would lie)
           and on mobile (the drawer is always expanded). Labels are hardcoded
           English because only App.vue may change (no new i18n keys). -->
      <button
        class="collapse-toggle"
        :title="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'"
        :aria-label="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'"
        @click="sidebarCollapsed = !sidebarCollapsed"
      >
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="15 18 9 12 15 6"/>
        </svg>
      </button>
    </aside>

    <div class="main-column">
      <header class="top-bar">
        <button
          class="sidebar-toggle"
          aria-label="Toggle navigation"
          @click="sidebarOpen = !sidebarOpen"
        >
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M3 5H17M3 10H17M3 15H17" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
        </button>
        <FilterBar />
        <div class="top-bar-actions">
          <LanguageSwitcher />
          <ProfileMenu
            @show-profile-details="showProfileDetails = true"
            @show-tasks="showTasks = true"
          />
        </div>
      </header>

      <main class="main-content">
        <router-view />
      </main>
    </div>

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
import { ref, onMounted, computed } from 'vue'
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

    // Off-canvas sidebar state for narrow viewports (<900px); a class toggle
    // slides the sidebar in/out — no effect on desktop where it is always visible.
    const sidebarOpen = ref(false)

    // Manual icons-only mode for the desktop sidebar (>1160px). Independent of
    // sidebarOpen: the <900px drawer deliberately ignores this flag (its styles
    // are scoped to min-width: 901px) because an off-canvas icons-only rail
    // would leave users with unlabeled icons and no hover affordance.
    const sidebarCollapsed = ref(false)

    // First letter of the (localized) company name, shown as a 32px monogram
    // when the sidebar is collapsed to its icons-only rail.
    const brandInitial = computed(() => t('nav.companyName').charAt(0))

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

    onMounted(loadTasks)

    return {
      t,
      showProfileDetails,
      showTasks,
      sidebarOpen,
      sidebarCollapsed,
      brandInitial,
      tasks,
      addTask,
      deleteTask,
      toggleTask
    }
  }
}
</script>

<style>
:root {
  --bg: #f8fafc;
  --surface: #ffffff;
  --border: #e2e8f0;
  --text: #0f172a;
  --text-muted: #64748b;
  --accent: #2563eb;
  --success: #16a34a;
  --warning: #d97706;
  --danger: #dc2626;
  --accent-soft: #eff6ff; /* accent-tinted background (active nav, brand monogram) */
  --radius: 8px;
  --shadow: 0 1px 3px rgb(15 23 42 / 0.06);
  /* Single source of truth for the sidebar width: because the app shell is a
     flex row, the main column's offset follows the sidebar width automatically
     (and animates with the same width transition). */
  --sidebar-w: 240px;
  --sidebar-w-collapsed: 64px;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: var(--bg);
  color: var(--text);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* App shell: fixed-width sidebar + flexible main column */
.app {
  display: flex;
  min-height: 100vh;
}

/* ---------- Sidebar ---------- */

.sidebar {
  width: var(--sidebar-w);
  transition: width 0.2s ease;
  flex-shrink: 0;
  background: var(--surface);
  border-right: 1px solid var(--border);
  /* Sticky full-height column: stays put while the main column scrolls */
  position: sticky;
  top: 0;
  height: 100vh;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  z-index: 200;
}

.brand {
  min-height: 64px;
  padding: 12px 16px;
  border-bottom: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.brand h1 {
  font-size: 15px;
  font-weight: 700;
  color: var(--text);
  letter-spacing: -0.025em;
  line-height: 1.3;
}

.brand .subtitle {
  font-size: 12px;
  color: var(--text-muted);
  font-weight: 400;
}

.brand-full {
  display: flex;
  flex-direction: column;
}

/* Compact monogram shown only in icons-only mode (see collapsed rules below) */
.brand-monogram {
  display: none;
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: var(--accent-soft);
  color: var(--accent);
  font-size: 15px;
  font-weight: 700;
  align-items: center;
  justify-content: center;
}

.nav-section-label {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--text-muted);
  padding: 16px 16px 8px;
}

.side-nav {
  display: flex;
  flex-direction: column;
  gap: 2px;
  padding: 0 8px; /* 8px inset from sidebar edges */
}

.side-nav a {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 16px;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 500;
  color: var(--text-muted);
  text-decoration: none;
  transition: all 0.15s ease;
}

/* Fixed-width icon slot so labels stay left-aligned regardless of icon shape.
   Icons use stroke="currentColor", so they inherit the link's muted/accent color. */
.nav-icon {
  width: 20px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav-icon svg {
  display: block;
}

/* nowrap + hidden overflow keeps labels from wrapping mid width-transition */
.nav-label {
  white-space: nowrap;
  overflow: hidden;
}

.side-nav a:hover {
  background: #f1f5f9;
  color: var(--text);
}

.side-nav a.active {
  background: #eff6ff;
  color: var(--accent);
  font-weight: 600;
}

/* Manual collapse toggle: margin-top auto pins it to the sidebar bottom
   (the sidebar is a flex column) */
.collapse-toggle {
  margin: auto 8px 12px;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: transparent;
  border: 1px solid var(--border);
  border-radius: 6px;
  color: var(--text-muted);
  cursor: pointer;
  flex-shrink: 0;
}

.collapse-toggle:hover {
  background: #f1f5f9;
  color: var(--text);
}

.collapse-toggle svg {
  transition: transform 0.2s ease;
}

/* ---------- Icons-only (collapsed) sidebar ----------
   Scoped to min-width: 901px on purpose: below 900px the sidebar becomes the
   off-canvas drawer, which must always render fully expanded (labels visible)
   even when sidebarCollapsed is true — an off-canvas icons-only rail would be
   useless (unlabeled icons, no room for hover tooltips). */

@media (min-width: 901px) {
  .sidebar.collapsed {
    width: var(--sidebar-w-collapsed);
  }

  .sidebar.collapsed .brand {
    padding: 12px 0;
    align-items: center;
  }

  .sidebar.collapsed .brand-full {
    display: none;
  }

  .sidebar.collapsed .brand-monogram {
    display: flex;
  }

  /* visibility (not display) keeps the label's vertical slot so the nav
     doesn't jump upward while the width transition runs */
  .sidebar.collapsed .nav-section-label {
    visibility: hidden;
  }

  .sidebar.collapsed .side-nav a {
    justify-content: center;
    padding: 10px 0;
  }

  .sidebar.collapsed .nav-label {
    display: none;
  }

  /* Chevron points right (toward expansion) when collapsed */
  .sidebar.collapsed .collapse-toggle svg {
    transform: rotate(180deg);
  }
}

/* Tablet range (901–1160px): force icons-only regardless of the manual
   toggle — same declarations as .sidebar.collapsed above, minus the class. */
@media (min-width: 901px) and (max-width: 1160px) {
  .sidebar {
    width: var(--sidebar-w-collapsed);
  }

  .sidebar .brand {
    padding: 12px 0;
    align-items: center;
  }

  .sidebar .brand-full {
    display: none;
  }

  .sidebar .brand-monogram {
    display: flex;
  }

  .sidebar .nav-section-label {
    visibility: hidden;
  }

  .sidebar .side-nav a {
    justify-content: center;
    padding: 10px 0;
  }

  .sidebar .nav-label {
    display: none;
  }

  /* Icons-only is forced here, so the manual toggle would be a lying control */
  .collapse-toggle {
    display: none;
  }
}

/* Scrim behind the off-canvas sidebar (mobile only) */
.sidebar-scrim {
  display: none;
}

/* ---------- Main column ---------- */

.main-column {
  flex: 1;
  min-width: 0; /* prevents flex overflow -> no horizontal scrollbar */
  display: flex;
  flex-direction: column;
}

.top-bar {
  height: 60px;
  flex-shrink: 0;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 0 24px;
}

.top-bar-actions {
  margin-left: auto;
  display: flex;
  align-items: center;
  gap: 12px;
  flex-shrink: 0;
}

/* Hamburger: hidden on desktop, shown below 900px */
.sidebar-toggle {
  display: none;
  align-items: center;
  justify-content: center;
  padding: 8px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 6px;
  color: var(--text-muted);
  cursor: pointer;
  flex-shrink: 0;
}

.sidebar-toggle:hover {
  background: #f1f5f9;
  color: var(--text);
}

.main-content {
  flex: 1;
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 24px 32px;
  background: var(--bg);
}

/* ---------- Responsive: off-canvas sidebar below 900px ---------- */

@media (max-width: 900px) {
  .sidebar {
    position: fixed;
    left: 0;
    top: 0;
    transform: translateX(-100%);
    transition: transform 0.2s ease;
    box-shadow: none;
  }

  .sidebar.open {
    transform: translateX(0);
    box-shadow: 0 10px 25px rgb(15 23 42 / 0.15);
  }

  .sidebar-scrim {
    display: block;
    position: fixed;
    inset: 0;
    background: rgb(15 23 42 / 0.4);
    z-index: 150;
  }

  .sidebar-toggle {
    display: flex;
  }

  /* Collapse has no effect in drawer mode (rules are scoped to >900px),
     so hide its toggle rather than show a control that does nothing */
  .collapse-toggle {
    display: none;
  }
}

/* ---------- Shared view styles (used by views/*.vue) ---------- */

.page-header {
  margin-bottom: 24px;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: var(--text-muted);
  font-size: 0.938rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: var(--surface);
  padding: 24px;
  border-radius: var(--radius);
  border: 1px solid var(--border);
  box-shadow: var(--shadow);
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgb(15 23 42 / 0.06);
}

.stat-label {
  color: var(--text-muted);
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: var(--text);
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value {
  color: var(--warning);
}

.stat-card.success .stat-value {
  color: var(--success);
}

.stat-card.danger .stat-value {
  color: var(--danger);
}

.stat-card.info .stat-value {
  color: var(--accent);
}

.card {
  background: var(--surface);
  border-radius: var(--radius);
  padding: 24px;
  border: 1px solid var(--border);
  box-shadow: var(--shadow);
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid var(--border);
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: var(--text);
  letter-spacing: -0.025em;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: var(--bg);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
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
  background: var(--bg);
}

.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
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
  background: #dbeafe;
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
  background: #dbeafe;
  color: #1e40af;
}

.loading {
  text-align: center;
  padding: 3rem;
  color: var(--text-muted);
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: var(--radius);
  margin: 1rem 0;
  font-size: 0.938rem;
}
</style>
