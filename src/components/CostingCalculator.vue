<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 py-8 px-4 sm:px-6 lg:px-8">
    <div class="max-w-7xl mx-auto">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-gray-900 mb-2">Sandwich Panel Costing Calculator</h1>
        <p class="text-lg text-gray-600">Calculate manufacturing costs for sandwich panels</p>
      </div>

      <!-- Main Form -->
      <div class="bg-white rounded-2xl shadow-xl p-6 md:p-8">
        <form @submit.prevent="calculateCost">
          <!-- Section 1: Quotation Info -->
          <div class="mb-8">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-blue-500">
              📋 Quotation Information
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Quote No</label>
                <input
                  v-model="formData.quoteNo"
                  type="text"
                  placeholder="EMAAR/QUO/2025/104"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Customer Name</label>
                <input
                  v-model="formData.customerName"
                  type="text"
                  placeholder="Enter customer name"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Date</label>
                <input
                  v-model="formData.date"
                  type="date"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Delivery/Sales Info</label>
                <input
                  v-model="formData.deliveryInfo"
                  type="text"
                  placeholder="Optional"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
            </div>
          </div>

          <!-- Section 2: Material Rates -->
          <div class="mb-8">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-yellow-500">
              💰 Material Rates (Cost per Ton)
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 bg-yellow-50 p-4 rounded-lg">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Top Skin Cost/Ton (₹)</label>
                <input
                  v-model.number="formData.topSkinCostPerTon"
                  type="number"
                  step="0.01"
                  class="w-full px-4 py-2 border border-yellow-300 rounded-lg focus:ring-2 focus:ring-yellow-500 focus:border-transparent bg-white"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Bottom Skin Cost/Ton (₹)</label>
                <input
                  v-model.number="formData.bottomSkinCostPerTon"
                  type="number"
                  step="0.01"
                  class="w-full px-4 py-2 border border-yellow-300 rounded-lg focus:ring-2 focus:ring-yellow-500 focus:border-transparent bg-white"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Core Cost/Ton (₹)</label>
                <input
                  v-model.number="formData.coreCostPerTon"
                  type="number"
                  step="0.01"
                  class="w-full px-4 py-2 border border-yellow-300 rounded-lg focus:ring-2 focus:ring-yellow-500 focus:border-transparent bg-white"
                />
              </div>
            </div>
          </div>

          <!-- Section 3: Panel Specifications -->
          <div class="mb-8">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-green-500">
              🏗️ Panel Specifications
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Material Type</label>
                <select
                  v-model="formData.materialType"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option value="PPAL">PPAL</option>
                  <option value="PIR">PIR</option>
                  <option value="PPGI">PPGI</option>
                  <option value="GI">GI</option>
                  <option value="TRADING">TRADING</option>
                  <option value="SERVICE">SERVICE</option>
                </select>
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Panel Type</label>
                <select
                  v-model="formData.panelType"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option value="ROOF">ROOF</option>
                  <option value="WALL">WALL</option>
                  <option value="CEILING">CEILING</option>
                  <option value="PARTITION">PARTITION</option>
                </select>
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Color</label>
                <input
                  v-model="formData.color"
                  type="text"
                  placeholder="e.g., 9002"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Core Material</label>
                <select
                  v-model="formData.coreMaterial"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="PU">PU</option>
                  <option value="PIR">PIR</option>
                  <option value="Rockwool">Rockwool</option>
                  <option value="EPS">EPS</option>
                </select>
              </div>
            </div>
          </div>

          <!-- Section 4: Top Skin Details -->
          <div class="mb-8">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-purple-500">
              📐 Top Skin Details
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Thickness (mm)</label>
                <input
                  v-model.number="formData.topSkinThickness"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 0.5"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Girth/Width (mm)</label>
                <input
                  v-model.number="formData.topSkinGirth"
                  type="number"
                  step="1"
                  placeholder="e.g., 1220"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Length (mm)</label>
                <input
                  v-model.number="formData.topSkinLength"
                  type="number"
                  step="1"
                  placeholder="e.g., 1000"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input
                  v-model.number="formData.topSkinDensity"
                  type="number"
                  step="1"
                  placeholder="e.g., 2720"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
            </div>
          </div>

          <!-- Section 5: Bottom Skin Details -->
          <div class="mb-8">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-indigo-500">
              📐 Bottom Skin Details
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Thickness (mm)</label>
                <input
                  v-model.number="formData.bottomSkinThickness"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 0.5"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Girth/Width (mm)</label>
                <input
                  v-model.number="formData.bottomSkinGirth"
                  type="number"
                  step="1"
                  placeholder="e.g., 1080"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Length (mm)</label>
                <input
                  v-model.number="formData.bottomSkinLength"
                  type="number"
                  step="1"
                  placeholder="e.g., 1000"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input
                  v-model.number="formData.bottomSkinDensity"
                  type="number"
                  step="1"
                  placeholder="e.g., 2720"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
            </div>
          </div>

          <!-- Section 6: Core Details -->
          <div class="mb-8">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-pink-500">
              📐 Core Details
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Thickness (mm)</label>
                <input
                  v-model.number="formData.coreThickness"
                  type="number"
                  step="1"
                  placeholder="e.g., 50"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Girth/Width (mm)</label>
                <input
                  v-model.number="formData.coreGirth"
                  type="number"
                  step="1"
                  placeholder="e.g., 1000"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Length (mm)</label>
                <input
                  v-model.number="formData.coreLength"
                  type="number"
                  step="1"
                  placeholder="e.g., 1000"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input
                  v-model.number="formData.coreDensity"
                  type="number"
                  step="1"
                  placeholder="e.g., 40"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Core Constant</label>
                <input
                  v-model.number="formData.coreConstant"
                  type="number"
                  step="0.001"
                  placeholder="0.215"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
            </div>
          </div>

          <!-- Section 7: Quantity & Pricing -->
          <div class="mb-8">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-orange-500">
              💵 Quantity & Pricing
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4">

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Panel Thickness (mm)</label>
                <select
                  v-model.number="formData.panelThickness"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option :value="30">30</option>
                  <option :value="40">40</option>
                  <option :value="50">50</option>
                  <option :value="60">60</option>
                  <option :value="80">80</option>
                  <option :value="100">100</option>
                </select>
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Panel Width (mm)</label>
                <select
                  v-model.number="formData.panelWidth"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option :value="1000">1000</option>
                  <option :value="1050">1050</option>
                  <option :value="1200">1200</option>
                </select>
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Skin Thickness (mm)</label>
                <select
                  v-model.number="formData.skinThickness"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option :value="0.35">0.35</option>
                  <option :value="0.40">0.40</option>
                  <option :value="0.45">0.45</option>
                  <option :value="0.50">0.50</option>
                  <option :value="0.60">0.60</option>
                </select>
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Coating Type</label>
                <select
                  v-model="formData.coatingType"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option value="PE">PE</option>
                  <option value="PVDF">PVDF</option>
                  <option value="SMP">SMP</option>
                </select>
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Panel Type</label>
                <select
                  v-model="formData.panelType"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option value="Roof">Roof</option>
                  <option value="Wall">Wall</option>
                  <option value="Ceiling">Ceiling</option>
                  <option value="Partition">Partition</option>
                </select>
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Density (kg/m³)</label>
                <input
                  v-model.number="formData.density"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 40"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Core Material</label>
                <select
                  v-model="formData.coreMaterial"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option value="PU">PU</option>
                  <option value="PIR">PIR</option>
                  <option value="Rockwool">Rockwool</option>
                  <option value="EPS">EPS</option>
                </select>
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Color</label>
                <input
                  v-model="formData.color"
                  type="text"
                  placeholder="e.g., RAL 9002"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Sheet Grade</label>
                <select
                  v-model="formData.sheetGrade"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option value="AZ150">AZ150</option>
                  <option value="AZ70">AZ70</option>
                  <option value="Z275">Z275</option>
                </select>
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Base Metal</label>
                <select
                  v-model="formData.baseMetal"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="">Select...</option>
                  <option value="Alu-Zinc">Alu-Zinc</option>
                  <option value="Galvanized">Galvanized</option>
                </select>
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Panel Length (m)</label>
                <input
                  v-model.number="formData.panelLength"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 6.0"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Quantity (m²)</label>
                <input
                  v-model.number="formData.quantity"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 1000"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Unit</label>
                <select
                  v-model="formData.unit"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="m²">m²</option>
                  <option value="m">m</option>
                  <option value="nos">nos</option>
                </select>
              </div>
            </div>
          </div>

          <!-- Section 3: Cost Factors -->
          <div class="mb-8">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-purple-500">
              ⚙️ Cost Factors
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Material Rate (₹)</label>
                <input
                  v-model.number="formData.materialRate"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 150"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Labour Cost (%)</label>
                <input
                  v-model.number="formData.labourCost"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 10"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Overhead (%)</label>
                <input
                  v-model.number="formData.overhead"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 5"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Transport Cost (₹)</label>
                <input
                  v-model.number="formData.transportCost"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 5000"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Profit Margin (%)</label>
                <input
                  v-model.number="formData.profitMargin"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 15"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Tax (%)</label>
                <input
                  v-model.number="formData.tax"
                  type="number"
                  step="0.01"
                  placeholder="e.g., 18"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                />
              </div>
            </div>
          </div>

          <!-- Calculate Button -->
          <div class="flex justify-center mb-6">
            <button
              type="submit"
              class="bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700 text-white font-bold py-3 px-8 rounded-lg shadow-lg transform transition hover:scale-105 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
            >
              🧮 Calculate Total Cost
            </button>
          </div>

          <!-- Section 4: Result Output -->
          <div v-if="result.calculated" class="bg-gradient-to-r from-green-50 to-emerald-50 rounded-xl p-6 border-2 border-green-300">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4 pb-2 border-b-2 border-green-500">
              💰 Cost Breakdown
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
              <div class="bg-white rounded-lg p-4 shadow">
                <p class="text-sm text-gray-600 mb-1">Material Cost</p>
                <p class="text-xl font-bold text-gray-900">₹{{ formatCurrency(result.materialCost) }}</p>
              </div>
              <div class="bg-white rounded-lg p-4 shadow">
                <p class="text-sm text-gray-600 mb-1">Labour Cost</p>
                <p class="text-xl font-bold text-gray-900">₹{{ formatCurrency(result.labourCostAmount) }}</p>
              </div>
              <div class="bg-white rounded-lg p-4 shadow">
                <p class="text-sm text-gray-600 mb-1">Overhead</p>
                <p class="text-xl font-bold text-gray-900">₹{{ formatCurrency(result.overheadAmount) }}</p>
              </div>
              <div class="bg-white rounded-lg p-4 shadow">
                <p class="text-sm text-gray-600 mb-1">Transport Cost</p>
                <p class="text-xl font-bold text-gray-900">₹{{ formatCurrency(result.transportCost) }}</p>
              </div>
              <div class="bg-white rounded-lg p-4 shadow">
                <p class="text-sm text-gray-600 mb-1">Subtotal</p>
                <p class="text-xl font-bold text-gray-900">₹{{ formatCurrency(result.subtotal) }}</p>
              </div>
              <div class="bg-white rounded-lg p-4 shadow">
                <p class="text-sm text-gray-600 mb-1">Profit</p>
                <p class="text-xl font-bold text-gray-900">₹{{ formatCurrency(result.profitAmount) }}</p>
              </div>
              <div class="bg-white rounded-lg p-4 shadow">
                <p class="text-sm text-gray-600 mb-1">Tax Amount</p>
                <p class="text-xl font-bold text-gray-900">₹{{ formatCurrency(result.taxAmount) }}</p>
              </div>
              <div class="bg-gradient-to-r from-green-400 to-emerald-500 rounded-lg p-4 shadow-lg md:col-span-2">
                <p class="text-sm text-white mb-1">Total Cost</p>
                <p class="text-3xl font-bold text-white">₹{{ formatCurrency(result.totalCost) }}</p>
              </div>
            </div>
            <div class="mt-4 bg-white rounded-lg p-4 shadow">
              <p class="text-sm text-gray-600 mb-1">Cost per Unit ({{ formData.unit }})</p>
              <p class="text-2xl font-bold text-indigo-600">₹{{ formatCurrency(result.costPerUnit) }}</p>
            </div>
          </div>
        </form>
      </div>

      <!-- Footer -->
      <div class="text-center mt-8 text-gray-600">
        <p class="text-sm">© 2025 Sandwich Panel Manufacturing Calculator</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

// Form data - Based on Excel structure
const formData = reactive({
  // Quotation Info
  quoteNo: '',
  customerName: '',
  date: new Date().toISOString().split('T')[0],
  deliveryInfo: '',
  
  // Material Rates (from Excel N1, N2, N3)
  topSkinCostPerTon: 1400,
  bottomSkinCostPerTon: 400,
  coreCostPerTon: 900,
  
  // Panel Specifications
  materialType: '',
  panelType: '',
  color: '',
  
  // Top Skin Details (Row 6 in Excel)
  topSkinThickness: 0.5,      // G6 - mm
  topSkinGirth: 1220,          // H6 - mm
  topSkinLength: 1000,         // I6 - mm
  topSkinDensity: 2720,        // K6 - kg/m³
  
  // Bottom Skin Details (Row 7 in Excel)
  bottomSkinThickness: 0.5,    // G7 - mm
  bottomSkinGirth: 1080,       // H7 - mm
  bottomSkinLength: 1000,      // I7 - mm
  bottomSkinDensity: 2720,     // K7 - kg/m³
  
  // Core Details (Row 8 in Excel)
  coreThickness: 50,           // G8 - mm
  coreGirth: 1000,             // H8 - mm
  coreLength: 1000,            // I8 - mm
  coreDensity: 40,             // K8 - kg/m³
  coreConstant: 0.215,         // Additional constant in core formula
  
  // Quantity
  quantity: 1000,              // J6 - Quantity in m² or units
  
  // Pricing
  quotedPrice: 7.7,            // P6 - Price per unit
  wastagePercent: 10,          // P12 - Wastage % (default 10%)
  overheadPercent: 5,          // Fixed 5% overhead (O13)
  
  // Additional fields
  unit: 'm²',
  coreMaterial: 'PU',
  coatingType: '',
  sheetGrade: '',
  baseMetal: ''
})

// Result data - Based on Excel columns
const result = reactive({
  calculated: false,
  
  // Top Skin (Row 6)
  topSkinWeight: 0,            // L6
  topSkinTotalWeight: 0,       // M6
  topSkinMaterialCost: 0,      // O6
  
  // Bottom Skin (Row 7)
  bottomSkinWeight: 0,         // L7
  bottomSkinTotalWeight: 0,    // M7
  bottomSkinMaterialCost: 0,   // O7
  
  // Core (Row 8)
  coreWeight: 0,               // L8
  coreTotalWeight: 0,          // M8
  coreMaterialCost: 0,         // O8
  
  // Totals (Row 9)
  totalWeight: 0,              // L9
  
  // Summary (Row 11)
  totalMaterialCost: 0,        // O11
  valueAddition: 0,            // Q11
  valueAddTotal: 0,            // R11
  totalSales: 0,               // S11
  percentValueAdd: 0,          // T11
  
  // Overheads (Row 12, 13)
  wastageCost: 0,              // O12
  overheadCost: 0,             // O13
  
  // Grand Total (Row 14)
  grandTotal: 0,               // O14
  costPerUnit: 0
})

// Calculate cost function - Using exact Excel formulas
const calculateCost = () => {
  // Basic validation
  if (!formData.quantity) {
    alert('Please enter Quantity to calculate.')
    return
  }

  // ========== TOP SKIN CALCULATIONS (Row 6) ==========
  // L6: =G6*H6/1000*I6/1000*K6/1000
  // Weight = (Thickness × Girth × Length × Density) / 1,000,000,000
  result.topSkinWeight = (
    formData.topSkinThickness * 
    formData.topSkinGirth / 1000 * 
    formData.topSkinLength / 1000 * 
    formData.topSkinDensity / 1000
  )
  
  // M6: =L6*J6/1000
  // Total Weight = Weight × Quantity / 1000
  result.topSkinTotalWeight = result.topSkinWeight * formData.quantity / 1000
  
  // O6: =N6/1000*L6
  // Material Cost = (Cost per Ton / 1000) × Weight
  result.topSkinMaterialCost = (formData.topSkinCostPerTon / 1000) * result.topSkinWeight

  // ========== BOTTOM SKIN CALCULATIONS (Row 7) ==========
  // L7: =G7*H7/1000*I7/1000*K7/1000
  result.bottomSkinWeight = (
    formData.bottomSkinThickness * 
    formData.bottomSkinGirth / 1000 * 
    formData.bottomSkinLength / 1000 * 
    formData.bottomSkinDensity / 1000
  )
  
  // M7: =L7*J7/1000
  result.bottomSkinTotalWeight = result.bottomSkinWeight * formData.quantity / 1000
  
  // O7: =N7/1000*L7
  result.bottomSkinMaterialCost = (formData.bottomSkinCostPerTon / 1000) * result.bottomSkinWeight

  // ========== CORE CALCULATIONS (Row 8) ==========
  // L8: =G8/1000*H8/1000*I8/1000*K8+0.215
  // Special formula for core with additional constant
  result.coreWeight = (
    formData.coreThickness / 1000 * 
    formData.coreGirth / 1000 * 
    formData.coreLength / 1000 * 
    formData.coreDensity + 
    formData.coreConstant
  )
  
  // M8: =L8*J8/1000
  result.coreTotalWeight = result.coreWeight * formData.quantity / 1000
  
  // O8: =N8/1000*L8
  result.coreMaterialCost = (formData.coreCostPerTon / 1000) * result.coreWeight

  // ========== TOTAL WEIGHT (Row 9) ==========
  // L9: =SUM(L6:L8)
  result.totalWeight = result.topSkinWeight + result.bottomSkinWeight + result.coreWeight

  // ========== SUMMARY CALCULATIONS (Row 11) ==========
  // O11: =SUM(O6:O10) - Total Material Cost
  result.totalMaterialCost = (
    result.topSkinMaterialCost + 
    result.bottomSkinMaterialCost + 
    result.coreMaterialCost
  )
  
  // Q11: =P6-O11 - Value Addition
  result.valueAddition = formData.quotedPrice - result.totalMaterialCost
  
  // R11: =Q11*J6 - Value Add Total
  result.valueAddTotal = result.valueAddition * formData.quantity
  
  // S11: =P6*J6 - Total Sales
  result.totalSales = formData.quotedPrice * formData.quantity
  
  // T11: =R11/S11% - Percentage of Value Add
  result.percentValueAdd = result.totalSales > 0 
    ? (result.valueAddTotal / result.totalSales) * 100 
    : 0

  // ========== OVERHEADS (Row 12, 13) ==========
  // O12: =P12*O11 - Wastage Cost
  result.wastageCost = (formData.wastagePercent / 100) * result.totalMaterialCost
  
  // O13: =O11*0.05 - Additional Overhead (5%)
  result.overheadCost = result.totalMaterialCost * (formData.overheadPercent / 100)

  // ========== GRAND TOTAL (Row 14) ==========
  // O14: =SUM(O11:O13)
  result.grandTotal = result.totalMaterialCost + result.wastageCost + result.overheadCost
  
  // Cost per Unit
  result.costPerUnit = formData.quantity > 0 ? result.grandTotal / formData.quantity : 0

  // Mark as calculated
  result.calculated = true

  // Scroll to result
  setTimeout(() => {
    const resultElement = document.querySelector('.bg-gradient-to-r.from-green-50')
    if (resultElement) {
      resultElement.scrollIntoView({ behavior: 'smooth', block: 'nearest' })
    }
  }, 100)
}

// Format currency helper
const formatCurrency = (value) => {
  return new Intl.NumberFormat('en-IN', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  }).format(value || 0)
}

// Format weight helper (3 decimal places like Excel)
const formatWeight = (value) => {
  return new Intl.NumberFormat('en-IN', {
    minimumFractionDigits: 3,
    maximumFractionDigits: 3
  }).format(value || 0)
}

// Format percentage helper
const formatPercent = (value) => {
  return new Intl.NumberFormat('en-IN', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  }).format(value || 0)
}
</script>

<style scoped>
/* Additional custom styles if needed */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type=number] {
  -moz-appearance: textfield;
}
</style>
