# exmedt: Exposome Mediation Module
### Author: Mengyuan Ren (myren@bjmu.edu.cn)
### Date: 2022-11-25

The exmedt package is designed for estimating and screening potential mediation pathways of indigenous biomarkers (i.e., mediator) between external environmental exposure and health outcome in a user friendly and efficient way. Especially, when the number and category of exposures and mediators are of high dimension. Please see the website (http://www.exposomex.cn/#/expomediation) for more information. Users can install the package using the following code:

	if (!requireNamespace("devtools", quietly = TRUE)){

    	install.packages("devtools")
    
    	devtools::install_github('ExposomeX/excros/exmedt',force = TRUE)
    
    	devtools::install_github('ExposomeX/extidy/extidy',force = TRUE) #"extidy" package is optional if the data file has been well prepared. However, the it is recommended as users may need tidy the data to meet the modeling requirement, such as deleting varaibles with low variance, transforming data type, classifying variable into several level, etc.
	}

	library(exmedt)
	library(extidy) 


### Tips:
1. Before using the package, a user defined physical output path (i.e., OutPath) is recommended. For example

		OutPath = "D:/test" #The default path is the current working directory of R. Users can use this code to set the preferred path.
		
2. For each step, the returned values can be named as users' like by following R language requirement. 

3. All the PID must be the same with the one provided by InitMedt function, e.g., res$PID.


### Example codes:
##### 1. Initial Mediation module:
	res <- InitMedt()
	res$PID
	
##### 2. Load data for mediation analyses
	res2 <- LoadMedt(PID=res$PID,
		            UseExample = "example#1",
		            DataPath = NULL,
		            VocaPath = NULL)
	res2$Expo$Data
	res2$Expo$Voca

##### 3 Set Output physical path (optional)
	OutPath = "E:/Medt_test" #The default path is the current working directory of R.
	
##### 4 Tidy procedures
	res3 = TransImput(PID=res$PID,
	                  Group="F",
	                  Vars="all.x",
	                  Method="lod")
	res4 = TransType(PID=res$PID,
	                 Vars="C1",
	                 To="factor")
					 
	res5 = TransScale(PID=res$PID,
	                  Group="F",
	                  Vars="all.x",
	                  Method="normal")
	res6 = TransScale(PID=res$PID,
	                 Group="F",
	                 Vars="all.m",
	                 Method="normal")
	
	res7 = TransDistr(PID=res$PID,
	                  Vars="all.x",
	                  Method="ln")
	res8 = TransDistr(PID=res$PID,
	                 Vars="all.m",
	                 Method="ln")
					 
	res9 = TransDummy(PID=res$PID,
	                   Vars="default")

##### 4. Divide exposures and mediators into separate groups
	res10 <- XMlists(PID=res$PID)
	res10$ExpoList
	res10$MediList

##### 5. Pairwise mediation modelling
	res11 <- Pairwise(PID=res$PID,
	                  VarsY = "Y1",
	                  VarsX = "X1,X2,X3",
	                  VarsM = "M1,M2,M3",
	                  VarsC = "C1,C2,C3",
	                  Family = "linear",
	                  Iter = 500)
	res11$MedtPairWise_Stats
	res11$MedtPairWise_Stats

##### 6. Visualize pairwise mediation modelling result
	res12 <- VizMedtPair(PID=res$PID,
	                     Brightness = "light",
	                     Pallette = "default1")
	res12$plotdata
	res12$plot

##### 7. Exposures dimension reduction
	res13 <- RedX(PID=res$PID,
	                VarsY = "Y1",
	                VarsC = "C1,C2,C3",
	                VarsM = "M1,M2,M3",
	                Method = "mean",
	                Folds = 10,
	                Family = "linear",
	                Iter = 500)
	res13$MedtRedX_ERSall
	res13$MedtRedX_ERSlist
	res13$MedtRedX_Stats

##### 8. Mediators dimension reduction
	res14 <- RedM(PID=res$PID,
	              VarsY = "Y1",
	              VarsX = "X1,X2,X3",
	              VarsC = "C1,C2,C3",
	              Method = "mean",
	              Family = "linear",
	              Iter = 500)
	res14$MedtRedM_all
	res14$MedtRedM_list

##### 9. Exposure and mediator dimension reduction
	res15 <- RedXM(PID=res$PID,
	                 VarsY = "Y1",
	                 VarsC = "C1,C2,C3",
	                 Method = "mean",
	                 Family = "linear",
	                 Iter = 500)
	res15$MedtRedXM_all
	res15$MedtRedXM_list

##### 10. Visualize MedtRedXM mediation modelling result
	res16 <- VizRedXM(PID=res$PID,
	                    Brightness = "light",
	                    Pallette = "nature")
	res16$MedtRedXM_plotadta
	res16$MedtRedXM_plot

##### 11. Estimation the importance of mediators
	res17  <- ImptM(PID = res$PID,
	                 VarsY = "Y1",
	                 VarsX = "X1,X2,X3",
	                 VarsC = "C1")
	res17$MedtImptM_all
	res17$MedtImptM_list
	res17$MedtImptM_table1
	res17$MedtImptM_table2

##### 12. Exit
	FuncExit(PID = res$PID)
