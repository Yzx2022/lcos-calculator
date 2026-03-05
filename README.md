# LCOS Calculator (v1.0)


## Model summary

### Definition
The calculator reports:
- **LCOS** in **CNY/kWh** (output-energy basis)
- Present-value cost components and discounted delivered energy

LCOS is computed as:
- **LCOS = (present value of total costs) / (present value of delivered energy)**

### Degradation
A multiplicative per-cycle degradation factor is used:
- `a = 0.8^(1/TN)`  
where `TN` is the cycle life to 80% capacity retention.

### Replacements (key rules)
Let `Ctot = N * Tn` (total equivalent cycles within the project horizon).

- Replacement count:  
  `K = max(0, ceil(Ctot/TN) - 1)`  
  This definition avoids counting a replacement when the project ends exactly at a lifetime boundary.

- Replacement year and in-year replacement point:
  - `t_k = ceil(k*TN/Tn)`
  - `m_k = k*TN - (t_k - 1)*Tn`

### Charging cost and residual value
- Charging cost: `Cchg = (Pel/phi) * Etotal`
- Residual value (credited at end of life): `Crec = (Y * Cinv) / (1+r)^N`

---

## Inputs and units

**System & operation**
- `P` (MW), `E` (MWh), `DOD` (%)
- `Tn` (cycles/year), `N` (years)
- `Pel` (CNY/kWh) charging electricity price

**Finance**
- `X` (%) annual O&M fraction of initial CAPEX
- `r` (%) discount rate
- `Y` (%) residual value fraction of initial CAPEX
- `ηself` (0–1) self-loss factor (currently applied as a multiplicative factor on delivered energy)

**Performance & lifetime**
- `φ` (%) round-trip efficiency
- `TN` (cycles) cycle life to 80% retention

**Costs & replacements**
- `Cpower` (CNY/kW), `Cenergy` (CNY/kWh)
- `Crep_P` (CNY/kW), `Crep_E` (CNY/kWh)
- `i` (%) replacement cost learning rate (applied to replacement costs only)

---

## Outputs

- LCOS (CNY/kWh)
- Discounted delivered energy `Etotal`
- Present value (PV) components: `Cinv`, `COM`, `Crep`, `Cchg`, `Crec`, `Ctotal`
- Replacement schedule: `K`, `t_k`, `m_k`
- Annual energy table (downloadable as CSV)

---

