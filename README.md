# RCC Frame Analysis of a G+4 Residential Building — Guwahati (STAAD.Pro)

**Model / Job Name:** `CE4102P_G+4_Residential_Building`
**File:** `CE4102P_GPlus4_Building_FINAL.std`
**Software:** STAAD.Pro (STAAD SPACE input file)
**Design Basis:** Indian Standards (IS 456, IS 875 Part 3:2015)
**Location:** Guwahati, Assam, India

---

## 1. Project Overview

This project is a **3D structural analysis model of a Ground + 4 storey (G+4) reinforced concrete residential building**, built and analyzed in STAAD.Pro. The model captures the complete space frame of the building — columns, beams, and stair/lift-core slab panels — and subjects it to gravity (dead + live), wall, wind, and modal/dynamic load cases in accordance with Indian design codes.

The `.std` file is a plain-text STAAD input file: it defines geometry, member/element properties, material, supports, loads, load combinations, and analysis commands that STAAD.Pro reads and solves.

### 1.1 Objectives

- Model the complete gravity and lateral load-resisting RCC frame of a G+4 residential building.
- Assign realistic member sizes (columns, beams) and slab elements for the stair/lift core.
- Apply Indian-code-based loading:
  - Self-weight and superimposed dead loads (walls, slab finishes)
  - Floor live loads (residential occupancy)
  - Wind loads per **IS 875 (Part 3):2015** for Guwahati (basic wind speed 50 m/s)
  - A modal/dynamic (natural frequency) load case for seismic mass evaluation
- Generate serviceability and ultimate limit-state load combinations (DL+LL, 1.5(DL+LL), and wind combinations).
- Run linear static analysis and modal analysis to obtain:
  - Support reactions
  - Member forces (axial, shear, bending)
  - Mode shapes / natural frequencies
- Provide a base analytical model that can later be extended with **concrete design (IS 456) code-checking** to obtain reinforcement details.

### 1.2 What This Model Currently Does *Not* Include

- No `DESIGN CONCRETE` / `CODE CHECK` commands — this is an **analysis-only** model. Reinforcement/bar-bending design has not yet been performed.
- No explicit static seismic lateral load case (per IS 1893) — only a modal/natural-frequency case (`LOAD 14`) is defined for dynamic characteristics.
- No foundation/footing design — supports are modeled as `FIXED` at plinth level.

---

## 2. Building Description

| Parameter | Value |
|---|---|
| Configuration | Ground + 4 upper floors (G+4), plus terrace/roof |
| Structure type | RCC framed structure (STAAD SPACE / 3D space frame) |
| Plan dimensions | 17.5 m × 11.0 m |
| Bay spacing (X-direction) | 4.5 m – 2.0 m – 4.5 m – 2.0 m – 4.5 m (5 bays; the 2.0 m bays house the stair/lift core) |
| Bay spacing (Z-direction) | 5.5 m – 5.5 m (2 bays) |
| Floor-to-floor height | 3.0 m (typical) |
| Base/plinth level | Y = 0.6 m (supports fixed here) |
| Total height (plinth to terrace) | 18.0 m |
| Slab (stair/lift core landings) | 125 mm thick RCC plate elements |
| Typical floor slab (assumed) | 125 mm RCC + finishes |

### 2.1 Member Sizing

| Member Group | Members | Section (Depth × Width) |
|---|---|---|
| Lower-storey columns | 1 to 54 | 600 mm × 300 mm |
| Upper-storey columns | 55–90, 253–270 | 450 mm × 300 mm |
| Beams (all floors) | 106–180, 193–252, 271–297 | 450 mm × 230 mm |
| Stair/lift core landing slabs | 501–524 | 125 mm thick plate elements |

### 2.2 Material Properties (Concrete)

| Property | Value |
|---|---|
| Grade (from FCU) | M25 (characteristic strength 25 MPa) |
| Modulus of Elasticity, E | 2.5 × 10⁷ kN/m² |
| Poisson's Ratio | 0.17 |
| Density | 24 kN/m³ |
| Coefficient of thermal expansion (α) | 1 × 10⁻⁵ /°C |
| Damping ratio | 0.05 |

---

## 3. Loads and Load Cases

| Load No. | Type | Description |
|---|---|---|
| 1 | Dead | Self-weight of frame members (Y = -1) |
| 2 | Dead | Wall loads on beams — UDL of 13.2 kN/m (main walls) and 7.6 kN/m (partition walls) |
| 3 | Live | Floor live load — residential bays (1.5–3.0 kN/m²) and terrace (0.75 kN/m²) |
| 6 | Wind | Wind load in +X direction (IS 875 Part 3:2015) |
| 7 | Wind | Wind load in −X direction |
| 8 | Wind | Wind load in +Z direction |
| 9 | Wind | Wind load in −Z direction |
| 14 | Seismic-H (Modal) | Self-weight-based mass load case with `MODAL CALCULATION REQUESTED`, for natural frequency / mode shape analysis |
| 15 | Dead | Slab dead load — self-weight + finishes (4.13 kN/m² typical floors, 4.63 kN/m² terrace) |

**Wind load parameters (IS 875 Part 3:2015):** Basic wind speed 50 m/s, terrain/risk/topography factors as per code, location set to Guwahati, building height profile defined at 0, 5, 10, 15, and 18.6 m.

### 3.1 Load Combinations

| Comb. No. | Description | Factors |
|---|---|---|
| 4 | Service: DL + LL | 1(1) + 2(1) + 3(1) + 15(1) |
| 5 | Ultimate: 1.5(DL + LL) | 1(1.5) + 2(1.5) + 3(1.5) + 15(1.5) |
| 10 | Ultimate + Wind +X | 1(1.2) + 2(1.2) + 3(1.2) + 15(1.2) + 6(1.2) |
| 11 | Ultimate + Wind +Z | 1(1.2) + 2(1.2) + 3(1.2) + 15(1.2) + 8(1.2) |
| 12 | Ultimate + Wind −X | 1(1.2) + 2(1.2) + 3(1.2) + 15(1.2) + 7(1.2) |
| 13 | Ultimate + Wind −Z | 1(1.2) + 2(1.2) + 3(1.2) + 15(1.2) + 9(1.2) |

### 3.2 Analysis Commands Run

- `PERFORM ANALYSIS PRINT ALL` — linear static analysis with full output
- `PRINT MODE SHAPES` — mode shapes from modal analysis
- `PRINT SUPPORT REACTION` — reactions at the 18 fixed base joints (101–118)
- `PRINT MEMBER FORCES ALL` — axial force, shear, and bending moment for all members
- `LOAD LIST 1 TO 15` — all 9 primary/derived load cases and combinations considered in output

---

## 4. Prerequisites

To open, edit, or re-analyze this model you need:

- **STAAD.Pro** (CONNECT Edition or later recommended). A trial/student license from Bentley Systems will open `.std` files.
- Windows OS (STAAD.Pro is Windows-only; use a VM or Bentley's cloud offering if on macOS/Linux).
- Basic familiarity with STAAD.Pro's Graphical User Interface (GUI) and Indian design codes (IS 456, IS 875, IS 1893) is recommended for interpreting/modifying results.

No installation of external libraries or software is needed beyond STAAD.Pro itself — the `.std` file is a self-contained plain-text input file.

---

## 5. Setup Instructions

### 5.1 Opening the Model

1. Install **STAAD.Pro** if not already installed (Bentley Systems).
2. Launch STAAD.Pro.
3. From the **File** menu, choose **Open**.
4. Browse to and select `CE4102P_GPlus4_Building_FINAL.std`.
5. The model will load directly into the STAAD.Pro GUI, showing the 3D frame geometry, member properties, and defined loads.

> Alternatively, you can double-click the `.std` file directly if STAAD.Pro is registered as the default handler for `.std` files on your system.

### 5.2 Running the Analysis

1. Once the file is open, go to the **Analysis/Print** tab (or use the **Analyze** menu).
2. Confirm the following are already defined (they are pre-set in this file, no changes needed to just re-run):
   - Material: Concrete (`ISOTROPIC CONCRETE`)
   - Supports: Joints 101–118, `FIXED`
   - Load cases 1–15 and combinations 4, 5, 10–13
3. Click **Run Analysis** (or **Analyze > Run Analysis**).
4. STAAD.Pro will compile the input, solve the stiffness matrix, and generate an output file (`.anl`) alongside the `.std` file.
5. Review analysis warnings/errors in the STAAD Analysis window before proceeding — resolve any instability or singularity warnings first.

### 5.3 Viewing Results

After a successful run, use the **Post Processing** mode in STAAD.Pro to review:

- **Support reactions** — under Node > Reactions (matches `PRINT SUPPORT REACTION`)
- **Member forces** — under Beam > Forces (matches `PRINT MEMBER FORCES ALL`)
- **Mode shapes / natural frequencies** — under Dynamic tab (matches `PRINT MODE SHAPES`, driven by Load 14)
- **Deflected shapes** and **Bending Moment / Shear Force diagrams** for each load combination (4, 5, 10–13)

### 5.4 Editing the Model

The `.std` file can also be edited directly as plain text (e.g., in the STAAD.Pro **Input File** editor, or any text editor) if you need to:

- Adjust member sizes (`MEMBER PROPERTY INDIAN` block)
- Modify material grade (`DEFINE MATERIAL START...END DEFINE MATERIAL`)
- Change load magnitudes (`LOAD 1`–`LOAD 15` blocks)
- Add/modify load combinations (`LOAD COMB` blocks)
- Add concrete design commands (e.g., `START CONCRETE DESIGN ... DESIGN BEAM ALL ... DESIGN COLUMN ALL ... END CONCRETE DESIGN`) to extend the model into a full RCC design check

After editing, re-open the file in STAAD.Pro and re-run the analysis (Section 5.2).

---

## 6. Project File Structure

```
├── CE4102P_GPlus4_Building_FINAL.std   # Main STAAD.Pro input file (geometry, loads, analysis)
└── README.md                            # This file
```

> Note: Running the analysis in STAAD.Pro will generate additional output files (e.g., `.anl`, `.std.bak`, database folders) in the same directory. These are analysis artifacts and are not required to share the model — only the `.std` file is needed to reproduce results.

---

## 7. Suggested Next Steps

- [ ] Add `START CONCRETE DESIGN` block to perform IS 456-based beam and column design (reinforcement detailing).
- [ ] Add static seismic load cases per **IS 1893 (Part 1)** if seismic zone factor for Guwahati (high seismic Zone V) needs to be explicitly combined with gravity loads.
- [ ] Verify assumed slab thickness (125 mm) and finish loads against the actual architectural drawing/design brief (flagged in the file's own comments).
- [ ] Cross-check wind load parameters (terrain category, risk coefficient, topography factor) against the project's site-specific data.
- [ ] Perform a P-Delta or second-order analysis if drift/stability under wind is a governing concern for an 18 m tall structure.
