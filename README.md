# Script Classification and Writer Identification
# Two Tasks for a Common Understanding of Cultural Heritage

Writer identification and Script classification are usually considered as two separated and very different tasks, as well in palaeography as in computer science. Following the ICDAR competition on the CLAMM  corpus  about  script  classification  and  dating,  this  contribution  proposes  to  reconsider  the  tasks  and  methods  of  Palaeography  and Computer  Vision applied  for  Artificial  Paleography.  We  argue  that,  while  aiming  at  understanding  past  societies  and  the  written  cultural  heritage  in  its  whole  complexity,  palaeography  and  its  core  tasks  may  be  defined  as  discretizing  the  historical  and  social  continuum  at  different levels of granularity. In this sense, we can consider Writer identification and Script classification as  one  single  task.  We  then  transfer  this  hypothesis  to  computer  science,  by  running  two  infrastructures  
created for Script classification on a more homogeneous dataset with a focus on Writer Identification. The analysis of the results confirms the unicity of the tasks and let us consider in a novel way how to evidence and illustrate the historical continuum, while discretizing it in a non-binary way

If you use the present repository, please cite:
Stutzmann, Dominique, Christopher Tensmeyer, and Vincent Christlein. « Writer Identification and Script Classification: Two Tasks for a Common Understanding of Cultural Heritage ». *manuscript cultures*, 15 (2020): 11-24. 
[https://www.csmc.uni-hamburg.de/publications/mc/files/articles/mc15-02-stutzmann.pdf](https://www.csmc.uni-hamburg.de/publications/mc/files/articles/mc15-02-stutzmann.pdf)

```
@article{stutzmann_writer_2020,
	title = {Writer {Identification} and {Script} {Classification}: {Two} {Tasks} for a {Common} {Understanding} of {Cultural} {Heritage}},
	volume = {15},
	issn = {1867{\textendash}9617},
	url = {https://www.csmc.uni-hamburg.de/publications/mc/files/articles/mc15-02-stutzmann.pdf},
	abstract = {Writer identification and Script classification are usually considered as two separated and very different tasks, as well in palaeography as in computer science. Following the ICDAR competition on the CLAMM  corpus  about  script  classification  and  dating,  this  contribution  proposes  to  reconsider  the  tasks  and  methods  of  Palaeography  and Computer  Vision applied  for  Artificial  Paleography.  We  argue  that,  while  aiming  at  understanding  past  societies  and  the  written  cultural  heritage  in  its  whole  complexity,  palaeography  and  its  core  tasks  may  be  defined  as  discretizing  the  historical  and  social  continuum  at  different levels of granularity. In this sense, we can consider Writer identification and Script classification as  one  single  task.  We  then  transfer  this  hypothesis  to  computer  science,  by  running  two  infrastructures  
created for Script classification on a more homogeneous dataset with a focus on Writer Identification. The analysis of the results confirms the unicity of the tasks and let us consider in a novel way how to evidence and illustrate the historical continuum, while discretizing it in a non-binary way},
	urldate = {2018-08-21},
	journal = {manuscript cultures},
	author = {Stutzmann, Dominique and Tensmeyer, Christopher and Christlein, Vincent},
	year = {2020},
	pages = {11--24},
	}

```

The .Rhistory file and furthuer visualizations were created for the workshop "La paléographie à l’heure de l’intelligence artificielle / Artificial Intelligence and palaeography". The data is the same as the one used in the aforementioned paper, but processed with R and with an additional Rtsne visualisation.


```
#install packages and load

install.packages(c("FactoMineR", "factoextra")) 
library(FactoMineR)
library(factoextra)

#load data files
ChrB <- read.csv("Script-Classification-Writer-Identification/ScriptClass/Icdar2017_Christlein_belonging.csv", header = TRUE, sep=";") 
ChrB <- read.csv("ScriptClass/Icdar2017_Christlein_belonging.csv", header = TRUE, sep=";")
ChrB <- read.csv("ScriptClass/Icdar2017_Christlein_belonging.csv", header = TRUE, sep=";")
ChrD <- read.csv("ScriptClass/Icdar2017_Christlein_distance.csv", header = TRUE, sep=";")
TenB <- read.csv("ScriptClass/Icdar2017_Tensmeyer_belonging.csv", header = TRUE, sep=";")
TenD <- read.csv("ScriptClass/Icdar2017_Tensmeyer_distance.csv", header = TRUE, sep=";")
GT <- read.csv("ScriptClass/Ground-Truth.csv", header = TRUE, sep=";")

#checking the size and type of data
dim(ChrB)
head(ChrB)
dim(TenB)
head(TenB)
dim(ChrD)
head(ChrD)
dim(TenD)
head(TenD)

#formating the imported tables and adding metadata
rownames(ChrB) <- ChrB$FILENAME
rownames(TenB) <- ChrB$FILENAME
ChrB$Script_type_ICDAR2017 <- factor(ChrB$Script_type_ICDAR2017, levels = c("1_Caroline", "2_Cursiva", "3_Half_Uncial", "4_Humanistic", "5_Humanistic_Cursive", "6_Hybrida", "7_Praegothica", "8_Semihybrida", "9_Semitextualis", "10_Textualis_meridionalis", "11_Textualis", "12_Uncial"))
TenB$Script_type_ICDAR2017 <- factor(TenB$Script_type_ICDAR2017, levels = c("1_Caroline", "2_Cursiva", "3_Half_Uncial", "4_Humanistic", "5_Humanistic_Cursive", "6_Hybrida", "7_Praegothica", "8_Semihybrida", "9_Semitextualis", "10_Textualis_meridionalis", "11_Textualis", "12_Uncial"))

#Principal Component Analysis and vizualisations on axes 1-2 and 1-3
ChrB.pca <- PCA(ChrB, quali.sup=1:6, ncp=3, scale.unit=TRUE, graph=TRUE)
ChrB.pca.viz <- fviz_pca_ind(ChrB.pca, axes = c(1, 2), geom="point", col.ind=ChrB$Script_type_ICDAR2017) + geom_point(aes(col=ChrB$Script_type_ICDAR2017, shape=ChrB$Script_type_ICDAR2017)) + scale_shape_manual(values=c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12))
ChrB.pca.viz 
ChrB.pca.viz.13 <- fviz_pca_ind(ChrB.pca, axes = c(1, 3), geom="point", col.ind=ChrB$Script_type_ICDAR2017) + geom_point(aes(col=ChrB$Script_type_ICDAR2017, shape=ChrB$Script_type_ICDAR2017)) + labs(col = "Script type", shape = "Script type") + scale_shape_manual(values=c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12))
ChrB.pca.viz.13

#Create a more user friendly graph and saving it
install.packages("plotly")
library(htmlwidgets)
library(plotly)
ChrB.pca.plotly <- ggplotly(ChrB.pca.viz)
saveWidget(ChrB.pca.plotly, "Icdar2017_Christlein_belonging_plotly_pca.html")

#RTSNE and associated graphs
library(Rtsne)
set.seed(4000)
ChrB.rtsne <- Rtsne(ChrB[,6:18], theta=0.0, pca_scale = TRUE)
ChrB.rtsne.frame <- data.frame(x = ChrB.rtsne$Y[,1], y = ChrB.rtsne$Y[,2])
rownames(ChrB.rtsne.frame) <- rownames(ChrB)
ChrB.rtsne.frame <- cbind(ChrB.rtsne.frame, ChrB$Script_type_ICDAR2017)
colnames(ChrB.rtsne.frame) <- c("x", "y", "Script_type")
ChrB.rtsne.plot <- ggplot(ChrB.rtsne.frame, aes(x=x, y=y)) + geom_point(aes(col=ChrB$Script_type_ICDAR2017, shape=ChrB$Script_type_ICDAR2017)) + labs(col = "Script type", shape = "Script type") + scale_shape_manual(values=c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12))
ChrB.rtsne.plot 
ChrB.rtsne.plotly <- ggplotly(ChrB.rtsne.plot)
saveWidget(ChrB.rtsne.plotly, "Icdar2017_Christlein_belonging_plotly_rtnse.html")

```
