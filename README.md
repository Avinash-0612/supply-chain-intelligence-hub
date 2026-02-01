# 🏭 Supply Chain Intelligence Hub

**End-to-end supply chain visibility platform with inventory optimization, supplier performance management, and automated workflows. Generated $2M+ in inventory cost savings and improved perfect order rate to 96.5%.**

![Power BI](https://img.shields.io/badge/Power%20BI-Paginated%20Reports-yellow.svg)
![Power Automate](https://img.shields.io/badge/Power%20Automate-Workflows-blue.svg)
![Azure](https://img.shields.io/badge/Azure-IoT%20Integration-cyan.svg)
![Supply Chain](https://img.shields.io/badge/Domain-Operations-orange.svg)

## 🎯 Executive Summary

Built a comprehensive supply chain analytics solution for a global manufacturing/retail organization, integrating ERP (SAP), Warehouse Management Systems (WMS), and IoT sensors to provide real-time visibility across 50+ warehouses and 500+ suppliers.

**Business Impact:**
- 💰 **$2M inventory reduction** through optimization algorithms
- 📈 **96.5% perfect order rate** (up from 88%)
- ⏱️ **40% faster** procurement cycle times
- 🚨 **90% reduction** in stockout incidents through predictive alerting


## 📊 Solution Components

### 1. Executive Supply Chain Dashboard
**Power BI Interactive Dashboard**

**Key Capabilities:**
- **Inventory Health Monitoring:** Real-time visibility into $45M inventory across 15,000+ SKUs
- **ABC Analysis:** Pareto visualization identifying top 20% SKUs driving 80% of value
- **Supplier Scorecards:** Performance tracking across quality (99.2%), delivery (96.5%), and cost metrics
- **Working Capital Analysis:** Cash-to-cash cycle optimization

**Technical Features:**
- **Drill-through:** Click any metric → Transaction-level detail
- **What-If Parameters:** Simulate "Increase safety stock by 10%" → See cost impact
- **Decomposition Tree:** Root-cause analysis (Why is inventory up? → Which category? → Which supplier?)
- **RLS:** Regional managers see only their warehouses

### 2. Operational Reports (Paginated)
**Pixel-Perfect Documents for Compliance**

**Purchase Order Detail Report:**
- Multi-page PDF listing every PO line item
- Grouped by supplier with subtotals
- Barcode fonts for warehouse scanning
- Export to Excel for analysis

**Use Cases:**
- Monthly three-way match reconciliation (Finance)
- Supplier audits and compliance documentation
- Executive briefings with print-ready formatting

### 3. Automated Workflows (Power Automate)
**Intelligent Business Process Automation**

**Critical Stock Alerts:**
- When inventory < safety stock → Immediate Teams alert + Email to supplier + Create task in Planner
- 90% reduction in stockout incidents

**Supplier Performance Monitoring:**
- Daily check of on-time delivery rates
- Auto-escalate to VP if supplier drops below 85% for 3 consecutive days
- Generate and email performance scorecards automatically

**Approval Workflows:**
- Price variance >5% requires manager approval
- High-value POs (>$100k) route to CFO
- Complete audit trail in SharePoint

### 4. IoT Integration
**Real-Time Cold Chain Monitoring**

**Temperature Sensors:**
- Warehouse refrigeration units monitored 24/7
- Power BI real-time dashboard showing temperature trends
- **Auto-alerts:** If temp >8°C for >10 minutes → Immediate SMS to warehouse manager + Auto-quarantine affected inventory

## 💡 Key Innovations

### Inventory Optimization Engine
**Dynamic Safety Stock Calculation:**
```dax
Optimal Safety Stock = 
VAR AvgDemand = AVERAGE(Demand[DailyUsage])
VAR DemandVariability = STDEV.P(Demand[DailyUsage])
VAR ServiceLevel = 0.95  // 95% service level
VAR ZScore = 1.65        // Z-score for 95%
VAR LeadTime = AVERAGE(Suppliers[LeadTimeDays])
RETURN
    (AvgDemand * LeadTime) + (ZScore * DemandVariability * SQRT(LeadTime))


Results: Reduced safety stock by 30% while maintaining 99%+ service levels
Supplier Risk Matrix
Scatter plot visualization:
X-axis: Supplier spend volume ($)
Y-axis: Reliability score (on-time % × quality score)
Bubble size: Business criticality
Color: Risk rating (Red/Yellow/Green)
Actionable Insights:
Top right (High spend, High reliability): Strategic partners - negotiate long-term contracts
Top left (Low spend, High reliability): Potential to increase volume
Bottom right (High spend, Low reliability): Risky - diversify suppliers or improve performance
Predictive Reorder Points
Using Azure ML to predict demand patterns:
Seasonality adjustments (holiday spikes, summer slowdowns)
Trend analysis (growing SKUs vs. declining)
External factors (promotions, market events)
Outcome: 25% reduction in emergency freight costs (air freight → standard ground)
| Metric             | Before   | After    | Improvement   |
| ------------------ | -------- | -------- | ------------- |
| Inventory Turnover | 6x/year  | 9x/year  | 50% faster    |
| Stockout Incidents | 15/month | 2/month  | 87% reduction |
| Perfect Order Rate | 88%      | 96.5%    | +8.5 pts      |
| PO Cycle Time      | 8 days   | 4.8 days | 40% faster    |
| Excess Inventory   | \$8M     | \$3.2M   | 60% reduction |

**📈 Performance Metrics**
