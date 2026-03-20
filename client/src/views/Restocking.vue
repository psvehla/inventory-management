<template>
  <div class="restocking">
    <div class="page-header">
      <h2>{{ t('restocking.title') }}</h2>
      <p>{{ t('restocking.description') }}</p>
    </div>

    <div v-if="loading" class="loading">{{ t('common.loading') }}</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    <div v-else>
      <div class="card budget-card">
        <div class="card-header">
          <h3 class="card-title">{{ t('restocking.budgetLabel') }}</h3>
        </div>
        <div class="budget-controls">
          <input
            type="range"
            min="1000"
            max="50000"
            step="500"
            v-model.number="budget"
            class="budget-slider"
          />
          <div class="budget-display">{{ formattedBudget }}</div>
        </div>
      </div>

      <div v-if="submitted" class="success-message">
        {{ t('restocking.orderSubmitted') }}
      </div>

      <div class="card">
        <div class="card-header">
          <h3 class="card-title">{{ t('restocking.recommendations') }}</h3>
        </div>
        <div v-if="activeRecommendations.length === 0" class="no-data">
          {{ t('restocking.noRecommendations') }}
        </div>
        <div v-else class="table-container">
          <table class="recommendations-table">
            <thead>
              <tr>
                <th class="col-select">
                  <input
                    type="checkbox"
                    :checked="selectedCount === activeRecommendations.length && activeRecommendations.length > 0"
                    @change="toggleAll"
                    class="row-checkbox"
                  />
                </th>
                <th class="col-sku">{{ t('restocking.table.sku') }}</th>
                <th class="col-name">{{ t('restocking.table.itemName') }}</th>
                <th class="col-trend">{{ t('restocking.table.trend') }}</th>
                <th class="col-gap">{{ t('restocking.table.demandGap') }}</th>
                <th class="col-cost">{{ t('restocking.table.unitCost') }}</th>
                <th class="col-qty">{{ t('restocking.table.recommendedQty') }}</th>
                <th class="col-line">{{ t('restocking.table.lineCost') }}</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="rec in activeRecommendations"
                :key="rec.item_sku"
                :class="{ 'row-selected': selectedSkus.has(rec.item_sku) }"
              >
                <td class="col-select">
                  <input
                    type="checkbox"
                    :checked="selectedSkus.has(rec.item_sku)"
                    @change="toggleItem(rec.item_sku)"
                    class="row-checkbox"
                  />
                </td>
                <td class="col-sku"><strong>{{ rec.item_sku }}</strong></td>
                <td class="col-name">{{ rec.item_name }}</td>
                <td class="col-trend">
                  <span :class="['badge', rec.trend]">
                    {{ t('trends.' + rec.trend) }}
                  </span>
                </td>
                <td class="col-gap">{{ rec.demand_gap }}</td>
                <td class="col-cost">{{ currencySymbol }}{{ rec.unit_cost.toLocaleString() }}</td>
                <td class="col-qty"><strong>{{ rec.recommended_qty }}</strong></td>
                <td class="col-line">
                  <strong>{{ currencySymbol }}{{ rec.line_cost.toLocaleString() }}</strong>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <div v-if="activeRecommendations.length > 0" class="summary-bar">
          <div class="summary-stats">
            <div class="stat-card">
              <div class="stat-label">{{ t('restocking.summary.selectedItems') }}</div>
              <div class="stat-value">{{ selectedCount }}</div>
            </div>
            <div class="stat-card" :class="{ danger: totalCost > budget }">
              <div class="stat-label">{{ t('restocking.summary.totalCost') }}</div>
              <div class="stat-value">{{ currencySymbol }}{{ totalCost.toLocaleString() }}</div>
            </div>
            <div class="stat-card" :class="budgetRemaining >= 0 ? 'success' : 'danger'">
              <div class="stat-label">{{ t('restocking.summary.budgetRemaining') }}</div>
              <div class="stat-value">{{ currencySymbol }}{{ budgetRemaining.toLocaleString() }}</div>
            </div>
          </div>
          <div class="order-action">
            <button
              class="place-order-btn"
              :disabled="selectedCount === 0 || submitting"
              @click="placeOrder"
            >
              <span v-if="submitting">{{ t('restocking.submitting') }}</span>
              <span v-else>{{ t('restocking.placeOrder') }}</span>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, watch, onMounted } from 'vue'
import { api } from '../api'
import { useI18n } from '../composables/useI18n'

export default {
  name: 'Restocking',
  setup() {
    const { t, currentCurrency } = useI18n()

    const budget = ref(10000)
    const recommendations = ref([])
    const selectedSkus = ref(new Set())
    const loading = ref(true)
    const error = ref(null)
    const submitting = ref(false)
    const submitted = ref(false)

    const currencySymbol = computed(() => currentCurrency.value === 'JPY' ? '¥' : '$')

    // Only show items with recommended_qty > 0
    const activeRecommendations = computed(() => {
      return recommendations.value.filter(r => r.recommended_qty > 0)
    })

    const selectedRecommendations = computed(() => {
      return activeRecommendations.value.filter(r => selectedSkus.value.has(r.item_sku))
    })

    const totalCost = computed(() => {
      return selectedRecommendations.value.reduce((sum, r) => sum + r.line_cost, 0)
    })

    const selectedCount = computed(() => selectedRecommendations.value.length)

    const budgetRemaining = computed(() => budget.value - totalCost.value)

    const formattedBudget = computed(() => {
      return currencySymbol.value + budget.value.toLocaleString()
    })

    const loadRecommendations = async () => {
      try {
        loading.value = true
        error.value = null
        submitted.value = false
        recommendations.value = await api.getRestockingRecommendations(budget.value)
        // Select all items with qty > 0 by default
        const newSelected = new Set()
        recommendations.value.forEach(r => {
          if (r.recommended_qty > 0) newSelected.add(r.item_sku)
        })
        selectedSkus.value = newSelected
      } catch (err) {
        console.error('Failed to load recommendations:', err)
        error.value = 'Failed to load recommendations: ' + err.message
      } finally {
        loading.value = false
      }
    }

    const toggleItem = (sku) => {
      const newSet = new Set(selectedSkus.value)
      if (newSet.has(sku)) {
        newSet.delete(sku)
      } else {
        newSet.add(sku)
      }
      selectedSkus.value = newSet
    }

    const toggleAll = () => {
      if (selectedCount.value === activeRecommendations.value.length) {
        selectedSkus.value = new Set()
      } else {
        const newSelected = new Set()
        activeRecommendations.value.forEach(r => newSelected.add(r.item_sku))
        selectedSkus.value = newSelected
      }
    }

    const placeOrder = async () => {
      try {
        submitting.value = true
        const items = selectedRecommendations.value.map(r => ({
          sku: r.item_sku,
          name: r.item_name,
          quantity: r.recommended_qty,
          unit_price: r.unit_cost,
          line_cost: r.line_cost
        }))
        await api.submitRestockingOrder({
          items,
          total_cost: totalCost.value,
          budget: budget.value
        })
        submitted.value = true
      } catch (err) {
        console.error('Failed to submit restocking order:', err)
        error.value = 'Failed to submit order: ' + err.message
      } finally {
        submitting.value = false
      }
    }

    // Debounced watch on budget
    let debounceTimer = null
    watch(budget, () => {
      if (debounceTimer) clearTimeout(debounceTimer)
      debounceTimer = setTimeout(() => {
        loadRecommendations()
      }, 300)
    })

    onMounted(loadRecommendations)

    return {
      t,
      budget,
      recommendations,
      activeRecommendations,
      selectedSkus,
      selectedRecommendations,
      selectedCount,
      totalCost,
      budgetRemaining,
      formattedBudget,
      currencySymbol,
      loading,
      error,
      submitting,
      submitted,
      toggleItem,
      toggleAll,
      placeOrder
    }
  }
}
</script>

<style scoped>
.restocking {
  /* inherits from global .main-content padding */
}

/* Budget Card */
.budget-card {
  margin-bottom: 1.5rem;
}

.budget-controls {
  display: flex;
  align-items: center;
  gap: 2rem;
  padding: 0.5rem 0;
}

.budget-slider {
  flex: 1;
  height: 6px;
  appearance: none;
  -webkit-appearance: none;
  background: #e2e8f0;
  border-radius: 3px;
  outline: none;
  cursor: pointer;
  accent-color: #3b82f6;
}

.budget-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #3b82f6;
  cursor: pointer;
  box-shadow: 0 1px 4px rgba(59, 130, 246, 0.4);
  transition: box-shadow 0.2s;
}

.budget-slider::-webkit-slider-thumb:hover {
  box-shadow: 0 2px 8px rgba(59, 130, 246, 0.5);
}

.budget-slider::-moz-range-thumb {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #3b82f6;
  cursor: pointer;
  border: none;
  box-shadow: 0 1px 4px rgba(59, 130, 246, 0.4);
}

.budget-display {
  font-size: 2rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
  min-width: 160px;
  text-align: right;
  white-space: nowrap;
}

/* Success message */
.success-message {
  background: #d1fae5;
  border: 1px solid #6ee7b7;
  color: #065f46;
  padding: 1rem 1.25rem;
  border-radius: 8px;
  margin-bottom: 1.25rem;
  font-size: 0.938rem;
  font-weight: 500;
}

/* No data state */
.no-data {
  padding: 3rem;
  text-align: center;
  color: #64748b;
  font-size: 0.938rem;
}

/* Table */
.recommendations-table {
  table-layout: fixed;
  width: 100%;
}

.col-select {
  width: 44px;
}

.col-sku {
  width: 110px;
}

.col-name {
  /* flexible */
}

.col-trend {
  width: 110px;
}

.col-gap {
  width: 110px;
}

.col-cost {
  width: 110px;
}

.col-qty {
  width: 130px;
}

.col-line {
  width: 120px;
}

.row-checkbox {
  width: 16px;
  height: 16px;
  cursor: pointer;
  accent-color: #3b82f6;
}

.row-selected {
  background: #eff6ff;
}

.row-selected:hover {
  background: #dbeafe !important;
}

/* Summary bar */
.summary-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
  margin-top: 1.25rem;
  padding-top: 1.25rem;
  border-top: 1px solid #e2e8f0;
  flex-wrap: wrap;
}

.summary-stats {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

.summary-stats .stat-card {
  min-width: 160px;
  margin-bottom: 0;
}

.summary-stats .stat-value {
  font-size: 1.5rem;
}

.order-action {
  flex-shrink: 0;
}

.place-order-btn {
  padding: 0.75rem 2rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 0.938rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.place-order-btn:hover:not(:disabled) {
  background: #2563eb;
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.35);
}

.place-order-btn:disabled {
  background: #94a3b8;
  cursor: not-allowed;
  box-shadow: none;
}
</style>
