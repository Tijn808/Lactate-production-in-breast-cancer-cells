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
We used FBA to compare the reference model to the SHMT1 knockout model in order to examine the impact of SHMT1 knockout on cancer cell metabolism. The analysis focused on exchange reactions, pyruvate-linked pathways,  and internal flux rerouting, all under aerobic conditions. 

First we looked at the potential effects of deleting SHMT1 on pyruvate secretion, which is essential for the production of energy and biomass. In both models (Reference and SHMT1 KO) , the pyruvate exchange reaction on EX_pyr (e) exhibited no input flux and an equivalent output flux of 2.6339 mmol/gDW/hr (Figure 3). 

![Image 04-07-2025 at 12 03](https://github.com/user-attachments/assets/1c2196ca-ba2e-44c3-a6b0-844f7b9dc04a)
Figure 3: Flux values for the pyruvate exchange reaction (EX_pyr(e))
in the reference and SHMT1 knockout (KO) models.
  
To identify which reactions were affected by SHMT1 deletion, first we reviewed a flux difference table generated from the model (see appendix). The table allowed us to compare flux values before and after the knockout by highlighting non-zero changes in internal pathways. From this, we pinpointed key reactions which had significant shifts in flux:  LDH-L, r0160, and r0153. 

Next, we analyzed lactate secretion, a key marker of the Warburg effect. By using the COBRA function findExcRXns, we discovered that the lactate exchange reaction (EX_lac_L(e)) had zero flux in both models. However, internal processes catalyzed by LDH_L (Lactate dehydrogenase), namely the reaction: lac_L[c] + nad[c] ⇌ h[c] + nadh[c] + pyr[c],  exhibited significant flux variations (from -850.81 to 1000.00 mmol/gDW/hr), this resulted in no extracellular lactate export (Figure 4). 
 
![Image 04-07-2025 at 12 04](https://github.com/user-attachments/assets/455f823f-60b1-40af-b300-5db8f152a138)
Figure 4: Flux Change in Lactate Dehydrogenase Reaction (LDH_L) After SHMT1 Knockout.
 
 
Following SHMT1 knockout, two internal reactions connected to pyruvate use demonstrated significant upregulation. The flux through the Serine-pyruvate transaminase reaction (r0160) increased from 1.0602 to 465.8800 mmol/gDW/hr respectively (Figure 5).  Additionally, the flux through the CTP synthase reaction using PEP (reaction r0153),  was increased from 0.0000 to 1000.0000 mmol/gDW/hr (Figure 5). 

![Image 04-07-2025 at 12 04 (1)](https://github.com/user-attachments/assets/9f63b547-5387-4bbf-b3e8-aab0d380ea31)
Figure 5: Flux Changes in Pyruvate-Linked Reactions r0160 and r0153 After SHMT1 Knockout.

> **Discussion**
According to our results, SHMT1 knockout has no effect on extracellular pyruvate secretion (Figure 3),  and did not affect lactate export. The flux of internal LDH changed direction, which might indicate that the cell maintained redox balance, by internally producing lactate, possibly to regenerate NAD+. Li et al. (2009)  demonstrated a similar idea, by using a computational model on skeletal muscle, showing that the production of lactate is firmly regulated by the cytosolic NADH/NAD⁺ redox balance. They also found that mitochondrial NADH/NAD+ shuttles capacity increased, lactate levels were reduced significantly  without disrupting energy production [11]. This suggests that lactate excretion is not necessarily required for the maintenance of energy balance. Instead, it indicates a compensatory mechanism under redox stress.
> 
SMHT1 KO results in a big repositioning of intracellular metabolic fluxes. Phosphoenolpyruvate (PEP) and pyruvate recycling reactions, cdp[c] + h[c] + pep[c] → ctp[c] + pyr[c], ( r0153 ) were significantly upregulated, sustaining biosynthetic and redox balance. Its activation after SHMT1 knockout indicates that the cell compensates for disrupted 1C nucleotide synthesis by upregulating that alternative, glycolysis-dependent nucleotide production pathways (r0153), which might indicate the flexibility of a metabolic network that is able to adapt to genetic perturbations. In addition, ATP production kept its stability despite those changes, meaning that the cell was able to maintain its energy output. 

In a similar study, by Zhang et al (2024), it found that SHMT1 inhibition, in lung cancer, induced apoptosis by inducing p53, and cell cycle arrest [12]. This supports the idea that the inhibition of SHMT1 may induce tumor suppression pathways. On the other hand, a different study, by Yang et al (2023), found that overexpression of SHMT1, in renal cancer, slowed down the growth of the cancer cells [13]. This paradox illustrates the possibility of depending on the tumor's metabolic context and/or its reliance on 1C metabolism.

Another study, made by Dou et al. (2019) demonstrated in their study that due to damaged NADPH regeneration and upregulation of its oxidase NOX1, knocking out SHMT1 in hepatic cancer cells increased ROS levels [14]. These findings support our findings that SHMT1-KO disrupted redox balance that would result in a compensatory pathway.

Additionally, after knocking out SHMT1, serine starts likely to accumulate, due to disabled 1C flux. Amelio et al (2014), found that serine is an allosteric activator of PKM2, which helps in glycolysis. Therefore, the more serine, the higher PKM2 activity and thus more lactate production [15]. As a result of SHMT1 KO, PKM2 increases and more lactate is produced, which promotes a Warburg-like behaviour. Another way to use the excess serine is serine-pyruvate transaminase reaction (r0160), as after the knockout, its activity was dramatically increased (Figure.5). r0160 simply uses serine to generate alanine, for the biomass, and hydroxypyruvate, that might contribute to redox balance. This reaction is also considered as a compensatory (or alternative) pathway.

Our study faced some difficulties and limitations. One of the main limitations we faced is finding relevant studies about knocking out SHMT1 that would support ours. We suggest that more studies regarding this topic in the future might be helpful. Another limitation of our model is that the data is very narrow and specific. The gene expression data was collected from Caucasian women aged 60–75 with stage IA tumors. Which reduces the applicability to more breast cancer cases, with different ethnicities, ages, and stages.

**To conclude, SHMT1 deletion does not reduce pyruvate secretion but induces intracellular reorganization. Pyruvate production is increased, serine is diverted from one-carbon metabolism, and nucleotide synthesis is redirected to promote glycolysis. Eventually, lactate production was supported by serine, via PKM2.**

> **References:**
**Introduction:**
[1]	Yang M, Vousden KH. Serine and one-carbon metabolism in cancer. Nat Metab. 2021;3(1):21–32. https://doi.org/10.1038/s42255-020-00329-9

[2] 	Anderson NM, Mucka P, Kern JG, Feng H. The emerging role and targetability of the TCA cycle in cancer metabolism. Protein Cell. 2018 Mar;9(2):216–32. doi:10.1007/s13238-017-0422-1.

[3]	Pérez‑Tomás R, Pérez‑Guillén I. Lactate in the Tumor Microenvironment: An Essential Molecule in Cancer Progression and Treatment. Cancers (Basel). 2020 Nov 3;12(11):3244.
doi:10.3390/cancers12113244.  

[4] 	Tymoczko JL, Berg JM, Stryer L. Biochemistry: A Short Course. 1st ed. New York: W.H. Freeman and Company; 2010.

[5]	Yang M, Vousden KH. Serine and one‑carbon metabolism in cancer. Nat Rev Cancer. 2016 Oct;16(10):650–662. doi:10.1038/nrc.2016.81. 

[6]	Sánchez‑Castillo A, Vooijs M, Kampen KR. Linking Serine/Glycine Metabolism to Radiotherapy Resistance. Cancers. 2021 Mar 10;13(6):1191. doi:10.3390/cancers13061191. 

[7]	Orth JD, Thiele I, Palsson BØ. What is flux balance analysis? Nature Biotechnology. 2010 Mar;28(3):245–248. doi:10.1038/nbt.1614. 

[8]	Edwards JS, Covert M, Palsson BØ. Metabolic modelling of microbes: the flux‑balance approach. Environ Microbiol. 2002 Jun;4(3):133–140. doi:10.1046/j.1462-2920.2002.00282.x

**Methods:**
[9] 	Cancer Genome Atlas Network (2012) “Comprehensive molecular portraits of human breast tumours.” Nature, 490(7418):61-70. doi: 10.1038/nature11412

[10] 	Swainston N, et al. Recon 2.2: from reconstruction to model of human metabolism. Metabolomics. 2016;12:109. doi:10.1007/s11306-016-1051-4.

**Discussion:**

[11] Li, Y., Dash, R. K., Kim, J., Saidel, G. M., & Cabrera, M. E. (2009). Role of NADH/NAD+transport activity and glycogen store on skeletal muscle energy metabolism during exercise: in silico studies. American Journal of Physiology-Cell Physiology, 296(1), C25–C46. https://doi.org/10.1152/ajpcell.00094.2008.

[12] Zhang, X., & Wang, Z. (2024). Targeting SHMTs and MTHFDs in cancer: attractive opportunity for anti-tumor strategy. Frontiers in Pharmacology, 15. https://doi.org/10.3389/fphar.2024.1335785

[13] Yang, Y., Zhang, M., Zhao, Y., Deng, T., Zhou, X., Qian, H., Wang, M., Zhang, C., Huo, Z., Mao, Z., Shao, Z., Liu, M., Yang, C., Lin, C., Xu, F., Tian, G., & Zhang, Y. (2023). HOXD8 suppresses renal cell carcinoma growth by upregulating SHMT1 expression. Cancer Science, 114(12), 4583–4595. https://doi.org/10.1111/cas.15982

[14]	Dou, C., Xu, Q., Liu, J., Wang, Y., Zhou, Z., Yao, W., Jiang, K., Cheng, J., Zhang, C., & Tu, K. (2019). SHMT1 inhibits the metastasis of HCC by repressing NOX1-mediated ROS production. Journal of Experimental & Clinical Cancer Research : CR, 38(1), 70. https://doi.org/10.1186/s13046-019-1067-5 

[15] Amelio, I., Cutruzzolá, F., Antonov, A., Agostini, M., & Melino, G. (2014). Serine and glycine metabolism in cancer. Trends in Biochemical Sciences, 39(4), 191–198. https://doi.org/10.1016/j.tibs.2014.02.004 

**Programs used:**
[16] MATLAB and Statistics Toolbox Release 2021b, The MathWorks, Inc., Natick, Massachusetts, United States. 

[17] Heirendt, L., Arreckx, S., Pfau, T. et al. Creation and analysis of biochemical constraint-based models using the COBRA Toolbox v.3.0. Nat Protoc 14, 639–702 (2019).  https://doi.org/10.1038/s41596-018-0098-2

[18] Makhorin A. GNU Linear Programming Kit (GLPK). Available at: https://www.gnu.org/software/glpk/ 

[19] Tijn808. Lactate production in breast cancer cells [Internet]. GitHub; 2024 [cited 2025 Jul 2]. Available from: https://github.com/Tijn808/Lactate-production-in-breast-cancer-cells













