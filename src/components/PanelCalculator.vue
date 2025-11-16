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
          
          <!-- Quotation Info (Columns A-E) -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-blue-500">
              📋 Quotation Information
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-5 gap-4">
              <!-- A: Quote -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Quote</label>
                <input v-model="formData.quoteNo" type="text" placeholder="MAHFAR TRADING" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500" />
              </div>
              <!-- B: Date -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Date</label>
                <input v-model="formData.date" type="date" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500" />
              </div>
              <!-- C: Material -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Material</label>
                <select v-model="formData.materialType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                  <option value="PPAL">PPAL</option>
                  <option value="PPAZ">PPAZ</option>
                  <option value="PPGI">PPGI</option>
                  <option value="GI">GI</option>
                </select>
              </div>
              <!-- D: Type of Panel -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Type of Panel</label>
                <select v-model="formData.panelType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                  <option value="ROOF">ROOF</option>
                  <option value="WALL">WALL</option>
                  <option value="CEILING">CEILING</option>
                  <option value="PARTITION">PARTITION</option>
                  <option value="HIDDEN">HIDDEN</option>
                  <option value="SS">SS</option>
                </select>
              </div>
              <!-- E: Color -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Color</label>
                <input v-model="formData.color" type="text" placeholder="9002" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500" />
              </div>
            </div>
          </div>

          <!-- Top Skin (Columns F-K, N) -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-purple-500">📐 Top Skin</h2>
            <div class="grid grid-cols-2 md:grid-cols-7 gap-4">
              <!-- F: Material Type -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Material Type</label>
                <select v-model="formData.topSkinMaterialType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                  <option value="PU">PU</option>
                  <option value="PIR">PIR</option>
                  <option value="RW">RW</option>
                </select>
              </div>
              <!-- G: Thickness -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Thickness (mm)</label>
                <input v-model.number="formData.topSkinThickness" type="number" step="0.01" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- H: Girth -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Girth (mm)</label>
                <input v-model.number="formData.topSkinGirth" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- I: Length -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Length (mm)</label>
                <input v-model.number="formData.topSkinLength" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- J: Qty -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Qty</label>
                <input v-model.number="formData.quantity" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg bg-yellow-50" />
              </div>
              <!-- K: Density -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input v-model.number="formData.topSkinDensity" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- N: Cost/Ton -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Cost/Ton (₹)</label>
                <input v-model.number="formData.topSkinCostPerTon" type="number" step="0.01" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-yellow-50" />
              </div>
            </div>
          </div>

          <!-- Bottom Skin (Columns F-K, N) -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-indigo-500">📐 Bottom Skin</h2>
            <div class="grid grid-cols-2 md:grid-cols-7 gap-4">
              <!-- F: Material Type -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Material Type</label>
                <select v-model="formData.bottomSkinMaterialType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                  <option value="PU">PU</option>
                  <option value="PIR">PIR</option>
                  <option value="RW">RW</option>
                </select>
              </div>
              <!-- G: Thickness -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Thickness (mm)</label>
                <input v-model.number="formData.bottomSkinThickness" type="number" step="0.01" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- H: Girth -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Girth (mm)</label>
                <input v-model.number="formData.bottomSkinGirth" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- I: Length -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Length (mm)</label>
                <input v-model.number="formData.bottomSkinLength" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- J: Qty (same as above) -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Qty</label>
                <input :value="formData.quantity" disabled class="w-full px-3 py-2 border border-gray-200 rounded-lg bg-gray-50 text-gray-600" />
              </div>
              <!-- K: Density -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input v-model.number="formData.bottomSkinDensity" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- N: Cost/Ton -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Cost/Ton (₹)</label>
                <input v-model.number="formData.bottomSkinCostPerTon" type="number" step="0.01" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-yellow-50" />
              </div>
            </div>
          </div>

          <!-- Core (Columns F-K, N) -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-pink-500">Core</h2>
            <div class="grid grid-cols-2 md:grid-cols-7 gap-4">
              <!-- F: Material Type -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Material Type</label>
                <select v-model="formData.coreMaterialType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                  <option value="PU">PU</option>
                  <option value="PIR">PIR</option>
                  <option value="RW">RW</option>
                </select>
              </div>
              <!-- G: Thickness -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Thickness (mm)</label>
                <input v-model.number="formData.coreThickness" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- H: Girth -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Girth (mm)</label>
                <input v-model.number="formData.coreGirth" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- I: Length -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Length (mm)</label>
                <input v-model.number="formData.coreLength" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- J: Qty (same as above) -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Qty</label>
                <input :value="formData.quantity" disabled class="w-full px-3 py-2 border border-gray-200 rounded-lg bg-gray-50 text-gray-600" />
              </div>
              <!-- K: Density -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input v-model.number="formData.coreDensity" type="number" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
              <!-- N: Cost/Ton -->
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Cost/Ton (₹)</label>
                <input v-model.number="formData.coreCostPerTon" type="number" step="0.01" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-yellow-50" />
              </div>
            </div>
            <!-- Core Constant -->
            <div class="grid grid-cols-1 md:grid-cols-7 gap-4 mt-3">
              <div class="md:col-start-6">
                <label class="block text-sm font-medium text-gray-700 mb-1">Core Constant</label>
                <input v-model.number="formData.coreConstant" type="number" step="0.001" class="w-full px-3 py-2 border border-gray-300 rounded-lg" />
              </div>
            </div>
          </div>

          <!-- Pricing & Additional Costs -->
          <div class="mb-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-3 pb-2 border-b-2 border-orange-500">💵 Pricing & Additional Costs</h2>
            
            <!-- Additional Cost Components -->
            <div class="bg-yellow-50 p-4 rounded-lg">
              <h3 class="text-sm font-semibold text-gray-700 mb-3">Additional Cost Components</h3>
              <div class="grid grid-cols-1 md:grid-cols-6 gap-4">
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">D-Cost (₹)</label>
                  <input v-model.number="formData.dCost" type="number" step="0.001" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-yellow-500" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Labour (₹)</label>
                  <input v-model.number="formData.labour" type="number" step="0.001" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-yellow-500" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Over head (₹)</label>
                  <input v-model.number="formData.overHeadCost" type="number" step="any" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-yellow-500" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Lc (₹)</label>
                  <input v-model.number="formData.lc" type="number" step="any" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-yellow-500" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Packing Cost (₹)</label>
                  <input v-model.number="formData.packingCost" type="number" step="any" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-yellow-500" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Duty (₹)</label>
                  <input v-model.number="formData.duty" type="number" step="any" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-yellow-500" />
                </div>
              </div>
            </div>
          </div>

          <!-- Calculate Button -->
          <div class="flex justify-center mb-6">
            <button type="submit" class="bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700 text-white font-bold py-3 px-8 rounded-lg shadow-lg transform transition hover:scale-105">
              🧮 Calculate Total Cost
            </button>
          </div>

          <!-- Results (Excel Columns L-T) -->
          <div v-if="result.calculated" class="bg-gradient-to-r from-green-50 to-emerald-50 rounded-xl p-6 border-2 border-green-300">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-green-500">💰 Calculation Results</h2>
            
            <!-- Results Table -->
            <div class="overflow-x-auto mb-6">
              <table class="w-full text-sm border-collapse">
                <thead>
                  <tr class="bg-gray-100 border-b-2 border-gray-300">
                    <th class="px-3 py-2 text-left border">Type of Panel</th>
                    <th class="px-3 py-2 text-right border">Weight/Meter</th>
                    <th class="px-3 py-2 text-right border">Total Weight/Meter</th>
                    <th class="px-3 py-2 text-right border">Cost/Ton</th>
                    <th class="px-3 py-2 text-right border">Material Cost</th>
                  </tr>
                </thead>
                <tbody>
                  <tr class="border-b hover:bg-gray-50">
                    <td class="px-3 py-2 font-medium border">Top Skin</td>
                    <td class="px-3 py-2 text-right border">{{ formatWeight(result.topSkinWeight) }}</td>
                    <td class="px-3 py-2 text-right border">{{ formatWeight(result.topSkinTotalWeight) }}</td>
                    <td class="px-3 py-2 text-right border">{{ formatCurrency(formData.topSkinCostPerTon) }}</td>
                    <td class="px-3 py-2 text-right font-semibold border">{{ formatCurrency(result.topSkinMaterialCost) }}</td>
                  </tr>
                  <tr class="border-b hover:bg-gray-50">
                    <td class="px-3 py-2 font-medium border">Bottom Skin</td>
                    <td class="px-3 py-2 text-right border">{{ formatWeight(result.bottomSkinWeight) }}</td>
                    <td class="px-3 py-2 text-right border">{{ formatWeight(result.bottomSkinTotalWeight) }}</td>
                    <td class="px-3 py-2 text-right border">{{ formatCurrency(formData.bottomSkinCostPerTon) }}</td>
                    <td class="px-3 py-2 text-right font-semibold border">{{ formatCurrency(result.bottomSkinMaterialCost) }}</td>
                  </tr>
                  <tr class="border-b hover:bg-gray-50">
                    <td class="px-3 py-2 font-medium border">Core</td>
                    <td class="px-3 py-2 text-right border">{{ formatWeight(result.coreWeight) }}</td>
                    <td class="px-3 py-2 text-right border">{{ formatWeight(result.coreTotalWeight) }}</td>
                    <td class="px-3 py-2 text-right border">{{ formatCurrency(formData.coreCostPerTon) }}</td>
                    <td class="px-3 py-2 text-right font-semibold border">{{ formatCurrency(result.coreMaterialCost) }}</td>
                  </tr>
                  <tr class="bg-blue-50 font-bold border-b-2 border-blue-300">
                    <td class="px-3 py-2 border">TOTAL</td>
                    <td class="px-3 py-2 text-right border">{{ formatWeight(result.totalWeight) }}</td>
                    <td class="px-3 py-2 text-right border">-</td>
                    <td class="px-3 py-2 text-right border">-</td>
                    <td class="px-3 py-2 text-right border">{{ formatCurrency(result.totalMaterialCost) }}</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <!-- Columns P, Q, R, S, T -->
            <div class="bg-white rounded-lg p-4 shadow mb-4">
              <h3 class="text-lg font-semibold text-gray-700 mb-3">Value Addition Analysis</h3>
              <div class="grid grid-cols-2 md:grid-cols-5 gap-3">
                <!-- Q: Value Addition -->
                <div class="bg-green-50 rounded-lg p-3">
                  <p class="text-xs text-gray-600 font-semibold">Value Addition (Q)</p>
                  <p class="text-lg font-bold" :class="result.valueAddition >= 0 ? 'text-green-600' : 'text-red-600'">
                    {{ formatCurrency(result.valueAddition) }}
                  </p>
                </div>
                <!-- R: Value Add Total -->
                <div class="bg-indigo-50 rounded-lg p-3">
                  <p class="text-xs text-gray-600 font-semibold">Value Add Total (R)</p>
                  <p class="text-lg font-bold text-indigo-600">{{ formatCurrency(result.valueAddTotal) }}</p>
                </div>
                <!-- S: Total Sales -->
                <div class="bg-purple-50 rounded-lg p-3">
                  <p class="text-xs text-gray-600 font-semibold">Total Sales (S)</p>
                  <p class="text-lg font-bold text-purple-600">{{ formatCurrency(result.totalSales) }}</p>
                </div>
                <!-- T: % of Value Add -->
                <div class="bg-pink-50 rounded-lg p-3">
                  <p class="text-xs text-gray-600 font-semibold">% of Value Add (T)</p>
                  <p class="text-lg font-bold text-pink-600">{{ formatPercent(result.percentValueAdd) }}%</p>
                </div>
              </div>
            </div>

            <!-- Additional Cost Components -->
            <div class="bg-red-50 rounded-lg p-4 mb-4 border-2 border-red-200">
              <h3 class="text-lg font-semibold text-gray-800 mb-3">📋 Additional Cost Components</h3>
              <div class="grid grid-cols-2 md:grid-cols-6 gap-3 mb-3">
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">D-Cost</p>
                  <p class="text-lg font-bold text-red-600">{{ formatCurrency(formData.dCost) }}</p>
                </div>
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Labour</p>
                  <p class="text-lg font-bold text-red-600">{{ formatCurrency(formData.labour) }}</p>
                </div>
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Over head</p>
                  <p class="text-lg font-bold text-red-600">{{ formatCurrency(formData.overHeadCost) }}</p>
                </div>
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Lc</p>
                  <p class="text-lg font-bold text-red-600">{{ formatCurrency(formData.lc) }}</p>
                </div>
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Packing Cost</p>
                  <p class="text-lg font-bold text-red-600">{{ formatCurrency(formData.packingCost) }}</p>
                </div>
                <div class="bg-white rounded-lg p-3 shadow">
                  <p class="text-xs text-gray-600">Duty</p>
                  <p class="text-lg font-bold text-red-600">{{ formatCurrency(formData.duty) }}</p>
                </div>
              </div>
              <div class="grid grid-cols-1 md:grid-cols-4 gap-3">
                <div class="bg-gradient-to-r from-yellow-200 to-yellow-300 rounded-lg p-3 shadow-lg text-center">
                  <p class="text-sm text-gray-800 mb-1">Cost per Unit</p>
                  <p class="text-2xl font-bold text-gray-800">{{ formatCurrency(result.costPerUnit) }}</p>
                </div>
                <div class="bg-gradient-to-r from-blue-500 to-blue-600 rounded-lg p-3 shadow-lg">
                  <p class="text-sm text-white mb-1">Cost (Subtotal)</p>
                  <p class="text-xl font-bold text-white">{{ formatCurrency(result.cost) }}</p>
                  <p class="text-xs text-blue-100 mt-1">Material Cost + D-Cost + Labour</p>
                </div>
                <div class="bg-gradient-to-r from-red-500 to-red-600 rounded-lg p-3 shadow-lg">
                  <p class="text-sm text-white mb-1">Total Cost (Final)</p>
                  <p class="text-2xl font-bold text-white">{{ formatCurrency(result.totalCost) }}</p>
                  <p class="text-xs text-red-100 mt-1">Cost + Over head + Lc + Packing Cost + Duty</p>
                </div>
                <div class="bg-gradient-to-r from-green-500 to-green-600 rounded-lg p-3 shadow-lg">
                  <p class="text-sm text-white mb-1">Quoted Price</p>
                  <p class="text-2xl font-bold text-white">{{ formatCurrency(formData.quotedPrice) }}</p>
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>

      <!-- Footer -->
      <div class="text-center mt-6 text-gray-600">
        <p class="text-lg font-semibold text-gray-800 mb-1">
          <a href="https://emaarllc.com/" target="_blank" class="text-gray-800 hover:text-blue-600 hover:underline">Emaar Industries</a>
        </p>
        <p class="text-sm">© 2025 Panel Cost Calculator | Developed by <a href="https://www.codemub.com" target="_blank" class="text-blue-600 hover:text-blue-800 hover:underline">CodeMub</a></p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, watch } from 'vue'

const formData = reactive({
  quoteNo: 'MAHFAR TRADING',
  date: new Date().toISOString().split('T')[0],
  materialType: 'PPAL',
  panelType: 'ROOF',
  color: '9002',
  
  topSkinMaterialType: 'PU',
  topSkinCostPerTon: 1400,
  bottomSkinMaterialType: 'PU',
  bottomSkinCostPerTon: 1400,
  coreMaterialType: 'PU',
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
  
  // Additional cost components from Excel
  dCost: 0.100,
  labour: 0.200,
  overHeadCost: 0,
  lc: 0,
  packingCost: 0,
  duty: 0
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
  costPerUnit: 0,
  // New fields from Excel
  cost: 0,  // Subtotal before additional costs
  totalCost: 0  // Final total with all costs
})

// Helper function to calculate RW cost based on thickness
// Formula: Cost = Thickness × 0.06 (based on pattern: 50mm=3, 100mm=6, 150mm=9)
const calculateRWCost = (thickness) => {
  if (!thickness || thickness <= 0) return 0
  return thickness * 0.06
}

// Helper function to get cost for a material type
const getMaterialCost = (materialType, thickness, manualCost) => {
  if (materialType === 'RW') {
    return calculateRWCost(thickness)
  }
  return manualCost
}

// Helper function to get density based on material type (for skins)
const getDensityByMaterial = (materialType) => {
  const densityMap = {
    'PPAL': 2720,
    'PPGI': 7850,
    'PPAZ': 7850
  }
  return densityMap[materialType] || 2720
}

// Watchers to update costs when material type or thickness changes
watch(() => [formData.topSkinMaterialType, formData.topSkinThickness], () => {
  if (formData.topSkinMaterialType === 'RW') {
    formData.topSkinCostPerTon = calculateRWCost(formData.topSkinThickness)
  }
})

watch(() => [formData.bottomSkinMaterialType, formData.bottomSkinThickness], () => {
  if (formData.bottomSkinMaterialType === 'RW') {
    formData.bottomSkinCostPerTon = calculateRWCost(formData.bottomSkinThickness)
  }
})

// Watcher to update density based on global material type (for top and bottom skins)
watch(() => formData.materialType, () => {
  formData.topSkinDensity = getDensityByMaterial(formData.materialType)
  formData.bottomSkinDensity = getDensityByMaterial(formData.materialType)
})

watch(() => [formData.coreMaterialType, formData.coreThickness], () => {
  if (formData.coreMaterialType === 'RW') {
    formData.coreCostPerTon = calculateRWCost(formData.coreThickness)
  }
})

// Watcher to auto-calculate additional costs based on result.cost
watch(() => result.cost, () => {
  if (result.cost > 0) {
    formData.overHeadCost = result.cost * 0.1
    formData.lc = result.cost * 0.05
    formData.packingCost = result.cost * 0.05
    formData.duty = result.cost * 0.1
  }
})

const calculateCost = () => {
  console.clear()
  console.log('%c═══════════════════════════════════════════════════════════', 'color: #2563eb; font-weight: bold')
  console.log('%c🧮 SANDWICH PANEL COST CALCULATION - DETAILED LOG', 'color: #2563eb; font-weight: bold; font-size: 16px')
  console.log('%c═══════════════════════════════════════════════════════════', 'color: #2563eb; font-weight: bold')
  
  // Input Summary
  console.log('\n%c📋 INPUT VALUES:', 'color: #059669; font-weight: bold; font-size: 14px')
  console.log('Quote:', formData.quoteNo)
  console.log('Date:', formData.date)
  console.log('Material:', formData.materialType)
  console.log('Panel Type:', formData.panelType)
  console.log('Color:', formData.color)
  console.log('Quantity:', formData.quantity)
  console.log('Quoted Price:', `₹${formData.quotedPrice}/unit`)
  
  // Top Skin Calculations
  console.log('\n%c━━━ TOP SKIN CALCULATIONS (Row 6) ━━━', 'color: #7c3aed; font-weight: bold')
  console.log('Inputs:')
  console.log('  Thickness (G6):', formData.topSkinThickness, 'mm')
  console.log('  Girth (H6):', formData.topSkinGirth, 'mm')
  console.log('  Length (I6):', formData.topSkinLength, 'mm')
  console.log('  Density (K6):', formData.topSkinDensity, 'kg/m³')
  console.log('  Cost/Ton (N6):', `₹${formData.topSkinCostPerTon}`)
  
  // L6 = G6*H6/1000*I6/1000*K6/1000
  result.topSkinWeight = formData.topSkinThickness * formData.topSkinGirth / 1000 * formData.topSkinLength / 1000 * formData.topSkinDensity / 1000
  console.log('\n📐 Weight Calculation (L6):')
  console.log(`  Formula: (Thickness × Girth / 1000) × (Length / 1000) × (Density / 1000)`)
  console.log(`  = (${formData.topSkinThickness} × ${formData.topSkinGirth} / 1000) × (${formData.topSkinLength} / 1000) × (${formData.topSkinDensity} / 1000)`)
  console.log(`  = ${result.topSkinWeight.toFixed(6)} tons`)
  
  // M6 = L6*J6/1000
  result.topSkinTotalWeight = result.topSkinWeight * formData.quantity / 1000
  console.log('\n📦 Total Weight/Ton (M6):')
  console.log(`  Formula: Weight × Quantity / 1000`)
  console.log(`  = ${result.topSkinWeight.toFixed(6)} × ${formData.quantity} / 1000`)
  console.log(`  = ${result.topSkinTotalWeight.toFixed(6)} tons`)
  
  // O6 = N6/1000*L6
  const topSkinCost = getMaterialCost(formData.topSkinMaterialType, formData.topSkinThickness, formData.topSkinCostPerTon)
  result.topSkinMaterialCost = (topSkinCost / 1000) * result.topSkinWeight
  console.log('\n💰 Material Cost (O6):')
  console.log(`  Formula: (Cost/Ton / 1000) × Weight`)
  console.log(`  = (${topSkinCost} / 1000) × ${result.topSkinWeight.toFixed(6)}`)
  console.log(`  = ₹${result.topSkinMaterialCost.toFixed(6)}`)

  // Bottom Skin Calculations
  console.log('\n%c━━━ BOTTOM SKIN CALCULATIONS (Row 7) ━━━', 'color: #4f46e5; font-weight: bold')
  console.log('Inputs:')
  console.log('  Thickness (G7):', formData.bottomSkinThickness, 'mm')
  console.log('  Girth (H7):', formData.bottomSkinGirth, 'mm')
  console.log('  Length (I7):', formData.bottomSkinLength, 'mm')
  console.log('  Density (K7):', formData.bottomSkinDensity, 'kg/m³')
  console.log('  Cost/Ton (N7):', `₹${formData.bottomSkinCostPerTon}`)
  
  // L7 = G7*H7/1000*I7/1000*K7/1000
  result.bottomSkinWeight = formData.bottomSkinThickness * formData.bottomSkinGirth / 1000 * formData.bottomSkinLength / 1000 * formData.bottomSkinDensity / 1000
  console.log('\n📐 Weight Calculation (L7):')
  console.log(`  Formula: (Thickness × Girth / 1000) × (Length / 1000) × (Density / 1000)`)
  console.log(`  = (${formData.bottomSkinThickness} × ${formData.bottomSkinGirth} / 1000) × (${formData.bottomSkinLength} / 1000) × (${formData.bottomSkinDensity} / 1000)`)
  console.log(`  = ${result.bottomSkinWeight.toFixed(6)} tons`)
  
  // M7 = L7*J7/1000
  result.bottomSkinTotalWeight = result.bottomSkinWeight * formData.quantity / 1000
  console.log('\n📦 Total Weight/Ton (M7):')
  console.log(`  Formula: Weight × Quantity / 1000`)
  console.log(`  = ${result.bottomSkinWeight.toFixed(6)} × ${formData.quantity} / 1000`)
  console.log(`  = ${result.bottomSkinTotalWeight.toFixed(6)} tons`)
  
  // O7 = N7/1000*L7
  const bottomSkinCost = getMaterialCost(formData.bottomSkinMaterialType, formData.bottomSkinThickness, formData.bottomSkinCostPerTon)
  result.bottomSkinMaterialCost = (bottomSkinCost / 1000) * result.bottomSkinWeight
  console.log('\n💰 Material Cost (O7):')
  console.log(`  Formula: (Cost/Ton / 1000) × Weight`)
  console.log(`  = (${bottomSkinCost} / 1000) × ${result.bottomSkinWeight.toFixed(6)}`)
  console.log(`  = ₹${result.bottomSkinMaterialCost.toFixed(6)}`)

  // Core Calculations
  console.log('\n%c━━━ CORE CALCULATIONS (Row 8) ━━━', 'color: #ec4899; font-weight: bold')
  console.log('Inputs:')
  console.log('  Thickness (G8):', formData.coreThickness, 'mm')
  console.log('  Girth (H8):', formData.coreGirth, 'mm')
  console.log('  Length (I8):', formData.coreLength, 'mm')
  console.log('  Density (K8):', formData.coreDensity, 'kg/m³')
  console.log('  Core Constant:', formData.coreConstant)
  console.log('  Cost/Ton (N8):', `₹${formData.coreCostPerTon}`)
  
  // L8 = G8/1000*H8/1000*I8/1000*K8+0.215 (Special formula with constant)
  result.coreWeight = formData.coreThickness / 1000 * formData.coreGirth / 1000 * formData.coreLength / 1000 * formData.coreDensity + formData.coreConstant
  console.log('\n📐 Weight Calculation (L8) - SPECIAL FORMULA:')
  console.log(`  Formula: (Thickness / 1000) × (Girth / 1000) × (Length / 1000) × Density + Core Constant`)
  console.log(`  = (${formData.coreThickness} / 1000) × (${formData.coreGirth} / 1000) × (${formData.coreLength} / 1000) × ${formData.coreDensity} + ${formData.coreConstant}`)
  console.log(`  = ${result.coreWeight.toFixed(6)} tons`)
  
  // M8 = L8*J8/1000
  result.coreTotalWeight = result.coreWeight * formData.quantity / 1000
  console.log('\n📦 Total Weight/Ton (M8):')
  console.log(`  Formula: Weight × Quantity / 1000`)
  console.log(`  = ${result.coreWeight.toFixed(6)} × ${formData.quantity} / 1000`)
  console.log(`  = ${result.coreTotalWeight.toFixed(6)} tons`)
  
  // O8 = N8/1000*L8
  const coreCost = getMaterialCost(formData.coreMaterialType, formData.coreThickness, formData.coreCostPerTon)
  result.coreMaterialCost = (coreCost / 1000) * result.coreWeight
  console.log('\n💰 Material Cost (O8):')
  console.log(`  Formula: (Cost/Ton / 1000) × Weight`)
  console.log(`  = (${coreCost} / 1000) × ${result.coreWeight.toFixed(6)}`)
  console.log(`  = ₹${result.coreMaterialCost.toFixed(6)}`)

  // Totals
  console.log('\n%c━━━ TOTALS (Row 9) ━━━', 'color: #0891b2; font-weight: bold')
  
  // L9 = SUM(L6:L8)
  result.totalWeight = result.topSkinWeight + result.bottomSkinWeight + result.coreWeight
  console.log('\n⚖️ Total Weight (L9):')
  console.log(`  Formula: SUM(L6:L8)`)
  console.log(`  = ${result.topSkinWeight.toFixed(6)} + ${result.bottomSkinWeight.toFixed(6)} + ${result.coreWeight.toFixed(6)}`)
  console.log(`  = ${result.totalWeight.toFixed(6)} tons`)

  // O11 = SUM(O6:O10)
  result.totalMaterialCost = result.topSkinMaterialCost + result.bottomSkinMaterialCost + result.coreMaterialCost
  console.log('\n💵 Total Material Cost (O11):')
  console.log(`  Formula: SUM(O6:O8)`)
  console.log(`  = ${result.topSkinMaterialCost.toFixed(6)} + ${result.bottomSkinMaterialCost.toFixed(6)} + ${result.coreMaterialCost.toFixed(6)}`)
  console.log(`  = ${result.totalMaterialCost.toFixed(6)}`)

  // COST CALCULATION (Excel pattern)
  console.log('\n%c━━━ COST CALCULATION ━━━', 'color: #b91c1c; font-weight: bold')
  
  console.log('\n💰 D-Cost:')
  console.log(`  Value: ${formData.dCost}`)
  
  console.log('\n👷 Labour:')
  console.log(`  Value: ${formData.labour}`)
  
  // Cost = Total Material Cost + D-Cost + Labour
  result.cost = result.totalMaterialCost + formData.dCost + formData.labour
  console.log('\n📊 Cost (Subtotal):')
  console.log(`  Formula: Total Material Cost + D-Cost + Labour`)
  console.log(`  = ${result.totalMaterialCost.toFixed(3)} + ${formData.dCost} + ${formData.labour}`)
  console.log(`  = ${result.cost.toFixed(3)}`)
  
  // Value Addition Calculations
  console.log('\n%c━━━ VALUE ADDITION ANALYSIS ━━━', 'color: #dc2626; font-weight: bold')
  
  // Value Addition = Quoted Price - Cost (NOT Total Material Cost)
  result.valueAddition = formData.quotedPrice - result.cost
  console.log('\n💎 Value Addition:')
  console.log(`  Formula: Quoted Price - Cost`)
  console.log(`  = ${formData.quotedPrice} - ${result.cost.toFixed(3)}`)
  console.log(`  = ${result.valueAddition.toFixed(3)}`)
  console.log(`  ${result.valueAddition >= 0 ? '✅ PROFIT' : '❌ LOSS'}`)

  // R11 = Q11*J6
  result.valueAddTotal = result.valueAddition * formData.quantity
  console.log('\n💰 Value Add Total:')
  console.log(`  Formula: Value Addition × Quantity`)
  console.log(`  = ${result.valueAddition.toFixed(3)} × ${formData.quantity}`)
  console.log(`  = ${result.valueAddTotal.toFixed(2)}`)

  // S11 = P6*J6
  result.totalSales = formData.quotedPrice * formData.quantity
  console.log('\n📊 Total Sales:')
  console.log(`  Formula: Quoted Price × Quantity`)
  console.log(`  = ${formData.quotedPrice} × ${formData.quantity}`)
  console.log(`  = ${result.totalSales.toFixed(2)}`)

  // T11 = R11/S11%
  result.percentValueAdd = result.totalSales > 0 ? (result.valueAddTotal / result.totalSales) * 100 : 0
  console.log('\n📈 % of Value Add:')
  console.log(`  Formula: (Value Add Total / Total Sales) × 100`)
  console.log(`  = (${result.valueAddTotal.toFixed(2)} / ${result.totalSales.toFixed(2)}) × 100`)
  console.log(`  = ${result.percentValueAdd.toFixed(2)}%`)

  console.log('\n🏭 Over head:')
  console.log(`  Value: ${formData.overHeadCost}`)
  
  console.log('\n📦 Lc:')
  console.log(`  Value: ${formData.lc}`)
  
  console.log('\n📦 Packing Cost:')
  console.log(`  Value: ${formData.packingCost}`)
  
  console.log('\n🚚 Duty:')
  console.log(`  Value: ${formData.duty}`)
  
  // Total Cost = Cost + Over head + Lc + Packing Cost + Duty
  result.totalCost = result.cost + formData.overHeadCost + formData.lc + formData.packingCost + formData.duty
  console.log('\n💵 Total Cost (Final):')
  console.log(`  Formula: Cost + Over head + Lc + Packing Cost + Duty`)
  console.log(`  = ${result.cost.toFixed(3)} + ${formData.overHeadCost} + ${formData.lc} + ${formData.packingCost} + ${formData.duty}`)
  console.log(`  = ${result.totalCost.toFixed(3)}`)

  result.costPerUnit = formData.quantity > 0 ? result.totalCost / formData.quantity : 0
  console.log('\n🎯 Cost per Unit:')
  console.log(`  Formula: Total Cost / Quantity`)
  console.log(`  = ${result.totalCost.toFixed(3)} / ${formData.quantity}`)
  console.log(`  = ${result.costPerUnit.toFixed(3)} per unit`)

  // Summary
  console.log('\n%c═══════════════════════════════════════════════════════════', 'color: #16a34a; font-weight: bold')
  console.log('%c📋 CALCULATION SUMMARY', 'color: #16a34a; font-weight: bold; font-size: 14px')
  console.log('%c═══════════════════════════════════════════════════════════', 'color: #16a34a; font-weight: bold')
  console.table({
    'Total Weight': `${result.totalWeight.toFixed(3)} tons`,
    'Total Material Cost': `${result.totalMaterialCost.toFixed(3)}`,
    'D-Cost': `${formData.dCost}`,
    'Labour': `${formData.labour}`,
    'Cost (Subtotal)': `${result.cost.toFixed(3)}`,
    'Over head': `${formData.overHeadCost}`,
    'Lc': `${formData.lc}`,
    'Total Cost': `${result.totalCost.toFixed(3)}`,
    'Cost per Unit': `${result.costPerUnit.toFixed(3)}`,
    'Quoted Price': `${formData.quotedPrice}`,
    'Value Addition': `${result.valueAddition.toFixed(3)}`,
    'Profit %': `${result.percentValueAdd.toFixed(2)}%`
  })
  
  console.log('\n%c✅ Calculation Complete!', 'color: #16a34a; font-weight: bold; font-size: 14px')
  console.log('%c═══════════════════════════════════════════════════════════\n', 'color: #16a34a; font-weight: bold')

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
