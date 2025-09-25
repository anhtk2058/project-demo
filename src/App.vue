<template>
  <div id="app">
    <div class="container">
      <h1>Vue.js Configuration Modal Demo</h1>
      <p>Click the button below to open the configuration modal.</p>
      
      <button @click="showModal = true" class="open-btn">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="12" cy="12" r="3"/>
          <path d="M12 1v6m0 6v6m11-7h-6m-6 0H1"/>
        </svg>
        Mở cài đặt config
      </button>

      <div v-if="savedConfig" class="config-display">
        <h3>Cấu hình đã lưu:</h3>
        <pre>{{ JSON.stringify(savedConfig, null, 2) }}</pre>
      </div>
    </div>

    <ConfigModal 
      v-model="showModal" 
      @save="handleSave"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ConfigModal from './components/ConfigModal.vue'

const showModal = ref(false)
const savedConfig = ref(null)

const handleSave = (config) => {
  savedConfig.value = config
  console.log('Configuration saved:', config)
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f8fafc;
  color: #334155;
  line-height: 1.6;
}

#app {
  min-height: 100vh;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 40px 20px;
}

h1 {
  font-size: 2.5rem;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 16px;
  text-align: center;
}

p {
  font-size: 1.1rem;
  color: #64748b;
  margin-bottom: 32px;
  text-align: center;
}

.open-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  margin: 0 auto 40px;
  padding: 12px 24px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.open-btn:hover {
  background: #2563eb;
  transform: translateY(-1px);
  box-shadow: 0 6px 8px -1px rgba(0, 0, 0, 0.15);
}

.config-display {
  background: white;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  border: 1px solid #e2e8f0;
}

.config-display h3 {
  color: #1e293b;
  margin-bottom: 16px;
  font-size: 1.25rem;
}

.config-display pre {
  background: #f1f5f9;
  padding: 16px;
  border-radius: 8px;
  overflow-x: auto;
  font-size: 14px;
  color: #475569;
  border: 1px solid #e2e8f0;
}
</style>