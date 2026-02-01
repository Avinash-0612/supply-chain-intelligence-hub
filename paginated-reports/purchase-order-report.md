# Purchase Order Detail Report (Paginated)

## Overview
Professional pixel-perfect report designed for auditors, suppliers, and compliance documentation. Generated using Power BI Report Builder (Paginated Reports).

## Report Specifications

**Format:** Multi-page PDF / Word / Excel export  
**Orientation:** Portrait (Letter size)  
**Page Layout:** Tabular with headers/footers repeating per page  

## Report Sections

### Page Header (Repeats on every page)

Company Logo                                    Purchase Order Detail Report
Report Date: 01/31/2025                         Page: 1 of 15


### Report Parameters
- **Date Range:** Start Date / End Date (Calendar pickers)
- **Supplier:** Multi-select dropdown (All suppliers or specific)
- **PO Status:** Checkboxes (Open, Closed, Cancelled, Pending)
- **Warehouse:** Location filter
- **SKU Category:** Product category dropdown

### Main Table Columns
| Column | Source | Format |
|--------|--------|--------|
| PO Number | ERP.PurchaseOrderID | Text link (drill to PO details) |
| Order Date | ERP.OrderDate | MM/DD/YYYY |
| Supplier Name | ERP.SupplierName | Text |
| SKU | ERP.ItemNumber | Barcode font |
| Description | ERP.ItemDescription | Text |
| Qty Ordered | ERP.Quantity | Number (#,##0) |
| Qty Received | ERP.ReceivedQty | Number (#,##0) |
| Unit Price | ERP.UnitCost | Currency ($#,##0.00) |
| Total Line Amount | Calculated | Currency ($#,##0.00) |
| Expected Delivery | ERP.PromisedDate | MM/DD/YYYY |
| Actual Delivery | ERP.ReceiptDate | MM/DD/YYYY |
| Status | ERP.POStatus | Conditional formatting |
| Variance % | Calculated | Percentage (+0.0%;-0.0%) |

### Grouping Structure
**Group 1:** Supplier Name (Sort alphabetically)  
**Group 2:** PO Number (Sort by date descending)  
**Group 3:** Line Items (Sort by SKU)

**Subtotals per PO:** Sum(Total Line Amount)  
**Subtotals per Supplier:** Sum(Total Line Amount), Count(POs)  
**Grand Total:** Sum(All Amounts), Count(All Lines)

### Conditional Formatting
- **Late Items:** If Actual > Expected + 2 days → Background light red
- **Price Variance:** If Variance % > 5% → Font color red
- **High Value:** If Line Amount > $10,000 → Bold text

### Page Footer

Confidential - Internal Use Only                    Printed: 01/31/2025 14:30 EST
Data Source: SAP ERP & Power BI Supply Chain Hub


### Interactive Features
Even in static PDF export:
- **Drill-through:** Click PO Number → Opens detailed PO line item report
- **Bookmarks:** Table of contents with page numbers for each supplier section
- **Document Map:** Navigation pane in PDF viewer jumping to specific supplier

## Use Cases

### 1. Monthly Close Process
Finance exports this report for:
- Three-way match verification (PO vs. Receipt vs. Invoice)
- Accrual calculations for goods received not invoiced (GRNI)
- Variance analysis (standard cost vs. actual)

### 2. Supplier Audits
Procurement provides to suppliers showing:
- All open POs and their status
- Historical performance (delivery date adherence)
- Quantity and pricing discrepancies

### 3. Executive Briefings
Condensed version (summary only) for:
- Monthly supply chain reviews
- Board presentations on procurement efficiency
- Working capital analysis

## Technical Implementation

**Tool:** Power BI Report Builder (Paginated Reports)  
**Data Source:** Power BI Dataset (shared dataset with dashboard)  
**Delivery:** 
- On-demand via Power BI Service
- Scheduled email (PDF attachment) to procurement team every Monday
- Embedded in SharePoint portal for self-service download

**Performance:**
- Optimized for <30 seconds generation time even for 10,000+ line items
- Uses query parameters to filter at source (don't load all data then filter)
- Pagination ensures memory efficiency

## Comparison: Paginated vs. Interactive

| Feature | Paginated Report | Interactive Dashboard |
|---------|-----------------|---------------------|
| **Format** | Print-ready PDF | Clickable web interface |
| **Best For** | Audits, Compliance, External sharing | Exploration, Daily monitoring |
| **Data Volume** | Handles 100k+ rows easily | Best for aggregated data |
| **Export** | Native pixel-perfect PDF | Export to Excel/analysis |
| **Interactivity** | Static (view only) | Slicers, drill-through, filters |

**Both are linked:** Dashboard has button "Generate Detailed Report" → Opens this paginated report with pre-filtered context (current selections passed as parameters).

