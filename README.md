# Module-Challenge-4

# IMPORT DATA

1. We use three main imports (pandas, numpy, Path) along with %matplotlib for plotting. Pandas was assigned as 'pd' and numpy 'np' where as matplotlib uses 'inline.' 

2. To create the dataframe, we use the read_csv function with pandas and parameters index_col, parse_dates, infer_datetime_format to read the whale_navs.csv file into a dataframe, which I named whale_navs as seen below:

whale_navs = pd.read_csv(
    Path('./Resources/whale_navs.csv'),
    index_col="date",
    parse_dates=True,
    infer_datetime_format=True) 

This was followed by using the .head() function to preview the first five rows.

3. To calculate the percent change, we use the .pct_change() function on the whale_navs dataframe and assign it to a new dataframe for daily returns, whale_navs_dr. Once the new dataframe is made, we drop any null values with the dropna() function using whale_navs_dr and assigning it back to itself. We then use the .head() function again to check.

# QUANTITATIVE ANALYSIS
## Analyze the Performance

1. Now we have the whale_navs_dr dataframe, we want to plot the results. To do this, we use the .plot() function on whale_navs_dr with parameters (figsize=(25,15), title = "Daily Returns for Whale Navs", ylabel = "Percent Change (%)").

2. In calculating the cumulative returns, we use the .cumprod() function on (1 + whale_navs_dr) and assign it to a new variable, whale_navs_cr. Due to this being a cumulative statistic with higher numbers at the end, we use the .tail() to preview the data as shown:

whale_navs_cr = (1 + whale_navs_dr).cumprod()

whale_navs_cr.tail()

3. Following similar steps as 1., we plot whale_navs_cr with the .plot() function and parameters (figsize=(25,15), title= "Cumulative Returns for Whale Navs", ylabel = "Growth Multiple"). The y-axis is how much the stock has grown relative the the inital price (ex. the S&P grew around 1.8x max cumulatively from the original price in the data).

4. For answer, please refer to the risk_return_analysis notebook.

## Analyze the Volatility

1. This follows the same steps with the same labels as 1. from "Analyze the Performance," however, we also insert the parameter kind = 'box' into the .plot() function to designate a boxplot.

2. We first create a new dataframe varaible, portfolios_dr, and assign it to the whale_navs_dr using the .drop() function, with the only parameter (columns ='['S&P 500']) inserted. We use the .head() function to check that it worked, followed by creating a boxplot with the same steps as 1., except using the portfolios_dr dataframe and unique titles, as shown below: 

portfolios_dr = whale_navs_dr.drop(columns = ['S&P 500']) 

portfolios_dr.head() 

portfolios_dr.plot(kind ='box', figsize=(25,15), title= "Daily Returns for Whale Navs Excluding S&P 500", ylabel = "Percent Change(%)") 

3. For answer, please refer to the risk_return_analysis notebook. 

## Analyze the Risk

1. We want to calculate standard deviations sing the daily returns dataframe. We create a new variable, standard_deviations, and assign it to whale_navs_dr with the .std() function, then using .head() to check. We then want to sort these values, so we create a new variable called sorted_sd which is assigned to standard_deviations using the sprt_values() function. This sorts the funds from lowest to highest. We then use .head() on sorted_sd to check the ordering. 

2. First we made a days variable set to 252 for trading days. To calculate the annualized standard deviations, we make a new variable, annualized_sd, and assign it to standard_deviations multipled by .sqrt() function using numpy as np, with the parameters days, which we know is 252. Again, we then sort the annualized_sd using the .sort_values() function and assign it to a new variable, sorted_annualized_sd, followed by using the .head() function to check. Note, we do not just use sorted_sd from before in the annualized_sd varibale assignment because the sorted order of sorted_sd and sorted_annualized_sd is not the same.

3. To create this plot, we will use a combo of three functions, rolling(), std(), and plot(), using the whale_navs_dr dataframe. For a 21-day rolling window, we use .rolling(window=21) with the rest of the code expressed as:

whale_navs_dr.rolling(window=21).std().plot(title = 'Standard Deviations: 21-Day Rolling Window Including S&P 500', figsize = (25,15), ylabel = "Standard Deviations")

4. For just the portfolios, we now do the exact same thing as 3. except using the portfolios_dr dataframe from earlier. 

5. For answers, please refer to the risk_return_analysis notebook. 

## Analyze the Risk-Return Profile 

1. For annualized average returns, we make an annualized_avg_returns varaible and assign it to whale_navs_dr using the .mean() function multipled by 'days' from before. Like before, we then use the sort_values() function on the annualized_avg_returns dataframe and assign it to a new variable, sorted_aar, which we preview using .head().

2. For the sharpe ratio, using variable sharpe, we divide annualized_avg_return by annualized_sd from before. Note we don't use our "sorted" variables as the columns may provide us issues. We then sort the sharpe ratio with sort_values() function for accurate ordering. 

3. For a bar graph, we will use .plot.bar() on the sharpe variable and include unique title, ylabel, and figsize variables, as shown as:

sharpe.plot.bar(title = 'Sharpe Ratios Including S&P 500', figsize = (20,10), ylabel = "Sharpe Ratios")

4. For answer, please refer to the risk_return_analysis notebook.

## Diversify the Portfolio 

1. We first create a variable, sp_sixty_var, for the variance using a 60-day rolling window. We assign this variable to whale_navs_dr['S&P 500'] to specifically use the S&P column, This is followed by .rolling(window=60).var() to establish the rolling window of 60 days and calculate variance. We preview this with the tail() function (no data in beginning with rolling windows)

### Portfolio 1: Berkshire Hathaway Inc. 

1. For covariance, we follow the above step closely, but replace .var with .cov and 'S&P 500' with 'BERKSHIRE HATHAWAY INC'. we assigned this to variable bh_cov and previewed it with the tail() function.

2. For beta, we created a variable, bh_beta, and calculate it with bh_cov just created divided by sp_sixty_var, the S&P 500 variance. We similarly preview the data with the tail() function. 

3. We simply create a bhbeta_mean variable and assign it to bh_beta.rolling(window=60).mean(), using the tail() function to preview the mean values (Note: if you plotted this, it would appear as a smoothed-out version of the graph create in the next step). 

4. Note: this only asks to plot the 60-day rolling beta, not average 60-day rolling beta. To do this, we take bh_beta with the plot() function with parameters of figsize, title (specific to Berkshire Hathaway), and ylabel. 

### Porfolio 2: Paulson & Co.Inc.

1. Following the exact procedure as in Portfolio 1, we create a variable, pco_cov, and assign it to:

pco_cov = whale_navs_dr['PAULSON & CO.INC.'].rolling(window = 60).cov() 

Followed by using the tail function on pco_cov. 

2. We create pco_beta variable and assign it to pco_cov / sp_sixty_var similar to Portfolio 1, followed by the tail function on pco_beta.

3. We create a pcomean_beta variable and assign it to pco_beta.rolling(window=60).mean(), followed by the tail function on pcomean_beta.

4. Exactly like in Portfolio 1, we plot pco_beta with the .plot function and parameters (figsize = (20,15), title = '60-Day Rolling Beta for Paulson & co.Inc.', ylabel = 'Beta').

5. For Questions 1 and 2 answers, please refer to the risk_return_analysis notebook. 


