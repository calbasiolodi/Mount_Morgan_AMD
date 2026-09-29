# Mount_Morgan_AMD
An Acid Mine Drainage (AMD) potential evaluation with simple statistics and machine learning (scikit-learn), at Mount Morgan mine with downstream data such rainfall, pH and conductivity.


<img width="1872" height="977" alt="Location_Fitzroy_Basin" src="https://github.com/user-attachments/assets/bbc0264e-aa52-4fac-b24d-735d8faccbb8" />


------------------------------------------------------------------------------------------
3. COMPACT SUMMARY: EC Mean & pH Mean vs. Key Environmental Variables @ Kenbula (~500 downstread Mt. Morgan Mine) 
------------------------------------------------------------------------------------------
Hydrological and Temp.       |  EC Pearson  EC Spearman (N) |  pH Pearson  pH Spearman (N)
------------------------------------------------------------------------------------------
Rainfall (mm)_Total          |      -0.284       -0.247 (332) |      +0.222       +0.171 (280)
Level (Metres)_Mean          |      -0.532       -0.465 (342) |      +0.522       +0.616 (289)
Discharge (Cumecs)_Mean      |      -0.327       -0.399 (343) |      +0.216       +0.543 (291)
Discharge (ML/day)_Mean      |      -0.327       -0.403 (343) |      +0.216       +0.545 (291)
Volume ML_Total              |      -0.329       -0.403 (343) |      +0.212       +0.545 (291)
Water Temp (Deg. C)_Mean     |      -0.046       -0.036 (342) |      +0.008       -0.021 (286)


In this case the Spearman correlation is preferred as it reduces the effect of outliers. Overall we can claim that a higher dischardge has a significant correlation with the pH (that is the pH rises with more rainfall). The pH at Kenbula has been consistently acidic at ~3.5 to 4, highlighting there is a significant acidification ongoing due presumably to the mine. The positive correlation with water level, volume and discharge is interpreted as a dilution effect, that is the H+ ions concentration simply drops relative with the volume. 

<img width="4170" height="2661" alt="ph_trends_by_location" src="https://github.com/user-attachments/assets/ceb6e3d2-45a2-4be8-9ce2-5867ce93c11a" />
