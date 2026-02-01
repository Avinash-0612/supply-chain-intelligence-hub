# Power Automate: Inventory Alert Workflows

## Overview
Automated business process flows triggered by Power BI data alerts, enabling proactive supply chain management without manual monitoring.

## Flow 1: Critical Stock Alert

**Trigger:** Power BI Data Alert (When inventory &lt; safety stock threshold)

**Frequency:** Checked every 15 minutes

### Workflow Steps:

1. **Trigger: Power BI Alert Fired**
   - Dataset: Supply Chain Hub
   - Alert rule: CurrentStock &lt; SafetyStockLevel
   - Scope: Any SKU in Active status

2. **Action: Get Alert Details**
   - Extract: SKU, Description, CurrentQty, WarehouseLocation, SupplierEmail, BuyerName

3. **Condition: Critical vs. Warning**
   - If CurrentQty &lt;= 0: **Critical Stockout** (Immediate action)
   - If CurrentQty &gt; 0 but &lt; SafetyStock: **Low Stock** (Standard reorder)

4. **Branch A: Critical Stockout**
   - **Send Teams Urgent Message** to:
     - Inventory Manager
     - Procurement Buyer
     - Warehouse Supervisor
   - **Subject:** 🚨 CRITICAL: Stockout Imminent - [SKU]
   - **Message:** "Item [Description] at [Warehouse] is at [Qty] units (below zero safety stock). Immediate action required."
   
   - **Send Email to Alternate Suppliers** (lookup from vendor master):
     - Request emergency shipment
     - CC: Supply Chain Director
   
   - **Create Planner Task:**
     - Assigned to: Buyer
     - Due: Today
     - Priority: Urgent
     - Title: "Emergency PO for [SKU]"

5. **Branch B: Standard Reorder**
   - **Send Email to Primary Supplier:**
     - PO template auto-populated
     - Suggested order qty: (MaxStock - CurrentStock)
     - Delivery date request: Lead time + 2 days buffer
   
   - **Post to Teams Channel:**
     - #inventory-alerts channel
     - Adaptive card with "Mark as Ordered" button

6. **Log to SharePoint List** (Audit trail):
   - Alert Date/Time
   - SKU
   - Triggered By: Flow
   - Action Taken
   - Status: Resolved/Pending

---

## Flow 2: Supplier Performance Degradation

**Trigger:** Scheduled (Daily at 8:00 AM)

**Logic:** Check yesterday's supplier metrics

### Workflow:

1. **Recurrence:** Daily 8:00 AM

2. **Get Data from Power BI Dataset** (Power BI REST API):
   - Query: Suppliers with OnTime% &lt; 90% (last 7 days rolling)
   - Or QualityScore &lt; 8.0

3. **Apply to Each (Loop through suppliers):**

4. **Condition: Severity**
   - **High Risk:** OnTime% &lt; 85% for 3 consecutive days
   - **Medium Risk:** OnTime% 85-90%

5. **High Risk Actions:**
   - **Start Approval:** Email to VP Procurement
     - Approve: Send warning to supplier, initiate backup supplier qualification
     - Reject: Add to watch list only
   
   - **Update Supplier Master:** Flag status as "Performance Review"
   
   - **Schedule Review Meeting:** Create Outlook meeting with:
     - Category Manager
     - Supplier (via Teams or in-person)
     - Quality Engineer
     - Agenda: Performance improvement plan (PIP)

6. **Medium Risk Actions:**
   - **Send Automated Email to Supplier:**
     - "Performance Alert: Your on-time delivery rate is [X]%, below our 90% target."
     - Attach performance report (PDF from Paginated Reports)
     - Request corrective action plan within 48 hours

---

## Flow 3: Price Variance Approval

**Trigger:** When new PO created with cost variance &gt;5%

**Workflow:**

1. **Trigger:** SharePoint list "Purchase Orders" - When item created

2. **Condition:** Variance % &gt; 5% or UnitCost &gt; $10,000

3. **Start Approval Flow:**
   - **Stage 1:** Department Manager approval (if &lt;$25k)
   - **Stage 2:** Director approval (if $25k-$100k)
   - **Stage 3:** VP + CFO approval (if &gt;$100k)

4. **Approval Actions:**
   - **Approve:** Update ERP status to "Approved", notify buyer to issue PO
   - **Reject:** Return to buyer with comments, status "Revision Required"
   - **Reassign:** Send to different approver

5. **Escalation:**
   - If no response in 24 hours → Reminder email
   - If no response in 48 hours → Escalate to next level manager

6. **Completion:**
   - Log approval chain to SharePoint
   - Update Power BI dataset (refresh)
   - Archive email thread

---

## Flow 4: Automated Report Distribution

**Weekly Supply Chain Digest**

**Schedule:** Every Monday 7:00 AM

**Recipients:**
- Supply Chain Leadership Team (5 people)
- CFO (monthly summary only)
- Regional Warehouse Managers (regional data only via RLS)

**Content:**
1. **Get Data:** Power BI Export API (summary metrics)
2. **Create HTML Table:** Last week's KPIs vs. targets
3. **Attach PDF:** Paginated Report (last week's transactions)
4. **Send Email:**
   - Subject: Weekly Supply Chain Performance - Week of [Date]
   - Body: Key highlights, red/yellow/green status indicators
   - Attachment: Detailed PDF report

---

## Flow 5: Predictive Maintenance Alert (IoT Integration)

**Trigger:** IoT sensor data (Azure IoT Hub) → Power BI Streaming → Condition met

**Scenario:** Cold chain temperature monitoring for pharmaceuticals

**Workflow:**
1. **Sensor Reading:** Temperature &gt;8°C (threshold exceeded)
2. **Immediate Actions:**
   - Alert Warehouse Manager (SMS + Teams)
   - Log incident in Quality Management System
   - Check inventory in affected zone (lot numbers)
   
3. **If temperature not normalized in 30 minutes:**
   - Quarantine inventory (auto-update WMS status)
   - Notify Quality Assurance for inspection
   - Initiate insurance claim documentation workflow

---

## Integration Architecture


## Business Impact

**Before Automation:**
- Manual checking of inventory levels (2 hours/day)
- Reactive stockouts (average 5 per month costing $50k each)
- Supplier issues discovered monthly in meetings (too late)

**After Automation:**
- Zero manual monitoring (fully automated)
- Proactive reordering (stockouts reduced by 90%)
- Real-time supplier intervention (issues resolved in 24-48 hours vs. weeks)
- **ROI:** $450k annual savings in stockout prevention + labor reduction

## Security & Governance

- **Connections:** Service account used (not personal credentials)
- **Data Loss Prevention (DLP):** Policies prevent sensitive cost data from being emailed outside domain
- **Audit Logging:** All flow runs logged in Power Platform Admin Center
- **Error Handling:** Try-catch blocks with notifications to admin if flow fails
