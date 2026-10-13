# AirAsia Cash-Flow Stress Lab

Standalone, mobile-first educational simulator for **AirAsia Group Berhad (Bursa Malaysia: 5238)**. It models quarterly cash flow with two sliders:

- **Jet fuel price:** US$20–220 per barrel.
- **Quarterly revenue:** RM2–8 billion.

The model adjusts jet fuel consumption along with revenue as a proxy for flight volume, and adjusts specified portions of airport charges, maintenance, staff and other expenses. It displays modeled EBITDA, cash after lease repayments/interest/debt-principal/capex, the break-even jet fuel price, break-even quarterly revenue, a refinancing comparison, cash bridge and response chart.

## Run locally

Open `index.html` in any current browser. The simulator is self-contained: no packages, tracking, subscription, API calls or back end.

## Deploy on GitHub Pages

1. Create a **public** GitHub repository named `airasia-cashflow-simulator` (or another name).
2. Add `index.html` and this `README.md` to the main branch.
3. Navigate to **Settings → Pages → Build and deployment**.
4. Select **Deploy from a branch**, `main`, `/(root)`, **Save**.
5. Once deployed, use the HTTPS URL shown under GitHub Pages (normally `https://USERNAME.github.io/airasia-cashflow-simulator/`).

## Model baseline and formula

Reported Q2 2026 (RM billion):

| Input | Reported |
|---|---:|
| Revenue | 5.085584 |
| Fuel expense | 2.796020 |
| Average jet fuel | US$183/bbl |
| Staff | 0.563000 |
| Aircraft maintenance and overhaul | 0.537342 |
| Airport/user charges | 0.506509 |
| Aircraft operating lease expense | 0.019873 |
| Other operating expenses | 0.269503 |
| Other income | 0.049252 |
| EBITDA | 0.442589 |

For assumed quarterly revenue `R` in RM billion and fuel price `P` in US$/barrel:

```
volume_factor = R / 5.085584
fuel = 2.796020 * (P / 183) * volume_factor
staff = 0.563000 * (0.80 + 0.20 * volume_factor)
maintenance = 0.537342 * (0.70 + 0.30 * volume_factor)
airport_charges = 0.506509 * volume_factor
other_opex = 0.269503 * (0.60 + 0.40 * volume_factor)
EBITDA = R - fuel - staff - maintenance - airport_charges - 0.019873 - other_opex + 0.049252
net_cash = EBITDA - cash_interest - lease_repayments - debt_principal_repayments - cash_tax - sustaining_capex
```

Costs after EBITDA (RM billion per quarter), using half of reported 1H2026 cash flows except as identified:

- Interest paid: `0.254894 / 2`.
- Lease liability repayments: `1.207371 / 2`.
- Bank/debenture principal repayments: `(1.050152 + 0.147170) / 2`.
- Taxes: `0.020558 / 2`.
- Sustaining capex: **0.150** — illustrative assumption, not company disclosure.

**Important:** The price-volume mechanism assumes constant revenue per unit of flight activity. In reality, fares, ancillary revenue, load factors, aircraft utilization, fuel burn and exchange rates move independently. Reported cash maintenance cost is embedded in EBITDA; extraordinary settlements of maintenance provisions, working capital, and additional aircraft purchases are not separately modeled. Historical debt principal repayments may not repeat; the refinancing scenario is illustrative.

### Baseline verification

- At US$183/barrel and RM5.10B revenue, modeled cash is approximately **−RM1.044B/quarter**.
- Break-even fuel price is approximately **US$115/barrel** if historical bank/debenture principal repayments recur.
- Break-even revenue is approximately **RM8.89B/quarter** at US$183/barrel (outside the slider range).
- If bank/debenture principal is fully refinanced, the modeled fuel ceiling at baseline revenue rises to approximately **US$154/barrel**.

**Source:** [AirAsia Group Q2 2026 financial statements](https://investor.airasiagroup.com/newsroom/2026_Q2_Aviation_Group_Financial_Statements_13082026.pdf).

This is an **independent financial sensitivity tool**; it is **not an AirAsia forecast or investment recommendation**.