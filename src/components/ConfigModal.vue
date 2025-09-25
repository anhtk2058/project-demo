<template>
  <div v-if="isVisible" class="modal-overlay" @click="closeModal">
    <div class="modal-container" @click.stop>
      <!-- Header -->
      <div class="modal-header">
        <h2 class="modal-title">Cài đặt config</h2>
        <button class="close-btn" @click="closeModal">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M18 6L6 18M6 6l12 12"/>
          </svg>
        </button>
      </div>

      <!-- Content -->
      <div class="modal-content">
        <div class="section">
          <h3 class="section-title">Configure How Totals Are Calculated</h3>
          <p class="section-description">Choose the formula you want to use for totals.</p>
        </div>

        <!-- General Settings -->
        <div class="section">
          <h4 class="subsection-title">General Settings</h4>
          <div class="form-row">
            <label class="form-label">Apply for all</label>
            <div class="select-wrapper">
              <select v-model="generalSetting" class="form-select">
                <option value="sum">Sum</option>
                <option value="avg">Average</option>
                <option value="custom">Custom Formula</option>
              </select>
              <svg class="select-arrow" width="12" height="8" viewBox="0 0 12 8">
                <path d="M1 1l5 5 5-5" stroke="currentColor" stroke-width="2" fill="none"/>
              </svg>
            </div>
          </div>
        </div>

        <!-- Configure Each Metric -->
        <div class="section">
          <h4 class="subsection-title">Configure Each Metric</h4>
          
          <!-- Search -->
          <div class="search-container">
            <svg class="search-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="11" cy="11" r="8"/>
              <path d="m21 21-4.35-4.35"/>
            </svg>
            <input 
              v-model="searchQuery" 
              type="text" 
              placeholder="Search Metrics" 
              class="search-input"
            >
          </div>

          <!-- Metrics List -->
          <div class="metrics-list">
            <div 
              v-for="metric in filteredMetrics" 
              :key="metric.id" 
              class="metric-row"
            >
              <span class="metric-name">{{ metric.name }}</span>
              <div class="select-wrapper">
                <select v-model="metric.formula" class="form-select">
                  <option value="sum">Sum</option>
                  <option value="avg">AVG</option>
                  <option value="custom">CUSTOM FORMULA</option>
                </select>
                <svg class="select-arrow" width="12" height="8" viewBox="0 0 12 8">
                  <path d="M1 1l5 5 5-5" stroke="currentColor" stroke-width="2" fill="none"/>
                </svg>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Footer -->
      <div class="modal-footer">
        <button class="btn btn-secondary" @click="closeModal">Hủy thay đổi</button>
        <button class="btn btn-primary" @click="saveChanges">Lưu</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['update:modelValue', 'save'])

const isVisible = computed({
  get: () => props.modelValue,
  set: (value) => emit('update:modelValue', value)
})

const generalSetting = ref('sum')
const searchQuery = ref('')

const metrics = ref([
  { id: 1, name: 'Click', formula: 'sum' },
  { id: 2, name: 'Click', formula: 'sum' },
  { id: 3, name: 'Click', formula: 'sum' }
])

const filteredMetrics = computed(() => {
  if (!searchQuery.value) return metrics.value
  return metrics.value.filter(metric => 
    metric.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const closeModal = () => {
  isVisible.value = false
}

const saveChanges = () => {
  const config = {
    generalSetting: generalSetting.value,
    metrics: metrics.value
  }
  emit('save', config)
  closeModal()
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 20px;
}

.modal-container {
  background: white;
  border-radius: 12px;
  width: 100%;
  max-width: 600px;
  max-height: 90vh;
  overflow: hidden;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  border: 2px solid #3b82f6;
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 24px;
  border-bottom: 1px solid #e5e7eb;
  background: #f8fafc;
}

.modal-title {
  font-size: 18px;
  font-weight: 600;
  color: #3b82f6;
  margin: 0;
}

.close-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  color: #6b7280;
  border-radius: 4px;
  transition: all 0.2s;
}

.close-btn:hover {
  background: #f3f4f6;
  color: #374151;
}

.modal-content {
  padding: 24px;
  max-height: 60vh;
  overflow-y: auto;
}

.section {
  margin-bottom: 32px;
}

.section:last-child {
  margin-bottom: 0;
}

.section-title {
  font-size: 20px;
  font-weight: 600;
  color: #111827;
  margin: 0 0 8px 0;
}

.section-description {
  color: #6b7280;
  margin: 0 0 24px 0;
  line-height: 1.5;
}

.subsection-title {
  font-size: 16px;
  font-weight: 600;
  color: #374151;
  margin: 0 0 16px 0;
}

.form-row {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px;
  background: #f9fafb;
  border-radius: 8px;
  border: 1px solid #e5e7eb;
}

.form-label {
  font-weight: 500;
  color: #374151;
  min-width: 100px;
}

.select-wrapper {
  position: relative;
  flex: 1;
  max-width: 200px;
}

.form-select {
  width: 100%;
  padding: 8px 32px 8px 12px;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  background: white;
  font-size: 14px;
  color: #64748b;
  appearance: none;
  cursor: pointer;
  transition: all 0.2s;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

.form-select:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

.select-arrow {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none;
  color: #94a3b8;
}

/* Custom dropdown styling */
.select-wrapper select option {
  padding: 12px 16px;
  background: white;
  color: #374151;
  border: none;
}

.select-wrapper select option:hover {
  background: #f1f5f9;
}

.select-wrapper select option:checked {
  background: #dbeafe;
  color: #3b82f6;
  font-weight: 500;
}

.search-container {
  position: relative;
  margin-bottom: 16px;
}

.search-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: #9ca3af;
}

.search-input {
  width: 100%;
  padding: 12px 12px 12px 40px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  background: #f9fafb;
  font-size: 14px;
  color: #374151;
  transition: all 0.2s;
}

.search-input:focus {
  outline: none;
  border-color: #3b82f6;
  background: white;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.search-input::placeholder {
  color: #9ca3af;
}

.metrics-list {
  background: #f9fafb;
  border-radius: 8px;
  border: 1px solid #e5e7eb;
  overflow: hidden;
}

.metric-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px;
  border-bottom: 1px solid #e5e7eb;
  background: white;
}

.metric-row:last-child {
  border-bottom: none;
}

.metric-row:hover {
  background: #f8fafc;
}

.metric-name {
  font-weight: 500;
  color: #374151;
}

.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  padding: 20px 24px;
  border-top: 1px solid #e5e7eb;
  background: #f8fafc;
}

.btn {
  padding: 10px 20px;
  border-radius: 6px;
  font-weight: 500;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.2s;
  border: 1px solid transparent;
}

.btn-secondary {
  background: white;
  color: #6b7280;
  border-color: #d1d5db;
}

.btn-secondary:hover {
  background: #f9fafb;
  color: #374151;
}

.btn-primary {
  background: #3b82f6;
  color: white;
}

.btn-primary:hover {
  background: #2563eb;
}

/* Scrollbar styling */
.modal-content::-webkit-scrollbar {
  width: 6px;
}

.modal-content::-webkit-scrollbar-track {
  background: #f1f5f9;
}

.modal-content::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 3px;
}

.modal-content::-webkit-scrollbar-thumb:hover {
  background: #94a3b8;
}
</style>