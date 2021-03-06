# Visualizing the Canadian Economy during the COVID-19 Pandemic

This report summarizes Canada's economic conditions as of early April 2021. It replicatsthe visuals found in Bank of Canada’s Monetary Policy Report 2021 with new data, using ggplot2 package of R. Any continuation of trends or notable changes with respect to the figures in the report will be discussed. 


## COVID-19 Cases

![1](https://github.com/jiayi-ong/Visualizing-the-Canadian-Economy/blob/main/Images/1.COVID-Advanced%20Economies.png)

* The vertical dashed line indicates the last observation of the previous policy report.

Since the report, the number of new cases of COVID-19 has been steadily decreasing, with current levels well below the peak in around early January for most advanced economies. This may be attributed to tighter measures in containment and the rollout of effective vaccines. However, in recent days, an upward surge in new cases can be seen in some countries, reflecting high uncertainty in the evolution of the virus infection despite mass vaccination programs that have been implemented. This surge is especially noticeable for the European Union, and moderately noticeable for the United States and Canada. Canada’s proportion of daily new cases has remained steady as the second lowest amongst the five advanced economies, but the recent surge has elevated its position to third. 


## Equity Markets and Exchange Rates

![2](https://github.com/jiayi-ong/Visualizing-the-Canadian-Economy/blob/main/Images/2a.Equity%20Prices.png)

![3](https://github.com/jiayi-ong/Visualizing-the-Canadian-Economy/blob/main/Images/2b.Exchange%20Rates.png)

* The vertical dashed line indicates the last observation of the previous policy report.

The equity indices that have continued to rise since the report was published include the SP/TSX composite, the S&P500, and the STOXX 50. This reflects the decreasing uncertainty among investors and their expectations of improved growth prospects. These observations are consistent with decrease rates of new COVID-19 cases as seen in Chart 1. Also, as “bond yields continue to be very low for most advanced economies” (Governing Council of the Bank of Canada, 2021, pg. 4) due to low policy rates, it has supported higher demand for investment in equity, thus raising prices in the equity markets. The two indices that have decreased are the SSE composite and the MSCI. The Canadian dollar continues to strengthen against the USD, and the rising CEER indices also indicate an appreciating Canadian dollar against Canada’s major trading partners.


## Unemployment

![4](https://github.com/jiayi-ong/Visualizing-the-Canadian-Economy/blob/main/Images/9.Unemployment%20in%20Canada.png)

* The vertical dashed line indicates the last observation of the previous policy report.

This plot contains only one month of new data since the publishing of the report. Both total unemployment rate and long-term unemployment rate in Canada have increased from December 2020 to January 2021, continuing the increasing trend since the peak in 2020. The persistence observed in high levels of long-term unemployment rate may be exacerbated by skill erosion from prolonged periods without work, and “their attachment to the labor force may decrease” (Governing Council of the Bank of Canada, 2021, pg. 14). Recovery may also be more difficult as the pandemic has hit harder than the recession in 2008, as seen in the height of the peak in total unemployment in 2020 (which is at least two-fold that of the peak in 2008).


## Change in Employment by Sector

![5](https://github.com/jiayi-ong/Visualizing-the-Canadian-Economy/blob/main/Images/10a.Change%20in%20Employment.png)

* The vertical dashed line indicates the last observation of the previous policy report.

This plot in the previous policy report compare changes in unemployment over a period of a quarter, while Chart 10 above shows changes over only 2 months. Jobs are still being lost in the first two industries (by descending height arrangement in the chart), a trend which has not reversed since the last policy report. However, the magnitude of job loss is now much lower (e.g. -10,000 vs. -140,000 people in the last report, for the first industry). Notable reverses in trends include the Wholesale and retail trade industry, which had enjoyed about 80,000 employments in the previous report’s period but had now suffered a job loss of 45,000. The other services industry has also employed about 20,000 people, compared to a loss of nearly 50,000 jobs in the report. Industries such as Construction, Education, and Financial Services have continued to increase the number of people employed.


## CPI

![6](https://github.com/jiayi-ong/Visualizing-the-Canadian-Economy/blob/main/Images/11.CPI%20Inflation.png)

* The vertical dashed line indicates the last observation of the previous policy report.

Chart 11 above shows that inflation is heading towards to target of 2.0%, a sign of increasing aggregate demand, but remains well below the target. The range of core inflation measures have also widened.


## References

Governing Council of the Bank of Canada (2021, January 20). Monetary Policy Report: January 2021. Retrieved from https://www.bankofcanada.ca/2021/01/mpr-2021-01-20/

For data sources, view "Data Sources.pdf".
