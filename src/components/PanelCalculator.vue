<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 py-8 px-4 sm:px-6 lg:px-8">
    <div class="max-w-[1600px] mx-auto">
      
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-gray-900 mb-2">Sandwich Panel Costing Calculator</h1>
        <p class="text-lg text-gray-600">Multi-Panel Costing System</p>
      </div>

      <!-- Navigation Tabs -->
      <div class="flex flex-wrap justify-center gap-4 mb-8">
        <button 
          v-for="type in ['ROOF', 'WALL', 'CONCEALED', 'SS']" 
          :key="type"
          @click="currentPanelType = type"
          class="px-6 py-2 rounded-full font-bold transition-all duration-200 shadow-sm"
          :class="currentPanelType === type 
            ? 'bg-blue-600 text-white shadow-lg scale-105' 
            : 'bg-white text-gray-600 hover:bg-gray-50'"
        >
          {{ type }}
        </button>
        <button 
        @click="scrollToSummary"
        class="flex items-center space-x-2 px-6 py-3 bg-gray-800 text-white rounded-lg hover:bg-gray-700 transition-colors shadow-lg"
      >
        <span>📊</span>
        <span>View Project Summary</span>
      </button>

      <button 
        @click="logDetailedCalculations"
        class="flex items-center space-x-2 px-6 py-3 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors shadow-lg"
      >
        <span>🖨️</span>
        <span>Print Calculation Details</span>
      </button>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
        <!-- LEFT COLUMN: INPUTS -->
        <div class="lg:col-span-7 space-y-6">
          
          <!-- Quotation Info (Shared/Common or Per Panel? User implies separate calculation, so per panel inputs) -->
          <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
            <h2 class="text-lg font-semibold text-gray-800 mb-4 pb-2 border-b border-gray-100 flex items-center">
              <span class="bg-blue-100 text-blue-600 p-1.5 rounded-md mr-2">📋</span> 
              {{ currentPanelType }} Panel Details
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div>
                <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Customer Name</label>
                <input v-model="activeData.quoteNo" type="text" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition-colors" />
              </div>
              <div>
                <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Date</label>
                <input v-model="activeData.date" type="date" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition-colors" />
              </div>
              <div>
                <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Type of Panel</label>
                <select v-model="currentPanelType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition-colors font-medium">
                  <option value="ROOF">ROOF</option>
                  <option value="WALL">WALL</option>
                  <option value="CONCEALED">CONCEALED</option>
                  <option value="SS">SS (Stainless Steel)</option>
                </select>
              </div>
                <div v-show="currentPanelType !== 'SS'">
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Color</label>
                  <input v-model="activeData.color" type="text" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-pink-500" />
                </div>
                <div v-show="currentPanelType !== 'SS'">
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Quantity (m²)</label>
                  <input v-model.number="activeData.quantity" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-pink-500" />
                </div>
            </div>
          </div>

          <!-- STANDARD PANEL INPUTS (ROOF, WALL, CONCEALED) -->
          <div v-show="currentPanelType !== 'SS'" class="space-y-6">
            
            <!-- Top Skin -->
            <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
              <h2 class="text-lg font-semibold text-gray-800 mb-4 pb-2 border-b border-gray-100 flex items-center">
                <span class="bg-purple-100 text-purple-600 p-1.5 rounded-md mr-2">📐</span> Top Skin
              </h2>
              <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Material Type</label>
                  <select v-model="activeData.topSkinMaterialType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500">
                    <option value="PPAL">PPAL</option>
                    <option value="PPAZ">PPAZ</option>
                    <option value="PPGI">PPGI</option>
                    <option value="GI">GI</option>
                  </select>
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Thickness (mm)</label>
                  <input v-model.number="activeData.topSkinThickness" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Girth (mm)</label>
                  <input v-model.number="activeData.topSkinGirth" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Length (mm)</label>
                  <input v-model.number="activeData.topSkinLength" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Density (kg/m³)</label>
                  <input v-model.number="activeData.topSkinDensity" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Cost/Ton</label>
                  <input v-model.number="activeData.topSkinCostPerTon" type="number" step="any" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-yellow-50 focus:ring-2 focus:ring-yellow-500" />
                </div>
              </div>
            </div>

            <!-- Bottom Skin -->
            <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
              <h2 class="text-lg font-semibold text-gray-800 mb-4 pb-2 border-b border-gray-100 flex items-center">
                <span class="bg-indigo-100 text-indigo-600 p-1.5 rounded-md mr-2">📐</span> Bottom Skin
              </h2>
              <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Material Type</label>
                  <select v-model="activeData.bottomSkinMaterialType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500">
                    <option value="PPAL">PPAL</option>
                    <option value="PPAZ">PPAZ</option>
                    <option value="PPGI">PPGI</option>
                    <option value="GI">GI</option>
                  </select>
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Thickness (mm)</label>
                  <input v-model.number="activeData.bottomSkinThickness" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Girth (mm)</label>
                  <input v-model.number="activeData.bottomSkinGirth" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Length (mm)</label>
                  <input v-model.number="activeData.bottomSkinLength" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Density (kg/m³)</label>
                  <input v-model.number="activeData.bottomSkinDensity" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Cost/Ton</label>
                  <input v-model.number="activeData.bottomSkinCostPerTon" type="number" step="any" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-yellow-50 focus:ring-2 focus:ring-yellow-500" />
                </div>
              </div>
            </div>

            <!-- Core -->
            <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
              <h2 class="text-lg font-semibold text-gray-800 mb-4 pb-2 border-b border-gray-100 flex items-center">
                <span class="bg-pink-100 text-pink-600 p-1.5 rounded-md mr-2">🧱</span> Core
              </h2>
              <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Material Type</label>
                  <select v-model="activeData.coreMaterialType" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-pink-500">
                    <option value="PU">PU</option>
                    <option value="PIR">PIR</option>
                    <option value="RW">RW</option>
                  </select>
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Thickness (mm)</label>
                  <select v-model.number="activeData.coreThickness" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-pink-500">
                    <option :value="30">30</option>
                    <option :value="50">50</option>
                    <option :value="75">75</option>
                    <option :value="100">100</option>
                    <option :value="150">150</option>
                    <option :value="200">200</option>
                  </select>
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Girth (mm)</label>
                  <input v-model.number="activeData.coreGirth" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-pink-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Length (mm)</label>
                  <input v-model.number="activeData.coreLength" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-pink-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Density (kg/m³)</label>
                  <input v-model.number="activeData.coreDensity" type="number" step="any" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-pink-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1 flex items-center justify-between">
                    Core Constant
                    <button @click.prevent="showCoreConstant = !showCoreConstant" class="text-gray-400 hover:text-blue-500 focus:outline-none" title="Toggle Visibility">
                      <span v-if="showCoreConstant">👁️</span>
                      <span v-else>👁️‍🗨️</span>
                    </button>
                  </label>
                  <input v-show="showCoreConstant" v-model.number="activeData.coreConstant" type="number" step="0.001" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-pink-500" />
                </div>
                <div>
                  <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Cost/Ton</label>
                  <input v-model.number="activeData.coreCostPerTon" type="number" step="any" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-yellow-50 focus:ring-2 focus:ring-yellow-500" />
                </div>
              </div>
            </div>
          </div>

          <!-- SS Products Input -->
          <div v-show="currentPanelType === 'SS'">
             <SSProducts
              ref="ssProductsRef"
              :quote="activeData.quoteNo"
              :date="activeData.date"
              :color="activeData.color"
              @update="handleSSUpdate"
            />
          </div>

          <!-- Additional Costs -->
          <div v-show="currentPanelType !== 'SS'" class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
            <h2 class="text-lg font-semibold text-gray-800 mb-4 pb-2 border-b border-gray-100 flex items-center">
              <span class="bg-orange-100 text-orange-600 p-1.5 rounded-md mr-2">💵</span> Additional Costs
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
              <div>
                <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">D-Cost (Manual)</label>
                <input v-model.number="activeData.dCost" type="number" step="any" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-orange-500" />
              </div>
              <div>
                <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Labour (Manual)</label>
                <input v-model.number="activeData.labour" type="number" step="any" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-orange-500" />
              </div>
              <div>
                <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Over head (Factor)</label>
                <input v-model.number="activeData.overHeadCost" type="number" step="0.001" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-orange-500" />
              </div>
              <div>
                <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Lc (Factor)</label>
                <input v-model.number="activeData.lc" type="number" step="0.001" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-orange-500" />
              </div>
              <div>
                <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Packing Cost (Factor)</label>
                <input v-model.number="activeData.packingCost" type="number" step="0.001" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-orange-500" />
              </div>
              <div>
                <label class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-1">Duty (Factor)</label>
                <input v-model.number="activeData.duty" type="number" step="0.001" class="w-full px-3 py-2 border border-yellow-300 rounded-lg bg-white focus:ring-2 focus:ring-orange-500" />
              </div>
            </div>
          </div>

        </div>

        <!-- RIGHT COLUMN: RESULTS (Sticky) -->
        <div class="lg:col-span-5 space-y-6">
          <div class="sticky top-6 space-y-6">
            
            <!-- Standard Panel Results -->
            <div v-if="currentPanelType !== 'SS'" class="bg-white rounded-xl shadow-lg border border-gray-200 overflow-hidden">
              <div class="bg-gray-50 px-6 py-4 border-b border-gray-200">
                <h2 class="text-xl font-bold text-gray-800 flex items-center">
                  <span class="mr-2">📊</span> {{ currentPanelType }} Calculation
                </h2>
              </div>
              
              <div class="p-6">
                <!-- Main Table -->
                <div class="overflow-x-auto mb-6">
                  <table class="w-full text-sm">
                    <thead>
                      <tr class="text-gray-500 border-b border-gray-100">
                        <th class="text-left pb-2 font-medium">Component</th>
                        <th class="text-right pb-2 font-medium">Weight</th>
                        <th class="text-right pb-2 font-medium">Cost</th>
                      </tr>
                    </thead>
                    <tbody class="divide-y divide-gray-50">
                      <tr>
                        <td class="py-2 text-gray-600">Top Skin</td>
                        <td class="py-2 text-right">{{ formatWeight(activeResult.topSkinWeight) }}</td>
                        <td class="py-2 text-right font-medium">{{ formatCurrency(activeResult.topSkinMaterialCost) }}</td>
                      </tr>
                      <tr>
                        <td class="py-2 text-gray-600">Bottom Skin</td>
                        <td class="py-2 text-right">{{ formatWeight(activeResult.bottomSkinWeight) }}</td>
                        <td class="py-2 text-right font-medium">{{ formatCurrency(activeResult.bottomSkinMaterialCost) }}</td>
                      </tr>
                      <tr>
                        <td class="py-2 text-gray-600">Core</td>
                        <td class="py-2 text-right">{{ formatWeight(activeResult.coreWeight) }}</td>
                        <td class="py-2 text-right font-medium">{{ formatCurrency(activeResult.coreMaterialCost) }}</td>
                      </tr>
                      <!-- Additional Costs Removed from Table -->
                      <tr class="bg-blue-50 font-bold text-blue-900">
                        <td class="py-2 pl-2 rounded-l-md">TOTAL</td>
                        <td class="py-2 text-right">{{ formatWeight(activeResult.totalWeight) }}</td>
                        <td class="py-2 text-right pr-2 rounded-r-md">{{ formatCurrency(activeResult.totalMaterialCost) }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>

                <!-- Additional Costs Grid -->
                <div class="mb-6">
                  <h3 class="text-xs font-semibold text-gray-500 uppercase tracking-wider mb-2">Additional Costs Breakdown</h3>
                  <div class="grid grid-cols-3 gap-2">
                    <div class="bg-sky-50 p-2 rounded border border-sky-100 text-center">
                      <p class="text-[10px] text-sky-600 font-bold uppercase">D-Cost</p>
                      <p class="text-sm font-bold text-gray-800">{{ formatCurrency(activeResult.dCostVal) }}</p>
                    </div>
                    <div class="bg-sky-50 p-2 rounded border border-sky-100 text-center">
                      <p class="text-[10px] text-sky-600 font-bold uppercase">Labour</p>
                      <p class="text-sm font-bold text-gray-800">{{ formatCurrency(activeResult.labourVal) }}</p>
                    </div>
                    <div class="bg-sky-50 p-2 rounded border border-sky-100 text-center">
                      <p class="text-[10px] text-sky-600 font-bold uppercase">Overhead</p>
                      <p class="text-sm font-bold text-gray-800">{{ formatCurrency(activeResult.overheadVal) }}</p>
                    </div>
                    <div class="bg-sky-50 p-2 rounded border border-sky-100 text-center">
                      <p class="text-[10px] text-sky-600 font-bold uppercase">LC</p>
                      <p class="text-sm font-bold text-gray-800">{{ formatCurrency(activeResult.lcVal) }}</p>
                    </div>
                    <div class="bg-sky-50 p-2 rounded border border-sky-100 text-center">
                      <p class="text-[10px] text-sky-600 font-bold uppercase">Packing</p>
                      <p class="text-sm font-bold text-gray-800">{{ formatCurrency(activeResult.packingVal) }}</p>
                    </div>
                    <div class="bg-sky-50 p-2 rounded border border-sky-100 text-center">
                      <p class="text-[10px] text-sky-600 font-bold uppercase">Duty</p>
                      <p class="text-sm font-bold text-gray-800">{{ formatCurrency(activeResult.dutyVal) }}</p>
                    </div>
                  </div>
                </div>

                <!-- Key Metrics Grid -->
                <div class="grid grid-cols-3 gap-4 mb-6">
                  <!-- Sub Total (Yellow) -->
                  <div class="bg-yellow-50 p-3 rounded-lg border border-yellow-100">
                    <p class="text-xs text-yellow-700 font-semibold uppercase">Sub Total</p>
                    <p class="text-xl font-bold text-yellow-900">{{ formatCurrency(activeResult.cost) }}</p>
                  </div>
                  
                  <!-- Total Price (Green) -->
                  <div class="bg-green-50 p-3 rounded-lg border border-green-100">
                    <p class="text-xs text-green-700 font-semibold uppercase">Total Price</p>
                    <p class="text-xl font-bold text-green-900">{{ formatCurrency(activeResult.totalCost) }}</p>
                  </div>

                  <!-- Quoted Price (Blue) -->
                  <div class="bg-blue-50 p-3 rounded-lg border border-blue-100">
                    <p class="text-xs text-blue-700 font-semibold uppercase">Quoted Price</p>
                    <p class="text-xl font-bold text-blue-900">{{ formatCurrency(activeResult.quotedPrice) }}</p>
                  </div>
                </div>

                <!-- Value Addition -->
                <div class="bg-gray-50 rounded-lg p-4 border border-gray-100">
                  <div class="flex justify-between items-center mb-2">
                    <span class="text-sm text-gray-600">Value Addition</span>
                    <span class="font-bold" :class="activeResult.valueAddition >= 0 ? 'text-green-600' : 'text-red-600'">
                      {{ formatCurrency(activeResult.valueAddition) }}
                    </span>
                  </div>
                  <div class="flex justify-between items-center mb-2">
                    <span class="text-sm text-gray-600">Total Sales</span>
                    <span class="font-bold text-gray-800">{{ formatCurrency(activeResult.totalSales) }}</span>
                  </div>
                  <div class="flex justify-between items-center pt-2 border-t border-gray-200">
                    <span class="text-sm font-medium text-gray-700">% Value Add</span>
                    <span class="font-bold text-purple-600">{{ formatPercent(activeResult.percentValueAdd) }}%</span>
                  </div>
                </div>
              </div>
            </div>

            <!-- SS Results -->
            <div v-if="currentPanelType === 'SS'" class="bg-white rounded-xl shadow-lg border border-gray-200 overflow-hidden">
              <div class="bg-emerald-50 px-6 py-4 border-b border-emerald-100">
                <h2 class="text-xl font-bold text-emerald-800 flex items-center">
                  <span class="mr-2">🔩</span> SS Panel Results
                </h2>
              </div>
              <div class="p-6">
                <div class="overflow-x-auto mb-4">
                  <table class="w-full text-sm">
                    <thead class="bg-gray-50 text-gray-500 uppercase text-xs">
                      <tr>
                        <th class="py-2 text-left">Product</th>
                        <th class="py-2 text-right">Weight<br>(kg)</th>
                        <th class="py-2 text-right">Total Weight<br>(T)</th>
                        <th class="py-2 text-right">Val. Add<br>(Unit)</th>
                        <th class="py-2 text-right">Val. Add<br>(Total)</th>
                        <th class="py-2 text-right">Total Sales</th>
                        <th class="py-2 text-right">% Val. Add</th>
                        <th class="py-2 text-right">Cost / Ton</th>
                      </tr>
                    </thead>
                    <tbody class="divide-y divide-gray-100">
                      <tr v-for="(item, index) in activeResult.ssItems" :key="index">
                        <td class="py-2 text-gray-600">{{ item.name }}</td>
                        <td class="py-2 text-right">{{ formatWeight(item.unitWeight) }}</td>
                        <td class="py-2 text-right">{{ formatWeight(item.totalWeight) }}</td>
                        <td class="py-2 text-right">{{ formatCurrency(item.unitValueAdd) }}</td>
                        <td class="py-2 text-right font-medium" :class="item.totalValueAdd >= 0 ? 'text-green-600' : 'text-red-600'">
                          {{ formatCurrency(item.totalValueAdd) }}
                        </td>
                        <td class="py-2 text-right">{{ formatCurrency(item.totalSales) }}</td>
                        <td class="py-2 text-right text-purple-600">{{ formatPercent(item.percentValueAdd) }}%</td>
                        <td class="py-2 text-right font-mono text-xs">{{ formatCurrency(item.profitPerTon) }}</td>
                      </tr>
                      <tr class="bg-emerald-50 font-bold text-emerald-900">
                        <td class="py-2 pl-2 rounded-l-md">TOTAL</td>
                        <td class="py-2 text-right">-</td>
                        <td class="py-2 text-right">{{ formatWeight(activeResult.ssTotalWeight) }}</td>
                        <td class="py-2 text-right">-</td>
                        <td class="py-2 text-right" :class="activeResult.ssTotalValueAdd >= 0 ? 'text-green-600' : 'text-red-600'">
                          {{ formatCurrency(activeResult.ssTotalValueAdd) }}
                        </td>
                        <td class="py-2 text-right">{{ formatCurrency(activeResult.ssTotalSales) }}</td>
                        <td class="py-2 text-right text-purple-600">{{ formatPercent(activeResult.ssPercentValueAdd) }}%</td>
                        <td class="py-2 text-right pr-2 rounded-r-md">{{ formatCurrency(activeResult.ssOverallProfitPerTon) }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                
                <div class="grid grid-cols-2 gap-4">
                   <div class="bg-gray-50 p-3 rounded-lg border border-gray-100">
                    <p class="text-xs text-gray-500 font-semibold uppercase">Total Sales</p>
                    <p class="text-lg font-bold text-gray-800">{{ formatCurrency(activeResult.ssTotalSales) }}</p>
                  </div>
                  <div class="bg-gray-50 p-3 rounded-lg border border-gray-100">
                    <p class="text-xs text-gray-500 font-semibold uppercase">Total Value Add</p>
                    <p class="text-lg font-bold" :class="activeResult.ssTotalValueAdd >= 0 ? 'text-green-600' : 'text-red-600'">
                      {{ formatCurrency(activeResult.ssTotalValueAdd) }}
                    </p>
                  </div>
                </div>
              </div>
            </div>

          </div>
        </div>
      </div>
      
      <!-- OVERALL SUMMARY SECTION -->
      <div id="project-summary" class="mt-12 bg-white rounded-2xl shadow-xl overflow-hidden border border-gray-200">
        <div class="p-6 border-b border-gray-200 bg-gray-50">
          <h2 class="text-2xl font-bold text-gray-800 flex items-center">
            <span class="bg-indigo-100 text-indigo-600 p-2 rounded-lg mr-3">📊</span> Overall Project Summary
          </h2>
        </div>
        
        <div class="p-8 grid grid-cols-1 lg:grid-cols-2 gap-8">
          <!-- Table View -->
          <div class="overflow-x-auto">
            <h3 class="text-lg font-semibold text-gray-700 mb-4">Detailed Breakdown</h3>
            <table class="w-full text-left border-collapse">
              <thead>
                <tr class="bg-gray-100 text-gray-600 uppercase text-sm tracking-wider">
                  <th class="p-3 border-b">Panel</th>
                  <th class="p-3 border-b text-right">Weight (T)</th>
                  <th class="p-3 border-b text-right">Sales</th>
                  <th class="p-3 border-b text-right">Margin</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-100">
                <tr v-for="type in ['ROOF', 'WALL', 'CONCEALED', 'SS']" :key="type" class="hover:bg-gray-50">
                  <td class="p-3 font-medium text-gray-800">{{ type }}</td>
                  <td class="p-3 text-right text-gray-600">{{ formatWeight(getPanelTotalWeight(type)) }}</td>
                  <td class="p-3 text-right font-medium text-gray-800">{{ formatCurrency(getPanelTotalSales(type)) }}</td>
                  <td class="p-3 text-right text-purple-600">{{ formatPercent(getPanelMargin(type)) }}%</td>
                </tr>
                <!-- Grand Totals -->
                <tr class="bg-gray-900 text-white font-bold">
                  <td class="p-3">TOTAL</td>
                  <td class="p-3 text-right">{{ formatWeight(grandTotalWeight) }}</td>
                  <td class="p-3 text-right text-yellow-400">{{ formatCurrency(grandTotalSales) }}</td>
                  <td class="p-3 text-right text-purple-300">{{ formatPercent(grandTotalMargin) }}%</td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- Bar Chart View -->
          <div>
            <h3 class="text-lg font-semibold text-gray-700 mb-4">Performance Visualization</h3>
            <div class="space-y-6">
              <div v-for="type in ['ROOF', 'WALL', 'CONCEALED', 'SS']" :key="'chart-'+type" class="space-y-2">
                <div class="flex justify-between text-sm font-medium">
                  <span class="text-gray-700">{{ type }} Panel</span>
                  <span class="text-gray-500">{{ formatCurrency(getPanelTotalSales(type)) }}</span>
                </div>
                <div class="h-4 bg-gray-100 rounded-full overflow-hidden flex">
                  <!-- Cost Bar -->
                  <div 
                    class="h-full bg-red-400" 
                    :style="{ width: (getPanelTotalCost(type) / grandTotalSales * 100) + '%' }"
                    title="Cost"
                  ></div>
                  <!-- Profit Bar -->
                  <div 
                    class="h-full bg-green-500" 
                    :style="{ width: (getPanelValueAdd(type) / grandTotalSales * 100) + '%' }"
                    title="Profit"
                  ></div>
                </div>
                <div class="flex justify-between text-xs text-gray-400">
                  <span>Cost: {{ formatCurrency(getPanelTotalCost(type)) }}</span>
                  <span class="text-green-600 font-bold">Profit: {{ formatCurrency(getPanelValueAdd(type)) }}</span>
                </div>
              </div>
              
              <!-- Total Summary Bar -->
              <div class="pt-4 border-t border-gray-200 mt-4">
                <div class="flex justify-between text-sm font-bold mb-2">
                  <span class="text-gray-900">TOTAL PROJECT</span>
                  <span class="text-indigo-600">{{ formatCurrency(grandTotalSales) }}</span>
                </div>
                <div class="h-6 bg-gray-100 rounded-full overflow-hidden flex shadow-inner">
                  <div class="h-full bg-red-500 flex items-center justify-center text-[10px] text-white font-bold" :style="{ width: (grandTotalCost / grandTotalSales * 100) + '%' }">
                    COST
                  </div>
                  <div class="h-full bg-green-500 flex items-center justify-center text-[10px] text-white font-bold" :style="{ width: (grandTotalValueAdd / grandTotalSales * 100) + '%' }">
                    PROFIT
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
          


      <!-- Footer -->
      <div class="text-center mt-12 text-gray-500 text-sm">
        <p class="mb-1">
          <a href="https://emaarllc.com/" target="_blank" class="hover:text-blue-600 hover:underline">Emaar Industries</a>
        </p>
        <p>© 2025 Panel Cost Calculator | Developed by <a href="https://www.codemub.com" target="_blank" class="text-blue-500 hover:underline">CodeMub</a></p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, watchEffect, ref, watch, computed } from 'vue'
import SSProducts from './SSProducts.vue'

const ssProductsRef = ref(null)
const trigger = ref(0)
const showSummary = ref(false)
const currentPanelType = ref('ROOF')
const showCoreConstant = ref(false)

const handleSSUpdate = () => {
  trigger.value++
}

const scrollToSummary = () => {
  const element = document.getElementById('project-summary')
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}


// Default Data Factory
const createDefaultData = () => ({
  quoteNo: 'MAHFAR TRADING',
  date: new Date().toISOString().split('T')[0],
  color: '9002',
  
  topSkinMaterialType: 'PPAL',
  topSkinCostPerTon: 1400,
  bottomSkinMaterialType: 'PPAL',
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
  
  dCost: 0.1,
  labour: 0.2,
  overHeadCost: 0.1,
  lc: 0.05,
  packingCost: 0.05,
  duty: 0.1
})

const createDefaultResult = () => ({
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
  cost: 0,
  totalCost: 0,
  
  // Additional Cost Values
  dCostVal: 0,
  labourVal: 0,
  overheadVal: 0,
  lcVal: 0,
  packingVal: 0,
  dutyVal: 0,
  
  quotedPrice: 0, // Calculated field
  
  // SS Specific
  ssItems: [],
  ssTotalWeight: 0,
  ssTotalMaterialCost: 0,
  ssTotalValueAdd: 0,
  ssPercentValueAdd: 0,
  ssTotalSales: 0
})

// Multi-Panel State
const panels = reactive({
  ROOF: { inputs: createDefaultData(), results: createDefaultResult() },
  WALL: { inputs: createDefaultData(), results: createDefaultResult() },
  CONCEALED: { inputs: createDefaultData(), results: createDefaultResult() },
  SS: { inputs: createDefaultData(), results: createDefaultResult() }
})

// Active State Helpers
const activeData = computed(() => panels[currentPanelType.value].inputs)
const activeResult = computed(() => panels[currentPanelType.value].results)

// Helper function to calculate RW cost based on thickness
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

// --- WATCHERS FOR ACTIVE PANEL ---
// We need to watch the *active* panel's inputs. 
// Since activeData is a computed ref returning a reactive object, deep watching it works.

watch(() => [activeData.value.topSkinMaterialType, activeData.value.topSkinThickness], () => {
  if (activeData.value.topSkinMaterialType === 'RW') {
    activeData.value.topSkinCostPerTon = calculateRWCost(activeData.value.topSkinThickness)
  }
})

watch(() => [activeData.value.bottomSkinMaterialType, activeData.value.bottomSkinThickness], () => {
  if (activeData.value.bottomSkinMaterialType === 'RW') {
    activeData.value.bottomSkinCostPerTon = calculateRWCost(activeData.value.bottomSkinThickness)
  }
})

watch(() => activeData.value.topSkinMaterialType, () => {
  activeData.value.topSkinDensity = getDensityByMaterial(activeData.value.topSkinMaterialType)
})

watch(() => activeData.value.bottomSkinMaterialType, () => {
  activeData.value.bottomSkinDensity = getDensityByMaterial(activeData.value.bottomSkinMaterialType)
})

watch(() => activeData.value.coreMaterialType, (newVal) => {
  if (newVal === 'RW') {
    activeData.value.coreCostPerTon = 0
  }
})

// --- REACTIVE CALCULATION (Runs for ALL panels or just active? Better to run for active to save perf, but need all for summary)
// Actually, we can run calculation for the *active* panel in a watchEffect. 
// For the summary, we can either calculate all on demand or keep them reactive.
// Given the low number of panels (4), we can just use a single watchEffect that iterates or just watch active.
// But wait, if I switch tabs, I want the previous tab's calculation to stay.
// So I should probably have a calculation function that takes inputs and returns results, and run it for the active panel.
// OR, simply have a watcher for EACH panel type.

// Let's create a reusable calculation function
const calculatePanelCost = (inputs, results, isSS = false) => {
  if (isSS) {
     // SS logic is handled via the component event for now, but we need to store it in results
     // The handleSSUpdate triggers the watchEffect, which pulls from ssProductsRef
     // This is tricky because ssProductsRef is only valid when SS is active/mounted.
     // But since we use v-show, it is mounted.
     if (ssProductsRef.value) {
        const ssData = ssProductsRef.value
        results.ssItems = ssData.products.map(p => {
          const unitWeight = ssData.calculateWeight(p)
          const totalWeightTons = (unitWeight * p.qty) / 1000
          const unitValueAdd = ssData.calculateValueAddition(p)
          const totalValueAdd = unitValueAdd * p.qty
          const totalSales = p.quotedPrice * p.qty
          const percentValueAdd = totalSales > 0 ? (totalValueAdd / totalSales) * 100 : 0
          const profitPerTon = totalWeightTons > 0 ? totalValueAdd / totalWeightTons : 0

          return {
            name: p.name,
            unitWeight: unitWeight,
            totalWeight: totalWeightTons,
            unitValueAdd: unitValueAdd,
            totalValueAdd: totalValueAdd,
            totalSales: totalSales,
            percentValueAdd: percentValueAdd,
            profitPerTon: profitPerTon
          }
        })
        results.ssTotalWeight = ssData.totalWeight
        results.ssTotalMaterialCost = ssData.totalMaterialCost
        results.ssTotalValueAdd = ssData.totalValueAdd
        results.ssPercentValueAdd = ssData.percentValueAdd
        results.ssTotalSales = ssData.totalSales
        results.ssOverallProfitPerTon = ssData.totalWeight > 0 ? ssData.totalValueAdd / ssData.totalWeight : 0

     }
     return
  }

  // Standard Calculation
  results.topSkinWeight = inputs.topSkinThickness * inputs.topSkinGirth / 1000 * inputs.topSkinLength / 1000 * inputs.topSkinDensity / 1000
  results.topSkinTotalWeight = results.topSkinWeight * inputs.quantity / 1000
  
  const topSkinCost = getMaterialCost(inputs.topSkinMaterialType, inputs.topSkinThickness, inputs.topSkinCostPerTon)
  results.topSkinMaterialCost = (topSkinCost / 1000) * results.topSkinWeight

  results.bottomSkinWeight = inputs.bottomSkinThickness * inputs.bottomSkinGirth / 1000 * inputs.bottomSkinLength / 1000 * inputs.bottomSkinDensity / 1000
  results.bottomSkinTotalWeight = results.bottomSkinWeight * inputs.quantity / 1000
  
  const bottomSkinCost = getMaterialCost(inputs.bottomSkinMaterialType, inputs.bottomSkinThickness, inputs.bottomSkinCostPerTon)
  results.bottomSkinMaterialCost = (bottomSkinCost / 1000) * results.bottomSkinWeight

  results.coreWeight = inputs.coreThickness / 1000 * inputs.coreGirth / 1000 * inputs.coreLength / 1000 * inputs.coreDensity + inputs.coreConstant
  results.coreTotalWeight = results.coreWeight * inputs.quantity / 1000
  
  if (inputs.coreMaterialType === 'RW') {
    // RW Special Logic: Cost is directly based on thickness (e.g., 50mm -> 3)
    // Formula: Thickness * 0.06
    results.coreMaterialCost = inputs.coreThickness * 0.06
  } else {
    // Standard Logic: (CostPerTon / 1000) * Weight
    const coreCost = getMaterialCost(inputs.coreMaterialType, inputs.coreThickness, inputs.coreCostPerTon)
    results.coreMaterialCost = (coreCost / 1000) * results.coreWeight
  }

  results.totalWeight = results.topSkinWeight + results.bottomSkinWeight + results.coreWeight
  results.totalMaterialCost = results.topSkinMaterialCost + results.bottomSkinMaterialCost + results.coreMaterialCost

  results.dCostVal = inputs.dCost
  results.labourVal = inputs.labour

  results.cost = results.totalMaterialCost + results.dCostVal + results.labourVal
  
  results.overheadVal = results.cost * inputs.overHeadCost
  results.lcVal = results.cost * inputs.lc
  results.packingVal = results.cost * inputs.packingCost
  results.dutyVal = results.cost * inputs.duty

  results.totalCost = results.cost + results.overheadVal + results.lcVal + results.packingVal + results.dutyVal
  results.costPerUnit = results.totalCost

  // Auto-calculate Quoted Price
  results.quotedPrice = Math.round(results.totalCost)
  
  results.valueAddition = results.quotedPrice - results.cost
  results.valueAddTotal = results.valueAddition * inputs.quantity
  results.totalSales = results.quotedPrice * inputs.quantity
  results.percentValueAdd = results.totalSales > 0 ? (results.valueAddTotal / results.totalSales) * 100 : 0
  
  results.calculated = true
}

// Watcher for Active Panel Calculation
watchEffect(() => {
  trigger.value // dependency for SS
  // Always calculate the current active panel
  calculatePanelCost(activeData.value, activeResult.value, currentPanelType.value === 'SS')
})

// Watch for SS updates specifically
watch(trigger, () => {
  calculatePanelCost(null, panels.SS.results, true)
})

// Summary Helpers
const getPanelTotalWeight = (type) => {
  if (type === 'SS') return panels.SS.results.ssTotalWeight
  return panels[type].results.totalWeight * panels[type].inputs.quantity / 1000 // Convert unit weight to total tons? 
  // Wait, result.totalWeight is per unit? 
  // In original code: result.totalWeight = top + bottom + core (per unit)
  // But result.topSkinTotalWeight = topSkinWeight * quantity / 1000 (Tons)
  // So for summary we need Total Weight in Tons.
  // Let's verify: result.topSkinTotalWeight is in Tons.
  // So we should sum the TotalWeights.
  // result.totalWeight is just sum of unit weights.
  // We need a totalWeightTons field or calculate it.
  // Let's calculate it on the fly:
  // (unitWeight * qty) / 1000
  return (panels[type].results.totalWeight * panels[type].inputs.quantity) / 1000
}

const props = defineProps({
  // No props needed as we manage state internally, 
  // but keeping this if we need to accept initial data later
})
const getPanelTotalCost = (type) => {
  if (type === 'SS') return panels.SS.results.ssTotalMaterialCost // This is just material cost? 
  // SS doesn't have overhead/labour in the current component?
  // The SS component calculates "Mat. Cost". 
  // The user prompt says "shows the summary of price and weights".
  // For standard panels, Total Cost is `results.totalCost * quantity`? No, `totalCost` is per unit?
  // `results.totalCost` = cost + overhead... (per unit)
  // So Total Project Cost = totalCost * quantity.
  return panels[type].results.totalCost * panels[type].inputs.quantity
}

const getPanelTotalSales = (type) => {
  if (type === 'SS') return panels.SS.results.ssTotalSales
  return panels[type].results.totalSales
}

const getPanelValueAdd = (type) => {
  if (type === 'SS') return panels.SS.results.ssTotalValueAdd
  return panels[type].results.valueAddTotal
}

const getPanelMargin = (type) => {
  if (type === 'SS') return panels.SS.results.ssPercentValueAdd
  return panels[type].results.percentValueAdd
}

// Grand Totals
const grandTotalWeight = computed(() => {
  return ['ROOF', 'WALL', 'CONCEALED', 'SS'].reduce((sum, type) => sum + getPanelTotalWeight(type), 0)
})

const grandTotalCost = computed(() => {
  return ['ROOF', 'WALL', 'CONCEALED', 'SS'].reduce((sum, type) => sum + getPanelTotalCost(type), 0)
})

const grandTotalSales = computed(() => {
  return ['ROOF', 'WALL', 'CONCEALED', 'SS'].reduce((sum, type) => sum + getPanelTotalSales(type), 0)
})

const grandTotalValueAdd = computed(() => {
  return ['ROOF', 'WALL', 'CONCEALED', 'SS'].reduce((sum, type) => sum + getPanelValueAdd(type), 0)
})

const grandTotalMargin = computed(() => {
  return grandTotalSales.value > 0 ? (grandTotalValueAdd.value / grandTotalSales.value) * 100 : 0
})


const formatCurrency = (value) => {
  return new Intl.NumberFormat('en-IN', { minimumFractionDigits: 2, maximumFractionDigits: 2 }).format(value || 0)
}

const formatWeight = (value) => {
  return new Intl.NumberFormat('en-IN', { minimumFractionDigits: 4, maximumFractionDigits: 4 }).format(value || 0)
}

const formatPercent = (value) => {
  return new Intl.NumberFormat('en-IN', { minimumFractionDigits: 2, maximumFractionDigits: 2 }).format(value || 0)
}

const logDetailedCalculations = () => {
  const type = currentPanelType.value || activeTab.value
  const separator = '═══════════════════════════════════════════════════════════'
  
  console.log(`%c🧮 SANDWICH PANEL COST CALCULATION - DETAILED LOG`, 'font-weight: bold; font-size: 14px; color: #10b981;')
  console.log(separator)

  if (type === 'SS') {
    if (ssProductsRef.value) {
      const ssData = ssProductsRef.value
      console.log('\n📋 INPUT VALUES:')
      console.log(`Panel Type: SS`)
      console.log(`Total Products: ${ssData.products.length}`)
      
      ssData.products.forEach((p, index) => {
        console.log(`\n━━━ PRODUCT ${index + 1}: ${p.name} ━━━`)
        console.log('Inputs:')
        console.log(`  Material: ${p.material}`)
        console.log(`  Thickness: ${p.thickness} mm`)
        console.log(`  Girth: ${p.girth} mm`)
        console.log(`  Length: ${p.length} mm`)
        console.log(`  Density: ${p.density} kg/m³`)
        console.log(`  Qty: ${p.qty}`)
        console.log(`  Cost/Ton: ₹${p.costPerTon}`)
        console.log(`  Quoted Price: ₹${p.quotedPrice}`)

        const weight = ssData.calculateWeight(p)
        const matCost = ssData.calculateMaterialCost(p)
        const valAdd = p.quotedPrice - matCost

        console.log('\n📐 Weight Calculation:')
        console.log(`  Formula: (Thickness × Girth × Length × Density) / 1,000,000,000`)
        console.log(`  = (${p.thickness} × ${p.girth} × ${p.length} × ${p.density}) / 1,000,000,000`)
        console.log(`  = ${weight.toFixed(6)} tons (per unit)`)
        console.log(`  Total Weight: ${(weight * p.qty / 1000).toFixed(4)} tons`)

        console.log('\n💰 Material Cost:')
        console.log(`  Formula: (Cost/Ton / 1000 × Weight) + 0.1`)
        console.log(`  = (${p.costPerTon} / 1000 × ${weight.toFixed(6)}) + 0.1`)
        console.log(`  = ₹${matCost.toFixed(6)} (per unit)`)
        console.log(`  Total Cost: ₹${(matCost * p.qty).toFixed(2)}`)

        console.log('\n💎 Value Addition:')
        console.log(`  Formula: Quoted Price - Material Cost`)
        console.log(`  = ${p.quotedPrice} - ${matCost.toFixed(6)}`)
        console.log(`  = ₹${valAdd.toFixed(3)}`)
      })

      console.log(`\n━━━ SS SUMMARY ━━━`)
      console.log(`Total Weight: ${ssData.totalWeight.toFixed(3)} tons`)
      console.log(`Total Material Cost: ₹${ssData.totalMaterialCost.toFixed(2)}`)
      console.log(`Total Sales: ₹${ssData.totalSales.toFixed(2)}`)
      console.log(`Total Value Add: ₹${ssData.totalValueAdd.toFixed(2)}`)
      console.log(`Percent Value Add: ${ssData.percentValueAdd.toFixed(2)}%`)

    } else {
      console.warn('SS Products reference not found.')
    }
  } else {
    const inputs = panels[type].inputs
    const results = panels[type].results
    
    console.log(`\n📋 INPUT VALUES:`)
    console.log(`Quote: ${props.quote || 'N/A'}`)
    console.log(`Date: ${props.date || 'N/A'}`)
    console.log(`Material: ${props.material || 'N/A'}`)
    console.log(`Panel Type: ${type}`)
    console.log(`Color: ${props.color || 'N/A'}`)
    console.log(`Quantity: ${inputs.quantity}`)
    console.log(`Quoted Price: ₹${inputs.quotedPrice}/unit`)

    // Top Skin
    console.log('\n━━━ TOP SKIN CALCULATIONS ━━━')
    console.log('Inputs:')
    console.log(`  Thickness: ${inputs.topSkinThickness} mm`)
    console.log(`  Girth: ${inputs.topSkinGirth} mm`)
    console.log(`  Length: ${inputs.topSkinLength} mm`)
    console.log(`  Density: ${inputs.topSkinDensity} kg/m³`)
    console.log(`  Cost/Ton: ₹${inputs.topSkinCostPerTon}`)
    
    console.log('\n📐 Weight Calculation:')
    console.log(`  Formula: (Thickness × Girth × Length × Density) / 1,000,000,000`)
    console.log(`  = (${inputs.topSkinThickness} × ${inputs.topSkinGirth} × ${inputs.topSkinLength} × ${inputs.topSkinDensity}) / 1,000,000,000`)
    console.log(`  = ${(results.topSkinWeight || 0).toFixed(6)} tons`)
    
    console.log('\n💰 Material Cost:')
    console.log(`  Formula: (Cost/Ton / 1000) × Weight`)
    console.log(`  = (${inputs.topSkinCostPerTon} / 1000) × ${(results.topSkinWeight || 0).toFixed(6)}`)
    console.log(`  = ₹${(results.topSkinMaterialCost || 0).toFixed(6)}`)

    // Bottom Skin
    console.log('\n━━━ BOTTOM SKIN CALCULATIONS ━━━')
    console.log('Inputs:')
    console.log(`  Thickness: ${inputs.bottomSkinThickness} mm`)
    console.log(`  Girth: ${inputs.bottomSkinGirth} mm`)
    console.log(`  Length: ${inputs.bottomSkinLength} mm`)
    console.log(`  Density: ${inputs.bottomSkinDensity} kg/m³`)
    console.log(`  Cost/Ton: ₹${inputs.bottomSkinCostPerTon}`)
    
    console.log('\n📐 Weight Calculation:')
    console.log(`  Formula: (Thickness × Girth × Length × Density) / 1,000,000,000`)
    console.log(`  = (${inputs.bottomSkinThickness} × ${inputs.bottomSkinGirth} × ${inputs.bottomSkinLength} × ${inputs.bottomSkinDensity}) / 1,000,000,000`)
    console.log(`  = ${(results.bottomSkinWeight || 0).toFixed(6)} tons`)
    
    console.log('\n💰 Material Cost:')
    console.log(`  Formula: (Cost/Ton / 1000) × Weight`)
    console.log(`  = (${inputs.bottomSkinCostPerTon} / 1000) × ${(results.bottomSkinWeight || 0).toFixed(6)}`)
    console.log(`  = ₹${(results.bottomSkinMaterialCost || 0).toFixed(6)}`)

    // Core
    console.log('\n━━━ CORE CALCULATIONS ━━━')
    console.log('Inputs:')
    console.log(`  Thickness: ${inputs.coreThickness} mm`)
    console.log(`  Girth: ${inputs.coreGirth} mm`)
    console.log(`  Length: ${inputs.coreLength} mm`)
    console.log(`  Density: ${inputs.coreDensity} kg/m³`)
    console.log(`  Core Constant: ${showCoreConstant.value ? inputs.coreConstant : 'Hidden'}`)
    console.log(`  Cost/Ton: ₹${inputs.coreCostPerTon}`)
    
    console.log('\n📐 Weight Calculation:')
    console.log(`  Formula: (Thickness / 1000) × (Girth / 1000) × (Length / 1000) × Density + Core Constant`)
    console.log(`  = (${inputs.coreThickness} / 1000) × (${inputs.coreGirth} / 1000) × (${inputs.coreLength} / 1000) × ${inputs.coreDensity} + ${inputs.coreConstant}`)
    console.log(`  = ${(results.coreWeight || 0).toFixed(6)} tons`)
    
    console.log('\n💰 Material Cost:')
    console.log(`  Formula: (Cost/Ton / 1000) × Weight`)
    console.log(`  = (${inputs.coreCostPerTon} / 1000) × ${(results.coreWeight || 0).toFixed(6)}`)
    console.log(`  = ₹${(results.coreMaterialCost || 0).toFixed(6)}`)

    // Totals
    console.log('\n━━━ TOTALS ━━━')
    console.log('\n⚖️ Total Weight:')
    console.log(`  Formula: Top Skin + Bottom Skin + Core`)
    console.log(`  = ${(results.topSkinWeight || 0).toFixed(6)} + ${(results.bottomSkinWeight || 0).toFixed(6)} + ${(results.coreWeight || 0).toFixed(6)}`)
    console.log(`  = ${(results.totalWeight || 0).toFixed(6)} tons`)
    
    console.log('\n💵 Total Material Cost:')
    console.log(`  Formula: Top Skin + Bottom Skin + Core`)
    console.log(`  = ${(results.topSkinMaterialCost || 0).toFixed(6)} + ${(results.bottomSkinMaterialCost || 0).toFixed(6)} + ${(results.coreMaterialCost || 0).toFixed(6)}`)
    console.log(`  = ₹${(results.totalMaterialCost || 0).toFixed(6)}`)

    // Cost Calculation
    console.log('\n━━━ COST CALCULATION ━━━')
    console.log(`\n💰 D-Cost: ${inputs.dCost}`)
    console.log(`👷 Labour: ${inputs.labour}`)
    
    console.log('\n📊 Cost (Subtotal):')
    console.log(`  Formula: Total Material Cost + D-Cost + Labour`)
    console.log(`  = ${(results.totalMaterialCost || 0).toFixed(3)} + ${inputs.dCost} + ${inputs.labour}`)
    console.log(`  = ₹${(results.totalCost || 0).toFixed(3)}`)

    // Value Addition
    console.log('\n━━━ VALUE ADDITION ANALYSIS ━━━')
    console.log('\n💎 Value Addition:')
    console.log(`  Formula: Quoted Price - Cost`)
    console.log(`  = ${inputs.quotedPrice} - ${(results.totalCost || 0).toFixed(3)}`)
    console.log(`  = ₹${(results.valueAddition || 0).toFixed(3)}`)
    console.log(`  ${(results.valueAddition || 0) >= 0 ? '✅ PROFIT' : '❌ LOSS'}`)

    console.log('\n💰 Value Add Total:')
    console.log(`  Formula: Value Addition × Quantity`)
    console.log(`  = ${(results.valueAddition || 0).toFixed(3)} × ${inputs.quantity}`)
    console.log(`  = ₹${(results.valueAddTotal || 0).toFixed(2)}`)

    console.log('\n📊 Total Sales:')
    console.log(`  Formula: Quoted Price × Quantity`)
    console.log(`  = ${inputs.quotedPrice} × ${inputs.quantity}`)
    console.log(`  = ₹${(results.totalSales || 0).toFixed(2)}`)

    console.log('\n📈 % of Value Add:')
    console.log(`  Formula: (Value Add Total / Total Sales) × 100`)
    console.log(`  = ${(results.valueAddTotal || 0).toFixed(2)} / ${(results.totalSales || 0).toFixed(2)} × 100`)
    console.log(`  = ${(results.percentValueAdd || 0).toFixed(2)}%`)
    
    console.log(`\n🏭 Overhead: ${inputs.overHeadCost}`)
    console.log(`📦 Lc: ${inputs.lc}`)
    
    const finalCost = (results.totalCost || 0) + (inputs.overHeadCost || 0) + (inputs.lc || 0)
    console.log('\n💵 Total Cost (Final):')
    console.log(`  Formula: Cost + Overhead + Lc`)
    console.log(`  = ${(results.totalCost || 0).toFixed(3)} + ${inputs.overHeadCost} + ${inputs.lc}`)
    console.log(`  = ₹${finalCost.toFixed(3)}`)
    
    console.log('\n🎯 Cost per Unit:')
    console.log(`  Formula: Total Cost / Quantity`)
    console.log(`  = ${finalCost.toFixed(3)} / ${inputs.quantity}`)
    console.log(`  = ${(finalCost / (inputs.quantity || 1)).toFixed(3)} per unit`)
  }
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
