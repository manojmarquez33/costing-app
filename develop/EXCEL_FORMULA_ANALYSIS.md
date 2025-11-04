# Excel Formula Analysis - Pug-cost.xlsx

## Sheet: REV-03
**Total Cells:** 746 | **Formulas:** 282

---

## Input Parameters (Yellow Highlighted Cells)

### Material Rates (Row 1-3):
- **N1**: Top Skin Cost/Ton = 1400
- **N2**: Bottom Skin Cost/Ton = 400  
- **N3**: Core Cost/Ton = 900

### Data Columns (Row 5 Headers):
- **A**: Quote
- **B**: Date
- **C**: Material (PPAL, PIR, PPGI, GI, TRADING, SERVICE)
- **D**: Type of Panel (ROOF, WALL, CEILING, PARTITION)
- **E**: Color
- **F**: Type of Product (Top Skin, Bottom Skin, Core)
- **G**: Thickness (mm) - e.g., 0.5
- **H**: Girth (mm) - e.g., 1220
- **I**: Length (mm) - e.g., 1000
- **J**: Qty (Quantity) - e.g., 1000
- **K**: Density (kg/m³) - e.g., 2720
- **L**: Weight (calculated)
- **M**: Total Weight/Ton (calculated)
- **N**: Cost/Ton
- **O**: Material Cost (calculated)
- **P**: Quoted Price
- **Q**: Value Addition (calculated)
- **R**: Value Add Total (calculated)
- **S**: Total Sales (calculated)
- **T**: % of Value Add (calculated)

---

## Key Formulas

### 1. Weight Calculation (Column L)
```excel
L6: =G6*H6/1000*I6/1000*K6/1000
```
**Formula:** `Weight = (Thickness × Girth / 1000) × (Length / 1000) × (Density / 1000)`

**Explanation:** Calculates weight in tons based on dimensions and density

**Alternative for Core (Row 8):**
```excel
L8: =G8/1000*H8/1000*I8/1000*K8+0.215
```
**Formula:** `Weight = (Thickness / 1000) × (Girth / 1000) × (Length / 1000) × Density + 0.215`

### 2. Total Weight/Ton (Column M)
```excel
M6: =L6*J6/1000
```
**Formula:** `Total Weight = Weight × Quantity / 1000`

### 3. Material Cost (Column O)
```excel
O6: =N6/1000*L6
```
**Formula:** `Material Cost = (Cost per Ton / 1000) × Weight`

### 4. Total Weight Sum (Row 9)
```excel
L9: =SUM(L6:L8)
```
**Formula:** `Total Weight = SUM of all component weights`

### 5. Value Add Total (Column R)
```excel
R9: =O9*J6
R10: =O10*J6
```
**Formula:** `Value Add Total = Material Cost × Quantity`

### 6. Total Material Cost (Row 11)
```excel
O11: =SUM(O6:O10)
```
**Formula:** `Total Material Cost = SUM of all material costs`

### 7. Value Addition (Column Q, Row 11)
```excel
Q11: =P6-O11
```
**Formula:** `Value Addition = Quoted Price - Total Material Cost`

### 8. Value Add Total (Row 11)
```excel
R11: =Q11*J6
```
**Formula:** `Value Add Total = Value Addition × Quantity`

### 9. Total Sales (Row 11)
```excel
S11: =P6*J6
```
**Formula:** `Total Sales = Quoted Price × Quantity`

### 10. Percentage of Value Add (Row 11)
```excel
T11: =R11/S11%
```
**Formula:** `% Value Add = (Value Add Total / Total Sales) × 100`

### 11. Wastage/Overhead (Row 12)
```excel
O12: =P12*O11
```
**Formula:** `Wastage Cost = Wastage % × Total Material Cost`

**Example:** If P12 = 0.10 (10%), then Wastage = 10% of Total Material Cost

### 12. Additional Overhead (Row 13)
```excel
O13: =O11*0.05
```
**Formula:** `Additional Overhead = Total Material Cost × 5%`

### 13. Grand Total (Row 14)
```excel
O14: =SUM(O11:O13)
```
**Formula:** `Grand Total = Total Material Cost + Wastage + Overhead`

---

## Calculation Flow

```
1. Input Parameters:
   - Thickness (G), Girth (H), Length (I), Quantity (J), Density (K)
   - Cost per Ton (N)

2. Calculate Weight:
   Weight (L) = (Thickness × Girth × Length × Density) / 1,000,000,000
   
3. Calculate Total Weight:
   Total Weight (M) = Weight × Quantity / 1000

4. Calculate Material Cost per Unit:
   Material Cost (O) = (Cost per Ton / 1000) × Weight

5. Sum all Material Costs:
   Total Material Cost (O11) = SUM(O6:O10)

6. Calculate Value Addition:
   Value Addition (Q11) = Quoted Price - Total Material Cost

7. Calculate Totals:
   - Value Add Total (R11) = Value Addition × Quantity
   - Total Sales (S11) = Quoted Price × Quantity
   - % Value Add (T11) = (Value Add Total / Total Sales) × 100

8. Add Overheads:
   - Wastage (O12) = Wastage % × Total Material Cost
   - Additional Overhead (O13) = Total Material Cost × 5%
   
9. Grand Total:
   Grand Total (O14) = Total Material Cost + Wastage + Overhead
```

---

## Example Calculation (Row 6 - Top Skin):

**Inputs:**
- Thickness (G6) = 0.5 mm
- Girth (H6) = 1220 mm
- Length (I6) = 1000 mm
- Quantity (J6) = 1000
- Density (K6) = 2720 kg/m³
- Cost/Ton (N6) = 1400

**Calculations:**
1. Weight (L6) = 0.5 × 1220 / 1000 × 1000 / 1000 × 2720 / 1000 = **1.6592 tons**
2. Total Weight (M6) = 1.6592 × 1000 / 1000 = **1.6592 tons**
3. Material Cost (O6) = 1400 / 1000 × 1.6592 = **2.32288**

---

## Notes:

1. **Multiple Product Types**: Each panel has 3 components:
   - Top Skin (uses N1 cost)
   - Bottom Skin (uses N2 cost)
   - Core (uses N3 cost, has different weight formula)

2. **Wastage Factor**: Row 12 applies a wastage percentage (P12) to account for material loss

3. **Fixed Overhead**: Row 13 adds a fixed 5% overhead

4. **Quantity Reference**: Most formulas use J6 (first row quantity) as the reference quantity

5. **Cost per Ton Division**: Cost/Ton is divided by 1000 in formulas (N6/1000) to normalize the calculation

---

## Files Generated:
1. **excel_data.json** - Complete data in JSON format
2. **excel_data.txt** - Human-readable format with all cell details
3. **excel_data_formulas.txt** - All 282 formulas extracted
