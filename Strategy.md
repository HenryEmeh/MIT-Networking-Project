# Power Resilience Plan – AgroLink Nigeria Smart-Farming Network
**Topic 10: Power Resilience Plan**  
**Student Name:** [Your Name]  
**Date:** July 2026  
**Word Count:** ~1,350 words

---

## Executive Summary

Rural agricultural environments in Nigeria face critical power instability, with frequent grid outages and voltage fluctuations threatening continuous IoT sensor operation, drone charging cycles, and cloud connectivity for real-time analytics. This power resilience plan integrates solar photovoltaic (PV) arrays, lithium-ion battery storage, and intelligent energy management to ensure uninterrupted operation of AgroLink Nigeria's smart-farming network. The design prioritizes sustainability, cost-efficiency, and alignment with Nigeria's renewable energy policy while guaranteeing mission-critical equipment uptime—targeting ≥99.5% availability for sensor telemetry and gateway redundancy.

---

## 1. Power Demand Analysis & Load Profiling

### 1.1 Equipment Power Requirements

AgroLink Nigeria's IoT infrastructure comprises three primary power consumers:

- **Field Sensors (Wireless Sensor Network):** Soil moisture sensors, temperature/humidity nodes, and moisture-analysis probes consume 5–15W per sensor array. A typical 10-hectare farm deployment (40–50 nodes) requires ~400W aggregate during active telemetry intervals.
  
- **Gateway & Edge Compute Node:** LoRaWAN gateway (12W continuous), edge processing device (25W), and local MQTT broker (10W) total ~47W constant load, scaling to 60W during peak data ingestion (drone imagery processing).

- **Drone Charging Dock:** Agricultural drone (2.5 kg payload capacity) requires a 5,000 mAh battery pack (~75Wh). Full charge from empty requires 150W over 60 minutes; daily operations assume two charge cycles (~300Wh per day).

- **Cloud Connectivity Hardware:** 4G LTE modem (8W), WiFi access point (12W), and router (10W) total 30W continuous.

- **Secondary Equipment:** LED monitoring displays, junction box electronics, and sensor calibration devices: 15W.

**Daily Peak Load Profile:**
- 06:00–18:00 (daylight): 540W average (sensors active, drone charging possible)
- 18:00–06:00 (night): 120W (gateway and connectivity only; reduced sensor polling)
- Emergency Load (all systems active): 650W

### 1.2 Daily Energy Demand

Using 18-hour active (daylight) + 6-hour standby cycle:
- **Active Period Energy:** 540W × 18 hours = 9.72 kWh
- **Standby Period Energy:** 120W × 6 hours = 0.72 kWh
- **Total Daily Requirement:** ~10.5 kWh

Accounting for 20% system losses (converter inefficiency, wiring losses): **Daily requirement = 12.6 kWh**

---

## 2. Solar PV System Design

### 2.1 Solar Array Sizing

Nigeria's average solar irradiance in rural areas (Ibadan, Kano latitudes) is approximately **4.8–5.2 kWh/m²/day**. Using conservative **4.8 kWh/m²/day** and accounting for panel degradation (1% annually), dust accumulation (5% loss), and seasonal variation (20% winter reduction):

**Effective Available Daily Insolation:** 4.8 × 0.95 × 0.85 = **3.87 kWh/m²/day**

To generate 12.6 kWh daily with PV:
- **Required PV Capacity:** 12.6 kWh ÷ 3.87 = **3.26 kW**

**Recommended Configuration:** Four (4) × 850W monocrystalline panels (Tier-1 efficiency ≥22%) = **3.4 kW total**
- Panel dimensions: ~2.2m × 1.1m per unit
- Roof/pole footprint: ~10m² (east-west alignment, 15° tilt for equatorial positioning)
- Expected 25-year performance: 80% output retention post-year-1

### 2.2 Solar Inverter Specification

A hybrid inverter (DC-to-AC conversion + battery charging control) ensures real-time load matching and battery charge/discharge optimization:

- **Recommended Model:** 5 kVA hybrid inverter (e.g., Growatt SPH 5000 or equivalent)
  - **Input Rating:** 3–4 kW DC solar input with MPPT (Maximum Power Point Tracking)
  - **Output:** 5 kVA @ 230V, 50 Hz (Nigeria standard)
  - **Efficiency:** ≥95% DC-to-AC conversion
  - **Features:** Real-time monitoring, remote API access, islanding capability during grid failure
  - **Cost:** ~₦800,000–1,000,000 (~US$2,100–2,600)

---

## 3. Battery Energy Storage System (BESS)

### 3.1 Battery Chemistry & Sizing

**Selected Technology:** Lithium Iron Phosphate (LiFePO₄)
- **Rationale:** 4,000–6,000 cycle lifespan (~12–15 years), superior thermal stability (critical in tropical climates), minimal maintenance, no memory effect, and 95% usable capacity vs. lead-acid's 50%.

**Capacity Calculation:**
- Daily demand: 12.6 kWh
- Reserve days (grid outage contingency): 3 days (aligned with Nigeria's typical outage duration)
- Depth of Discharge (DoD): 80% (LiFePO₄ safety margin; 100% DoD degrades cycle life)
- **Required Total Capacity:** (12.6 × 3) ÷ 0.80 = **47.25 kWh**

**Recommended Configuration:** 48 kWh in 16 kWh modular units (e.g., BYD Battery-Box Premium or Growatt Infinity)
- **Total Investment:** 3 × 16 kWh units = 48 kWh
- **Cost:** ~₦12,000,000–15,000,000 (~US$31,000–39,000)

### 3.2 Battery Management System (BMS)

- **Integrated BMS:** Monitor cell voltage, temperature, current flow; enforce charge/discharge limits
- **Redundant Monitoring:** IoT telemetry sending battery state-of-charge (SOC), health metrics, and anomaly alerts to cloud dashboard
- **Thermal Management:** Active cooling via cabinet ventilation (10°C–40°C operating range); passive heat dissipation via aluminum enclosure

---

## 4. System Architecture & Control Logic

### 4.1 Hybrid Power Flow Priority

The inverter-controller implements a layered dispatch logic:

1. **Solar Priority (06:00–18:00):** All daytime loads draw from PV first; excess charges battery
2. **Battery Discharge (18:00–06:00):** Islanded operation; battery supplies all loads at configurable discharge rates
3. **Grid Fallback (if available):** Grid input powers loads + battery charging if solar insufficient
4. **Load Shedding (emergency):** If battery SOC < 10%, non-critical loads disconnect (e.g., WiFi AP, LED displays); gateway + sensors prioritized
5. **Blackstart Capability:** If grid + solar fail simultaneously, battery cold-starts inverter and critical systems

### 4.2 Smart Energy Management Rules

```
IF (Time = 06:00 AND Solar_Irradiance > 500 W/m²):
    Enable_Sensor_Polling_Interval = 5 min (normal)
    Drone_Charging_Window_Open = TRUE
ELSE IF (18:00 < Time < 06:00) AND (Battery_SOC > 50%):
    Enable_Sensor_Polling_Interval = 15 min (reduced)
    Drone_Charging_Window_Open = FALSE
ELSE IF (Battery_SOC < 30%):
    Load_Shed_Non_Critical = TRUE
    Alert_Cloud_Dashboard: "Power critical"
ELSE IF (Battery_SOC < 10%):
    Emergency_Shutdown_Sequence: Graceful shutdown over 2 minutes
```

---

## 5. Runtime Estimation & Resilience Metrics

### 5.1 Autonomy Duration

**Scenario A: Sunny Day (3.87 kWh solar generation)**
- Solar generation: 3.87 kWh → covers full 12.6 kWh demand with battery top-up
- **Status:** Full autonomy; battery SOC stable or increasing

**Scenario B: Cloudy Day (50% solar reduction)**
- Solar generation: 1.94 kWh → deficit = 12.6 − 1.94 = 10.66 kWh from battery
- Battery capacity: 48 kWh at 80% DoD = 38.4 kWh usable
- **Autonomy:** 38.4 ÷ 10.66 = **3.6 days of cloudy weather** before grid/manual intervention required

**Scenario C: Complete Grid/Solar Failure**
- Battery only: 48 kWh at 50% critical-load profile (120W avg) = 200 hours = **8.3 days** to near-depletion
- With load shedding (80W minimum): 48 ÷ (80/1000) = **600 hours = 25 days**

### 5.2 System Availability Guarantee

- **Designed Uptime:** 99.5% (≤36 minutes monthly downtime)
- **Battery MTBF:** 15 years (6,000 cycles @ daily discharge)
- **Inverter MTBF:** 10 years
- **PV Degradation:** <1% annually post-first-year

---

## 6. Sustainability & NDPR Compliance

### 6.1 Environmental Impact

- **Annual CO₂ Offset:** ~4.6 metric tons (vs. diesel generator equivalent)
- **Renewable Energy Fraction:** 95%+ (grid supplementary only)
- **Water Usage:** Zero (vs. 450L/kWh for coal-fired alternatives)

### 6.2 Data Protection Alignment

- **Battery telemetry anonymized:** Only SOC %, temperature, and alarm flags transmitted; no farm-identifier metadata
- **API Access Control:** OAuth 2.0 token-based access; battery data endpoint restricted to authenticated farm admin role
- **Audit Trail:** All charge/discharge decisions logged with timestamp, decision rule, and operator ID (if manual)

---

## 7. Cost-Benefit Analysis (5-Year Outlook)

| Component | Cost (₦) | Cost (US$) | Lifespan |
|-----------|----------|-----------|----------|
| 3.4 kW Solar Array | 1,200,000 | 3,100 | 25 years |
| Hybrid Inverter (5 kVA) | 900,000 | 2,330 | 10 years |
| 48 kWh Battery (LiFePO₄) | 14,000,000 | 36,200 | 15 years |
| Installation & Wiring | 800,000 | 2,070 | — |
| **Total CAPEX** | **16,900,000** | **43,700** | — |
| Annual Grid Cost Avoided | ~2,500,000 | ~6,480 | — |
| **5-Year ROI** | 12,500,000 saved | 32,400 saved | **73.9% payback** |

---

## 8. Recommendations & Implementation Roadmap

1. **Phase 1 (Month 1–2):** Install 3.4 kW PV + hybrid inverter + 16 kWh initial battery module
2. **Phase 2 (Month 3–4):** Expand to 48 kWh full BESS; commission energy dashboard
3. **Phase 3 (Month 5–6):** Integrate predictive load forecasting (ML model on edge); optimize charge cycles
4. **Ongoing:** Annual panel cleaning, biennial BMS firmware updates, 5-yearly inverter inspection

---

## Conclusion

AgroLink Nigeria's power resilience plan delivers sustainable, autonomous operation for smart-farming IoT infrastructure across rural deployment zones. The 3.4 kW solar + 48 kWh lithium battery architecture guarantees 99.5% uptime, ≥8 days autonomy during dual failures, and carbon-neutral operation aligned with Nigeria's renewable energy commitment and NDPR data-protection standards. Cost recovery within 5 years and operational lifetime of 12–15 years position this as a transformational investment in rural agricultural digitization.

---

## References

- IRENA (2022). "Renewable Power Generation Costs in 2022." International Renewable Energy Agency.
- BYD & Growatt. Technical datasheets: LiFePO₄ battery systems, hybrid inverters.
- Nigeria Data Protection Regulation (NDPR) 2019, Section 4.2: Data Subject Rights & Consent.
- IEEE 1547: Standard for Interconnection and Interoperability of Distributed Energy Resources.

