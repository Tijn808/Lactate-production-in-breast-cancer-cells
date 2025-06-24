# Lactate-production-in-breast-cancer-cells
BBS2061 
**Computational environment**
MATLAB2021b was used to conduct this research, during installation the Bioinformatics toolbox was added. Git and Curl were installed using CobraToolbox (initCobraToolbox). The data processing was conducted using MacOS Seqouia 15.4 (24E248). Because of issues using the defealt solver with MacOS, Gurobi Solver was installed. Additionally, the GLPK solver was suitable for linear optimization problems in flux balance analysis (changeCobraSolver('glpk','all')). 

**Data**
A Genome-Scale Metabolic (GEM) model 'recon22.mat' was provided by University of Maastricht which simulated the conditions of a breast cancer model. The model was changed to simulate RPMI-1640 medium conditions using a provided script ('RPMImediumSimulation.m'), which adjusts metabolic reactions according to given nutrient availability in the medium. A data file was imported which was called BreastCancer_geneExpressionData.txt, a file containing 25.000+ gene Ensembl IDs, the HGNC ID and the expression rate for these genes in breast cancer. All genes with an expression level of zero were filtered using:
gxData = readtable('BreastCancer_geneExpressionData.txt');
index = string(table2array(gxData(:,3))) == '0';
genesToRemove = intersect(table2cell(gxData(index,2)), modelRPMI.genes);.

**Code**
**Aerobic**
he objective function for this GEM was set to maximize ATP production (modelaerobic = changeObjective(modelaerobic, 'DM_atp_c_');). To uncontstraint this model, the uptake reactions have negative fluxes, this allows the cell to uptake 1000 mmol/gDW/min of oxigen (modelaerobic = changeRxnBounds(modelaerobic, 'EX_o2(e)', -1000, 'l');). this represents the hydrolysis of ATP to ADP and Pi taking place in the cytosol of the cell. 
To optimize the ATP flux function (DM_atp_c_) the following code was used:
FBAaerobic = optimizeCbModel(modelaerobic, 'max');
FBAaerobic
FBAaerobic.objmodelanaerobic = modelRPMI;

**Anearobic**
The code is needed to compute the model in anearobic conditions 
modelanaerobic = changeRxnBounds(modelanaerobic, 'EX_o2(e)', 0, 'l');
modelanaerobic = changeObjective(modelanaerobic,'DM_atp_c_');
To make a visual representation of the model (draw_by_met(model, {'lac_L[c]'}, 'true', 1, 'struc', {''}, FBAaerobic.v);draw_by_met(model, {'lac_L[c]'}, 'true', 1, 'struc', {''}, FBAanaerob.v);).

To answer the research question it is useful to be able to knock-out genes, and be able to see the effect. 





