<template>
  <div class="space-y-8">
    <!-- Header -->
    <div class="bg-white rounded-xl shadow-lg p-6 border-l-4 border-gray-800">
      <h2 class="text-2xl font-bold text-gray-800 mb-4">🔩 SS Products Calculation</h2>
      
      <!-- Global Inputs -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Cost/Ton (₹)</label>
          <input 
            v-model.number="globalCostPerTon" 
            type="number" 
            class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-yellow-50 focus:ring-2 focus:ring-yellow-500"
          />
        </div>
      </div>
    </div>

    <!-- Products Table -->
    <div class="bg-white rounded-xl shadow-lg overflow-hidden">
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead class="bg-gray-800 text-white">
            <tr>
              <th class="px-4 py-3 text-left">Product</th>
              <th class="px-2 py-3 text-center">Thickness<br>(mm)</th>
              <th class="px-2 py-3 text-center">Girth<br>(mm)</th>
              <th class="px-2 py-3 text-center">Length<br>(mm)</th>
              <th class="px-2 py-3 text-center">Qty</th>
              <th class="px-2 py-3 text-center">Density</th>
              <th class="px-2 py-3 text-center">Weight<br>(Tons)</th>
              <th class="px-2 py-3 text-center">Mat. Cost<br>(₹)</th>
              <th class="px-2 py-3 text-center">Quote<br>(₹)</th>
              <th class="px-2 py-3 text-center">Val. Add<br>(₹)</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200">
            <tr v-for="(product, index) in products" :key="index" class="hover:bg-gray-50">
              <td class="px-4 py-2 font-medium text-gray-900">{{ product.name }}</td>
              <td class="px-2 py-2">
                <input v-model.number="product.thickness" type="number" step="0.01" class="w-20 px-2 py-1 border rounded text-center bg-yellow-50 border-yellow-200" />
              </td>
              <td class="px-2 py-2">
                <input v-model.number="product.girth" type="number" class="w-20 px-2 py-1 border rounded text-center bg-yellow-50 border-yellow-200" />
              </td>
              <td class="px-2 py-2">
                <input v-model.number="product.length" type="number" class="w-20 px-2 py-1 border rounded text-center bg-yellow-50 border-yellow-200" />
              </td>
              <td class="px-2 py-2">
                <input v-model.number="product.qty" type="number" class="w-20 px-2 py-1 border rounded text-center bg-yellow-50 border-yellow-200" />
              </td>
              <td class="px-2 py-2">
                <input v-model.number="product.density" type="number" class="w-20 px-2 py-1 border rounded text-center bg-yellow-50 border-yellow-200" />
              </td>
              <td class="px-2 py-2 text-center font-mono text-blue-600">
                {{ calculateWeight(product).toFixed(4) }}
              </td>
              <td class="px-2 py-2 text-center font-mono text-red-600">
                {{ calculateMaterialCost(product).toFixed(3) }}
              </td>
              <td class="px-2 py-2">
                <input v-model.number="product.quotedPrice" type="number" step="0.01" class="w-20 px-2 py-1 border rounded text-center bg-yellow-50 border-yellow-200" />
              </td>
              <td class="px-2 py-2 text-center font-mono" :class="calculateValueAddition(product) >= 0 ? 'text-green-600' : 'text-red-600'">
                {{ calculateValueAddition(product).toFixed(3) }}
              </td>
            </tr>
          </tbody>
        </table>
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
            <span class="text-xl font-bold text-red-400">₹{{ totalMaterialCost.toFixed(2) }}</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Total Sales</span>
            <span class="text-xl font-bold text-green-400">₹{{ totalSales.toFixed(2) }}</span>
          </div>
          <div class="flex justify-between items-center pt-2 border-t border-gray-700">
            <span class="text-gray-300">Total Value Add</span>
            <span class="text-2xl font-bold" :class="totalValueAdd >= 0 ? 'text-blue-400' : 'text-red-400'">
              ₹{{ totalValueAdd.toFixed(2) }}
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
          <div v-for="(product, index) in products" :key="index" class="flex justify-between items-center text-sm">
            <span class="font-medium text-gray-600">{{ product.name }}</span>
            <div class="text-right">
              <span class="block font-bold" :class="calculateValueAddition(product) >= 0 ? 'text-green-600' : 'text-red-600'">
                ₹{{ (calculateValueAddition(product) * product.qty).toFixed(2) }}
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
import { ref, computed, reactive } from 'vue'

const props = defineProps({
  quote: String,
  date: String,
  material: String,
  color: String
})

const globalCostPerTon = ref(1400)

// Default data from Excel sample 2
const products = reactive([
  {
    name: 'RIDGE',
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 162,
    density: 2720,
    quotedPrice: 2.8
  },
  {
    name: 'PROFILE SHEET',
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 100,
    density: 2720,
    quotedPrice: 3.7
  },
  {
    name: 'PURLIN',
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 100,
    density: 2720,
    quotedPrice: 3.7
  },
  {
    name: 'DECKING 1',
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 100,
    density: 2720,
    quotedPrice: 3.7
  },
  {
    name: 'DECKING 2',
    thickness: 0.5,
    girth: 1220,
    length: 1000,
    qty: 100,
    density: 2720,
    quotedPrice: 3.7
  },
  {
    name: 'FLASHING (G-100)',
    thickness: 0.5,
    girth: 150,
    length: 1000,
    qty: 487.5,
    density: 2720,
    quotedPrice: 0.7
  },
  {
    name: 'FLASHING (G-200)',
    thickness: 0.5,
    girth: 150,
    length: 1000,
    qty: 487.5,
    density: 2720,
    quotedPrice: 0.7
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
  //const weight = calculateWeight(p)
  const totalWeightTons = weight * p.qty / 1000
  if (totalWeightTons === 0) return 0
  return (valAdd * p.qty) / totalWeightTons
}

// --- Computed Totals ---

const totalWeight = computed(() => {
  return products.reduce((sum, p) => {
    const weight = calculateWeight(p)
    return sum + (weight * p.qty / 1000)
  }, 0)
})

const totalMaterialCost = computed(() => {
  return products.reduce((sum, p) => {
    const matCost = calculateMaterialCost(p)
    return sum + (matCost * p.qty)
  }, 0)
})

const totalSales = computed(() => {
  return products.reduce((sum, p) => {
    return sum + (p.quotedPrice * p.qty)
  }, 0)
})

const totalValueAdd = computed(() => {
  return products.reduce((sum, p) => {
    const valAdd = calculateValueAddition(p)
    return sum + (valAdd * p.qty)
  }, 0)
})

const percentValueAdd = computed(() => {
  if (totalSales.value === 0) return 0
  return (totalValueAdd.value / totalSales.value) * 100
})

</script>
