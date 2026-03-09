# Manufacturing Planning & Simulation  
## MPS-MRP Design Under Demand Uncertainty

---

## Project Overview

This project analyzes a **single-line manufacturing planning and simulation game** focused on integrating:

- Aggregate Planning  
- Master Production Scheduling (MPS)  
- Material Requirements Planning (MRP)  
- Real-time production replanning during simulation  

The objective was to design a feasible production and material plan based on forecast demand, then adapt it dynamically as actual demand was revealed period by period.

The project combines **requirement-based planning** with **simulation-based operational decision-making**, showing how production plans break down and must be revised when actual demand diverges from forecasts.

---

## Manufacturing System Context

The company manufactures **three finished products** on a **single assembly line**:

- Product A  
- Product B  
- Product C  

Each product requires **two components**, and all parts are stored in a **components warehouse** before assembly. Finished goods are then stored in a **finished products warehouse** and sold to the market.

### Bill of Materials (BOM)

| Product | Components |
|--------|------------|
| A | Blue + Green |
| B | Pink + Green |
| C | Blue + Yellow |

### Component Lead Times

| Component | Lead Time |
|----------|-----------|
| Blue | 1 period |
| Green | 1 period |
| Yellow | 1 period |
| Pink | 2 periods |

The longer lead time of the pink component creates a planning constraint for Product B and makes material planning more sensitive to forecast error.

---

## Planning Logic

The production system follows a **Make-to-Stock (MTS)** logic driven by demand forecasts.

The planning process is structured into three levels:

1. **Aggregate Plan**  
2. **Master Production Schedule (MPS)**  
3. **Material Requirements Plan (MRP)**  

This hierarchy reflects the classic requirement-based production planning cycle:

- Aggregate planning  
- Disaggregation into MPS  
- Explosion into material requirements  

---

## 1. Aggregate Planning

The first planning layer requires building a **quarterly aggregate plan** for one full year.

### Aggregate Forecast Demand

| Quarter | Forecast Demand |
|--------|-----------------|
| I | 240 |
| II | 600 |
| III | 360 |
| IV | 240 |

### Aggregate Planning Parameters

- Average material cost per product unit: **15**
- Average labor units per product unit: **3**
- Regular time labor cost per unit: **10**
- Overtime labor cost per unit: **15**
- Average selling price per product unit: **70**
- Inventory holding cost per product unit per quarter: **6**
- Initial inventory acquisition cost per product unit: **45**

### Aggregate Plan Rules

- Initial inventory cannot exceed **300 units**
- Production must meet or exceed forecast demand
- Regular time capacity is defined by the **maximum requirement across quarters**
- Overtime cannot exceed **50% of regular time capacity**
- Ending inventory must be at least equal to initial inventory

This level determines the structural production capacity that constrains the lower-level plans.

---

## 2. Master Production Schedule (MPS)

The second planning layer translates the aggregate plan into a detailed product-level schedule.

Each quarter contains **6 periods**, and the MPS is developed for the first **12 periods**.

### Product Parameters

| Product | Labor Units | Inventory Cost / Unit / Period | Initial Acquisition Cost | Material Cost | Selling Price |
|--------|-------------|--------------------------------|--------------------------|--------------|--------------|
| A | 2 | 0.8 | 30 | 10 | 55 |
| B | 4 | 1.2 | 60 | 20 | 85 |
| C | 3 | 1.0 | 45 | 15 | 70 |

### Demand Mix

| Product | Mix | Avg/Period Q1 | Avg/Period Q2 | Avg/Period Q3 | Avg/Period Q4 | Coefficient of Variation |
|--------|-----|---------------|---------------|---------------|---------------|--------------------------|
| A | 0.2 | 8 | 20 | 12 | 8 | 0.40 |
| B | 0.2 | 8 | 20 | 12 | 8 | 0.40 |
| C | 0.6 | 24 | 60 | 36 | 24 | 0.25 |

### MPS Rules

- Initial inventory mix can be chosen freely
- Total initial finished goods inventory cannot exceed **300 units**
- Regular time is leveled across periods
- Overtime can be allocated by period
- Overtime cannot exceed **50% of regular time**
- Changeover costs must be considered

### Setup Cost

- Changeover cost: **40**

This step converts aggregate volume decisions into product-level production timing while balancing demand mix, setup costs, and capacity constraints.

---

## 3. Material Requirements Planning (MRP)

The third planning layer translates the MPS into part-level order release and receipt schedules.

### Part Parameters

| Part | Regular Purchase Cost | Inventory Cost / Unit / Period | Lead Time |
|------|------------------------|--------------------------------|-----------|
| Blue | 5 | 0.15 | 1 |
| Green | 5 | 0.15 | 1 |
| Pink | 15 | 0.45 | 2 |
| Yellow | 10 | 0.30 | 1 |

### Additional MRP Parameter

- Order issuing cost: **12 per order**

### MRP Rules

- Order receipts must satisfy net requirements
- Initial inventory can be selected for each part
- Initial inventory cannot exceed **300 units per part**
- Any lot sizing policy may be used

Because pink has a 2-period lead time and higher cost, it becomes the most sensitive material in the system.

---

## Simulation Phase

After creating the planning model, the system is tested in a **12-round simulation**.

In this phase, the forecast-based plan is exposed to actual demand, and players must revise MPS and MRP decisions as new information arrives.

### Actual Demand Revealed During Simulation

| Period | A | B | C |
|--------|---|---|---|
| 1 | 7 | 12 | 25 |
| 2 | 9 | 10 | 26 |
| 3 | 0 | 9 | 23 |
| 4 | 8 | 3 | 27 |
| 5 | 13 | 18 | 11 |
| 6 | 11 | 8 | 20 |
| 7 | 15 | 29 | 67 |
| 8 | 21 | 31 | 45 |
| 9 | 6 | 37 | 44 |
| 10 | 27 | 22 | 37 |
| 11 | 19 | 24 | 73 |
| 12 | 32 | 5 | 65 |

This demand pattern shows strong variability, especially for **Product C**, which creates significant replanning pressure on both production and materials.

---

## Optional Interventions During Simulation

Two emergency interventions are allowed when the original plan becomes infeasible.

### 1. External Workforce
If overtime exceeds the allowed limit, the excess is treated as contract labor.

- External workforce cost: **25 per labor unit**
- Applied on top of standard labor logic

### 2. Expedited Materials
If material requirements are unmet, components can be expedited.

- Expediting cost = **2x regular purchase cost**

These interventions preserve feasibility but reduce net worth.

---

## Performance Logic

The game evaluates performance through cumulative **net worth**.

### Revenue Side

- Product sales revenue
- Inventory credit for finished products
- Inventory credit for components

### Cost Side

#### Production Costs
- Regular workforce cost
- Overtime cost
- External workforce cost
- Setup cost
- Finished goods holding cost

#### Material Costs
- Component acquisition cost
- Expedited acquisition cost
- Order issuing cost
- Component holding cost

### Additional Penalties
- Lost margin due to stockout
- Cost of unutilized production capacity

This makes the game a trade-off between:

- service level
- labor flexibility
- material planning discipline
- inventory exposure
- setup efficiency

---

## Core Analytical Challenge

The project is not just about building a plan. It is about understanding why a feasible deterministic plan becomes fragile under stochastic demand.

The main planning tensions were:

- Capacity leveling vs responsiveness  
- Inventory buffering vs holding cost  
- Setup minimization vs schedule flexibility  
- Early purchasing vs material obsolescence risk  
- Forecast compliance vs real-time replanning  

The pink component was particularly critical because of its **longer lead time**, while Product C drove the largest demand volatility.

---

## Strategic Insights

Several important operational insights emerge from the game.

### 1. Aggregate Feasibility Does Not Guarantee Operational Feasibility
A plan may look feasible at the quarterly level but fail once product mix and lead times are introduced.

### 2. MPS Quality Depends on Mix Awareness
Because Product B and Product C have different labor and material structures, a demand-mix-sensitive MPS performs much better than a simple average-based plan.

### 3. MRP Stability Depends on Lead Time Asymmetry
The 2-period pink lead time requires earlier commitment and reduces flexibility.

### 4. Capacity Buffers Matter
A fully loaded regular-time plan becomes fragile once actual demand departs from forecast.

### 5. Emergency Actions Preserve Service but Destroy Margin
External labor and expediting are useful tactical tools, but they are economically punitive and should not become routine planning substitutes.

---

## Skills Demonstrated

This project demonstrates applied capability in:

- Aggregate Production Planning  
- Master Production Scheduling  
- Material Requirements Planning  
- Bill of Materials Explosion  
- Capacity-Constrained Planning  
- Demand-Driven Replanning  
- Manufacturing Cost Trade-off Analysis  
- Simulation-Based Decision Making  

---

## Academic Context

Politecnico di Milano - Graduate School of Management  
International Master in Digital Supply Chain Management  
Course: **Smart Manufacturing Management**
