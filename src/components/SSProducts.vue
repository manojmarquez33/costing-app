<template>
  <div class="space-y-6">
    <!-- SS Product Inputs -->
    <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
      <h2 class="text-lg font-semibold text-gray-800 mb-4 pb-2 border-b border-gray-100 flex items-center">
        <span class="bg-emerald-100 text-emerald-600 p-1.5 rounded-md mr-2">🔩</span> SS Product Details
      </h2>
      
      <!-- Dynamic Flashings Section -->
      <div class="mb-6 border-b border-gray-100 pb-4">
        <h3 class="text-sm font-semibold text-gray-700 mb-3">Add Flashings</h3>
        <div class="flex gap-2 items-end">
          <div class="flex-grow">
            <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Flashing Type</label>
            <select v-model="selectedFlashingType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-emerald-500">
              <option value="">Select Flashing...</option>
              <option v-for="g in flashingTypes" :key="g" :value="g">FLASHING ({{ g }})</option>
            </select>
          </div>
          <button 
            @click="addFlashing" 
            :disabled="!selectedFlashingType"
            class="px-4 py-2 bg-emerald-600 text-white rounded-lg hover:bg-emerald-700 disabled:opacity-50 disabled:cursor-not-allowed font-medium text-sm"
          >
            Add +
          </button>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        <!-- Static Products -->
        <div v-for="(product, index) in products" :key="'prod-'+index" class="bg-gray-50 p-4 rounded-lg border border-gray-200">
          <h3 class="font-bold text-gray-700 mb-2">{{ product.name }}</h3>
          
          <div class="space-y-2">
            <!-- Per-Product Material Inputs -->
            <div class="grid grid-cols-1 gap-2 mb-2 pb-2 border-b border-gray-200">
               <div>
                <label class="block text-[10px] font-medium text-gray-500 uppercase">Color</label>
                <input v-model="product.color" type="text" class="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-1 focus:ring-emerald-500" placeholder="Color" />
              </div>
              <div class="grid grid-cols-2 gap-2">
                <div>
                  <label class="block text-[10px] font-medium text-gray-500 uppercase">Material</label>
                  <select v-model="product.material" class="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-1 focus:ring-emerald-500">
                    <option value="PPAL">PPAL</option>
                    <option value="PPAZ">PPAZ</option>
                    <option value="PPGI">PPGI</option>
                    <option value="GI">GI</option>
                  </select>
                </div>
                <div>
                  <label class="block text-[10px] font-medium text-gray-500 uppercase">Cost/Ton</label>
                  <input v-model.number="product.costPerTon" type="number" step="any" class="w-full px-2 py-1 text-sm border border-yellow-300 bg-yellow-50 rounded focus:ring-1 focus:ring-yellow-500" />
                </div>
              </div>
            </div>

            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="block text-[10px] font-medium text-gray-500 uppercase">Thickness</label>
                <input v-model.number="product.thickness" type="number" step="any" class="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-1 focus:ring-emerald-500" />
              </div>
              <div>
                <label class="block text-[10px] font-medium text-gray-500 uppercase">Girth</label>
                <input v-model.number="product.girth" type="number" step="any" class="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-1 focus:ring-emerald-500" />
              </div>
            </div>
            
            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="block text-[10px] font-medium text-gray-500 uppercase">Length</label>
                <input v-model.number="product.length" type="number" step="any" class="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-1 focus:ring-emerald-500" />
              </div>
              <div>
                <label class="block text-[10px] font-medium text-gray-500 uppercase">Qty</label>
                <input v-model.number="product.qty" type="number" step="any" class="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-1 focus:ring-emerald-500" />
              </div>
            </div>

            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="block text-[10px] font-medium text-gray-500 uppercase">Density</label>
                <input v-model.number="product.density" type="number" step="any" class="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-1 focus:ring-emerald-500" />
              </div>
            </div>
          </div>
        </div>

        <!-- Dynamic Flashings -->
        <div v-for="(flashing, index) in flashings" :key="'flash-'+index" class="bg-blue-50 p-4 rounded-lg border border-blue-200 relative">
          <button @click="removeFlashing(index)" class="absolute top-2 right-2 text-red-400 hover:text-red-600 text-xs font-bold">✕</button>
          <h3 class="font-bold text-blue-800 mb-2">{{ flashing.name }}</h3>
          
          <div class="space-y-2">
             <!-- Per-Product Material Inputs -->
            <div class="grid grid-cols-1 gap-2 mb-2 pb-2 border-b border-blue-200">
               <div>
                <label class="block text-[10px] font-medium text-blue-600 uppercase">Color</label>
                <input v-model="flashing.color" type="text" class="w-full px-2 py-1 text-sm border border-blue-300 rounded focus:ring-1 focus:ring-blue-500" placeholder="Color" />
              </div>
              <div class="grid grid-cols-2 gap-2">
                <div>
                  <label class="block text-[10px] font-medium text-blue-600 uppercase">Material</label>
                  <select v-model="flashing.material" class="w-full px-2 py-1 text-sm border border-blue-300 rounded focus:ring-1 focus:ring-blue-500">
                    <option value="PPAL">PPAL</option>
                    <option value="PPAZ">PPAZ</option>
                    <option value="PPGI">PPGI</option>
                    <option value="GI">GI</option>
                  </select>
                </div>
                <div>
                  <label class="block text-[10px] font-medium text-blue-600 uppercase">Cost/Ton</label>
                  <input v-model.number="flashing.costPerTon" type="number" step="any" class="w-full px-2 py-1 text-sm border border-yellow-300 bg-yellow-50 rounded focus:ring-1 focus:ring-yellow-500" />
                </div>
              </div>
            </div>

            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="block text-[10px] font-medium text-blue-600 uppercase">Thickness</label>
                <input v-model.number="flashing.thickness" type="number" step="any" class="w-full px-2 py-1 text-sm border border-blue-300 rounded focus:ring-1 focus:ring-blue-500" />
              </div>
              <div>
                <label class="block text-[10px] font-medium text-blue-600 uppercase">Girth</label>
                <input v-model.number="flashing.girth" type="number" step="any" class="w-full px-2 py-1 text-sm border border-blue-300 rounded focus:ring-1 focus:ring-blue-500" />
              </div>
            </div>
            
            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="block text-[10px] font-medium text-blue-600 uppercase">Length</label>
                <input v-model.number="flashing.length" type="number" step="any" class="w-full px-2 py-1 text-sm border border-blue-300 rounded focus:ring-1 focus:ring-blue-500" />
              </div>
              <div>
                <label class="block text-[10px] font-medium text-blue-600 uppercase">Qty</label>
                <input v-model.number="flashing.qty" type="number" step="any" class="w-full px-2 py-1 text-sm border border-blue-300 rounded focus:ring-1 focus:ring-blue-500" />
              </div>
            </div>

            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="block text-[10px] font-medium text-blue-600 uppercase">Density</label>
                <input v-model.number="flashing.density" type="number" step="any" class="w-full px-2 py-1 text-sm border border-blue-300 rounded focus:ring-1 focus:ring-blue-500" />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Summary Section -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <!-- Totals -->
      <div class="bg-gradient-to-br from-gray-800 to-gray-900 rounded-xl p-6 text-white shadow-lg">
        <h3 class="text-lg font-semibold mb-4 border-b border-gray-700 pb-2">📊 Summary</h3>
        <div class="space-y-3">
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Total Weight</span>
            <span class="text-xl font-bold">{{ totalWeight.toFixed(3) }} tons</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Total Material Cost</span>
            <span class="text-xl font-bold text-red-400">{{ totalMaterialCost.toFixed(2) }}</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Total Sales</span>
            <span class="text-xl font-bold text-green-400">{{ totalSales.toFixed(2) }}</span>
          </div>
          <div class="flex justify-between items-center pt-2 border-t border-gray-700">
            <span class="text-gray-300">Total Value Add</span>
            <span class="text-2xl font-bold" :class="totalValueAdd >= 0 ? 'text-blue-400' : 'text-red-400'">
              {{ totalValueAdd.toFixed(2) }}
            </span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">% Value Add</span>
            <span class="text-lg font-bold text-purple-400">{{ percentValueAdd.toFixed(2) }}%</span>
          </div>
        </div>
      </div>

      <!-- Profit Analysis -->
      <div class="bg-white rounded-xl shadow-lg p-6 border border-gray-200">
        <h3 class="text-lg font-semibold text-gray-800 mb-4 border-b pb-2">💰 Profit Analysis</h3>
        <div class="space-y-4">
          <div v-for="(product, index) in allItems" :key="index" class="flex justify-between items-center text-sm">
            <span class="font-medium text-gray-600">{{ product.name }}</span>
            <div class="text-right">
              <span class="block font-bold" :class="calculateValueAddition(product) >= 0 ? 'text-green-600' : 'text-red-600'">
                {{ (calculateValueAddition(product) * product.qty).toFixed(2) }}
              </span>
              <span class="text-xs text-gray-400">Profit/Ton: {{ calculateProfitPerTon(product).toFixed(2) }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, reactive, watch } from 'vue'
const props = defineProps({
  quote: String,
  date: String,
  material: String,
  color: String
})

// Dynamic Flashings State
const selectedFlashingType = ref('')
const flashingTypes = [
  'G-100', 'G-200', 'G-300', 'G-400', 'G-500', 
  'G-600', 'G-700', 'G-800', 'G-900', 'G-1000', 'G-1220'
]
const flashings = reactive([])

const addFlashing = () => {
  if (!selectedFlashingType.value) return
  
  flashings.push({
    name: `FLASHING (${selectedFlashingType.value})`,
    color: props.color || '',
    material: props.material || 'PPAL',
    costPerTon: 1400,
    thickness: 0.5,
    girth: parseInt(selectedFlashingType.value.replace('G-', '')) || 150, 
    length: 1000,
    qty: 1,
    density: 2720,
    quotedPrice: 0
  })
  selectedFlashingType.value = ''
}

const removeFlashing = (index) => {
  flashings.splice(index, 1)
}

// Default data from Excel sample 2 (Static Products)
const products = reactive([
  {
    name: 'RIDGE',
    color: props.color || '',
    material: props.material || 'PPAL',
    costPerTon: 1400,
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 162,
    density: 2720,
    quotedPrice: 2.8
  },
  {
    name: 'PROFILE SHEET',
    color: props.color || '',
    material: props.material || 'PPAL',
    costPerTon: 1400,
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 100,
    density: 2720,
    quotedPrice: 3.7
  },
  {
    name: 'PURLIN',
    color: props.color || '',
    material: props.material || 'PPAL',
    costPerTon: 1400,
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 100,
    density: 2720,
    quotedPrice: 3.7
  },
  {
    name: 'DECKING 1',
    color: props.color || '',
    material: props.material || 'PPAL',
    costPerTon: 1400,
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 100,
    density: 2720,
    quotedPrice: 3.7
  },
  {
    name: 'DECKING 2',
    color: props.color || '',
    material: props.material || 'PPAL',
    costPerTon: 1400,
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 100,
    density: 2720,
    quotedPrice: 3.7
  }
])

// --- Calculation Functions ---

// Weight = (Thickness * Girth * Length * Density) / 1,000,000,000
const calculateWeight = (p) => {
  return (p.thickness * p.girth * p.length * p.density) / 1000000000
}

// Material Cost = (CostPerTon / 1000 * Weight) + 0.1
const calculateMaterialCost = (p) => {
  const weight = calculateWeight(p)
  return (p.costPerTon / 1000 * weight) + 0.1
}

// Value Addition = Quoted Price - Material Cost
const calculateValueAddition = (p) => {
  const matCost = calculateMaterialCost(p)
  return p.quotedPrice - matCost
}

// Profit Per Ton
const calculateProfitPerTon = (p) => {
  const weightTons = calculateWeight(p) * p.qty / 1000
  const valAddTotal = calculateValueAddition(p) * p.qty
  return weightTons > 0 ? valAddTotal / weightTons : 0
}

// --- Summary Computations ---

// Helper to get all active items (products + flashings)
const allItems = computed(() => {
  return [...products, ...flashings]
})

const totalWeight = computed(() => {
  return allItems.value.reduce((sum, p) => sum + (calculateWeight(p) * p.qty / 1000), 0)
})

const totalMaterialCost = computed(() => {
  return allItems.value.reduce((sum, p) => sum + (calculateMaterialCost(p) * p.qty), 0)
})

const totalSales = computed(() => {
  return allItems.value.reduce((sum, p) => sum + (p.quotedPrice * p.qty), 0)
})

const totalValueAdd = computed(() => {
  return allItems.value.reduce((sum, p) => sum + (calculateValueAddition(p) * p.qty), 0)
})

const percentValueAdd = computed(() => {
  return totalSales.value > 0 ? (totalValueAdd.value / totalSales.value) * 100 : 0
})

// Expose data to parent
defineExpose({
  products: allItems, // Expose combined list
  totalWeight,
  totalMaterialCost,
  totalSales,
  totalValueAdd,
  percentValueAdd,
  calculateWeight,
  calculateMaterialCost,
  calculateValueAddition
})

const emit = defineEmits(['update'])

watch([products, flashings], () => {
  emit('update')
}, { deep: true })

</script>