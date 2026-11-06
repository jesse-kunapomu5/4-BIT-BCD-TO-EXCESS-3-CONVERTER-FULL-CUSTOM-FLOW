
# 4-Bit BCD-to-Excess-3 Code Converter
## Full-Custom CMOS Design Using Cadence Virtuoso – GPDK90

## 1. Project Overview

This project presents the design and implementation of a **4-bit BCD-to-Excess-3 Code Converter** using full-custom CMOS design methodology in Cadence Virtuoso.

The circuit is implemented using transistor-level CMOS logic gates:

- CMOS Inverter (NOT)
- 2-Input CMOS NAND Gate
- 2-Input CMOS NOR Gate

The project covers the complete digital IC design process, starting from Boolean function derivation and transistor-level gate design to schematic integration, transient simulation and full-custom layout.

### Project Specifications

| Parameter | Description |
|---|---|
| Project | 4-Bit BCD-to-Excess-3 Converter |
| EDA Tool | Cadence Virtuoso |
| Technology | GPDK90 |
| Design Style | Full-Custom CMOS |
| Basic Logic Gates | Inverter, NAND2, NOR2 |
| Simulation | Transient Analysis |
| Physical Design | Custom CMOS Layout |
| Verification Tool | Assura DRC/LVS |

---

## 2. Objectives

1. Understand the operation of BCD and Excess-3 codes.
2. Derive Boolean expressions for a 4-bit BCD-to-Excess-3 converter.
3. Design CMOS inverter, NAND and NOR gates at transistor level.
4. Create reusable symbols for the designed logic gates.
5. Integrate the basic gates to implement the complete converter.
6. Perform transient simulation and verify the output waveforms.
7. Implement the complete circuit using full-custom layout methodology.

---

## 3. Theory

### 3.1 Binary-Coded Decimal (BCD)

Binary-Coded Decimal is a numerical representation in which each decimal digit is represented by its equivalent 4-bit binary number.

The four bits have weights 8, 4, 2 and 1.

For example:

- Decimal 5 = 0101
- Decimal 7 = 0111
- Decimal 9 = 1001

There are ten valid BCD combinations, ranging from 0000 to 1001. The remaining six combinations are invalid BCD inputs.

### 3.2 Excess-3 Code

Excess-3 is a non-weighted decimal code obtained by adding binary 0011 (decimal 3) to the corresponding 4-bit BCD number.

For example, consider decimal 5:

```text
BCD             = 0101
Excess-3 value  = 0011
                  ----
Result          = 1000
```

Therefore, the Excess-3 representation of decimal 5 is 1000.

### 3.3 BCD-to-Excess-3 Truth Table

| Decimal | BCD Input (ABCD) | Excess-3 Output (WXYZ) |
|---:|:---:|:---:|
| 0 | 0000 | 0011 |
| 1 | 0001 | 0100 |
| 2 | 0010 | 0101 |
| 3 | 0011 | 0110 |
| 4 | 0100 | 0111 |
| 5 | 0101 | 1000 |
| 6 | 0110 | 1001 |
| 7 | 0111 | 1010 |
| 8 | 1000 | 1011 |
| 9 | 1001 | 1100 |

Inputs 1010 to 1111 are invalid BCD combinations and are treated as don't-care conditions during Boolean minimization.

---

## 4. Boolean Expression Derivation

Let A, B, C and D represent the four BCD input bits.

Let W, X, Y and Z represent the four Excess-3 output bits.

The minimized Boolean expressions are:

### Output W

W = A + BC + BD

### Output X

X = B'C + B'D + BC'D'

### Output Y

Y = CD + C'D'

### Output Z

Z = D'

Where:

- '+' represents the OR operation.
- Multiplication represents the AND operation.
- Apostrophe (') represents logical complementation.

### Simplified Expressions

Let:

S = C + D

S' = (C + D)'

Then:

W = A + BS

X = B'S + BS'

Y = CD + C'D'

Z = D'

These functions are implemented using only 2-input NAND, 2-input NOR and inverter cells.

---

## 5. CMOS Basic Logic Gate Design

Static CMOS logic uses complementary PMOS and NMOS transistor networks to implement logic functions.

### 5.1 CMOS Inverter

The CMOS inverter performs the NOT operation.

Boolean expression:

Y = A'

It consists of:
- One PMOS transistor.
- One NMOS transistor.

The PMOS transistor forms the pull-up network, while the NMOS transistor forms the pull-down network.

When the input is LOW, the PMOS turns ON and the output becomes HIGH.

When the input is HIGH, the NMOS turns ON and the output becomes LOW.

#### Transistor-Level Schematic

![CMOS Inverter Schematic](schematics/cmos_inverter_transistor_schematic.png)

**Explanation:**

The above screenshot shows the transistor-level schematic of the CMOS inverter designed in Cadence Virtuoso using GPDK90 technology. The complementary operation of PMOS and NMOS transistors provides the inverted output.

#### Inverter Symbol

![Inverter Symbol](symbols/inverter_symbol.png)

**Explanation:**

This screenshot shows the symbol generated from the inverter schematic. The symbol is used as a reusable hierarchical cell in the complete BCD-to-Excess-3 converter.

---

### 5.2 CMOS 2-Input NAND Gate

The NAND gate performs the complement of the AND operation.

Boolean expression:

Y = (A.B)'

It consists of:
- Two PMOS transistors connected in parallel.
- Two NMOS transistors connected in series.

The output becomes LOW only when both inputs are HIGH. For all other input combinations, the output remains HIGH.

#### Transistor-Level Schematic

![CMOS NAND Schematic](schematics/cmos_nand2_transistor_schematic.png)

**Explanation:**

The screenshot shows the transistor-level implementation of the 2-input CMOS NAND gate. The PMOS pull-up network is connected in parallel, whereas the NMOS pull-down network is connected in series.

This gate is used extensively in the BCD-to-Excess-3 converter to implement the required Boolean functions.

#### NAND Symbol

![NAND Symbol](symbols/nand2_symbol.png)

**Explanation:**

This is the reusable symbol generated from the NAND transistor-level schematic. It is instantiated multiple times in the top-level converter circuit.

---

### 5.3 CMOS 2-Input NOR Gate

The NOR gate performs the complement of the OR operation.

Boolean expression:

Y = (A+B)'

It consists of:
- Two PMOS transistors connected in series.
- Two NMOS transistors connected in parallel.

The output becomes HIGH only when both inputs are LOW.

#### Transistor-Level Schematic

![CMOS NOR Schematic](schematics/cmos_nor2_transistor_schematic.png)

**Explanation:**

This screenshot shows the transistor-level implementation of the 2-input CMOS NOR gate in Cadence Virtuoso.

The PMOS transistors are connected in series to form the pull-up network, while the NMOS transistors are connected in parallel to form the pull-down network.

#### NOR Symbol

![NOR Symbol](symbols/nor2_symbol.png)

**Explanation:**

This screenshot shows the symbol generated from the NOR transistor-level schematic. The symbol is used in the complete converter circuit to generate the complemented intermediate signal.

---

## 6. Complete BCD-to-Excess-3 Converter Schematic

The complete converter is constructed using the previously designed CMOS inverter, NAND and NOR gates.

The intermediate logic signals are:

```text
A_bar = INV(A)
B_bar = INV(B)
C_bar = INV(C)
D_bar = INV(D)

S_bar = NOR(C,D)
S     = INV(S_bar)

N1 = NAND(B,S)
W  = NAND(A_bar,N1)

N2 = NAND(B_bar,S)
N3 = NAND(B,S_bar)
X  = NAND(N2,N3)

N4 = NAND(C,D)
N5 = NAND(C_bar,D_bar)
Y  = NAND(N4,N5)

Z = D_bar
```

### Complete Circuit Schematic

![BCD to Excess-3 Complete Schematic](schematics/bcd_to_excess3_top_level_schematic.png)

**Explanation:**

The screenshot represents the complete 4-bit BCD-to-Excess-3 converter designed using the inverter, NAND and NOR cells.

The circuit accepts four input bits A, B, C and D and produces four output bits W, X, Y and Z.

The individual gate symbols are interconnected according to the derived Boolean expressions.

This hierarchical design approach makes the complete circuit easier to understand, debug and modify.

---

## 7. Simulation and Functional Verification

Transient analysis is performed to verify the functionality of the complete converter.

Four pulse sources are connected to the input terminals A, B, C and D.

The pulse sources are configured to generate the required binary input combinations.

### 7.1 Input A Pulse Configuration

![Input A Pulse Configuration](simulations/input_A_pulse_configuration.png)

**Explanation:**

This screenshot shows the pulse-source configuration for input A, which represents the most significant bit of the BCD input.

Its pulse period is selected to generate the required binary counting sequence.

### 7.2 Input B Pulse Configuration

![Input B Pulse Configuration](simulations/input_B_pulse_configuration.png)

**Explanation:**

This screenshot represents the pulse-source configuration for input B.

The timing of this input is selected to generate the appropriate binary input combinations during transient simulation.

### 7.3 Input C Pulse Configuration

![Input C Pulse Configuration](simulations/input_C_pulse_configuration.png)

**Explanation:**

This screenshot shows the pulse-source configuration for input C.

It generates the third bit of the binary counting sequence and contributes to testing the converter's functionality.

### 7.4 Input D Pulse Configuration

![Input D Pulse Configuration](simulations/input_D_pulse_configuration.png)

**Explanation:**

This screenshot shows the pulse-source configuration for input D, which is the least significant bit of the BCD input.

It toggles at the highest frequency in the binary counting sequence.

### 7.5 Combined Input and Output Waveforms

![Combined Simulation Waveforms](simulations/combined_input_output_waveforms.png)

**Explanation:**

This screenshot shows the transient simulation waveforms of the four input signals and four output signals.

The input signals A, B, C and D generate the BCD combinations, while W, X, Y and Z represent the corresponding Excess-3 outputs.

The output waveforms are compared with the expected truth table to verify the functionality of the converter.

Propagation delays and transition intervals can be observed because the circuit is implemented using actual CMOS transistor-level gates.

### 7.6 Additional Simulation Waveform

![Additional Simulation Waveform](simulations/additional_waveform_view.png)

**Explanation:**

This screenshot provides an additional view of the transient simulation results and helps inspect the input/output signal transitions.

It serves as supplementary evidence of the circuit simulation.

---

## 8. Full-Custom Layout Design

After completing the schematic and simulation, the physical layout of the BCD-to-Excess-3 converter is created using the GPDK90 technology.

The layout represents the physical implementation of the MOS transistors and their interconnections.

### Final Layout

![Full Custom Layout](layout/full_custom_layout.png)

**Explanation:**

The screenshot shows the full-custom layout of the 4-bit BCD-to-Excess-3 converter designed in Cadence Virtuoso.

The layout contains the physical transistor structures, diffusion regions, polysilicon connections, contacts and metal interconnections.

The PMOS and NMOS devices are placed according to the requirements of the selected technology.

The physical layout is intended to represent the same circuit functionality as the corresponding schematic.

### Important Layout Considerations

- Proper placement of PMOS and NMOS transistors.
- Correct N-well and substrate connections.
- Appropriate power and ground routing.
- Proper contact and via placement.
- Compliance with minimum width and spacing rules.
- Correct connectivity between schematic and layout.

---

## 9. Physical Verification

Physical verification is an important stage of full-custom VLSI design.

Two major verification procedures are:

### 9.1 Design Rule Check (DRC)

DRC checks whether the physical layout satisfies the manufacturing design rules specified by the technology.

It checks parameters such as:
- Minimum metal width.
- Minimum spacing.
- Contact and via enclosure.
- Diffusion spacing.
- Poly spacing.
- Well and implant rules.

**Expected result:** Zero DRC violations.

Add the Assura DRC result screenshot here after completing verification.

```text
verification/assura_drc_results.png
```

### 9.2 Layout Versus Schematic (LVS)

LVS verifies whether the physical layout corresponds to the original schematic.

It compares:
- MOS transistor types.
- Transistor connectivity.
- Device dimensions.
- Circuit terminals.
- Overall electrical connectivity.

**Expected result:** Successful LVS match.

Add the Assura LVS result screenshot here after completing verification.

```text
verification/assura_lvs_results.png
```

**Note:** DRC and LVS results should only be reported as successful after the corresponding Assura checks have been executed and passed.

---

## 10. Design Flow

The complete project follows these steps:

1. Study of BCD and Excess-3 number systems.
2. Derivation of Boolean expressions.
3. Design of CMOS inverter.
4. Design of CMOS 2-input NAND gate.
5. Design of CMOS 2-input NOR gate.
6. Creation of reusable gate symbols.
7. Integration of gates into the complete converter schematic.
8. Configuration of input pulse sources.
9. Transient simulation.
10. Verification of input and output waveforms.
11. Full-custom layout implementation.
12. Assura DRC verification.
13. Assura LVS verification.

---

## 11. Repository Structure

```text
BCD-to-Excess-3-Converter/
│
├── README.md
│
├── schematics/
│   ├── cmos_inverter_transistor_schematic.png
│   ├── cmos_nand2_transistor_schematic.png
│   ├── cmos_nor2_transistor_schematic.png
│   └── bcd_to_excess3_top_level_schematic.png
│
├── symbols/
│   ├── inverter_symbol.png
│   ├── nand2_symbol.png
│   └── nor2_symbol.png
│
├── simulations/
│   ├── input_A_pulse_configuration.png
│   ├── input_B_pulse_configuration.png
│   ├── input_C_pulse_configuration.png
│   ├── input_D_pulse_configuration.png
│   ├── combined_input_output_waveforms.png
│   └── additional_waveform_view.png
│
├── layout/
│   └── full_custom_layout.png
│
└── verification/
    ├── assura_drc_results.png
    └── assura_lvs_results.png
```

---

## 12. Conclusion

The 4-bit BCD-to-Excess-3 code converter was designed and implemented using full-custom CMOS design methodology in Cadence Virtuoso with GPDK90 technology.

The project involved the transistor-level design of CMOS inverter, NAND and NOR gates, followed by the creation of reusable symbols and integration into the complete converter schematic.

Transient simulation was performed using pulse inputs to verify the conversion functionality. The corresponding full-custom physical layout was also created.

This project demonstrates the practical implementation of digital logic circuits using CMOS technology and provides experience in transistor-level schematic design, hierarchical circuit integration, simulation and physical layout.

---

## 13. Tools and Technologies Used

- **Cadence Virtuoso:** Schematic and layout design.
- **GPDK90:** CMOS technology library.
- **CMOS Technology:** Transistor-level implementation.
- **Transient Analysis:** Functional simulation.
- **Assura:** DRC and LVS physical verification.

---

## Author

**Jesse Vijay Kumar**

M.Tech – VLSI and Microelectronics

IIITDM Kurnool

**Project Title:** Full-Custom Design of a 4-Bit BCD-to-Excess-3 Code Converter
