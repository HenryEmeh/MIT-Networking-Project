# Diagram Creation Guide for Power Resilience Plan
**Topic 10: AgroLink Nigeria Power Resilience Plan**  
**For: DIY Visual Diagram Creation in draw.io, Lucidchart, or Visio**

---

## Overview

This guide provides **step-by-step instructions** to create three professional diagrams for your PDF submission. Each diagram includes:
- **Conceptual description** (what to show)
- **Data points** (exact values to plot)
- **Software recommendations** (draw.io is free; Visio/Excel also work)
- **Placement location** (section reference in main report)

All diagrams should be:
- ✅ High-resolution (300 DPI minimum for PDF)
- ✅ Color-coded for clarity
- ✅ Properly labeled with units, legends, and titles
- ✅ Professional appearance (no hand-drawn sketches)

---

## Diagram 1: Power Flow Architecture (Solar → Inverter → Battery → Loads)

**Placement in Report:** Section 3 (Solar Generation Architecture), after subsection 3.1  
**Word Count Impact:** NONE (diagrams don't count toward word limit)

### Conceptual Design

This diagram shows the **energy flow path** from solar generation through inverter decision logic to final loads, with priority levels highlighted.

### Visual Elements to Include

```
┌─────────────────────────────────────────────────────────────────────┐
│                     POWER FLOW ARCHITECTURE                         │
│              AgroLink Nigeria IoT Hub - Energy Routing               │
└─────────────────────────────────────────────────────────────────────┘

                          ☀️ SOLAR ARRAY
                          (1.8 kW PV)
                              ↓
                    ┌─────────────────────┐
                    │  MPPT Controller    │
                    │   (60A @ 48V)       │
                    │   Efficiency: 98%   │
                    └─────────────────────┘
                              ↓
                    ┌─────────────────────┐
                    │  Hybrid Inverter    │
                    │   (5 kVA Growatt)   │
                    │   Efficiency: 95%   │
                    └─────────────────────┘
                         ↙      ↓      ↖
                    ┌──────────────────────┐
                    │  BATTERY BANK        │
                    │  48V 300Ah LiFePO₄   │
                    │  (14.4 kWh capacity) │
                    │  Usable: 11.52 kWh   │
                    └──────────────────────┘
                              ↓
            ┌─────────────────┴──────────────────┐
            ↓                                     ↓
    ┌──────────────────┐            ┌──────────────────────┐
    │ CONTINUOUS LOAD  │            │ INTERMITTENT LOAD    │
    │    (24/7)        │            │  (Daytime Active)    │
    │  152W Total:     │            │  ~1,100 Wh/day:      │
    │  • Gateways: 24W │            │  • Drone Charging    │
    │  • Switch: 50W   │            │  • Analysis Equipment│
    │  • Edge: 30W     │            │  • WiFi Sync         │
    │  • LTE: 8W       │            └──────────────────────┘
    │  • VSAT: 15W     │                      ↓
    │  • Display: 10W  │            Available during:
    │  • Margin: 15W   │            06:00–18:00 (daylight)
    └──────────────────┘

PRIORITY DISPATCH LOGIC (Inverter Decision Rules):
────────────────────────────────────────────────

1. SOLAR PRIORITY (06:00–18:00, Sunny)
   └─→ All loads draw from PV first
   └─→ Excess charges battery
   └─→ Battery SOC increases

2. BATTERY DISCHARGE (18:00–06:00)
   └─→ Continuous load (152W) from battery
   └─→ Intermittent loads (1,100 Wh) already completed daytime
   └─→ Battery SOC decreases gradually

3. CLOUDY CONDITIONS
   └─→ Solar drops to ~2.16 kWh/day (1.5 PSH)
   └─→ Battery supplements deficit (3.0 kWh/day)
   └─→ Battery SOC drops ~3% per day

4. GRID FALLBACK (if available)
   └─→ Grid input charges battery + powers loads
   └─→ Reduces battery depletion rate

5. LOAD SHEDDING (Battery SOC < 15%)
   └─→ Non-critical loads disconnect (WiFi AP, display)
   └─→ Critical loads remain (gateways, edge, LTE)
   └─→ Battery life extended

6. EMERGENCY (Battery SOC < 5%)
   └─→ Graceful shutdown over 2 minutes
   └─→ System offline until solar recharge or manual intervention
```

### How to Create in Draw.io (Free Online Tool)

**Step-by-Step:**

1. **Go to draw.io** (draw.io — opens in browser, no login required initially)

2. **Create New Diagram:**
   - Select "Blank Diagram"
   - Choose canvas size: A4 Landscape

3. **Add Components (use these shapes):**
   - **Sun icon** (search "sun" in shapes panel) → Label: "Solar Array 1.8 kW"
   - **Rectangle boxes** (in Basic Shapes) → MPPT, Inverter, Battery, Loads
   - **Arrows** (Connectors panel) → Show energy flow direction
   - **Color coding:**
     - 🟢 Green: High-priority loads (gateways, edge node)
     - 🟡 Yellow: Intermittent loads (drone charging)
     - 🔴 Red: Battery-critical threshold alerts

4. **Add Data Labels:**
   - Each box includes: component name, power rating (W), efficiency (%)
   - Example: "Hybrid Inverter | 5 kVA | Efficiency: 95%"

5. **Add Legend:**
   - Solid lines = Energy flow during solar hours
   - Dashed lines = Energy flow during night (battery discharge)
   - Dotted lines = Fallback/emergency paths

6. **Export:**
   - File → Export as → PNG (300 DPI)
   - Name: "Diagram_1_PowerFlow.png"

### Alternative: Create in Visio or PowerPoint

If you prefer Microsoft tools:
- **Visio:** Shapes gallery → "Electrical Engineering" → Use power symbols
- **PowerPoint:** Insert → Shapes → Create flow diagram with SmartArt

---

## Diagram 2: Daily Load Profile Chart (Time vs. Power)

**Placement in Report:** Section 2 (Network Topology & Load Profile), after Table showing "Continuous vs. Intermittent Load"  
**Word Count Impact:** NONE (charts don't count)

### Chart Specifications

**Chart Type:** Line Chart (or Area Chart for visual impact)  
**X-Axis:** Time (00:00 to 24:00, in 2-hour intervals)  
**Y-Axis:** Power Consumption (0–700W)  
**Data Series:** 3 lines (Normal Day, Cloudy Day, Peak Emergency)

### Data Points to Plot

#### **Series 1: Normal Sunny Day**
```
Time    | Power (W)
--------|----------
00:00   | 152
02:00   | 152
04:00   | 152
06:00   | 152 (sunrise, solar starts)
08:00   | 540 (sensors active, drone charging starts)
10:00   | 540 (peak daylight)
12:00   | 540 (noon peak + WiFi sync)
14:00   | 540 (afternoon analysis running)
16:00   | 540 (pre-sunset)
18:00   | 152 (sunset, sensors offline, continuous only)
20:00   | 152
22:00   | 152
```

#### **Series 2: Cloudy Day (50% Solar Reduction)**
```
Time    | Power (W)
--------|----------
00:00   | 152
02:00   | 152
04:00   | 152
06:00   | 152 (sunrise, cloudy conditions)
08:00   | 480 (reduced drone charging capacity)
10:00   | 480 (cloudy, less solar available)
12:00   | 480
14:00   | 480
16:00   | 480
18:00   | 152 (sunset)
20:00   | 152
22:00   | 152
```

#### **Series 3: Peak Emergency Load (All Systems Max)**
```
Time    | Power (W)
--------|----------
00:00   | 650 (emergency scenario: all systems active)
02:00   | 650
04:00   | 650
06:00   | 650
08:00   | 650
10:00   | 650
12:00   | 650
14:00   | 650
16:00   | 650
18:00   | 650
20:00   | 650
22:00   | 650
```

### How to Create in Microsoft Excel

**Step-by-Step:**

1. **Open Excel and create a data table:**

   ```
   Column A: Time       | Column B: Normal Day | Column C: Cloudy Day | Column D: Emergency
   ──────────────────────────────────────────────────────────────────��───────────────
   00:00                | 152                 | 152                  | 650
   02:00                | 152                 | 152                  | 650
   04:00                | 152                 | 152                  | 650
   06:00                | 152                 | 152                  | 650
   08:00                | 540                 | 480                  | 650
   10:00                | 540                 | 480                  | 650
   12:00                | 540                 | 480                  | 650
   14:00                | 540                 | 480                  | 650
   16:00                | 540                 | 480                  | 650
   18:00                | 152                 | 152                  | 650
   20:00                | 152                 | 152                  | 650
   22:00                | 152                 | 152                  | 650
   ```

2. **Select Data Range:**
   - Highlight all three columns (A1:D13)

3. **Insert Chart:**
   - Go to: Insert → Chart → Line Chart (or Area Chart)
   - Select "Line Chart with Markers" for clarity

4. **Customize Chart:**
   - **Title:** "Daily Load Profile: AgroLink IoT Hub"
   - **X-Axis Label:** "Time of Day (24-hour)"
   - **Y-Axis Label:** "Power Consumption (Watts)"
   - **Legend:** Position at bottom (show all 3 series)
   - **Grid Lines:** Enable horizontal grid for easy reading

5. **Format Data Series:**
   - **Normal Day (Series 1):** Blue solid line, 2.5pt thickness
   - **Cloudy Day (Series 2):** Orange dashed line, 2.5pt thickness
   - **Emergency (Series 3):** Red solid line, 2.5pt thickness

6. **Add Data Labels** (optional):
   - Right-click data points → Add Data Labels
   - Show only at key points (06:00, 12:00, 18:00) to avoid clutter

7. **Add Shaded Regions** (optional in Excel):**
   - Use Conditional Formatting or Shape Rectangles to highlight:
     - 🌅 Sunrise–Sunset (06:00–18:00): Light yellow background
     - 🌙 Night (18:00–06:00): Light gray background

8. **Export:**
   - Right-click chart → Save as Image → PNG (300 DPI)
   - Name: "Diagram_2_DailyLoadProfile.png"

### Why These Three Series?

| Series | Purpose | Rubric Alignment |
|--------|---------|------------------|
| **Normal Day** | Baseline operation; shows continuous + intermittent balance | Technical Accuracy |
| **Cloudy Day** | Worst-case solar; demonstrates battery resilience | Problem Solving |
| **Emergency** | Peak demand; justifies battery capacity sizing | Documentation |

---

## Diagram 3: 5-Day Autonomy Scenario Visualization (Battery SOC Trend)

**Placement in Report:** Section 4 (Smart Battery Integration & Runtime Estimation), after "Runtime Estimation Proof: Worst-Case Scenarios"  
**Word Count Impact:** NONE (charts don't count)

### Chart Specifications

**Chart Type:** Line Chart (with area fill for visual impact)  
**X-Axis:** Days (1–5, in 6-hour intervals = 20 data points)  
**Y-Axis:** Battery State of Charge (0–100%)  
**Data Series:** 3 scenarios (Scenario A: Sunny, Scenario B: Cloudy, Scenario C: Failure)

### Data Points to Plot

**Assumption:** Battery starts at 100% SOC on Day 1, 00:00

#### **Scenario A: Continuous Sunny Days (Normal Operation)**
```
Time            | Days | SOC (%)
────────────────────────────────
Day 1, 00:00    | 1.0  | 100
Day 1, 06:00    | 1.25 | 100 (solar starts, continuous load only)
Day 1, 12:00    | 1.5  | 105 (solar > load, battery charges beyond 100%)
Day 1, 18:00    | 1.75 | 100 (sunset, excess solar ends)
Day 2, 00:00    | 2.0  | 98 (night discharge: 152W × 6h = 912Wh ÷ 11,520Wh = 7.9%)
Day 2, 06:00    | 2.25 | 98
Day 2, 12:00    | 2.5  | 105
Day 2, 18:00    | 2.75 | 100
Day 3, 00:00    | 3.0  | 98
Day 3, 06:00    | 3.25 | 98
Day 3, 12:00    | 3.5  | 105
Day 3, 18:00    | 3.75 | 100
Day 4, 00:00    | 4.0  | 98
Day 4, 06:00    | 4.25 | 98
Day 4, 12:00    | 4.5  | 105
Day 4, 18:00    | 4.75 | 100
Day 5, 00:00    | 5.0  | 98
```

**Interpretation:** Battery oscillates 98–105% SOC; stable indefinitely (solar > load)

#### **Scenario B: Extended Cloudy Period (3+ Days)**
```
Time            | Days | SOC (%)  | Notes
───────────────────────────────────────────────────────
Day 1, 00:00    | 1.0  | 100      | Start (night before cloudy spell)
Day 1, 06:00    | 1.25 | 100      | Cloudy sunrise
Day 1, 12:00    | 1.5  | 82       | Deficit: 3,000 Wh/day from battery
Day 1, 18:00    | 1.75 | 65       | Continued discharge
Day 2, 00:00    | 2.0  | 47       | Night + continued cloud discharge
Day 2, 06:00    | 2.25 | 47
Day 2, 12:00    | 2.5  | 29       | Battery < 30% (threshold alert)
Day 2, 18:00    | 2.75 | 11       | Battery < 15% (load shedding triggered)
Day 3, 00:00    | 3.0  | -7       | CRITICAL: Battery would be depleted
                       | 5        | (load shedding keeps it at ~5% minimum)
Day 3, 06:00    | 3.25 | 5
Day 3, 12:00    | 3.5  | 5        | Still cloudy; system in emergency mode
Day 3, 18:00    | 3.75 | 5
Day 4, 00:00    | 4.0  | 5
Day 4, 06:00    | 4.25 | 5
Day 4, 12:00    | 4.5  | 5        | Sunshine returns (scenario ends)
```

**Interpretation:** Network survives 3.2 days on battery alone; load shedding extends life

#### **Scenario C: Complete Failure (Zero Solar + Zero Grid)**
```
Time            | Days | SOC (%)  | Notes
───────────────────────────────────────────────────────
Day 1, 00:00    | 1.0  | 100      | Start: all systems active (650W emergency load)
Day 1, 12:00    | 1.5  | 77       | Discharge rate: 650W continuous
Day 2, 00:00    | 2.0  | 54       | 25% depletion per 24 hours
Day 2, 12:00    | 2.5  | 31
Day 3, 00:00    | 3.0  | 8        | Approaching critical (< 15%)
Day 3, 06:00    | 3.25 | 0        | Complete depletion at 75.8 hours (3.2 days)
```

**Alternative (With Load Shedding at SOC < 15%):**
```
Time            | Days | SOC (%)  | Notes
───────────────────────────────────────────────────────
Day 1, 00:00    | 1.0  | 100
Day 1, 12:00    | 1.5  | 77
Day 2, 00:00    | 2.0  | 54
Day 2, 12:00    | 2.5  | 31
Day 3, 00:00    | 3.0  | 15       | LOAD SHEDDING TRIGGERED
Day 3, 12:00    | 3.5  | 8        | Load reduces from 650W → 120W
Day 4, 00:00    | 4.0  | 4        | Discharge rate drops significantly
Day 4, 12:00    | 4.5  | 2        | Further reduction, near critical
Day 5, 00:00    | 5.0  | 0        | Depletion extended to 96+ hours (4 days)
```

**Interpretation:** Load shedding adds 24 hours of survival time

### How to Create in Microsoft Excel

**Step-by-Step:**

1. **Create Data Table (simplified for 20-point chart):**

   ```
   Column A: Time                | Column B: Scenario A (Sunny) | Column C: Scenario B (Cloudy) | Column D: Scenario C (Failure)
   ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   Day 1, 00:00                  | 100                          | 100                          | 100
   Day 1, 06:00                  | 100                          | 100                          | 96
   Day 1, 12:00                  | 105                          | 82                           | 77
   Day 1, 18:00                  | 100                          | 65                           | 61
   Day 2, 00:00                  | 98                           | 47                           | 54
   Day 2, 06:00                  | 98                           | 47                           | 50
   Day 2, 12:00                  | 105                          | 29                           | 31
   Day 2, 18:00                  | 100                          | 11                           | 15
   Day 3, 00:00                  | 98                           | 5                            | 8
   Day 3, 06:00                  | 98                           | 5                            | 0 (or 4 with load shed)
   Day 3, 12:00                  | 105                          | 5                            | —
   Day 3, 18:00                  | 100                          | 5                            | —
   Day 4, 00:00                  | 98                           | 5                            | —
   Day 4, 06:00                  | 98                           | 5                            | —
   Day 4, 12:00                  | 105                          | 5                            | —
   Day 4, 18:00                  | 100                          | 5                            | —
   Day 5, 00:00                  | 98                           | 5                            | —
   ```

2. **Select Data Range:**
   - Highlight A1:D17 (all data)

3. **Insert Chart:**
   - Go to: Insert → Chart → Line Chart
   - Select "Line Chart with Markers and Lines"

4. **Customize Chart:**
   - **Title:** "Battery State of Charge (SOC) Over 5 Days"
   - **Subtitle:** "Three Operational Scenarios"
   - **X-Axis Label:** "Time (Days + Hours)"
   - **Y-Axis Label:** "State of Charge (%)"
   - **Y-Axis Range:** 0–110% (to show overcharge ceiling)
   - **Legend:** Position at bottom

5. **Format Data Series (Color-Code):**
   - **Scenario A (Sunny):** Green solid line, 3pt thickness (stable/safe)
   - **Scenario B (Cloudy):** Orange dashed line, 2.5pt thickness (degrading)
   - **Scenario C (Failure):** Red solid line, 3pt thickness (critical)

6. **Add Reference Lines** (optional, but recommended for rubric):
   - **Horizontal line at 100%:** Black dotted line (battery full)
   - **Horizontal line at 30%:** Orange dotted line (alert threshold)
   - **Horizontal line at 15%:** Red dotted line (load shedding trigger)
   - **Horizontal line at 5%:** Red dashed line (emergency shutdown)
   
   **How to add reference lines in Excel:**
   - Insert → Shapes → Line
   - Draw horizontal lines across chart at 30%, 15%, 5% SOC levels
   - Right-click → Format → Dashed style

7. **Add Annotations** (optional):
   - Text box at Scenario B, Day 2.5: "Load Shedding Engaged"
   - Text box at Scenario C, Day 3: "System Offline"

8. **Export:**
   - Right-click chart → Save as Image → PNG (300 DPI)
   - Name: "Diagram_3_BatteryAutonomy.png"

---

## Final Assembly: Inserting Diagrams into PDF

### Step 1: Export All Three Diagrams
- **Diagram 1:** `Diagram_1_PowerFlow.png` (1200×800px minimum)
- **Diagram 2:** `Diagram_2_DailyLoadProfile.png` (1200×600px)
- **Diagram 3:** `Diagram_3_BatteryAutonomy.png` (1200×600px)

### Step 2: Convert Markdown to PDF with Diagrams

**Option A: Using Pandoc (Recommended for Professional Look)**
```bash
# Install Pandoc: https://pandoc.org/installing.html

# Convert with embedded images:
pandoc Power_Resilience_Plan_Revised_Draft.md \
  -o Power_Resilience_Plan_FINAL.pdf \
  --pdf-engine=xelatex \
  --toc

# Then manually insert diagrams at these sections (using Adobe Acrobat):
# - Page 3 (Section 3, after "Hardware Selection")
# - Page 4 (Section 2, after load profile table)
# - Page 5 (Section 4, after runtime scenarios)
```

**Option B: Using Google Docs (Easiest)**
1. Copy Markdown text into Google Docs
2. Format headings, tables, lists
3. Insert → Image → Upload `Diagram_1_PowerFlow.png`
4. Insert → Image → Upload `Diagram_2_DailyLoadProfile.png`
5. Insert → Image → Upload `Diagram_3_BatteryAutonomy.png`
6. File → Download → PDF Document

**Option C: Using Microsoft Word (Most Familiar)**
1. Open new Word document
2. Copy text from Revised Draft
3. Format headings and tables
4. Insert → Pictures → Select each PNG file
5. Position diagrams in appropriate sections
6. File → Export as PDF

### Step 3: Verify Diagram Placement

| Section | Diagram | After Subsection |
|---------|---------|-----------------|
| Section 2 | Diagram 2 (Daily Load Profile) | "Continuous vs. Intermittent Load" table |
| Section 3 | Diagram 1 (Power Flow Architecture) | "Hardware Selection: 1.8 kW Array Configuration" |
| Section 4 | Diagram 3 (Battery Autonomy) | "Runtime Estimation Proof: Worst-Case Scenarios" |

---

## Word Count Impact: ZERO ✅

**Important:** Diagrams, charts, tables, and images **do NOT count** toward the 1,000–1,500 word limit.

**Current Draft Status:**
- Text: ~1,480 words ✅ (within limit)
- Diagrams: 3 (non-word-count) ✅
- Tables: 8 (non-word-count) ✅
- **Total Submission Word Count: ~1,480 words** (diagrams/tables exempt)

---

## Rubric Scoring Impact: +2–3 Marks

| Rubric Dimension | Current (Text Only) | With Diagrams | Gain |
|------------------|-------------------|---------------|------|
| **Technical Accuracy** | 14–15/15 | 14–15/15 | — |
| **Documentation & Presentation** | 13–14/15 | 15/15 | **+1–2 marks** |
| **NDPR/Security** | 9–10/10 | 9–10/10 | — |
| **Estimated Total** | 36–39/40 | **38–40/40** | **+2 marks** |

**Why the gain?**
- Rubric explicitly requires: "**Accurate legends, headings, and units in diagrams**/tables"
- Visual diagrams demonstrate mastery of load profiling and battery management concepts
- Professional presentation = higher Documentation clarity score

---

## Quick Checklist Before Submission

- [ ] Diagram 1 (Power Flow) created with all labels and priorities
- [ ] Diagram 2 (Load Profile) shows three scenarios with legend
- [ ] Diagram 3 (Battery SOC) displays 5-day trend with threshold lines
- [ ] All diagrams exported as PNG, 300 DPI minimum
- [ ] Diagrams inserted in correct sections of PDF
- [ ] Word count verified: ~1,480 words (diagrams exempt)
- [ ] References included (IRENA, BYD, Growatt, IEEE, Cisco, NDPR)
- [ ] Professional PDF formatting applied (margins, fonts, headers)

---

## Troubleshooting Tips

**Issue: Excel chart looks pixelated in PDF**
- Solution: Export as PNG (300 DPI) instead of embedding Excel file directly

**Issue: draw.io shapes don't align**
- Solution: Use "Arrange" → "Align" menu to center components

**Issue: Diagram 3 shows line cutoff at bottom of page**
- Solution: Make chart taller (adjust row height) or reduce title font

**Issue: Colors don't print well in B&W**
- Solution: Use distinct line styles (solid, dashed, dotted) + colors

---

## Final Deliverables Summary

✅ **Main Report:**
- File: `Power_Resilience_Plan_Revised_Draft.md` (or converted to PDF)
- Word Count: ~1,480 words
- Sections: 7 (Introduction → Conclusion)
- Tables: 8 (load profile, cost analysis, LiFePO₄ comparison, etc.)

✅ **Supplementary Diagrams (PDF Only):**
- Diagram 1: Power Flow Architecture
- Diagram 2: Daily Load Profile Chart
- Diagram 3: 5-Day Battery Autonomy Trend

✅ **Expected Rubric Score:** 37–40/40 marks

---

**You're ready to create professional, publication-quality diagrams!** 🎯

