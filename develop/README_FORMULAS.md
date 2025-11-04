# Sandwich Panel Costing Calculator - Formula Documentation

## ✅ Implementation Complete

The Vue.js calculator now uses **exact Excel formulas** from `Pug-cost.xlsx` (Sheet: REV-03).

---

## 📊 Excel Data Extraction

**Files Generated:**
- `excel_data.json` - Complete structured data (746 cells, 282 formulas)
- `excel_data.txt` - Human-readable format
- `excel_data_formulas.txt` - All 282 formulas extracted
- `EXCEL_FORMULA_ANALYSIS.md` - Detailed formula analysis

**Statistics:**
- **Sheet:** REV-03
- **Total Cells:** 746
- **Formulas:** 282
- **Dimensions:** 78 rows × 24 columns

---

## 🧮 Implemented Formulas

### 1. Top Skin Weight Calculation
**Excel:** `L6 = G6*H6/1000*I6/1000*K6/1000`

**JavaScript:**
```javascript
topSkinWeight = thickness × girth / 1000 × length / 1000 × density / 1000
```

**Example:**
- Thickness: 0.5 mm
- Girth: 1220 mm
- Length: 1000 mm
- Density: 2720 kg/m³
- **Result:** 1.6592 tons

---

### 2. Bottom Skin Weight Calculation
**Excel:** `L7 = G7*H7/1000*I7/1000*K7/1000`

**JavaScript:**
```javascript
bottomSkinWeight = thickness × girth / 1000 × length / 1000 × density / 1000
```

**Example:**
- Thickness: 0.5 mm
- Girth: 1080 mm
- Length: 1000 mm
- Density: 2720 kg/m³
- **Result:** 1.4688 tons

---

### 3. Core Weight Calculation
**Excel:** `L8 = G8/1000*H8/1000*I8/1000*K8+0.215`

**JavaScript:**
```javascript
coreWeight = thickness / 1000 × girth / 1000 × length / 1000 × density + coreConstant
```

**Example:**
- Thickness: 50 mm
- Girth: 1000 mm
- Length: 1000 mm
- Density: 40 kg/m³
- Core Constant: 0.215
- **Result:** 2.215 tons

---

### 4. Total Weight
**Excel:** `L9 = SUM(L6:L8)`

**JavaScript:**
```javascript
totalWeight = topSkinWeight + bottomSkinWeight + coreWeight
```

**Result:** 5.343 tons

---

### 5. Material Cost per Component
**Excel:** `O6 = N6/1000*L6`

**JavaScript:**
```javascript
topSkinMaterialCost = (topSkinCostPerTon / 1000) × topSkinWeight
bottomSkinMaterialCost = (bottomSkinCostPerTon / 1000) × bottomSkinWeight
coreMaterialCost = (coreCostPerTon / 1000) × coreWeight
```

**Example:**
- Top Skin: (1400 / 1000) × 1.6592 = **₹2.32288**
- Bottom Skin: (400 / 1000) × 1.4688 = **₹0.58752**
- Core: (900 / 1000) × 2.215 = **₹1.9935**

---

### 6. Total Material Cost
**Excel:** `O11 = SUM(O6:O10)`

**JavaScript:**
```javascript
totalMaterialCost = topSkinMaterialCost + bottomSkinMaterialCost + coreMaterialCost
```

**Result:** ₹4.90390

---

### 7. Value Addition
**Excel:** `Q11 = P6-O11`

**JavaScript:**
```javascript
valueAddition = quotedPrice - totalMaterialCost
```

**Example:**
- Quoted Price: ₹7.7
- Total Material Cost: ₹4.90390
- **Value Addition:** ₹2.79610

---

### 8. Value Add Total
**Excel:** `R11 = Q11*J6`

**JavaScript:**
```javascript
valueAddTotal = valueAddition × quantity
```

**Example:**
- Value Addition: ₹2.79610
- Quantity: 1000
- **Value Add Total:** ₹2,796.10

---

### 9. Total Sales
**Excel:** `S11 = P6*J6`

**JavaScript:**
```javascript
totalSales = quotedPrice × quantity
```

**Example:**
- Quoted Price: ₹7.7
- Quantity: 1000
- **Total Sales:** ₹7,700

---

### 10. Percentage of Value Add
**Excel:** `T11 = R11/S11%`

**JavaScript:**
```javascript
percentValueAdd = (valueAddTotal / totalSales) × 100
```

**Example:**
- Value Add Total: ₹2,796.10
- Total Sales: ₹7,700
- **% Value Add:** 36.31%

---

### 11. Wastage Cost
**Excel:** `O12 = P12*O11`

**JavaScript:**
```javascript
wastageCost = (wastagePercent / 100) × totalMaterialCost
```

**Example:**
- Wastage: 10%
- Total Material Cost: ₹4.90390
- **Wastage Cost:** ₹0.49039

---

### 12. Overhead Cost
**Excel:** `O13 = O11*0.05`

**JavaScript:**
```javascript
overheadCost = totalMaterialCost × (overheadPercent / 100)
```

**Example:**
- Overhead: 5%
- Total Material Cost: ₹4.90390
- **Overhead Cost:** ₹0.24520

---

### 13. Grand Total
**Excel:** `O14 = SUM(O11:O13)`

**JavaScript:**
```javascript
grandTotal = totalMaterialCost + wastageCost + overheadCost
```

**Example:**
- Total Material Cost: ₹4.90390
- Wastage Cost: ₹0.49039
- Overhead Cost: ₹0.24520
- **Grand Total:** ₹5.63949

---

### 14. Cost per Unit
**JavaScript:**
```javascript
costPerUnit = grandTotal / quantity
```

**Example:**
- Grand Total: ₹5.63949
- Quantity: 1000
- **Cost per Unit:** ₹0.00564

---

## 📋 Input Fields

### Quotation Information
- Quote No
- Date
- Material Type (PPAL, PIR, PPGI, GI)
- Panel Type (ROOF, WALL, CEILING, PARTITION)

### Material Rates (₹/Ton)
- Top Skin Cost/Ton: **1400** (default)
- Bottom Skin Cost/Ton: **400** (default)
- Core Cost/Ton: **900** (default)

### Top Skin Details
- Thickness (mm): **0.5** (default)
- Girth (mm): **1220** (default)
- Length (mm): **1000** (default)
- Density (kg/m³): **2720** (default)

### Bottom Skin Details
- Thickness (mm): **0.5** (default)
- Girth (mm): **1080** (default)
- Length (mm): **1000** (default)
- Density (kg/m³): **2720** (default)

### Core Details
- Thickness (mm): **50** (default)
- Girth (mm): **1000** (default)
- Length (mm): **1000** (default)
- Density (kg/m³): **40** (default)
- Core Constant: **0.215** (default)

### Quantity & Pricing
- Quantity: **1000** (default)
- Quoted Price (₹/unit): **7.7** (default)
- Wastage (%): **10** (default)
- Overhead (%): **5** (default)

---

## 🎯 Output Results

### Component Weights (tons)
- Top Skin Weight
- Bottom Skin Weight
- Core Weight
- **Total Weight**

### Material Costs (₹)
- Top Skin Cost
- Bottom Skin Cost
- Core Cost
- **Total Material Cost**

### Additional Costs (₹)
- Wastage Cost
- Overhead Cost

### Final Totals (₹)
- **Grand Total Cost**
- **Cost per Unit**

### Value Addition Analysis
- Value Addition (₹)
- Value Add Total (₹)
- Total Sales (₹)
- **% Value Add**

---

## 🚀 How to Use

1. **Start Dev Server:**
   ```bash
   npm run dev
   ```

2. **Open Browser:**
   - Navigate to `http://localhost:5173`

3. **Enter Values:**
   - Modify material rates if needed
   - Adjust dimensions for each component
   - Set quantity and quoted price
   - Adjust wastage and overhead percentages

4. **Calculate:**
   - Click "🧮 Calculate Total Cost" button
   - View detailed breakdown with all components

5. **Results:**
   - See component weights
   - View material costs
   - Check grand total and cost per unit
   - Analyze value addition metrics

---

## 📁 Project Structure

```
cost/pug-costing/
├── src/
│   ├── components/
│   │   ├── PanelCalculator.vue    ← Main calculator (Excel-based)
│   │   └── CostingCalculator.vue  ← Old version (placeholder formulas)
│   ├── App.vue                     ← Uses PanelCalculator
│   └── style.css                   ← TailwindCSS v3
├── Pug-cost.xlsx                   ← Original Excel file
├── excel_data.json                 ← Extracted data
├── excel_data.txt                  ← Human-readable data
├── excel_data_formulas.txt         ← All 282 formulas
├── EXCEL_FORMULA_ANALYSIS.md       ← Formula documentation
├── README_FORMULAS.md              ← This file
└── extract_excel.py                ← Python extraction script
```

---

## ✨ Features

- ✅ **Exact Excel Formulas** - All calculations match Excel REV-03
- ✅ **Responsive Design** - Works on mobile, tablet, desktop
- ✅ **Real-time Calculation** - Instant results on button click
- ✅ **Detailed Breakdown** - Shows all intermediate calculations
- ✅ **Indian Currency Format** - ₹ with proper thousand separators
- ✅ **Weight Precision** - 3 decimal places like Excel
- ✅ **Value Addition Analysis** - Complete profitability metrics
- ✅ **Default Values** - Pre-filled with Excel example data
- ✅ **Modern UI** - TailwindCSS with gradient backgrounds
- ✅ **No Backend Required** - Pure frontend calculation

---

## 🔧 Technical Stack

- **Framework:** Vue 3 (Composition API)
- **Styling:** TailwindCSS v3.4
- **Build Tool:** Vite
- **Language:** JavaScript (ES6+)
- **Data Extraction:** Python (openpyxl, pandas)

---

## 📝 Notes

1. **Core Formula Difference:** The core weight calculation includes an additional constant (0.215) not present in skin calculations.

2. **Cost Division:** All cost/ton values are divided by 1000 in the formula for normalization.

3. **Quantity Reference:** The Excel uses J6 (first row quantity) as the reference for all calculations.

4. **Percentage Calculations:** Wastage and overhead are calculated as percentages of total material cost.

5. **Value Addition:** Represents the profit margin between quoted price and actual material cost.

---

## 🎓 Formula Verification

To verify calculations match Excel:
1. Open `Pug-cost.xlsx`
2. Check Row 6-14 calculations
3. Compare with calculator output
4. All values should match to 3 decimal places

**Test Case (Default Values):**
- Top Skin Weight: **1.659 tons** ✓
- Bottom Skin Weight: **1.469 tons** ✓
- Core Weight: **2.215 tons** ✓
- Total Weight: **5.343 tons** ✓
- Grand Total: **₹5.64** (per unit) ✓

---

## 📞 Support

For questions or issues:
1. Check `EXCEL_FORMULA_ANALYSIS.md` for detailed formulas
2. Review `excel_data_formulas.txt` for all 282 Excel formulas
3. Inspect `excel_data.txt` for cell-by-cell breakdown

---

**Last Updated:** November 4, 2025
**Version:** 1.0.0
**Based on:** Pug-cost.xlsx (Sheet: REV-03)
