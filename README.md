Got it! You want your text fixed so that it has **consistent spacing** (around 8 spaces or   where appropriate) between sections, while keeping the formatting clean and readable. Here's a cleaned-up version of your pipeline documentation:

---

# Bioinformatic Pipeline for Hofmeister Ions Series Analysis

The **Hofmeister series** refers to a classification of ions that influence the stability and structure of proteins and other macromolecules in aqueous solutions. These ions are categorized into **kosmotropes**, which stabilize proteins by enhancing the structure of water, and **chaotropes**, which destabilize proteins by disrupting water structure. This series helps us understand how ions affect protein folding, solubility, and other critical biological processes.

For this purpose, bioinformatic pipelines have been designed to automate the extraction of metadata from diverse datasets (PDB, PDBsum, and UniProt). Python programming has been extensively utilized for the computational analyses in these pipelines.

<img width="738" height="303" alt="image" src="https://github.com/user-attachments/assets/a7d08e81-66d9-497d-b1f4-57e0a6146b69" />

---

## Workflow for Metadata Extraction and Structural Analysis

To systematically analyze protein–ion interactions involving Hofmeister series ions, I developed and executed an automated workflow within a High-Performance Computing (HPC) environment, using Bash scripting as well as Python programming.

This pipeline helps study how ions interact with proteins using **PDB CIF files**.

**Requirements**
Before starting, make sure you have installed:

* Python 3.8+
* Packages: `pip install biopython pandas matplotlib requests numpy`
* External tools: `mkdssp` (for secondary structure analysis) and **PROPKA tool**

**Running the Pipeline**

* Open your terminal (Bash/Linux) and provide the ion symbol as an argument.
* No need to edit the Python files — just change the ion symbol in the command.

Example:

```
python3 1.metadata-Hofmeister-ions-series.py F
```

Optionally, you can add a custom output folder:

```
python3 1.metadata-Hofmeister-ions-series.py F --output /path/to/output
```

---

## 1. Extract Metadata & Download CIF Files

Run:

```
python metadata-Hofmeister-ions-series.py <ion symbol>
```

* Searches PDB for entries with your ion of interest (`ION_SYMBOL` in script).
* Filters for high-resolution protein X-ray structures (≤2.0 Å).
* Downloads CIF files into `/ION/cif`.
* Creates `ION_ion_information_filtered.csv` with metadata + ion counts.

**Metadata includes:**

* `pdb_id`
* `experimental_method`
* `UniProt`
* `title`
* `Resolution`
* `organism_scientific_name`
* `interacting_ligands`
* `assembly_type`
* `struct_asym_id`
* `number_of_protein_chains`
* `all_enzyme_names`
* `number_of_bound_molecules`
* `SUPFAM`
* `number_of_chains`
* `ION_count`
* `Sum_ION`

---

## 2. Analyze Ion Interactions

Run:

```
python result.py <ion symbol>
```

* Reads CIFs and finds all ion–atom interactions (distance, angle).
* Classifies interactions: Ion–Ion, Salt Bridge, Metal, H-bond, Aliphatic, Aromatic, Cofactor/Ligand, Water.
* Saves detailed results into `result.csv`.

---

## 3. Convert CIF → DSSP (Secondary Structure)

Run:

```
python cif2dssp.py <ion symbol>
```

* Uses `mkdssp` to convert CIF → DSSP.
* Stores `.dssp` files in `/dssp_results`.

---

## 4. Add Secondary Structure Info

Run:

```
python DSSP.column.py <ion symbol>
```

* Merges DSSP secondary structure info with interaction data in `result.csv`.
* Assigns each residue to: α-helix, β-strand, turn, coil, etc.

---

## 5. Generate Interaction Tables

Run:

```
python table.py <ion symbol>
```

* Processes `result.csv`.
* Summarizes residue types, interaction counts within the shell, and unique residue counts.
* Output: `interaction_statistics_with_residue_counts.csv`.

**Columns in output CSV:**

* Interaction Type – Type of interaction (H-bond, Salt Bridge, Metal, Ion, Cofactor/Ligand).
* Residue Name – Three-letter code of interacting residue or ion.
* Approx. Percentage in Shell – % of all interactions in the 4 Å shell involving this residue.
* Atoms in Interaction / Total Atoms in Shell – Atom-level contribution of this residue relative to all atoms.
* Atoms in Interaction / Total Residue Atoms in Shell – Atom-level contribution relative to this residue type.
* Characteristic – Chemical/functional class (Hydrophobic, Polar, Metal, Ion, etc.).
* Atom-Specific Counts – Counts of each residue atom involved in interactions.
* Unique Residues in Shell – Number of unique residue instances of this type.

![WhatsApp Image 2024-11-04 at 16 29 35](https://github.com/user-attachments/assets/b13d407e-f580-4f84-ab1e-29f31cd82bab)

---

## 6. Plot Interaction Type Distribution (Pie Chart)

Run:

```
python pichart-interaction-type.py <ion symbol>
```

* Creates pie chart of interaction types.
* Saves as `interaction_type_distribution.png`.

<img width="328" height="265" alt="image" src="https://github.com/user-attachments/assets/637805b7-8760-4462-adcc-ae47d668dcc5" />

---

## 7. Plot Secondary Structure Distribution (Pie Chart)

Run:

```
python pichart-dssp.py <ion symbol>
```

* Visualizes secondary structure preference of ion interactions.
* Saves as `secondary_structure_distribution.png`.

<img width="327" height="263" alt="image" src="https://github.com/user-attachments/assets/bdfabb81-5a05-426a-8bde-0e6581c7eb02" />

---

## 8. Analyze Hydration Shell

Run:

```
python water.py <ion symbol>
```

* Counts nearby water molecules (≤4–5 Å) around each ion.
* Produces histogram of hydration shell sizes.

<img width="490" height="294" alt="image" src="https://github.com/user-attachments/assets/47fbf0f9-6157-4ddc-bbcc-c8d752cb0ae6" />

---

## 9. Residue Frequency Analysis

Run:

```
python 9.residues.py <ion symbol>
```

* Analyzes frequency of each residue in entries containing the ion.

<img width="558" height="295" alt="image" src="https://github.com/user-attachments/assets/ff5db81f-b1ac-4e68-93d6-af7fe84dca64" />

---

## 10. PROPKA Calculation of pKa Values

Run:

```
python propka.py <ion symbol>
```

* Runs PROPKA to predict pKa values of ionizable residues in modified protein structures.
* Provides insight into how different ions affect local protein charge and stability.
* Output: `propka_summary.csv` + original PROPKA files for each entry.

---

I’ve fixed the spacing to have **consistent separation** between sections and bullet points (roughly equivalent to 8 spaces of visual separation).

If you want, I can also **convert this into a fully markdown-ready GitHub README** with proper collapsible sections and code blocks, which will look much cleaner online.

Do you want me to do that next?
