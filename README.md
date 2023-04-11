# Safe Have Currencies and the Russia-Ukraine War

## An estimation of linear and non-linear safe haven capabilities of the Swiss franc, the euro, the yen, and the British pound against the U.S. dollar

The main linear regression model used in this project is the following:

$$
R^e_t = \alpha + \beta_1 \text{SP}_t + \beta_2\text{TreasNote}_t + \beta_3'\text{Risk}_t +\\ \beta_4 \text{SP}_{t-1} + \beta_5\text{TreasNote}_{t-1} + \beta_6'\text{Risk}_{t-1} + \beta_7R_{t-1}^e + \varepsilon_t
$$

Where **excess FX returns** $R^e_t$ is estimated for all currency-pairs *CHF/USD*, *EUR/USD*, *JPY/USD*, and *GBP/USD*. The first three regressors are the returns on S&P500 futures ($\text{SP}_t$), Treasury Notes futures ($\text{TreasNote}_t$), and then three measures of risk ($\text{Risk}_t$). The rest of the regressors are lags of the regressors, and then also of the dependent variable. Of main interest are the first three coefficients. Ranaldo & Söderling (2010) argues that the first three coefficients measures statistical **safe haven capabilities**. A good safe haven has $\beta_1 < 0$, $\beta_2 > 0$ and $\beta_3 > 0$. 

Two non-linear models are also estimated. The first one is a partial linear model: 

$$ R^e_t = g(x_{1,t}) + \beta' x_{2,t} + \varepsilon_t $$

where one regressor $x_{1,t}$ takes on any non-parametric transformation $g(\cdot)$ estimated using *Robinson's double residual method* (Robinson, 1988), while the rest takes on linear transformations. This estimation is done for $\text{SP}_t$, $\text{TreasNote}_t$ and $\text{Risk}_t$. The second non-linear model used is a piecewise linear model:

$$ R^e_t = \gamma_0 + \gamma_1 x_1 + \gamma_2 \max\{x_t - \xi, 0\} + \varepsilon_t $$

where parameters $\gamma_0, \gamma_1, \gamma_2$ and $\xi$ are estimated using Hansen (1982) 2SGMM. This model is used to compare the non-linear effects of FX volatility to the results in Ranaldo & Söderlind (2010). In addition, a dummy variable regression is used to quantify the incremental effects on these coefficients during important dates relating to the Russia-Ukraine war, to sanctions, or macroeconomic events:

$$ R^e_t = \alpha + \beta_1'x_t + (D_t \times \beta_2)' x_t + \varepsilon_t $$

where $D_t$ is the dummy that takes on a value of unity at the specified event-dates. 

## 1. Code Outline 

Note that the code is separated into 11 sections:

1. Preliminaries
2. Data Importation
3. Data Handling
4. Figure "Y"
5. Linear regression: Model 1
6. Cross-currencies
7. Multiple frequency regressions
8. Coefficient Quantiles
9. Dummy Variable Regressions: Model 4
10. Partial-linear: Model 2
11. Piece-wise linear: Model 3

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

*Section 1: Preliminaries* is where the code imports the required modules for the project. To ensure proper replication without issues, make sure to install the packages under the above versions. 

*NOTE (a): The package "GMM" does not work on certain Mac computers due to missing "libgomp.1.dylib" dependency*. Please use Windows instead.
*NOTE (b): if a package is not installed on the computer, you need to install the module first*. 

## 3. Replication Files

To replicate the tables and figures used in the paper, the aforementioned modules needs to be used. The underlying data required for replication is the underlying data file:

- Data.xlsx

which contains data downloaded from Bloomberg and the event-dates used in the paper. The data is sectioned into different sheets for each category of data: foreign exchange rates, market indices, policy interest rates, and the event dates.  

*NOTE:* the *Data.xlsx" file muse be in **the same folder** as the R code. 

## 4. Running the Code for a Full Paper Replication


(NOT FINISHED!)
To run the code as intended, please make sure that all the modules in *section 2* are installed before importing them. Ideally, use the same versions of each module as we did. Furthermore, pay attention to *section 3: Data Importation*. In order to import the data as intended, find the working directory of all replication files and change the directory to that folder in the following line:

<img width="474" alt="Screenshot 2023-03-19 at 17 19 16" src="https://user-images.githubusercontent.com/123584534/226189518-cb964f6e-df43-456e-bd1a-235aabf8c4d4.png">

Do *not* change the filename or sheet-name variables in the cell.

To replicate all figures and tables used in the paper, take note of the only cell in *section 1: Model Specification*. It is in this cell where you can change between the model specifications used. As outlined in the paper, we consider 3 models: 

1. BHK: $e_t^{(2)}$ and $e_t^{(5)}$
2. SML: $e_t^{(1)}$, $e_t^{(5)}$ and $e_t^{(10)}$
3. S2: $e_t^{(1)}$ and $e_t^{(2)}$

To implement the BHK model, simply uncomment the 'VARS' variable where we indicate that the BHK specification is, and then run every cell in the notebook from start to end. Note that it may take some seconds for some of the cell to execute, depending on GPU.

<img width="296" alt="bhk_spec" src="https://user-images.githubusercontent.com/123584534/226125909-62f7ec69-46e8-4c65-8df2-960efaf1cf3b.png">

Similarly, to implement the SML model, *reset the kernel* and simply uncomment the 'VARS' variable where we indicate that the SML specification is and run every cell again.

<img width="286" alt="sml_spec" src="https://user-images.githubusercontent.com/123584534/226125910-a3184ce7-7965-456e-b3e6-e782f1ee2c6d.png">

Finally, to implement the S2 model, *reset the kernel* and simply uncomment the 'VARS' variable where we indicate that the S2 specification is and run every cell again.

<img width="299" alt="s2_spec" src="https://user-images.githubusercontent.com/123584534/226126010-f3a4b5c9-827c-4c96-a8af-aaa81f294d17.png">

## 5. Where to find outputs included in the paper?

Simply, for Figure 1, Figure 2, and the Appendix figures (Figure 5 and Figure 6), look at the output in sections 5, 7, and 11, respectively.

In the only cell in *section 8: Predictive OLS Model*, the OLS output in Table 2 for the active model is outputted, as well as its respective panel that is in Figure 3. Therefore, to get every number in the OLS portion of Table 2, you need to run the notebook for each model and take note of the output for each active model. Similarly, to get every panel of Figure 3, you need to save the figure output for each active model.

The rest of Table 2, the part that pertains to the VAR(1) model is contained under the following section, *section 9: VAR(1) Model*. In the first cell in that section you will find the output to the VAR(1) portion of Table 2. Again, the output pertains only to the active model, and to replicate the results for all models used, you will have to run the notebook for each model and take note of the output for each active model. 

The last cell in *section 9: VAR(1) Model* also contains the outputs to Figure 4 in the paper. The output contains the time-series decomposition of equity yields at all maturities between 1-10 years. In Figure 4, we only include maturities 2, 5, and 10 for brevity. To find these figures, simply save the 2nd, 5th, and 10th figure in the output. Again, the output pertains only to the active model, and to replicate the figures for all models used, you will have to run the notebook for each model and take note of the output for each active model. 

Finally, to replicate Table 3 from the paper, consider *section 10: Variance Decomposition*. There are 2 cells of code in the section and the second one outputs the decomposition for the active model. Again, the output pertains only to the active model, and to replicate the output for all models, you will have to run the notebook for each model and take note of the output for each active model. 

***Fin!***
