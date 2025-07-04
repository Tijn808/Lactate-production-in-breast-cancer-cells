Introduction:
Cancer cells reprogramme their metabolism to support unrestrained proliferation and survival in nutrient-poor conditions [1]. In breast cancer, high proliferation rates are essential for cell survival and are supported by both aerobic and anaerobic metabolic pathways. A hallmark of this reprogramming is their altered glucose metabolism.

Cancer cells convert glucose into pyruvate, lactate dehydrogenase (LDH) catalyzes the conversion of pyruvate to lactate under anaerobic conditions, coupled with the oxidation of NADH to NAD⁺. Regeneration of NAD⁺ by the LDH-catalyzed reaction is important to maintain glycolysis and so ATP production [2]. However, even in the presence of oxygen, many tumor cells still preferentially metabolize glucose into lactate, a phenomenon first described by Otto Warburg, now known as aerobic glycolysis or the Warburg effect [3]. Although this pathway is less energy-efficient than oxidative phosphorylation, it allows rapid ATP generation and supports biosynthesis. The underlying reasons for this metabolic shift remain under investigation, but growing evidence points to a combination of genetic, mitochondrial, and environmental factors contributing to elevated glycolysis in cancer cells [2].

Lactate produced via glycolysis exits the tumor cell through the MCT4 transporter and can enter neighboring cancer cells. There, it is converted back to pyruvate and fuels the Krebs cycle, promoting further ATP production and biosynthesis. This intercellular lactate exchange highlights a form of metabolic cooperation, helping tumor cells sustain proliferation. Reversing the Warburg effect may therefore reduce lactate accumulation, metastasis, immune evasion, proliferation and overall tumor aggressiveness [4]. 

However, the focus of this study is not on lactate shuttling between tumor cells, but rather on intracellular lactate flux within a single breast cancer cell. To explore this, we constructed a Genome-scale Metabolic Model (GEM) and implemented it in MATLAB to simulate metabolic behavior under both aerobic and anaerobic conditions. A GEM is a structured knowledge base that includes the organism’s metabolic reactions, associated genes, enzymes, and their stoichiometry, enabling constraint-based simulations of metabolism [5].

To study gene function without in vitro or in vivo manipulations, we employed in silico gene knock-outs. These computational approaches, based on Flux Balance Analysis (FBA), allow prediction of how gene deletions affect cellular metabolism. FBA Flux Balance Analysis is a mathematical approach to study the flow (flux) of metabolites through a metabolic network which uses the stoichiometric matrix, mass balance constraints (balance is 0), steady-state assumption and an objective function. Steady-state assumes that the concentration of internal metabolites does not change over time. Constraint-based modeling thus provides a high-resolution, systematic method to assess the metabolic impact of gene knockouts while avoiding experimental limitations [6].

Among the genes affecting cancer metabolism is serine hydroxymethyltransferase (SHMT). SHMT plays a central role in one-carbon metabolism, converting serine into glycine and transferring one-carbon units to tetrahydrofolate (THF). In most cultured cells, SHMT2, the mitochondrial isoform, catalyzes the transfer of serine’s β-carbon to THF, producing 5,10-methylene-THF, a key intermediate in nucleotide biosynthesis and redox balance [7] (Fig. 1). This reaction is also supported by the glycine cleavage system for both cytosolic and mitochondrial SHMT.

Serine + THF ⟷ Glycine + 5,10-methylene-THF + H2​O

While SHMT2 has been widely studied in the context of mitochondrial one-carbon metabolism in cancer, the role of cytosolic SHMT1 in regulating glycolytic flux and its potential to reverse the Warburg effect is less understood. Targeting SHMT1 may impact cancer cell proliferation, nucleotide synthesis, and lactate production, making it a candidate for therapeutic intervention.

When SHMT1 is knocked out in cancer cells, one-carbon metabolism is disrupted, causing serine accumulation, and so, reduced nucleotide synthesis. To adapt, cells shift metabolic flux through various reactions. One reaction is r0160. This reaction converts serine into alanine consuming pyruvate. Another reaction is r0153. A membrane enclosed enzyme reactor converts cytidine 5′-mono- phosphate (CMP) and phosphoenolpyruvate (PEP) to cytidine 5′- triphosphate (CTP) and pyruvate on a gram scale [8]. This supports nucleotide synthesis without 1C metabolism and further diverts glycolytic flux away from lactate. These reactions help compensate for SHMT1 loss and possibly suppress the Warburg effect, offering insight into potential therapeutic strategies.

This study investigates how SHMT1 knock-out affects lactate production and glycolytic flux in breast cancer cells under aerobic conditions, using constraint-based metabolic modeling. We aim to determine whether this intervention can help reverse the Warburg effect in silico.

![Image 03-07-2025 at 12 05](https://github.com/user-attachments/assets/1c30d1e0-147f-4028-8017-ea3587e2617e)
Figure 1: Overview of serine/glycine biosynthesis pathway activation in cancer. [9]




**Computational environment**
MATLAB2021b was used to conduct this research, during installation the Bioinformatics toolbox was added. Git and Curl were installed using CobraToolbox (initCobraToolbox). The data processing was conducted using MacOS Seqouia 15.4 (24E248). Because of issues using the default solver with MacOS, Gurobi Solver was installed. Additionally, the GLPK solver was suitable for linear optimization problems in flux balance analysis (changeCobraSolver('glpk','all')). For visualization and model interrogation, functions like draw_by_met, printRxnFormula, and optimizeCbModel were used.

**Data**
A Genome-Scale Metabolic (GEM) model 'recon22.mat' was provided by University of Maastricht which simulated the conditions of a breast cancer model. The model was changed to simulate RPMI-1640 medium conditions using a provided script (RPMImediumSimulation.m), which adjusts metabolic reactions according to given nutrient availability in the medium. A data file was imported which was called BreastCancer_geneExpressionData.txt, a file containing 25.000+ gene Ensembl IDs, the HGNC ID and the expression rate for these genes in breast cancer. From this dataset, genes with a 0 expression value were identified and marked for deletion. These were matched against the model’s genes and stored in the variable genesToRemove. The gene-protein-reaction (GPR) rule was used to investigate the effects of knocking out a gene or multiple genes. 

**Code**
The objective function for this GEM was set to maximize ATP production ('DM_atp_c_'). To reduce the constraint on this model, the uptake reactions have negative fluxes, this allows the cell to uptake 1000 mmol/gDW/min of oxygen. This represents the hydrolysis of ATP to ADP and Pi taking place in the cytosol of the cell. Optimization can be altered by changing the function from 'DM_atp_c_' to a different metabolite in the model.

**Add model aerobic**
The model was duplicated into a new variable called modelaerobic or modelaerobicC (for the cancer model), and the oxygen uptake reaction 'EX_o2(e)' was set to -1000, simulating aerobic conditions. Flux Balance Analysis (FBA) was then run with this setting using optimizeCbModel.
![Modelaerobicnormal](https://github.com/user-attachments/assets/f3a06213-3b0b-4290-824d-8875d2f3f971)
![ModelAerobic](https://github.com/user-attachments/assets/8c1f5c24-df51-40f2-b494-0cbbd043f3d8)

**Add model anaerobic**
To simulate anaerobic conditions, the model was copied to a new variable called modelanaerobic, and the oxygen uptake was set to 0 mmol/gDW/min using changeRxnBounds. This restricts all oxygen entry into the system. ATP production was again set as the objective, and the model was optimized. 
![Modelanaerobic](https://github.com/user-attachments/assets/ea609183-1924-4359-82e5-76b35e5d1a44)

**Knock out genes**
To answer the research question it is useful to be able to knock out genes, and be able to see the effect. Genes were knocked out by using the HGNC code provided by the dataset. The base cancer model was created by removing all genes with 0 expression (genesToRemove). An extended model, modelCadd, was then generated by adding specific gene knockouts (e.g., HGNC:1850 for SHMT1). The function deleteModelGenes was used with the list genesToRemoveAdd, which included the additional knockouts. The flux through reactions like ADK3 and LDH_L was examined before and after knockouts to study the effect.
![AerobicDelete](https://github.com/user-attachments/assets/5c523a7e-50d0-45cf-ac6c-602ac168488d)

**Additional model**
Comparative models such as modelaerobicCadd were created to analyze the metabolic effects of these knockouts under aerobic conditions. By adjusting the objective and constraints similarly, FBA was used again to examine ATP and lactate production. The reactions with the largest differences in fluxes were flagged using absolute flux difference vectors. These differences were visualized with draw_by_met, focusing on metabolites like lactate.
![ModelCadd](https://github.com/user-attachments/assets/37ce5f7e-8acc-4fea-8e17-81d4e9cd3ba6)

**Table**
Furthermore, a table represents all expressions of genes before and after knockout, the genes that showed no difference before and after knockout were also included. This allowed for easier research by looking at the expression rates differences indicated with the HGNC code. The variables in the table included the reaction ID, flux value in the reference cancer model, flux value after gene deletions, gene-protein-reaction (GPR) rule, and the difference in flux. Reactions related to ATP and LDH were manually appended to highlight central metabolic effects. Only non-zero flux changes were considered in a filtered version of the table for clarity.
![Image 03-07-2025 at 12 06](https://github.com/user-attachments/assets/899c53ce-b086-4ac9-affd-8ba26c4f2703)

Results
 
Pyruvate exchange was not influenced by SHMT1 knockout. The pyruvate exchange reaction EX_pyr (e) exhibited no input flux and an equivalent output flux of 2.6339 mmol/gDW/hr in both the reference cancer model and the SHMT1 knockout model (Figure 2). This suggests that pyruvate secretion remains mostly unaffected by the SHMT1 gene deletion in aerobic conditions. ATP flux change. 

Figure 2: Flux values for the pyruvate exchange reaction (EX_pyr(e))
in the reference and SHMT1 knockout (KO) models.
  
The COBRA Toolbox function findExcRxns revealed that no lactate exchange reaction (e.g., EX_lac_L(e)) produced active input or output fluxes under either model. Internal processes catalyzed by LDH_L (Lactate dehydrogenase), namely the reaction: lac_L[c] + nad[c] ⇌ h[c] + nadh[c] + pyr[c],  exhibited significant flux variations (from -850.81 to 1000.00 mmol/gDW/hr), this did not result in extracellular lactate export (figure 3). 
 

Figure 3: Flux Change in Lactate Dehydrogenase Reaction (LDH_L) After SHMT1 Knockout.
 
 
The reaction r0160, where serine-pyruvate transaminase catalyzes pyruvate and L-serine into L-alanine and hydroxypyruvate, was significantly upregulated. The flux through this transamination pathway, after SHMT1 knockout, increased from 1.0602 to 465.8800 mmol/gDW/hr respectively (Figure 3). 
 
Additionally, the CTP synthase reaction using PEP (reaction r0153), which  links glycolytic intermediates to nucleotide synthesis, was strongly activated. This reaction uses phosphoenolpyruvate (PEP), CDP and a proton to produce CTO and pyruvate: cdp[c] + h[c] + pep[c] → ctp[c] + pyr[c]. The flux increased from 0.0000 to 1000.0000 mmol/gDW/hr (Figure 4). This reaction links glycolysis directly to nucleotide synthesis.

Figure 4: Flux Changes in Pyruvate-Linked Reactions r0160 and r0153 After SHMT1 Knockout.








