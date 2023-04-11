# Safe Have Currencies and the Russia-Ukraine War

## An estimation of linear and non-linear safe haven capabilities of the Swiss franc, the euro, the yen, and the British pound against the U.S. dollar

The main linear regression model used in this project is the following:

$$
R^e_t = \alpha + \beta_1 \text{SP}_t + \beta_2\text{TreasNote}_t + \beta_3'\text{Risk}_t +\\ \beta_4 \text{SP}_{t-1} + \beta_5\text{TreasNote}_{t-1} + \beta_6'\text{Risk}_{t-1} + \beta_7R_{t-1}^e + \varepsilon_t
$$

Where **excess FX returns** $R^e_t$ is estimated for all currency-pairs *CHF/USD*, *EUR/USD*, *JPY/USD*, and *GBP/USD*. The first three regressors are the returns on S&P500 futures ($\text{SP}_t$), Treasury Notes futures ($\text{TreasNote}_t$), and then three measures of risk ($\text{Risk}_t$). The rest of the regressors are lags of the regressors, and then also of the dependent variable. Of main interest are the first three coefficients. Ranaldo & Söderlind (2010) argues that the first three coefficients measures statistical **safe haven capabilities**. A good safe haven has $\beta_1 < 0$, $\beta_2 > 0$ and $\beta_3 > 0$. 

Two non-linear models are also estimated. The first one is a partial linear model: 

$$ R^e_t = g(x_{1,t}) + \beta' x_{2,t} + \varepsilon_t $$

where one regressor $x_{1,t}$ takes on any non-parametric transformation $g(\cdot)$ estimated using *Robinson's double residual method* (Robinson, 1988), while the rest takes on linear transformations. This estimation is done for $\text{SP}_t$, $\text{TreasNote}_t$ and $\text{Risk}_t$. The second non-linear model used is a piecewise linear model:

$$ R^e_t = \gamma_0 + \gamma_1 x_1 + \gamma_2 \max(x_t - \xi, 0) + \varepsilon_t $$

where parameters $\gamma_0, \gamma_1, \gamma_2$ and $\xi$ are estimated using Hansen (1982) 2SGMM. This model is used to compare the non-linear effects of FX volatility to the results in Ranaldo & Söderlind (2010). In addition, a dummy variable regression is used to quantify the incremental effects on these coefficients during important dates relating to the Russia-Ukraine war, to sanctions, or macroeconomic events:

$$ R^e_t = \alpha + \beta_1'x_t + (D_t \times \beta_2)' x_t + \varepsilon_t $$

where $D_t$ is the dummy that takes on a value of unity at the specified event-dates. 

## 1. Code Outline 

Note that the code is separated into 11 sections:

1. Preliminaries
2. Data Importation
3. Data Handling
4. Figure 4
5. Linear regression: Model 1
6. Cross-currencies
7. Multiple frequency regressions
8. Coefficient Quantiles
9. Dummy Variable Regressions: Model 4
10. Partial linear: Model 2
11. Piecewise linear: Model 3

## 2. Preliminaries

The following specifications (version) were used for this project:
- Operating system: Windows 10 x64 (build 19042)
- Programming language: R (4.1.2)
- GMM: (1.7)
- Sandwich: (3.0.2)
- readxl: (1.4.2)
- tseries: (0.10.53)
- zoo: (1.8.8)
- xts: (0.12.0)
- ggplot2: (3.4.1)
- purrr: (1.0.1)
- dplyr: (1.1.1)
- factoextra: (1.0.7)
- lmtest: (0.9.40)
- stats: (4.1.2)
- kernlab: (0.9.29)
- MASS: (7.3.54)

*Section 1: preliminaries* is where the code imports the required modules for the project. To ensure proper replication without issues, make sure to install the packages under the above versions. 

*NOTE (a): The package "GMM" does not work on certain Mac computers due to missing "libgomp.1.dylib" dependency*. Please use Windows instead.
*NOTE (b): if a package is not installed on the computer, you need to install the module first*. 

## 3. Replication Files

To replicate the tables and figures used in the paper, the aforementioned packages needs to be used. The underlying data required for replication is the underlying data file:

- Data.xlsx

which contains data downloaded from Bloomberg and the event-dates used in the paper. The data is sectioned into different sheets for each category of data: foreign exchange rates, market indices, policy interest rates, and the event dates.  

*NOTE:* the *Data.xlsx" file muse be in **the same folder** as the R code. 

## 4. Step-by-step Replication

The code is meant to simply be executed from top to bottom. First off, importation of the packages are done in *section 1: preliminaries*. Line 26 contains a vector of all packages used and lines 29-31 imports these packages. Then *section 2: data importation* imports the data from the underlying data file (which needs to be in the same folder). First the FX data is imported, then central bank rates, market indices, and finally the event dates.

In *section 3: data handling*, transformations and merges of the data is done to get the final matrix of features for Model 1. The interest rate differential is calculated using the policy rates and excess returns is calculated by subtracting this differential from the log returns. Realized volatility and log volatility is calculated before PCA is done to extract the FX volatility variable. 

*section 4: Figure 4* simply produces Figure 4 in the paper. This is the figure in the appendix illustrating the appreciation (depreciation) of the currencies during the first month of the war. A *.csv* is created in the current working directory. This file was used in LaTex to format **Figure 4**. The first main results are produced in *section 5: linear regression*. The output in Table I is produced here. The time-period is delimited on lines 271-272 to October 27, 2021 to March 7, 2023. At the very end of the section, on line 312 is the dataframe *Tab.I* located. This variable contains the information in **Table I** in the paper.

The following section, *section 6: Cross-currencies* calculates the results of Model 1 onto different cross-currencies (non-USD denominated pairs). The interest-rate differentials must also be recalculated before running these regressions. At the very end of the section, in the variable *Tab.VI* on line 504 is the output to **Table VI** from the paper contained. In *section 7: multiple-frequency regressions* is Model 1 applied on 1-day to 5-day returns. The realized volatilites and the PCA needs to be redone at the new frequency before the regressions can be re-run. The end of the section plots each of the panels in **Figure 3** from the paper. The code also writes 3 *.csv* files (one for each panel) containing the same information. These files are used to format the figure in LaTeX. 

*section 8: coefficient quantiles* uses the output of Table I to estimate the linear effects at the extreme values of the regressors. The output to **Table II** is contained in the variable *Tab.II* stored on line 894. The following section, *section 9: dummy variable regressions* implements Model 4 from the paper. The War, Sanctions, and Macroeconomic dummies (and "All") are created using the event dates from *Table V*. The invasion dummy is also constructed for the period February 24, 2022 to May 31, 2022. In the middle of the section, on line 1104, is the variable *Tab.III* created, which contains the output to **Table III** in the paper. Immediately afterwards, the variable *Tab.IV* is constructed, which contains the output to **Table IV** from the paper. At the very end of the section, both *Tab.III* and *Tab.IV* are stored for increased availability. 

The two final sections contains the results of Model 2 (partial linear model) and Model 3 (piecewise linear model). *section 10: partial linear* finds the non-linear transformation $g(\cdot)$ using Robinson's double residual method. Conditional expectations are estimated using Gaussian kernel regression where the bandwidth is estimated using 10-fold cross-validation. Line 1181 defines the function implementing this validation procedure. The band-width is estimated several times using this procedure and it can take some time for this section to run because of this procedure. The best bandwidth (measured by RMSE) is chosen for the estimations. The comments of the sections outlines each step we use: a) get $E[x_2 | SP]$ by kernel regression, b) get $E[CHF | SP]$ by kernel regression, c) residualize $R^e_t$ and $x_2$, d) calculate $\beta$ from the residualized equation: $\tilde{R}^e_t = \tilde{x}_2 \beta + \tilde{u}$, and finally: e) get $g(\cdot)$ by kernel regression of $R^e_t - x_2\hat{\beta}$ on $SP$. The results are not presented in section 10, but rather jointly presented in the following section along with the piecewise linear model. 

In *section 11: piecewise linear*, the results of both the piecewise linear model and the partially linear model are presented. The section starts by defining the functions conditions *g5.chf*, *g5.eur*, *g5.jpy*, *g5.gbp*, *g5.trs*, and *g5.sp* which specifies the necessary moment conditions for the 2SGMM. Each function calculates and outputs the sample version of the orthogonality conditions, which is then used by the *gmm* function from the *gmm* package to perform the 2SGMM and find the estimated parameters $\theta = ( \gamma_0, \gamma_1, \gamma_2, \xi )$. The results of these estimations are stored in the variables *res_sdf.chf*, *res_sdf.eur*, *res_sdf.jpy*, *res_sdf.gbp*, *res_sdf.trs*, and *res_sdf.sp* on lines 1765-1770. The final portion of section 11 calculates the confidence bands at 95\% confidence using the confidence interval formula. From line 1868 onwards, the formatting and plotting of **Figure 1** and **Figure 2** is done. On lines 2007-2020, *.csv* files are created to output the data of Figure 1 and Figure 2. This data is used to plot the figures using te *pgflpots* package in LaTeX.

That concludes the replication of all output in the paper.

***Fin!***

