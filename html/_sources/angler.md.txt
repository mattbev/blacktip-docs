# Angler API
Angler is a SEC financial data analysis tool provided by Blacktip Research. 
You must make an account on the [Blacktip website](http://blacktipresearch.com) prior to use.

## Install, Import, and Initialization
You can find Angler on [PyPI](https://pypi.org/project/blacktip/), and can install via the 
terminal using `pip install blacktip`. To import Angler in a file, do the following:
```<!-- language: python --> 
>>> from blacktip.angler import Angler
>>> instance = Angler(username, password)
```
Replace `username` and `password` with your personal login credentials.

## Angler Functions
**The querying object.** Angler provides several querying functions to interface with the database. 

### *Angler.queryCIK(ticker)*
Get the CIK (central index key) of an inputted ticker.

**Parameters:**
* **ticker : string**   
    * the ticker for which to search the corresponding CIK

**Returns:**
* **tuple**
    * a tuple containing (ticker, CIK) as strings 
    
Example usage:
```<!-- language: python --> 
>>> Angler.queryCIK("AAPL")
('AAPL', '320193')
```
    
### *Angler.queryName(self, ticker_or_CIK)*
Get the name of a company from its ticker or CIK number.

**Parameters:**
* **ticker_or_CIK : string or int**
    * the ticker or CIK number of the company

**Returns:**
* **string**
    * the name of the company
    
Example usage:
```<!-- language: python --> 
>>> instance.queryName("AAPL") 
'APPLE INC'
>>> instance.queryName(320193)
'APPLE INC'
```

### *Angler.query10K(self, ticker_or_CIK, years)*
Get company 10-K reports for selected years.

**Parameters:**
* **ticker_or_CIK : string or int**
    * the ticker or CIK number of the company
* **years : int or iterable**
    * the year or years of interest
    
**Returns:**
* **Form10K**
    * returns a Form10K object with the selected query
    
Example usage:
```<!-- language: python --> 
>>> form10K =  instance.query10K("AAPL", [2018, 2019])
>>> print(form10K)
fy                                                                 2018          2019
tag                                                uom
AccountsPayableCurrent                             USD     4.424200e+10  4.623600e+10
AccountsReceivableNetCurrent                       USD     1.787400e+10  2.292600e+10
AccruedIncomeTaxesNoncurrent                       USD     2.570000e+08  3.358900e+10
AccumulatedDepreciationDepletionAndAmortization... USD     4.129300e+10  4.909900e+10
AccumulatedOtherComprehensiveIncomeLossNetOfTax    USD    -1.500000e+08 -5.840000e+08
...                                                                 ...           ...
UnrecordedUnconditionalPurchaseObligationBalanc... USD     9.328000e+09  8.211000e+09
UnrecordedUnconditionalPurchaseObligationDueAft... USD     6.600000e+07  1.100000e+08
WeightedAverageNumberDilutedSharesOutstandingAd... shares  2.946100e+07  3.445000e+07
WeightedAverageNumberOfDilutedSharesOutstanding    shares  5.500281e+09  5.251692e+09
WeightedAverageNumberOfSharesOutstandingBasic      shares  5.470820e+09  5.217242e+09
```

### *Angler.query10Q(self, ticker_or_CIK, periods)*
Get company 10-Q reports for selected periods.

**Parameters:**
* **ticker_or_CIK : string or int**
    * the ticker or CIK number of the company
* **periods : int or iterable of ints, tuple or iterable of tuples**
    * the periods of interest in (year, quarter) format
    
**Returns:**
* **Form10Q**
    * returns a Form10Q object with the selected query
    
Example usage:
```<!-- language: python --> 
# the following commands are equivalent Angler.query10Q() calls
>>> instance.query10Q("AAPL", 2019)
>>> instance.query10Q("AAPL", [(2019, "Q1"), (2019, "Q2"), (2019, "Q3")])
fy                                                                 2019
fp                                                                   Q1            Q2            Q3
tag                                                uom
AccountsPayableCurrent                             USD     5.588800e+10  3.044300e+10  5.588800e+10
AccountsReceivableNetCurrent                       USD     2.318600e+10  1.508500e+10  2.318600e+10
AccruedIncomeTaxesNoncurrent                       USD     3.358900e+10  3.098600e+10  3.358900e+10
AccumulatedDepreciationDepletionAndAmortization... USD     4.909900e+10  5.429000e+10  4.909900e+10
AccumulatedOtherComprehensiveIncomeLossNetOfTax    USD    -3.454000e+09 -1.499000e+09 -6.390000e+08
...                                                                 ...           ...           ...
UnrecognizedTaxBenefitsThatWouldImpactEffective... USD     8.200000e+09  7.300000e+09  8.100000e+09
UnrecordedUnconditionalPurchaseObligationBalanc... USD     7.400000e+09  7.600000e+09  8.100000e+09
WeightedAverageNumberDilutedSharesOutstandingAd... shares  4.491000e+07  2.657500e+07  3.074700e+07
WeightedAverageNumberOfDilutedSharesOutstanding    shares  5.157787e+09  4.700646e+09  4.601380e+09
WeightedAverageNumberOfSharesOutstandingBasic      shares  5.112877e+09  4.704945e+09  4.660175e+09
```
```<!-- language: python --> 
# additionally, one can query multiple years
>>> instance.query10Q("AAPL", [2018, 2019])
fy                                                                 2018                ...          2019
fp                                                                   Q1            Q2  ...            Q2            Q3
tag                                                uom                                 ...
AccountsPayableCurrent                             USD     6.298500e+10  4.904900e+10  ...  3.044300e+10  5.588800e+10
AccountsReceivableNetCurrent                       USD     2.344000e+10  1.787400e+10  ...  1.508500e+10  2.318600e+10
AccruedIncomeTaxesNoncurrent                       USD     3.491300e+10  2.570000e+08  ...  3.098600e+10  3.358900e+10
AccruedLiabilitiesCurrent                          USD     2.574400e+10  2.574400e+10  ...           NaN           NaN
AccumulatedDepreciationDepletionAndAmortization... USD     4.129300e+10  4.129300e+10  ...  5.429000e+10  4.909900e+10
...                                                                 ...           ...  ...           ...           ...
UnrecognizedTaxBenefitsThatWouldImpactEffective... USD     7.700000e+09  8.200000e+09  ...  7.300000e+09  8.100000e+09
UnrecordedUnconditionalPurchaseObligationBalanc... USD     8.700000e+09  8.300000e+09  ...  7.600000e+09  8.100000e+09
WeightedAverageNumberDilutedSharesOutstandingAd... shares  2.933400e+07  3.589700e+07  ...  2.657500e+07  3.074700e+07
WeightedAverageNumberOfDilutedSharesOutstanding    shares  5.327995e+09  5.261688e+09  ...  4.700646e+09  4.601380e+09
WeightedAverageNumberOfSharesOutstandingBasic      shares  5.298661e+09  5.225791e+09  ...  4.704945e+09  4.660175e+09
```

### *Angler.query(self, command)*
Query your own custom command on the XBRL dataset in MySQL format.

**Parameters:**
* **command : string**
    * the command string

**Returns:**
* **SQLAlchemy.engine.ResultProxy**
    * the query result
    
Example usage:
```<!-- language: python --> 
>>> _, cik = Angler.queryCIK("aapl")
>>> command = "SELECT * FROM sub WHERE cik={cik}".format(cik=cik)
>>> instance.query(command).first()
('0000320193-17-000009', Decimal('320193'), 'APPLE INC', Decimal('3571'), 'US', 'CA', 'CUPERTINO', '95014', 'ONE INFINITE LOOP', '', '(408) 996-10', 'US', 'CA', 'CUPERTINO', '95014', 'ONE INFINITE LOOP', '', 'US', 'CA', Decimal('942404110'), 'APPLE COMPUTER INC', '19970808', '1-LAF', 0, '0930', '10-Q', datetime.date(2017, 6, 30), 2017, 'Q3', datetime.date(2017, 8, 2), datetime.datetime(2017, 8, 2, 16, 31), 0, 1, 'aapl-20170701.xml', Decimal('1'), '', '2017q3')
```

### *Angler.dispose(self)*
Closes the connection to the database.

Example usage:
```<!-- language: python --> 
>>> instance.dispose()
```


## Form Functions
**The (parent) data manipulation function.** The **Form** object offers some convenient data 
manipulation functions that apply to all (10-K and 10-Q) SEC forms. **Form10K** and **Form10Q** 
objects are subclasses of **Form** and therefore all **Form** functions apply to them.

### *Form.form(self)*
Returns the contents of the Form object.

**Returns:**
* **pandas.DataFrame**
    * the contents of the Form as a pandas dataframs
    
Example usage:
```<!-- language: python --> 
>>> form = instance.query10K("AAPL", 2019)
>>> form.form() # returns a Form10K object, a subclass of Form 
fy                                                                 2019
tag                                                uom
AccountsPayableCurrent                             USD     4.623600e+10
AccountsReceivableNetCurrent                       USD     2.292600e+10
AccruedIncomeTaxesNoncurrent                       USD     2.954500e+10
AccumulatedDepreciationDepletionAndAmortization... USD     5.857900e+10
AccumulatedOtherComprehensiveIncomeLossNetOfTax    USD    -5.840000e+08
...                                                                 ...
UnrecordedUnconditionalPurchaseObligationBalanc... USD     8.211000e+09
UnrecordedUnconditionalPurchaseObligationDueAft... USD     1.100000e+08
WeightedAverageNumberDilutedSharesOutstandingAd... shares  3.107900e+07
WeightedAverageNumberOfDilutedSharesOutstanding    shares  4.648913e+09
WeightedAverageNumberOfSharesOutstandingBasic      shares  4.617834e+09
```

To save your form to csv, use the **Form.form()** function then **pandas.DataFrame.to_csv()**:
```<!-- language: python --> 
>>> pandas_dataframe = form.form()
>>> pandas_dataframe.to_csv(filepath) # specify filepath to save to
```

Note, one can print and perform basic indexing directly on the **Form** object:
```<!-- language: python --> 
>>> print(form)
...
>>> print(form[2019])
...
```

### *Form.filter(self, regex, index='tag')*
Filter rows of the form by a specific keyword regular expression (regex).

**Parameters:**
* **regex : string**
    * the regular expression to filter the rows by
* **index : string**
    * the index ("tag" or "uom") to apply the filter to; defaults to "tag"
        
**Returns:**
* **pandas.DataFrame**
    * a pandas dataframe with the filtered results
    
Example usage:
```<!-- language: python --> 
>>> form = instance.query10Q("AAPL", [2018, 2019])
>>> form.filter("^NetIncomeLoss$")
fy                         2018                                      2019
fp                           Q1            Q2            Q3            Q1            Q2            Q3
tag           uom
NetIncomeLoss USD  2.006500e+10  1.382200e+10  1.151900e+10  1.996500e+10  1.156100e+10  1.004400e+10
```

### *Form.asset_sheet(self)*
Filters the form to generate an asset sheet. Wrapper around `form.filter("Asset|Assets")`.

**Returns:**
* **pandas.DataFrame**
    * a pandas dataframe with just values pertaining to "assets"
    
Example usage:
```<!-- language: python --> 
>>> form = instance.query10K("aapl", 2019)
>>> form.asset_sheet()
fy                                                              2019
tag                                                uom
Assets                                             USD  3.385160e+11
AssetsCurrent                                      USD  1.628190e+11
AssetsNoncurrent                                   USD  1.756970e+11
DeferredTaxAssetsDeferredIncome                    USD  1.372000e+09
DeferredTaxAssetsGoodwillAndIntangibleAssets       USD  1.143300e+10
DeferredTaxAssetsLiabilitiesNet                    USD  8.045000e+09
DeferredTaxAssetsNet                               USD  1.964000e+10
DeferredTaxAssetsOther                             USD  6.970000e+08
DeferredTaxAssetsPropertyPlantAndEquipment         USD  1.370000e+08
DeferredTaxAssetsTaxDeferredExpenseCompensation... USD  7.490000e+08
DeferredTaxAssetsTaxDeferredExpenseReservesAndA... USD  5.389000e+09
DeferredTaxAssetsUnrealizedLossesOnAvailablefor... USD  0.000000e+00
DerivativeAssetsReductionforMasterNettingArrang... USD  2.700000e+09
IncreaseDecreaseInOtherOperatingAssets             USD -8.730000e+08
NoncurrentAssets                                   USD  3.737800e+10
OtherAssetsCurrent                                 USD  1.235200e+10
OtherAssetsNoncurrent                              USD  3.297800e+10
```

### *Form.liability_sheet(self)*
Filters the form to generate a liability sheet. Wrapper around `form.filter("Liability|Liabilities")`.

**Returns:**
* **pandas.DataFrame**
    * a pandas dataframe with just values pertaining to liabilities
    
Example usage:
```<!-- language: python --> 
>>> form = instance.query10Q("AAPL", (2019, "Q2"))
>>> form.liability_sheet()
fy                                                              2019
fp                                                                Q2
tag                                                uom
ContractWithCustomerLiability                      USD  8.200000e+09
ContractWithCustomerLiabilityCurrent               USD  5.532000e+09
ContractWithCustomerLiabilityRevenueRecognized     USD  1.900000e+09
DerivativeLiabilitiesReductionforMasterNettingA... USD  1.700000e+09
IncreaseDecreaseInContractWithCustomerLiability    USD -5.400000e+08
IncreaseDecreaseInOtherOperatingLiabilities        USD -3.273000e+09
Liabilities                                        USD  2.361380e+11
LiabilitiesAndStockholdersEquity                   USD  3.419980e+11
LiabilitiesCurrent                                 USD  9.377200e+10
LiabilitiesNoncurrent                              USD  1.423660e+11
OtherAccruedLiabilitiesNoncurrent                  USD  2.117900e+10
OtherLiabilitiesCurrent                            USD  3.536800e+10
OtherLiabilitiesNoncurrent                         USD  5.216500e+10
```

### *Form.debt_sheet(self)*
Filters the form to generate a debt sheet. Wrapper around `form.filter("Debt|Debts")`.

**Returns:**
* **pandas.DataFrame**
    * a pandas dataframe with just values pertaining to debts
    
Example usage:
```<!-- language: python --> 
>>> form = instance.query10Q("AAPL", 2019)
>>> form.debt_sheet()
fy                                                               2019
fp                                                                 Q1            Q2            Q3
tag                                                uom
AvailableForSaleDebtSecuritiesAccumulatedGrossU... USD   9.400000e+07  3.850000e+08  8.920000e+08
AvailableForSaleDebtSecuritiesAccumulatedGrossU... USD   3.871000e+09  1.532000e+09  4.610000e+08
AvailableForSaleDebtSecuritiesAmortizedCostBasis   USD   2.488120e+11  2.265580e+11  2.101790e+11
AvailableForSaleSecuritiesDebtSecurities           USD   2.450350e+11  2.254110e+11  2.106100e+11
...
ProceedsFromShortTermDebtMaturingInMoreThanThre... USD   2.166000e+09  1.023500e+10  1.297700e+10
RepaymentsOfLongTermDebt                           USD            NaN  2.500000e+09  5.500000e+09
RepaymentsOfShortTermDebtMaturingInMoreThanThre... USD   4.171000e+09  7.787000e+09  1.128300e+10
ShortTermDebtWeightedAverageInterestRate           pure  2.390000e-02  2.560000e-02  2.490000e-02
```

### *Form.to_csv(self, path_or_buf, ...)*
Saves the form as a csv file. Wrapper around [pandas.DataFrame.to_csv](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_csv.html); 
follow that link for the full documentation.

Example usage:
```<!-- language: python --> 
>>> form = instance.query10Q("aapl", 2018)
>>> form.to_csv("Apple_2018_10Q")
```


## Form10K Functions
**Form10K** represents a SEC form 10-K and is a subclass of **Form**,
thus inheriting all of the functions included in the **Form** class. 

### *Form10K.calc_ROE(self, as_list=False)*
Calculates return on equity (ROE) ratio. ROE = (net income) / (total stockholders equity).

**Parameters:**
* **as_list : boolean**
    * returns the values as a list if True, otherwise returns values in a dataframe; defaults
    to False
    
**Returns:**
* **list or pandas.DataFrame**
    * a list or pandas dataframe detailing the ROE for every year in the query
    
Example usage:
```<!-- language: python --> 
>>> form10K = instance.query10K("aapl", [2017, 2018, 2019])
>>> form10K.calc_ROE()
fy             2017      2018      2019
tag uom
ROE ratio  0.360702  0.555601  0.610645
```

### *Form10K.calc_CurrentRatio(self, as_list=False)*
Calculates the current ratio. CurrentRatio = (current assets) / (current liabilities)

**Parameters:**
* **as_list : boolean**
    * returns the values as a list if True, otherwise returns values in a dataframe; defaults
    to False
    
**Returns:**
* **list or pandas.DataFrame**
    * a list or pandas dataframe detailing the CurrentRatio for every year in the query
    
Example usage:
```<!-- language: python --> 
>>> form10K = instance.query10K("aapl", [2017, 2018, 2019])
>>> form10K.calc_CurrentRatio()
fy                      2017      2018      2019
tag          uom
CurrentRatio ratio  1.276063  1.123843  1.540126
```

### *Form10K.calc_DebtToEquity(self, as_list=False)*
Calculates the debt to equity ratio. DebtToEquity = (total liabilities) / (total stockholders equity)

**Parameters:**
* **as_list : boolean**
    * returns the values as a list if True, otherwise returns values in a dataframe; defaults
    to False
    
**Returns:**
* **list or pandas.DataFrame**
    * a list or pandas dataframe detailing the DebtToEquity for every year in the query
    
Example usage:
```<!-- language: python --> 
>>> form10K = instance.query10K("aapl", [2017, 2018, 2019])
>>> form10K.calc_DebtToEquity()
fy                      2017      2018      2019
tag          uom
DebtToEquity ratio  1.799906  2.413301  2.741004
```

### *Form10K.calc_BookValue(self, as_list=False)*
Calculates the book value. BookValue = (total assets) - (total liabilities)

**Parameters:**
* **as_list : boolean**
    * returns the values as a list if True, otherwise returns values in a dataframe; defaults
    to False
    
**Returns:**
* **list or pandas.DataFrame**
    * a list or pandas dataframe detailing the BookValue for every year in the query
    
Example usage:
```<!-- language: python --> 
>>> form10K = instance.query10K("aapl", [2017, 2018, 2019])
>>> form10K.calc_BookValue(as_list=True)
[1.340470e+11, 1.071470e+11, 9.048800e+10]
```

## Form10Q Functions
**Form10K** represents a SEC form 10-Q and is a subclass of **Form**,
thus inheriting all of the functions included in the **Form** class. 

Currently, there are no functions specifically for **Form10Q**.
