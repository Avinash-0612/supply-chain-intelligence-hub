# Supply Chain Power BI Dashboard Design

## Overview
Operational intelligence hub for global supply chain management, providing real-time visibility into inventory levels, supplier performance, and procurement analytics across 50+ warehouses and 500+ suppliers.

## Dashboard Pages

### 1. Executive Supply Chain Summary
**Audience:** VP of Supply Chain, COO, CFO

**Key Visuals:**
- **Inventory Health Gauge:** Total inventory value vs. target (color-coded)
- **Working Capital Impact:** Cash tied up in excess inventory
- **Supplier Risk Matrix:** Reliability vs. Spend volume (scatter plot)
- **Perfect Order Rate:** % orders delivered on-time, in-full, damage-free

**KPI Cards:**
- Total SKUs: 15,000+
- Inventory Value: $45M
- Stockout Incidents (MTD): 12
- Avg Supplier Lead Time: 14 days

### 2. Inventory Optimization Workbench
**Audience:** Inventory Planners, Demand Forecasters

**Features:**
- **ABC Analysis:** Pareto chart showing 80/20 rule (which SKUs drive 80% value)
- **Safety Stock Calculator:** Dynamic calculation based on demand variability
- **Reorder Point Alerts:** Visual indicators for SKUs below reorder threshold
- **Excess Inventory Heatmap:** Warehouse locations with &gt;90 days of stock

**Drill-Through Capability:**
Click any SKU → Drill to Transaction Detail Page showing:
- 12-month demand trend
- Supplier lead time distribution
- Last 10 purchase orders
- Alternative supplier suggestions

### 3. Supplier Performance Scorecard
**Audience:** Procurement Managers, Vendor Relations

**Metrics Table:**
| Supplier | On-Time % | Quality Score | Cost Variance | Risk Rating | Trend |
|----------|-----------|---------------|---------------|-------------|-------|
| Supplier A | 98% | 9.2/10 | +2% | 🟢 Low | ↑ |
| Supplier B | 85% | 7.8/10 | -5% | 🟡 Med | ↓ |

**Conditional Formatting:**
- Red: On-time &lt;90% or Quality &lt;8.0
- Yellow: On-time 90-95%
- Green: On-time &gt;95%

**What-If Analysis:**
- Slider to simulate "Increase Supplier B orders by 20%" → See impact on overall cost/risk

### 4. Procurement & Spend Analysis
**Audience:** Procurement Team, Finance

**Visualizations:**
- **Spend by Category:** Treemap (Raw Materials, Packaging, MRO, Logistics)
- **Contract Compliance:** Actual price vs. contracted price variance
- **Purchase Order Cycle Time:** Requisition to PO issue (days)
- **Three-Way Match Exception Rate:** PO vs. Receipt vs. Invoice mismatches

**Decomposition Tree:**
Start with Total Spend → Break down by Category → Subcategory → Supplier → Individual PO

### 5. Warehouse Operations (Real-time)
**Audience:** Warehouse Managers, Logistics

**Real-time Metrics (DirectQuery to WMS):**
- **Receiving Backlog:** POs received but not put-away (&gt;4 hours)
- **Picking Efficiency:** Lines picked per hour by shift
- **Shipping Accuracy:** Orders shipped correctly first time
- **Dock Door Utilization:** % of dock doors occupied by hour

**Map Visual:**
Geographic heat map showing:
- Warehouse locations (bubble size = inventory value)
- Inbound shipment delays (color-coded by severity)
- Service level coverage (radius circles)

## Special Features

### Paginated Report Integration
**Button Action:** "View Detailed PO Report" → Links to Paginated Report (Pixel-perfect PDF)
- Multi-page purchase order detail
- Print-ready format for auditors
- Parameters: Date range, Supplier, PO Status

### Power Automate Alerts
**Automated Workflows triggered from dashboard:**
1. **Stock Alert:** When inventory &lt; safety stock → Teams message to buyer + Email to supplier
2. **Supplier Performance:** When on-time % drops below 90% → Escalation to Procurement Director
3. **Cost Variance:** When actual cost &gt;5% above standard → Alert to Finance controller

### Mobile Executive View
**Optimized for iPad:**
- High-level KPIs only (perfect for walking the warehouse)
- Barcode scanning integration (scan SKU to see dashboard for that item)
- Offline mode (cached data for warehouse floor where WiFi is spotty)

## Data Sources

| System | Data Type | Refresh Frequency |
|--------|-----------|-------------------|
| SAP/ERP | Inventory, POs, Invoices | Hourly |
| WMS (Manhattan/Körber) | Warehouse transactions | Real-time (DirectQuery) |
| Supplier Portal | Supplier scorecards, certifications | Daily |
| IoT Sensors | Temperature, humidity (cold chain) | Real-time streaming |
| External | Freight rates, commodity prices | Daily API pull |
