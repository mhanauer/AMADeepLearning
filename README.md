## Gatekeeper
rm(list=ls())
setwd("P:/Evaluation/TN Lives Count_Writing/3_Target1_SUICClinicalTrainingComparison/3_Data & Analyses")
datPre = read.csv("Pre.csv", header = FALSE, row.names = NULL)
datPost = read.csv("Post.csv", header = FALSE, row.names = NULL)
dat3month = read.csv("3month.csv", header  = TRUE)

#Connections
rm(list=ls())
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/ConnectionsPaperData")
GPRAAll = read.csv("ConnGPRA.csv", header = TRUE) 
PHQ9Base = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/PHQ9/PHQ9 Baseline.sav", to.data.frame = TRUE)
PHQ96month = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/PHQ9/PHQ9 6 Month.sav", to.data.frame = TRUE)
PHQ9All = merge(PHQ9Base, PHQ96month, by = "ParticipantID", all = TRUE)
GAD7Base = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/GAD7/GAD7 Baseline.sav", to.data.frame = TRUE)
GAD76month = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/GAD7/GAD7 6 Month.sav", to.data.frame = TRUE)


setwd("S:/Indiana Research & Evaluation/FISCH (Jon's Study)/Data")
data_FISCH = read.csv("FISCH_DATA_2019-01-17_1012.csv", header = TRUE, na.strings = "")
dat_FISCH = data_FISCH
#BAHCS-10
rm(list=ls())
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/HealthCapitalScale/7-13-18HCSData")
CIL = read.csv("CIL HCS Dataset_07172018.csv", header = TRUE)
CKY = read.csv("CKY HCS Dataset_07172018.csv", header = TRUE)
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/HealthCapitalScale/7-13-18HCSData")
CILDemo = read.csv("Brief HCS - IL - Demographics - 20180813.csv", header = TRUE)
CKYDemo = read.csv("Brief HCS - KY - Demographics - 20180813.csv", header = TRUE)
CIL_South_HCS_tobacco_use = read.csv("CIL_South_HCS_tobacco_use.csv", header = TRUE)
CIL_West_HCS_tobacco_use = read.csv("CIL_West_HCS_tobacco_use.csv", header = TRUE)
CKY_HCS_tobacco_use = read.csv("CKY_HCS_tobacco_use.csv", header = TRUE)
CIL_South_HCS_vitals = read.csv("CIL_South_HCS_vitals.csv", header = TRUE)
CIL_West_HCS_vitals = read.csv("CIL_West_HCS_vitals.csv", header = TRUE)
CKY_HCS_Vitals = read.csv("CKY_HCS_Vitals.csv", header = TRUE)
CIL_South_HCS_PHQ9 = read.csv("CIL_South_HCS_PHQ9.csv", header = TRUE)
CIL_West_HCS_PHQ9 = read.csv("CIL_West_HCS_PHQ9.csv", header = TRUE)
CKY_HCS_PHQ9 = read.csv("CKY_HCS_PHQ9.csv", header = TRUE)
HCS = read.csv("HCS.csv", header = TRUE)


#October 2018 BACHS-10 and Passport
rm(list=ls())
setwd("T:/Clinical Model Materials/Clinical Models/Adult Health Home/Evaluation Materials/Data/1. Health Capital Scale/6. October 2018/Matt'sData")
CIL_South_HCS_10052018 = read.csv("CIL_South_HCS_10052018.csv", header = TRUE)
CIL_West_HCS_10052018 = read.csv("CIL_West_HCS_10052018.csv", header = TRUE)
CKY_HCS_10052018 = read.csv("CKY_HCS_10052018.csv", header = TRUE)
setwd("T:/PP Claims Data/Matt'sStuff")
InfoMC10_PassPort_10_18 = read.csv("InfoMC10-18.csv", header = TRUE)
CIL_CKYFull = read.csv("CIL_CKYFull10-18.csv", header = TRUE)
setwd("T:/PP Claims Data/Matt'sStuff")
Member2017 =  read.csv("MEMBER_20170809_00.csv", header = TRUE)
Member2018 = read.csv("MEMBER_20180816_00.csv", header = TRUE)
SystemIDConverter = read.csv("SYSTEM_ID_to_AvatarClient_ID_Mapping.csv", header = TRUE)
Claims2017 = read.csv("CLAIMS_20170809_00.csv", head = TRUE) 
Claims2018 = read.csv("CLAIMS_20180816_00.csv", header = TRUE)
PHARMCLAIMS_2017 = read.csv("PHARMCLAIMS_20170809_00.csv", header = TRUE)
PHARMCLAIMS_2018 = read.csv("PHARMCLAIMS_20180816_00.csv", header = TRUE)
Provider2017 = read.csv("PROVIDER_20170809_00.csv", header = TRUE)
Provider2018 = read.csv("PROVIDER_20180816_00.csv", header = TRUE)
InpatClaimsData = read.csv("InpatClaimsData.csv", header = TRUE)
InpatAvatarData = read.csv("InpatAvatarData.csv", header = TRUE)
OutpatClaimsData = read.csv("OutpatClaimsData.csv", header = TRUE)
OutpatAvatarData = read.csv("OutpatAvatarData.csv", header = TRUE)

## Adult Health Home Outcomes
rm(list=ls())
setwd("T:/Clinical Model Materials/Clinical Models/Adult Health Home/Evaluation Materials/Data/2. Vitals/3. October 2018/Matt'sData")
CIL_South_HCS_vitals_10052018 = read.csv("CIL_South_HCS_vitals_10052018.csv", header = TRUE)
CIL_west_HCS_vitals_10052018 = read.csv("CIL_west_HCS_vitals_10052018.csv", header = TRUE)
CIN_HCS_Vitals_10052018 = read.csv("CIN_HCS_Vitals_10052018.csv", header = TRUE)
CKY_HCS_Vitals_10052018 = read.csv("CKY_HCS_Vitals_10052018.csv", header = TRUE)

setwd("T:/Clinical Model Materials/Clinical Models/Adult Health Home/Evaluation Materials/Data/10. PHQ9/October 2018/Matt'sData")
CIL_South_HCS_PHQ9_10102018 = read.csv("CIL_South_HCS_PHQ9_10102018.csv", header= TRUE)
CIL_West_HCS_PHQ9_10102018 = read.csv("CIL_West_HCS_PHQ9_10102018.csv", header= TRUE)
CIN_HCS_PHQ9_10102018 = read.csv("CIN_HCS_PHQ9_10102018.csv", header = TRUE)
CKY_HCS_PHQ9_10102018 = read.csv("CKY_HCS_PHQ9_10102018.csv", header = TRUE)

setwd("T:/Clinical Model Materials/Clinical Models/Adult Health Home/Evaluation Materials/Data/11. Tobacco/October 2018/Matt'sData")
CIL_South_HCS_Tobacco_10052018 = read.csv("CIL_South_HCS_Tobacco_10052018.csv", header = TRUE)
CIL_west_HCS_Tobacco_10052018 = read.csv("CIL_west_HCS_Tobacco_10052018.csv", header = TRUE)
CIN_HCS_Tobacco_10052018 = read.csv("CIN_HCS_Tobacco_10052018.csv", header = TRUE)
CKY_HCS_Tobacco_10052018 = read.csv("CKY_HCS_Tobacco_10052018.csv", header= TRUE)


setwd("T:/Clinical Model Materials/Clinical Models/Adult Health Home/Evaluation Materials/Data/7. Enrollment Lists/November 2018/Matt'sData")
HCS_Enrollment_Demo = read.csv("HCS_Enrollment_Demo.csv", header= TRUE)

#Enhanced
rm(list=ls())
setwd("P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses")
datPreAdult = read.csv("Target1EnhancedBaseAdult.csv", header = TRUE)
datPostAdult = read.csv("Target1EnhancedPostAdult.csv", header = TRUE)
datAdultTreat = read.csv("AdultTreatments.csv", header = TRUE)


install.packages(“countreg”, repos=http://R-Forge.R-project.org)

#RCS
rm(list=ls())
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/RCS/Data")
dat = read.csv("AAC_RCS_Intake_Clean.csv", header = TRUE)

#Zero Suicide Data
rm(list=ls())
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/ZeroSuicide")
ITSTest = read.csv("ZSData.csv", header = TRUE, na.strings = "N/A")
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/ZeroSuicide")
ITSRolling = read.csv("ZeroSuicideRollingSum.csv", header = TRUE, na.strings = "N/A")

#Enhanced Bayes
setwd("P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses")
dat = read.csv("EnhancedDataSet.csv", header = TRUE)
#setwd("C:/Users/Matthew.Hanauer/Desktop/BEST")
#source("BEST.R")

#Connections reassessment
setwd("C:/Users/Matthew.Hanauer/Desktop")
reassesData = read.csv("ConnectionsReassessment.csv", header = TRUE)

#FISCH Data
setwd("S:/Indiana Research & Evaluation/FISCH (Jon's Study)/Data")
dat_FISCH = read.csv("FISCH_DATA_2018-12-19_1132.csv", header= TRUE)

### CCPE Youth data
setwd("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/YOUTH Datasets")
BaseYouth = read.csv("Baseline CCPE GPRA Youth.csv", header= TRUE, na.strings = c(97,98,99))
ReassessYouth = read.csv("Reassess 3M CCPE GPRA Youth.csv", header = TRUE, na.strings = c(97,98,99))

### FHHC
setwd("S:/Indiana Research & Evaluation/FHHC Homelessness/Data and QPR")
FHHC_other = read.csv("FHHCBaseline_DATA_2019-01-06_1046.csv", header = TRUE, na.strings = c(-4,-99, -97, -98, -99,-1))
FHHC_GPRA = read.csv("FHHC_GPRA_Data.csv", header = TRUE, na.strings = c(-4,-99, -97, -98, -99,-1))

### Target autocontent
setwd("P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses")
TargetAdults = read.csv("Target1EnhancedPostAdult.csv", header = TRUE, na.strings = c(-9, "N/A", "N/a", ""))
TargetYouth = read.csv("TargetYouth_clean.csv", header = TRUE, na.strings = c(-9, "N/A", "N/a", ""))

#### Condom scale data
setwd("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/Baseline ADULT/CondomScaleData")
grpa_redcap = read.csv("CCPE_RedCap_GRPA_Condom_Data.csv", header = TRUE, na.strings=c(98, 99, 97))
grpa=read.csv("CCPE_GRPA_Baseline_Condom.csv", header=TRUE, na.strings=c(98,-88,-99, 98, 97))
condom=read.csv("Condom Scale - Baseline.csv", header=TRUE, na.strings=c(98,-88,-99, 98, 97))

#### CCPE Data set
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/CCPEPaperData")
CCPEBaseline = read.csv("CCPEBaselineFull.csv", header = TRUE)

### Concept mapping testing
setwd("C:/Users/Matthew.Hanauer/Desktop")
datTest = read.csv("datCorMD.csv", header = TRUE)

#### Enhanced Auto content
setwd("P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses")
adult = read.csv("Target1EnhancedPostAdult.csv", header = TRUE, na.strings = c(-99, 9, -9, "", NA, "N/A", "NA", -8, " "))
youth = read.csv("TargetYouth_clean.csv", header = TRUE, na.strings = c(-99, 9, -9, "", NA, "N/A", -8, "NA", -8," "))
setwd("P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses/AutoContent")
undergrad.results = undergrad(control ="control.txt", python3 = TRUE, sep = ",")


