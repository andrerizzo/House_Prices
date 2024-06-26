---
title: "House Prices - Kaggle"
url: "https://www.kaggle.com/c/house-prices-advanced-regression-techniques"
author: "André Rizzo"
date: "2024-03-23"
output: 
  html_document:
    toc: true
    toc_float: true
    theme: cosmo
    highlight: tango
    fig_width: 8
    fig_height: 6
    fig_caption: true
    keep_md: true
    self_contained: no
    toc_depth: 3
---






### Check if libraries are installed

```r
if (!require("tidyverse")) install.packages("tidyverse")
if (!require("here")) install.packages("here")
```


### Load libraries

```r
library(tidyverse)
library(here)
```


### Load data

```r
path = here("data/raw/train.csv")
df_train = read_csv(path)
```


### Data Exploration


#### Check the dimensions of the data

```r
dim(df_train)
```

```
## [1] 1460   81
```


#### Check the first rows of the data

```r
head(df_train)
```

```
## # A tibble: 6 × 81
##      Id MSSubClass MSZoning LotFrontage LotArea Street Alley LotShape
##   <dbl>      <dbl> <chr>          <dbl>   <dbl> <chr>  <chr> <chr>   
## 1     1         60 RL                65    8450 Pave   <NA>  Reg     
## 2     2         20 RL                80    9600 Pave   <NA>  Reg     
## 3     3         60 RL                68   11250 Pave   <NA>  IR1     
## 4     4         70 RL                60    9550 Pave   <NA>  IR1     
## 5     5         60 RL                84   14260 Pave   <NA>  IR1     
## 6     6         50 RL                85   14115 Pave   <NA>  IR1     
## # ℹ 73 more variables: LandContour <chr>, Utilities <chr>, LotConfig <chr>,
## #   LandSlope <chr>, Neighborhood <chr>, Condition1 <chr>, Condition2 <chr>,
## #   BldgType <chr>, HouseStyle <chr>, OverallQual <dbl>, OverallCond <dbl>,
## #   YearBuilt <dbl>, YearRemodAdd <dbl>, RoofStyle <chr>, RoofMatl <chr>,
## #   Exterior1st <chr>, Exterior2nd <chr>, MasVnrType <chr>, MasVnrArea <dbl>,
## #   ExterQual <chr>, ExterCond <chr>, Foundation <chr>, BsmtQual <chr>,
## #   BsmtCond <chr>, BsmtExposure <chr>, BsmtFinType1 <chr>, BsmtFinSF1 <dbl>, …
```


#### Check the last rows of the data

```r
tail(df_train)
```

```
## # A tibble: 6 × 81
##      Id MSSubClass MSZoning LotFrontage LotArea Street Alley LotShape
##   <dbl>      <dbl> <chr>          <dbl>   <dbl> <chr>  <chr> <chr>   
## 1  1455         20 FV                62    7500 Pave   Pave  Reg     
## 2  1456         60 RL                62    7917 Pave   <NA>  Reg     
## 3  1457         20 RL                85   13175 Pave   <NA>  Reg     
## 4  1458         70 RL                66    9042 Pave   <NA>  Reg     
## 5  1459         20 RL                68    9717 Pave   <NA>  Reg     
## 6  1460         20 RL                75    9937 Pave   <NA>  Reg     
## # ℹ 73 more variables: LandContour <chr>, Utilities <chr>, LotConfig <chr>,
## #   LandSlope <chr>, Neighborhood <chr>, Condition1 <chr>, Condition2 <chr>,
## #   BldgType <chr>, HouseStyle <chr>, OverallQual <dbl>, OverallCond <dbl>,
## #   YearBuilt <dbl>, YearRemodAdd <dbl>, RoofStyle <chr>, RoofMatl <chr>,
## #   Exterior1st <chr>, Exterior2nd <chr>, MasVnrType <chr>, MasVnrArea <dbl>,
## #   ExterQual <chr>, ExterCond <chr>, Foundation <chr>, BsmtQual <chr>,
## #   BsmtCond <chr>, BsmtExposure <chr>, BsmtFinType1 <chr>, BsmtFinSF1 <dbl>, …
```


#### Check the structure of the data

```r
str(df_train)
```

```
## spc_tbl_ [1,460 × 81] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ Id           : num [1:1460] 1 2 3 4 5 6 7 8 9 10 ...
##  $ MSSubClass   : num [1:1460] 60 20 60 70 60 50 20 60 50 190 ...
##  $ MSZoning     : chr [1:1460] "RL" "RL" "RL" "RL" ...
##  $ LotFrontage  : num [1:1460] 65 80 68 60 84 85 75 NA 51 50 ...
##  $ LotArea      : num [1:1460] 8450 9600 11250 9550 14260 ...
##  $ Street       : chr [1:1460] "Pave" "Pave" "Pave" "Pave" ...
##  $ Alley        : chr [1:1460] NA NA NA NA ...
##  $ LotShape     : chr [1:1460] "Reg" "Reg" "IR1" "IR1" ...
##  $ LandContour  : chr [1:1460] "Lvl" "Lvl" "Lvl" "Lvl" ...
##  $ Utilities    : chr [1:1460] "AllPub" "AllPub" "AllPub" "AllPub" ...
##  $ LotConfig    : chr [1:1460] "Inside" "FR2" "Inside" "Corner" ...
##  $ LandSlope    : chr [1:1460] "Gtl" "Gtl" "Gtl" "Gtl" ...
##  $ Neighborhood : chr [1:1460] "CollgCr" "Veenker" "CollgCr" "Crawfor" ...
##  $ Condition1   : chr [1:1460] "Norm" "Feedr" "Norm" "Norm" ...
##  $ Condition2   : chr [1:1460] "Norm" "Norm" "Norm" "Norm" ...
##  $ BldgType     : chr [1:1460] "1Fam" "1Fam" "1Fam" "1Fam" ...
##  $ HouseStyle   : chr [1:1460] "2Story" "1Story" "2Story" "2Story" ...
##  $ OverallQual  : num [1:1460] 7 6 7 7 8 5 8 7 7 5 ...
##  $ OverallCond  : num [1:1460] 5 8 5 5 5 5 5 6 5 6 ...
##  $ YearBuilt    : num [1:1460] 2003 1976 2001 1915 2000 ...
##  $ YearRemodAdd : num [1:1460] 2003 1976 2002 1970 2000 ...
##  $ RoofStyle    : chr [1:1460] "Gable" "Gable" "Gable" "Gable" ...
##  $ RoofMatl     : chr [1:1460] "CompShg" "CompShg" "CompShg" "CompShg" ...
##  $ Exterior1st  : chr [1:1460] "VinylSd" "MetalSd" "VinylSd" "Wd Sdng" ...
##  $ Exterior2nd  : chr [1:1460] "VinylSd" "MetalSd" "VinylSd" "Wd Shng" ...
##  $ MasVnrType   : chr [1:1460] "BrkFace" "None" "BrkFace" "None" ...
##  $ MasVnrArea   : num [1:1460] 196 0 162 0 350 0 186 240 0 0 ...
##  $ ExterQual    : chr [1:1460] "Gd" "TA" "Gd" "TA" ...
##  $ ExterCond    : chr [1:1460] "TA" "TA" "TA" "TA" ...
##  $ Foundation   : chr [1:1460] "PConc" "CBlock" "PConc" "BrkTil" ...
##  $ BsmtQual     : chr [1:1460] "Gd" "Gd" "Gd" "TA" ...
##  $ BsmtCond     : chr [1:1460] "TA" "TA" "TA" "Gd" ...
##  $ BsmtExposure : chr [1:1460] "No" "Gd" "Mn" "No" ...
##  $ BsmtFinType1 : chr [1:1460] "GLQ" "ALQ" "GLQ" "ALQ" ...
##  $ BsmtFinSF1   : num [1:1460] 706 978 486 216 655 ...
##  $ BsmtFinType2 : chr [1:1460] "Unf" "Unf" "Unf" "Unf" ...
##  $ BsmtFinSF2   : num [1:1460] 0 0 0 0 0 0 0 32 0 0 ...
##  $ BsmtUnfSF    : num [1:1460] 150 284 434 540 490 64 317 216 952 140 ...
##  $ TotalBsmtSF  : num [1:1460] 856 1262 920 756 1145 ...
##  $ Heating      : chr [1:1460] "GasA" "GasA" "GasA" "GasA" ...
##  $ HeatingQC    : chr [1:1460] "Ex" "Ex" "Ex" "Gd" ...
##  $ CentralAir   : chr [1:1460] "Y" "Y" "Y" "Y" ...
##  $ Electrical   : chr [1:1460] "SBrkr" "SBrkr" "SBrkr" "SBrkr" ...
##  $ 1stFlrSF     : num [1:1460] 856 1262 920 961 1145 ...
##  $ 2ndFlrSF     : num [1:1460] 854 0 866 756 1053 ...
##  $ LowQualFinSF : num [1:1460] 0 0 0 0 0 0 0 0 0 0 ...
##  $ GrLivArea    : num [1:1460] 1710 1262 1786 1717 2198 ...
##  $ BsmtFullBath : num [1:1460] 1 0 1 1 1 1 1 1 0 1 ...
##  $ BsmtHalfBath : num [1:1460] 0 1 0 0 0 0 0 0 0 0 ...
##  $ FullBath     : num [1:1460] 2 2 2 1 2 1 2 2 2 1 ...
##  $ HalfBath     : num [1:1460] 1 0 1 0 1 1 0 1 0 0 ...
##  $ BedroomAbvGr : num [1:1460] 3 3 3 3 4 1 3 3 2 2 ...
##  $ KitchenAbvGr : num [1:1460] 1 1 1 1 1 1 1 1 2 2 ...
##  $ KitchenQual  : chr [1:1460] "Gd" "TA" "Gd" "Gd" ...
##  $ TotRmsAbvGrd : num [1:1460] 8 6 6 7 9 5 7 7 8 5 ...
##  $ Functional   : chr [1:1460] "Typ" "Typ" "Typ" "Typ" ...
##  $ Fireplaces   : num [1:1460] 0 1 1 1 1 0 1 2 2 2 ...
##  $ FireplaceQu  : chr [1:1460] NA "TA" "TA" "Gd" ...
##  $ GarageType   : chr [1:1460] "Attchd" "Attchd" "Attchd" "Detchd" ...
##  $ GarageYrBlt  : num [1:1460] 2003 1976 2001 1998 2000 ...
##  $ GarageFinish : chr [1:1460] "RFn" "RFn" "RFn" "Unf" ...
##  $ GarageCars   : num [1:1460] 2 2 2 3 3 2 2 2 2 1 ...
##  $ GarageArea   : num [1:1460] 548 460 608 642 836 480 636 484 468 205 ...
##  $ GarageQual   : chr [1:1460] "TA" "TA" "TA" "TA" ...
##  $ GarageCond   : chr [1:1460] "TA" "TA" "TA" "TA" ...
##  $ PavedDrive   : chr [1:1460] "Y" "Y" "Y" "Y" ...
##  $ WoodDeckSF   : num [1:1460] 0 298 0 0 192 40 255 235 90 0 ...
##  $ OpenPorchSF  : num [1:1460] 61 0 42 35 84 30 57 204 0 4 ...
##  $ EnclosedPorch: num [1:1460] 0 0 0 272 0 0 0 228 205 0 ...
##  $ 3SsnPorch    : num [1:1460] 0 0 0 0 0 320 0 0 0 0 ...
##  $ ScreenPorch  : num [1:1460] 0 0 0 0 0 0 0 0 0 0 ...
##  $ PoolArea     : num [1:1460] 0 0 0 0 0 0 0 0 0 0 ...
##  $ PoolQC       : chr [1:1460] NA NA NA NA ...
##  $ Fence        : chr [1:1460] NA NA NA NA ...
##  $ MiscFeature  : chr [1:1460] NA NA NA NA ...
##  $ MiscVal      : num [1:1460] 0 0 0 0 0 700 0 350 0 0 ...
##  $ MoSold       : num [1:1460] 2 5 9 2 12 10 8 11 4 1 ...
##  $ YrSold       : num [1:1460] 2008 2007 2008 2006 2008 ...
##  $ SaleType     : chr [1:1460] "WD" "WD" "WD" "WD" ...
##  $ SaleCondition: chr [1:1460] "Normal" "Normal" "Normal" "Abnorml" ...
##  $ SalePrice    : num [1:1460] 208500 181500 223500 140000 250000 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Id = col_double(),
##   ..   MSSubClass = col_double(),
##   ..   MSZoning = col_character(),
##   ..   LotFrontage = col_double(),
##   ..   LotArea = col_double(),
##   ..   Street = col_character(),
##   ..   Alley = col_character(),
##   ..   LotShape = col_character(),
##   ..   LandContour = col_character(),
##   ..   Utilities = col_character(),
##   ..   LotConfig = col_character(),
##   ..   LandSlope = col_character(),
##   ..   Neighborhood = col_character(),
##   ..   Condition1 = col_character(),
##   ..   Condition2 = col_character(),
##   ..   BldgType = col_character(),
##   ..   HouseStyle = col_character(),
##   ..   OverallQual = col_double(),
##   ..   OverallCond = col_double(),
##   ..   YearBuilt = col_double(),
##   ..   YearRemodAdd = col_double(),
##   ..   RoofStyle = col_character(),
##   ..   RoofMatl = col_character(),
##   ..   Exterior1st = col_character(),
##   ..   Exterior2nd = col_character(),
##   ..   MasVnrType = col_character(),
##   ..   MasVnrArea = col_double(),
##   ..   ExterQual = col_character(),
##   ..   ExterCond = col_character(),
##   ..   Foundation = col_character(),
##   ..   BsmtQual = col_character(),
##   ..   BsmtCond = col_character(),
##   ..   BsmtExposure = col_character(),
##   ..   BsmtFinType1 = col_character(),
##   ..   BsmtFinSF1 = col_double(),
##   ..   BsmtFinType2 = col_character(),
##   ..   BsmtFinSF2 = col_double(),
##   ..   BsmtUnfSF = col_double(),
##   ..   TotalBsmtSF = col_double(),
##   ..   Heating = col_character(),
##   ..   HeatingQC = col_character(),
##   ..   CentralAir = col_character(),
##   ..   Electrical = col_character(),
##   ..   `1stFlrSF` = col_double(),
##   ..   `2ndFlrSF` = col_double(),
##   ..   LowQualFinSF = col_double(),
##   ..   GrLivArea = col_double(),
##   ..   BsmtFullBath = col_double(),
##   ..   BsmtHalfBath = col_double(),
##   ..   FullBath = col_double(),
##   ..   HalfBath = col_double(),
##   ..   BedroomAbvGr = col_double(),
##   ..   KitchenAbvGr = col_double(),
##   ..   KitchenQual = col_character(),
##   ..   TotRmsAbvGrd = col_double(),
##   ..   Functional = col_character(),
##   ..   Fireplaces = col_double(),
##   ..   FireplaceQu = col_character(),
##   ..   GarageType = col_character(),
##   ..   GarageYrBlt = col_double(),
##   ..   GarageFinish = col_character(),
##   ..   GarageCars = col_double(),
##   ..   GarageArea = col_double(),
##   ..   GarageQual = col_character(),
##   ..   GarageCond = col_character(),
##   ..   PavedDrive = col_character(),
##   ..   WoodDeckSF = col_double(),
##   ..   OpenPorchSF = col_double(),
##   ..   EnclosedPorch = col_double(),
##   ..   `3SsnPorch` = col_double(),
##   ..   ScreenPorch = col_double(),
##   ..   PoolArea = col_double(),
##   ..   PoolQC = col_character(),
##   ..   Fence = col_character(),
##   ..   MiscFeature = col_character(),
##   ..   MiscVal = col_double(),
##   ..   MoSold = col_double(),
##   ..   YrSold = col_double(),
##   ..   SaleType = col_character(),
##   ..   SaleCondition = col_character(),
##   ..   SalePrice = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```


#### Check the summary of the data

```r
summary(df_train)
```

```
##        Id           MSSubClass      MSZoning          LotFrontage    
##  Min.   :   1.0   Min.   : 20.0   Length:1460        Min.   : 21.00  
##  1st Qu.: 365.8   1st Qu.: 20.0   Class :character   1st Qu.: 59.00  
##  Median : 730.5   Median : 50.0   Mode  :character   Median : 69.00  
##  Mean   : 730.5   Mean   : 56.9                      Mean   : 70.05  
##  3rd Qu.:1095.2   3rd Qu.: 70.0                      3rd Qu.: 80.00  
##  Max.   :1460.0   Max.   :190.0                      Max.   :313.00  
##                                                      NA's   :259     
##     LotArea          Street             Alley             LotShape        
##  Min.   :  1300   Length:1460        Length:1460        Length:1460       
##  1st Qu.:  7554   Class :character   Class :character   Class :character  
##  Median :  9478   Mode  :character   Mode  :character   Mode  :character  
##  Mean   : 10517                                                           
##  3rd Qu.: 11602                                                           
##  Max.   :215245                                                           
##                                                                           
##  LandContour         Utilities          LotConfig          LandSlope        
##  Length:1460        Length:1460        Length:1460        Length:1460       
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  Neighborhood        Condition1         Condition2          BldgType        
##  Length:1460        Length:1460        Length:1460        Length:1460       
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##   HouseStyle         OverallQual      OverallCond      YearBuilt   
##  Length:1460        Min.   : 1.000   Min.   :1.000   Min.   :1872  
##  Class :character   1st Qu.: 5.000   1st Qu.:5.000   1st Qu.:1954  
##  Mode  :character   Median : 6.000   Median :5.000   Median :1973  
##                     Mean   : 6.099   Mean   :5.575   Mean   :1971  
##                     3rd Qu.: 7.000   3rd Qu.:6.000   3rd Qu.:2000  
##                     Max.   :10.000   Max.   :9.000   Max.   :2010  
##                                                                    
##   YearRemodAdd   RoofStyle           RoofMatl         Exterior1st       
##  Min.   :1950   Length:1460        Length:1460        Length:1460       
##  1st Qu.:1967   Class :character   Class :character   Class :character  
##  Median :1994   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :1985                                                           
##  3rd Qu.:2004                                                           
##  Max.   :2010                                                           
##                                                                         
##  Exterior2nd         MasVnrType          MasVnrArea      ExterQual        
##  Length:1460        Length:1460        Min.   :   0.0   Length:1460       
##  Class :character   Class :character   1st Qu.:   0.0   Class :character  
##  Mode  :character   Mode  :character   Median :   0.0   Mode  :character  
##                                        Mean   : 103.7                     
##                                        3rd Qu.: 166.0                     
##                                        Max.   :1600.0                     
##                                        NA's   :8                          
##   ExterCond          Foundation          BsmtQual           BsmtCond        
##  Length:1460        Length:1460        Length:1460        Length:1460       
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  BsmtExposure       BsmtFinType1         BsmtFinSF1     BsmtFinType2      
##  Length:1460        Length:1460        Min.   :   0.0   Length:1460       
##  Class :character   Class :character   1st Qu.:   0.0   Class :character  
##  Mode  :character   Mode  :character   Median : 383.5   Mode  :character  
##                                        Mean   : 443.6                     
##                                        3rd Qu.: 712.2                     
##                                        Max.   :5644.0                     
##                                                                           
##    BsmtFinSF2        BsmtUnfSF       TotalBsmtSF       Heating         
##  Min.   :   0.00   Min.   :   0.0   Min.   :   0.0   Length:1460       
##  1st Qu.:   0.00   1st Qu.: 223.0   1st Qu.: 795.8   Class :character  
##  Median :   0.00   Median : 477.5   Median : 991.5   Mode  :character  
##  Mean   :  46.55   Mean   : 567.2   Mean   :1057.4                     
##  3rd Qu.:   0.00   3rd Qu.: 808.0   3rd Qu.:1298.2                     
##  Max.   :1474.00   Max.   :2336.0   Max.   :6110.0                     
##                                                                        
##   HeatingQC          CentralAir         Electrical           1stFlrSF   
##  Length:1460        Length:1460        Length:1460        Min.   : 334  
##  Class :character   Class :character   Class :character   1st Qu.: 882  
##  Mode  :character   Mode  :character   Mode  :character   Median :1087  
##                                                           Mean   :1163  
##                                                           3rd Qu.:1391  
##                                                           Max.   :4692  
##                                                                         
##     2ndFlrSF     LowQualFinSF       GrLivArea     BsmtFullBath   
##  Min.   :   0   Min.   :  0.000   Min.   : 334   Min.   :0.0000  
##  1st Qu.:   0   1st Qu.:  0.000   1st Qu.:1130   1st Qu.:0.0000  
##  Median :   0   Median :  0.000   Median :1464   Median :0.0000  
##  Mean   : 347   Mean   :  5.845   Mean   :1515   Mean   :0.4253  
##  3rd Qu.: 728   3rd Qu.:  0.000   3rd Qu.:1777   3rd Qu.:1.0000  
##  Max.   :2065   Max.   :572.000   Max.   :5642   Max.   :3.0000  
##                                                                  
##   BsmtHalfBath        FullBath        HalfBath       BedroomAbvGr  
##  Min.   :0.00000   Min.   :0.000   Min.   :0.0000   Min.   :0.000  
##  1st Qu.:0.00000   1st Qu.:1.000   1st Qu.:0.0000   1st Qu.:2.000  
##  Median :0.00000   Median :2.000   Median :0.0000   Median :3.000  
##  Mean   :0.05753   Mean   :1.565   Mean   :0.3829   Mean   :2.866  
##  3rd Qu.:0.00000   3rd Qu.:2.000   3rd Qu.:1.0000   3rd Qu.:3.000  
##  Max.   :2.00000   Max.   :3.000   Max.   :2.0000   Max.   :8.000  
##                                                                    
##   KitchenAbvGr   KitchenQual         TotRmsAbvGrd     Functional       
##  Min.   :0.000   Length:1460        Min.   : 2.000   Length:1460       
##  1st Qu.:1.000   Class :character   1st Qu.: 5.000   Class :character  
##  Median :1.000   Mode  :character   Median : 6.000   Mode  :character  
##  Mean   :1.047                      Mean   : 6.518                     
##  3rd Qu.:1.000                      3rd Qu.: 7.000                     
##  Max.   :3.000                      Max.   :14.000                     
##                                                                        
##    Fireplaces    FireplaceQu         GarageType         GarageYrBlt  
##  Min.   :0.000   Length:1460        Length:1460        Min.   :1900  
##  1st Qu.:0.000   Class :character   Class :character   1st Qu.:1961  
##  Median :1.000   Mode  :character   Mode  :character   Median :1980  
##  Mean   :0.613                                         Mean   :1979  
##  3rd Qu.:1.000                                         3rd Qu.:2002  
##  Max.   :3.000                                         Max.   :2010  
##                                                        NA's   :81    
##  GarageFinish         GarageCars      GarageArea      GarageQual       
##  Length:1460        Min.   :0.000   Min.   :   0.0   Length:1460       
##  Class :character   1st Qu.:1.000   1st Qu.: 334.5   Class :character  
##  Mode  :character   Median :2.000   Median : 480.0   Mode  :character  
##                     Mean   :1.767   Mean   : 473.0                     
##                     3rd Qu.:2.000   3rd Qu.: 576.0                     
##                     Max.   :4.000   Max.   :1418.0                     
##                                                                        
##   GarageCond         PavedDrive          WoodDeckSF      OpenPorchSF    
##  Length:1460        Length:1460        Min.   :  0.00   Min.   :  0.00  
##  Class :character   Class :character   1st Qu.:  0.00   1st Qu.:  0.00  
##  Mode  :character   Mode  :character   Median :  0.00   Median : 25.00  
##                                        Mean   : 94.24   Mean   : 46.66  
##                                        3rd Qu.:168.00   3rd Qu.: 68.00  
##                                        Max.   :857.00   Max.   :547.00  
##                                                                         
##  EnclosedPorch      3SsnPorch       ScreenPorch        PoolArea      
##  Min.   :  0.00   Min.   :  0.00   Min.   :  0.00   Min.   :  0.000  
##  1st Qu.:  0.00   1st Qu.:  0.00   1st Qu.:  0.00   1st Qu.:  0.000  
##  Median :  0.00   Median :  0.00   Median :  0.00   Median :  0.000  
##  Mean   : 21.95   Mean   :  3.41   Mean   : 15.06   Mean   :  2.759  
##  3rd Qu.:  0.00   3rd Qu.:  0.00   3rd Qu.:  0.00   3rd Qu.:  0.000  
##  Max.   :552.00   Max.   :508.00   Max.   :480.00   Max.   :738.000  
##                                                                      
##     PoolQC             Fence           MiscFeature           MiscVal        
##  Length:1460        Length:1460        Length:1460        Min.   :    0.00  
##  Class :character   Class :character   Class :character   1st Qu.:    0.00  
##  Mode  :character   Mode  :character   Mode  :character   Median :    0.00  
##                                                           Mean   :   43.49  
##                                                           3rd Qu.:    0.00  
##                                                           Max.   :15500.00  
##                                                                             
##      MoSold           YrSold       SaleType         SaleCondition     
##  Min.   : 1.000   Min.   :2006   Length:1460        Length:1460       
##  1st Qu.: 5.000   1st Qu.:2007   Class :character   Class :character  
##  Median : 6.000   Median :2008   Mode  :character   Mode  :character  
##  Mean   : 6.322   Mean   :2008                                        
##  3rd Qu.: 8.000   3rd Qu.:2009                                        
##  Max.   :12.000   Max.   :2010                                        
##                                                                       
##    SalePrice     
##  Min.   : 34900  
##  1st Qu.:129975  
##  Median :163000  
##  Mean   :180921  
##  3rd Qu.:214000  
##  Max.   :755000  
## 
```





