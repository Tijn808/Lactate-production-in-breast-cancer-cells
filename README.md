**Knockout  of SHMT1 Modifies One Carbon Metabolism and Reduces Warburg-Like Lactate Secretion**







Student authors
Helma Caune                                                                                       ID: I6341357
Iris Hovens                                                                                           ID: I6351766
Tijn Saes                                                                                              ID: I6344387
Rama Rihawi Name                                                                             ID: I6330233
 
 
 
 
Course:                                               BBS2061, Systems Biology
Faculty:                                               FHML                                                                           
Academic Year:                                  2025/26
Tutor group (1-5):                               1
Topic of the project:    	                      Metabolic Modelling 
 
Date:                                                    4th of July, 2025



> **Introduction:**
Cancer cells reprogram their metabolism to support survival and growth [1]. The involvement of many genes in this process can be studied using metabolic modelling, which helps identify altered metabolic pathways and predict intracellular fluxes. These models integrate global metabolic data and gene associations. In breast cancer, high metabolic activity supports cell survival via both aerobic and anaerobic pathways. Studying genes that alter these pathways can inform drug targets. Since it does not require in vitro or in vivo experiments, metabolic modelling is an efficient tool to study gene function.
A central feature of cancer metabolism is the conversion of glucose to pyruvate through glycolysis. This process generates NADH and ATP. Under anaerobic conditions, lactate dehydrogenase (LDH) catalyzes the conversion of pyruvate to lactate, regenerating NAD⁺ from NADH and maintaining glycolytic flux (Figure 1) [2]. Notably, many cancer cells favor lactate production even in the presence of oxygen—a phenomenon called aerobic glycolysis, or the Warburg effect [3]. Although less energy-efficient than oxidative phosphorylation, this allows rapid ATP generation and supports biosynthetic processes. Reversing the Warburg effect through gene knockouts could reactivate mitochondrial respiration and slow cancer cell growth.

![Image 04-07-2025 at 16 53](https://github.com/user-attachments/assets/c828bd00-ed66-4008-bf54-f265fe46729c)
Figure 1: Lactate dehydrogenase (LDH) catalyzes the conversion of pyruvate to lactate [4].

Besides ATP and NADH, glycolysis produces intermediates for serine biosynthesis. Serine is crucial for one-carbon metabolism, donating one-carbon units to the folate cycle, which supports nucleotide synthesis [2]. One key enzyme in this process is serine hydroxymethyltransferase (SHMT), which converts serine to glycine while transferring one-carbon units to tetrahydrofolate (THF). In most cells, the mitochondrial isoform SHMT2 catalyzes the formation of 5,10-methylene-THF, essential for nucleotide biosynthesis and redox balance [5] (Figure 2). This reaction is supported by the glycine cleavage system, which works alongside both mitochondrial and cytosolic SHMT.
Serine + THF ⟷ Glycine + 5,10-methylene-THF + H2​O

![Image 03-07-2025 at 12 05](https://github.com/user-attachments/assets/1c30d1e0-147f-4028-8017-ea3587e2617e)
Figure 2: Overview of serine/glycine biosynthesis pathway activation in cancer. [6]

While SHMT2’s role in mitochondrial one-carbon metabolism is well-studied, the function of cytosolic SHMT1 in regulating glycolytic flux and its potential to affect the Warburg effect is less clear. Targeting SHMT1 may influence cancer cell proliferation and lactate production, making it a potential therapeutic target. We hypothesize that knocking out SHMT impairs serine’s contribution to the folate cycle, reducing biosynthesis. As SHMT operates downstream of glycolysis, this will not reverse the Warburg effect directly. However, serine accumulation may stimulate glycolysis further, enhancing lactate and ATP production and thereby changing the Warburg effect pathways.
Genome-scale Metabolic Model (GEM) was used to map out tumor data from Caucasian women. These models incorporate metabolic reactions, associated genes, and stoichiometry, enabling constraint-based simulations [7]. We performed in silico gene knockouts using Flux Balance Analysis (FBA), a computational method that predicts how genetic changes impact metabolism. FBA models metabolite fluxes using a stoichiometric matrix under the steady-state assumption, where internal metabolite concentrations remain constant. It enforces mass balance constraints, ensuring that production and consumption rates for each metabolite are equal. By adding constraints to FBA, such as gene knock-outs, objectives can be reached to simulate aerobic conditions. An objective function, such as maximizing biomass or ATP, is defined to simulate cellular behavior. This method allows systematic analysis of gene deletions without laboratory experiments [8].
**This study investigates how SHMT1 knock-out affects lactate production and glycolytic flux in breast cancer cells under aerobic conditions. Using constraint-based modeling, we aim to assess whether SHMT1 inactivation could influence or reverse the Warburg effect in silico.**


> **Methodology**

Methods:
**Computational environment**

MATLAB2021b [16] was used to conduct this research, during installation the Bioinformatics toolbox was added during installation. Git and Curl were installed using CobraToolbox version 2.39.5 [17]. The data processing was conducted using MacOS Seqouia 15.4 (24E248). Additionally, the GLPK solver [18] was suitable for linear optimization problems in flux balance analysis (changeCobraSolver('glpk','all')). For visualization and model interrogation, functions like draw_by_met, printRxnFormula, and optimizeCbModel were used.
Study population and acquired data

Publicly available data from TCGA breast cancer project was used in this research, the dataset includes RNA sequences data from 20 caucasian females, between 60 and 75 years old all diagnosed with stage 1A breast cancer [9]. A Genome-Scale Metabolic (GEM) model 'recon22.mat' was used [10] which simulated the conditions of a breast cancer model. The model was changed to simulate RPMI-1640 medium conditions using a predefined script (RPMImediumSimulation.m), which adjusts metabolic reactions according to given nutrient availability in the medium. 

A data file was imported which was called BreastCancer_geneExpressionData.txt, a file containing 25.000+ gene Ensembl IDs, the HGNC ID and the expression rate for these genes . From this dataset, genes with a 0 expression value were identified and marked for deletion. These were matched against the model’s genes and stored in the variable genesToRemove. The gene-protein-reaction (GPR) rules were used to determine which reactions would be impacted by knocking out a gene or multiple genes.

**Objective function**
The objective function for this GEM was set to maximize ATP production ('DM_atp_c_'). A cancer cell’s objective is to replicate as much as possible, this is simulated by optimizing the objective function to maximize ATP flux. The model had no constraint on oxygen uptake. 

**Add model aerobic**
The model was duplicated into a new variable called modelaerobic or modelaerobicC (for the cancer model), and the oxygen uptake reaction 'EX_o2(e)' was set to -1000, simulating aerobic conditions. FBA was then run with this setting using the optimization for ATP and oxygen. 
![ModelAerobic](https://github.com/user-attachments/assets/89e1425e-1794-4af2-bd0c-f4761f5ac633)

**Knock out genes**
To answer the research question it is useful to be able to knock out genes, and be able to see the effect. The base cancer model was created by removing all genes with 0 expression (genesToRemove). An extended model, was then generated by adding a specific gene knockout (e.g., HGNC:10850 for SHMT1). Gene knockouts were simulated by removing the specified genes from the model. The results showed changes in flux through key reactions such as ADK3 and LDH_L, which were analyzed to understand their metabolic impact.
![AerobicDelete](https://github.com/user-attachments/assets/0442bdaa-641d-4f6c-905c-8fe716aa1980)


**Additional model**
Comparative models such as modelaerobicCadd were created to analyze the metabolic effects of these knockouts under aerobic conditions. By adjusting the objective and constraints similarly, FBA was used again to examine ATP and lactate production. The reactions with the largest differences in fluxes were flagged using absolute flux difference vectors. These differences were visualized with draw_by_met, focusing on metabolites like lactate.
![ModelCadd](https://github.com/user-attachments/assets/69e77a04-0d39-47d3-8006-efdb29093b1d)

**Table**
Furthermore, a table represents all expressions of genes before and after knockout, the genes that showed no difference before and after knockout were also included. This allowed for easier research by looking at the expression rates differences. The variables in the table included the reaction ID, flux value in the reference cancer model, flux value after gene deletions, gene-protein-reaction (GPR) rule, and the difference in flux. Reactions related to ATP and LDH were manually appended to highlight central metabolic effects. Only non-zero flux changes were considered in a filtered version of the table for clarity.
![Image 04-07-2025 at 17 19](https://github.com/user-attachments/assets/ad3cec07-6b5f-499c-8c00-7bd43ae767b9)


> **Results**
 
Pyruvate exchange was not influenced by SHMT1 knockout. The pyruvate exchange reaction EX_pyr (e) exhibited no input flux and an equivalent output flux of 2.6339 mmol/gDW/hr in both the reference cancer model and the SHMT1 knockout model (Figure 2). This suggests that pyruvate secretion remains mostly unaffected by the SHMT1 gene deletion in aerobic conditions. ATP flux change. 

![Image 04-07-2025 at 12 03](https://github.com/user-attachments/assets/1c2196ca-ba2e-44c3-a6b0-844f7b9dc04a)
Figure 2: Flux values for the pyruvate exchange reaction (EX_pyr(e))
in the reference and SHMT1 knockout (KO) models.
  
The COBRA Toolbox function findExcRxns revealed that no lactate exchange reaction (e.g., EX_lac_L(e)) produced active input or output fluxes under either model. Internal processes catalyzed by LDH_L (Lactate dehydrogenase), namely the reaction: lac_L[c] + nad[c] ⇌ h[c] + nadh[c] + pyr[c],  exhibited significant flux variations (from -850.81 to 1000.00 mmol/gDW/hr), this did not result in extracellular lactate export (figure 3). 
 
![Image 04-07-2025 at 12 04](https://github.com/user-attachments/assets/455f823f-60b1-40af-b300-5db8f152a138)
Figure 3: Flux Change in Lactate Dehydrogenase Reaction (LDH_L) After SHMT1 Knockout.
 
The reaction r0160, where serine-pyruvate transaminase catalyzes pyruvate and L-serine into L-alanine and hydroxypyruvate, was significantly upregulated. The flux through this transamination pathway, after SHMT1 knockout, increased from 1.0602 to 465.8800 mmol/gDW/hr respectively (Figure 3). 
 
Additionally, the CTP synthase reaction using PEP (reaction r0153), which  links glycolytic intermediates to nucleotide synthesis, was strongly activated. This reaction uses phosphoenolpyruvate (PEP), CDP and a proton to produce CTO and pyruvate: cdp[c] + h[c] + pep[c] → ctp[c] + pyr[c]. The flux increased from 0.0000 to 1000.0000 mmol/gDW/hr (Figure 4). This reaction links glycolysis directly to nucleotide synthesis.

![Image 04-07-2025 at 12 04 (1)](https://github.com/user-attachments/assets/9f63b547-5387-4bbf-b3e8-aab0d380ea31)
Figure 4: Flux Changes in Pyruvate-Linked Reactions r0160 and r0153 After SHMT1 Knockout.










