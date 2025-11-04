<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 py-8 px-4 sm:px-6 lg:px-8">
    <div class="max-w-7xl mx-auto">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-gray-900 mb-2">Sandwich Panel Costing Calculator</h1>
        <p class="text-lg text-gray-600">Based on Excel Formula - REV-03</p>
      </div>

      <!-- Main Form -->
      <div class="bg-white rounded-2xl shadow-xl p-6 md:p-8">
        <form @submit.prevent="calculateCost">
          
          <!-- Quotation Info -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-blue-500">
              📋 Quotation Information
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Quote No</label>
                <input v-model="formData.quoteNo" type="text" placeholder="MAHFAR TRADING" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Date</label>
                <input v-model="formData.date" type="date" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Material</label>
                <select v-model="formData.materialType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                  <option value="PPAL">PPAL</option>
                  <option value="PIR">PIR</option>
                  <option value="PPGI">PPGI</option>
                  <option value="GI">GI</option>
                </select>
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Panel Type</label>
                <select v-model="formData.panelType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                  <option value="ROOF">ROOF</option>
                  <option value="WALL">WALL</option>
                  <option value="CEILING">CEILING</option>
                </select>
              </div>
            </div>
          </div>

          <!-- Material Rates -->
          <div class="mb-6 bg-yellow-50 p-4 rounded-lg">
            <h2 class="text-xl font-semibold text-gray-800 mb-3">💰 Material Rates (₹/Ton)</h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Top Skin Cost/Ton</label>
                <input v-model.number="formData.topSkinCostPerTon" type="number" step="0.01" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Bottom Skin Cost/Ton</label>
                <input v-model.number="formData.bottomSkinCostPerTon" type="number" step="0.01" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Core Cost/Ton</label>
                <input v-model.number="formData.coreCostPerTon" type="number" step="0.01" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white" />
              </div>
            </div>
          </div>

          <!-- Top Skin -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-purple-500">📐 Top Skin</h2>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Thickness (mm)</label>
                <input v-model.number="formData.topSkinThickness" type="number" step="0.01" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Girth (mm)</label>
                <input v-model.number="formData.topSkinGirth" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Length (mm)</label>
                <input v-model.number="formData.topSkinLength" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input v-model.number="formData.topSkinDensity" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
            </div>
          </div>

          <!-- Bottom Skin -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-indigo-500">📐 Bottom Skin</h2>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Thickness (mm)</label>
                <input v-model.number="formData.bottomSkinThickness" type="number" step="0.01" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Girth (mm)</label>
                <input v-model.number="formData.bottomSkinGirth" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Length (mm)</label>
                <input v-model.number="formData.bottomSkinLength" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input v-model.number="formData.bottomSkinDensity" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
            </div>
          </div>

          <!-- Core -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-pink-500">📐 Core</h2>
            <div class="grid grid-cols-2 md:grid-cols-5 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Thickness (mm)</label>
                <input v-model.number="formData.coreThickness" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Girth (mm)</label>
                <input v-model.number="formData.coreGirth" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Length (mm)</label>
                <input v-model.number="formData.coreLength" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input v-model.number="formData.coreDensity" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Core Constant</label>
                <input v-model.number="formData.coreConstant" type="number" step="0.001" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
            </div>
          </div>

          <!-- Quantity & Pricing -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-orange-500">💵 Quantity & Pricing</h2>
            <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Quantity</label>
                <input v-model.number="formData.quantity" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Quoted Price (₹/unit)</label>
                <input v-model.number="formData.quotedPrice" type="number" step="0.01" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Wastage (%)</label>
                <input v-model.number="formData.wastagePercent" type="number" step="0.1" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Overhead (%)</label>
                <input v-model.number="formData.overheadPercent" type="number" step="0.1" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
            </div>
          </div>

          <!-- Calculate Button -->
          <div class="flex justify-center mb-6">
            <button type="submit" class="bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700 text-white font-bold py-3 px-8 rounded-lg shadow-lg transform transition hover:scale-105">
              🧮 Calculate Total Cost
            </button>
          </div>

          <!-- Results -->
          <div v-if="result.calculated" class="bg-gradient-to-r from-green-50 to-emerald-50 rounded-xl p-6 border-2 border-green-300">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-green-500">💰 Cost Breakdown</h2>
            
            <!-- Component Weights -->
            <div class="mb-4">
              <h3 class="text-lg font-semibold text-gray-700 mb-2">Component Weights</h3>
              <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Top Skin Weight</p>
                  <p class="text-lg font-bold text-gray-900">{{formatWeight(result.topSkinWeight)}} tons</p>
                </div>
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Bottom Skin Weight</p>
                  <p class="text-lg font-bold text-gray-900">{{ formatWeight(result.bottomSkinWeight)}} tons</p>
                </div>
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Core Weight</p>
                  <p class="text-lg font-bold text-gray-900">{{ formatWeight(result.coreWeight)}} tons</p>
                </div>
              </div>
            </div>

            <!-- Material Costs -->
            <div class="mb-4">
              <h3 class="text-lg font-semibold text-gray-700 mb-2">Material Costs</h3>
              <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Top Skin Cost</p>
                  <p class="text-lg font-bold text-gray-900">₹{{ formatCurrency(result.topSkinMaterialCost)}}</p>
                </div>
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Bottom Skin Cost</p>
                  <p class="text-lg font-bold text-gray-900">₹{{ formatCurrency(result.bottomSkinMaterialCost)}}</p>
                </div>
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Core Cost</p>
                  <p class="text-lg font-bold text-gray-900">₹{{ formatCurrency(result.coreMaterialCost)}}</p>
                </div>
              </div>
            </div>

            <!-- Summary -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-3 mb-4">
              <div class="bg-white rounded-lg p-3 shadow">
                <p class="text-xs text-gray-600">Total Weight</p>
                <p class="text-lg font-bold text-gray-900">{{ formatWeight(result.totalWeight)}} tons</p>
              </div>
              <div class="bg-white rounded-lg p-3 shadow">
                <p class="text-xs text-gray-600">Total Material Cost</p>
                <p class="text-lg font-bold text-gray-900">₹{{ formatCurrency(result.totalMaterialCost)}}</p>
              </div>
              <div class="bg-white rounded-lg p-3 shadow">
                <p class="text-xs text-gray-600">Wastage Cost</p>
                <p class="text-lg font-bold text-gray-900">₹{{ formatCurrency(result.wastageCost)}}</p>
              </div>
              <div class="bg-white rounded-lg p-3 shadow">
                <p class="text-xs text-gray-600">Overhead Cost</p>
                <p class="text-lg font-bold text-gray-900">₹{{ formatCurrency(result.overheadCost)}}</p>
              </div>
            </div>

            <!-- Grand Total -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-3 mb-4">
              <div class="bg-gradient-to-r from-green-400 to-emerald-500 rounded-lg p-4 shadow-lg">
                <p class="text-sm text-white mb-1">Grand Total Cost</p>
                <p class="text-3xl font-bold text-white">₹{{ formatCurrency(result.grandTotal)}}</p>
              </div>
              <div class="bg-gradient-to-r from-blue-400 to-indigo-500 rounded-lg p-4 shadow-lg">
                <p class="text-sm text-white mb-1">Cost per Unit</p>
                <p class="text-3xl font-bold text-white">₹{{ formatCurrency(result.costPerUnit)}}</p>
              </div>
            </div>

            <!-- Value Addition -->
            <div class="bg-white rounded-lg p-4 shadow">
              <h3 class="text-lg font-semibold text-gray-700 mb-2">Value Addition Analysis</h3>
              <div class="grid grid-cols-1 md:grid-cols-4 gap-3">
                <div>
                  <p class="text-xs text-gray-600">Value Addition</p>
                  <p class="text-lg font-bold text-indigo-600">₹{{ formatCurrency(result.valueAddition)}}</p>
                </div>
                <div>
                  <p class="text-xs text-gray-600">Value Add Total</p>
                  <p class="text-lg font-bold text-indigo-600">₹{{ formatCurrency(result.valueAddTotal)}}</p>
                </div>
                <div>
                  <p class="text-xs text-gray-600">Total Sales</p>
                  <p class="text-lg font-bold text-indigo-600">₹{{ formatCurrency(result.totalSales)}}</p>
                </div>
                <div>
                  <p class="text-xs text-gray-600">% Value Add</p>
                  <p class="text-lg font-bold text-indigo-600">{{ formatPercent(result.percentValueAdd)}}%</p>
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>

      <!-- Footer -->
      <div class="text-center mt-6 text-gray-600">
        <p class="text-sm">© 2025 Sandwich Panel Calculator | Based on Excel REV-03</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

const formData = reactive({
  quoteNo: 'MAHFAR TRADING',
  date: new Date().toISOString().split('T')[0],
  materialType: 'PPAL',
  panelType: 'ROOF',
  
  topSkinCostPerTon: 1400,
  bottomSkinCostPerTon: 400,
  coreCostPerTon: 900,
  
  topSkinThickness: 0.5,
  topSkinGirth: 1220,
  topSkinLength: 1000,
  topSkinDensity: 2720,
  
  bottomSkinThickness: 0.5,
  bottomSkinGirth: 1080,
  bottomSkinLength: 1000,
  bottomSkinDensity: 2720,
  
  coreThickness: 50,
  coreGirth: 1000,
  coreLength: 1000,
  coreDensity: 40,
  coreConstant: 0.215,
  
  quantity: 1000,
  quotedPrice: 7.7,
  wastagePercent: 10,
  overheadPercent: 5
})

const result = reactive({
  calculated: false,
  topSkinWeight: 0,
  topSkinTotalWeight: 0,
  topSkinMaterialCost: 0,
  bottomSkinWeight: 0,
  bottomSkinTotalWeight: 0,
  bottomSkinMaterialCost: 0,
  coreWeight: 0,
  coreTotalWeight: 0,
  coreMaterialCost: 0,
  totalWeight: 0,
  totalMaterialCost: 0,
  valueAddition: 0,
  valueAddTotal: 0,
  totalSales: 0,
  percentValueAdd: 0,
  wastageCost: 0,
  overheadCost: 0,
  grandTotal: 0,
  costPerUnit: 0
})

const calculateCost = () => {
  // Top Skin: L6 = G6*H6/1000*I6/1000*K6/1000
  result.topSkinWeight = formData.topSkinThickness * formData.topSkinGirth / 1000 * formData.topSkinLength / 1000 * formData.topSkinDensity / 1000
  result.topSkinTotalWeight = result.topSkinWeight * formData.quantity / 1000
  result.topSkinMaterialCost = (formData.topSkinCostPerTon / 1000) * result.topSkinWeight

  // Bottom Skin: L7 = G7*H7/1000*I7/1000*K7/1000
  result.bottomSkinWeight = formData.bottomSkinThickness * formData.bottomSkinGirth / 1000 * formData.bottomSkinLength / 1000 * formData.bottomSkinDensity / 1000
  result.bottomSkinTotalWeight = result.bottomSkinWeight * formData.quantity / 1000
  result.bottomSkinMaterialCost = (formData.bottomSkinCostPerTon / 1000) * result.bottomSkinWeight

  // Core: L8 = G8/1000*H8/1000*I8/1000*K8+0.215
  result.coreWeight = formData.coreThickness / 1000 * formData.coreGirth / 1000 * formData.coreLength / 1000 * formData.coreDensity + formData.coreConstant
  result.coreTotalWeight = result.coreWeight * formData.quantity / 1000
  result.coreMaterialCost = (formData.coreCostPerTon / 1000) * result.coreWeight

  // Totals: L9 = SUM(L6:L8)
  result.totalWeight = result.topSkinWeight + result.bottomSkinWeight + result.coreWeight

  // O11 = SUM(O6:O10)
  result.totalMaterialCost = result.topSkinMaterialCost + result.bottomSkinMaterialCost + result.coreMaterialCost

  // Q11 = P6-O11
  result.valueAddition = formData.quotedPrice - result.totalMaterialCost

  // R11 = Q11*J6
  result.valueAddTotal = result.valueAddition * formData.quantity

  // S11 = P6*J6
  result.totalSales = formData.quotedPrice * formData.quantity

  // T11 = R11/S11%
  result.percentValueAdd = result.totalSales > 0 ? (result.valueAddTotal / result.totalSales) * 100 : 0

  // O12 = P12*O11
  result.wastageCost = (formData.wastagePercent / 100) * result.totalMaterialCost

  // O13 = O11*0.05
  result.overheadCost = result.totalMaterialCost * (formData.overheadPercent / 100)

  // O14 = SUM(O11:O13)
  result.grandTotal = result.totalMaterialCost + result.wastageCost + result.overheadCost

  result.costPerUnit = formData.quantity > 0 ? result.grandTotal / formData.quantity : 0

  result.calculated = true

  setTimeout(() => {
    document.querySelector('.bg-gradient-to-r.from-green-50')?.scrollIntoView({ behavior: 'smooth', block: 'nearest' })
  }, 100)
}

const formatCurrency = (value) => {
  return new Intl.NumberFormat('en-IN', { minimumFractionDigits: 2, maximumFractionDigits: 2 }).format(value || 0)
}

const formatWeight = (value) => {
  return new Intl.NumberFormat('en-IN', { minimumFractionDigits: 3, maximumFractionDigits: 3 }).format(value || 0)
}

const formatPercent = (value) => {
  return new Intl.NumberFormat('en-IN', { minimumFractionDigits: 2, maximumFractionDigits: 2 }).format(value || 0)
}
</script>

<style scoped>
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type=number] {
  -moz-appearance: textfield;
}
</style>
