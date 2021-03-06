Plots
================
Jia Yi ONG
3/30/2021

``` r
library(dplyr)
library(ggplot2)
library(lubridate)
library(zoo)
```

# GDP by Industry

``` r
data = read.csv("GDP_by_industry.csv", stringsAsFactors = FALSE)
```

``` r
data = data %>% 
  rename(date = `ï..date`) %>%
  mutate(date = dmy(date),
         A = Accommodation.and.food.services + 
           Arts..entertainment.and.recreation,
         B = Transportation.and.warehousing,
         C = Mining..quarrying..and.oil.and.gas.extraction,
         D = Educational.services + Health.care.and.social.assistance,
         E = Other.services..except.public.administration.,
         F = Finance.and.insurance + Real.estate.and.rental.and.leasing,
         G = All.industries) %>%
  select(date, A, B, C, D, E, F, G)

data = data %>%
  mutate(A = A/data$A[1]*100,
         B = B/data$B[1]*100,
         C = C/data$C[1]*100,
         D = D/data$D[1]*100,
         E = E/data$E[1]*100,
         F = F/data$F[1]*100,
         G = G/data$G[1]*100)
```

``` r
colors = c("Accommodation, food, arts, \n entertainment and recreation" = "lightblue",
           "Transportation \n and \n warehousing" = "grey",
           "Oil and gas extraction \n and support activities" = "red",
           "Education, health care \n and social assistance" = "green",
           "Other industries" = "yellow",
           "Finance, insurance, real \n estate, rental and leasing" = "purple",
           "All industries" = "black")
size = 0.8
```

png(“6.GDP by Industry.png”, units=“in”, width=7, height=5, res=1080)

``` r
ggplot(data, aes(x = date)) +
  geom_line(aes(y = A, color = colors[1] %>% names()), size = size) +
  geom_line(aes(y = B, color = colors[2] %>% names()), size = size) +
  geom_line(aes(y = C, color = colors[3] %>% names()), size = size) +
  geom_line(aes(y = D, color = colors[4] %>% names()), size = size) +
  geom_line(aes(y = E, color = colors[5] %>% names()), size = size) +
  geom_line(aes(y = F, color = colors[6] %>% names()), size = size) +
  geom_line(aes(y = G, color = colors[7] %>% names()), size = size) +
  geom_vline(xintercept = dmy("01-10-2020"), linetype = "dashed") +
  scale_color_manual(name = "", values = colors) +
  scale_y_continuous(breaks = seq(30, 110, by = 10), position = "right") +
  labs(title = "Chart 6: Real GDP by Industry",
       subtitle = "Index: January 2020 = 100, monthly data",
       x = "", y = "Index", caption = "Last observation: JAN 2021") +
  theme(legend.position = "bottom", plot.caption.position = "plot")
```

![](Plots_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

# 11. CPI Inflation

``` r
data = read.csv("inflation.csv", stringsAsFactors = FALSE)
```

``` r
data = data %>%
  rename(date = `ï..date`) %>%
  mutate(date = dmy(date)) %>%
  filter(date >= dmy("01-01-2018")) %>%
  rowwise() %>%
  mutate(min = min(c(CPI_TRIM, CPI_COMMON, CPI_MEDIAN))) %>%
  mutate(max = max(c(CPI_TRIM, CPI_COMMON, CPI_MEDIAN)))
```

png(“11.CPI Inflation.png”, units=“in”, width=7, height=5, res=1080)

``` r
fill = c("Range of core \n inflation measures" = "lightblue")
colors = c("Total CPI" = "red", "Target" = "black")
size = 0.8

ggplot(data, aes(x = date)) +
  geom_ribbon(aes(ymin = min, ymax = max, fill = fill %>% names())) +
  geom_line(aes(y = CPI, color = colors[1] %>% names()), size = size) +
  geom_line(aes(y = 2, color = colors[2] %>% names()), size = size) +
  geom_vline(xintercept = dmy("01-11-2020"), linetype = "dashed") +
  scale_fill_manual(name = "", values = fill) +
  scale_color_manual(name = "", values = colors) +
  scale_y_continuous(breaks = seq(-0.5, 3, by = 0.5), position = "right") +
  labs(title = "Chart 11: CPI Inflation and Core Measures",
       subtitle = "Year-over-year percentage change, monthly data",
       x = "", y = "%", caption = "Last observation: FEB 2021") +
  theme(legend.position = "bottom", plot.caption.position = "plot")
```

![](Plots_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

# 9. CAN Unemployment

``` r
data = read.csv("CAN_unemployment.csv", stringsAsFactors = FALSE)
```

``` r
data = data %>%
  rename(date = `ï..DATE`) %>%
  mutate(date = ymd(date),
         rest_num = total_num - long_num,
         rest_rate = total_rate - long_rate)
```

``` r
# scaling for double axes plot
primary = data$rest_rate
secondary = data$rest_num
ylim.prim = c(floor(min(primary)), ceiling(max(primary)))
ylim.secon = c(min(secondary), max(secondary))
b = diff(ylim.prim)/diff(ylim.secon)/0.92
a = (ylim.prim[1] - ylim.secon[1])*b + 5.2

colors = c("Long-term unemployment \n rate (left scale)" = "red",
           "Total unemployment \n rate (left scale)" = "lightblue")
fills = c("Long-term unemployed \n people (right scale)" = "red",
          "Rest of unemployed \n people (right scale)" = "lightblue")
size = 0.8
alpha = 0.3
```

png(“9.Unemployment in Canada.png”, units=“in”, width=7, height=5,
res=1080)

``` r
ggplot(data, aes(x = date)) +
  geom_line(aes(y = long_rate, color = colors[1] %>% names()), size = size) +
  geom_line(aes(y = rest_rate, color = colors[2] %>% names()), size = size) +
  geom_area(aes(y = a + b*long_num, fill = fills[1] %>% names()), alpha = alpha) +
  geom_area(aes(y = a + b*rest_num, fill = fills[2] %>% names()), alpha = alpha) +
  geom_vline(xintercept = ymd("2020-12-01"), linetype = "dashed") +
  scale_color_manual(name = "", values = colors) +
  scale_fill_manual(name = "", values = fills) +
  scale_y_continuous(breaks = seq(0, 20, by = 5), limits = c(0, 20),
                     sec.axis = sec_axis(~ (.-a)/b, 
                                         name = "Thousands of People")) +
  labs(title = "Chart 9: Unemployment rates in Canada",
       subtitle = "Monthly data, seasonally adjusted",
       x = "", y = "%", caption = "Last observation: JAN 2021") +
  theme(legend.position = "bottom", plot.caption.position = "plot")
```

![](Plots_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

# 3. US Unemployment

``` r
data = read.csv("US_unemployment.csv", stringsAsFactors = FALSE)
```

``` r
data = data %>%
  rename(date = `ï..date`) %>%
  mutate(date = dmy(date),
         num_longterm = num_longterm/1000,
         num_rest = num_total/1000 - num_longterm,
         num_total = num_total/1000)
```

``` r
# scaling for double axes plot
primary = data$unemployment_rate
secondary = data$num_total
ylim.prim = c(floor(min(primary)), ceiling(max(primary)))
ylim.secon = c(min(secondary), max(secondary))
b = diff(ylim.prim)/diff(ylim.secon)/0.89
a = (ylim.prim[1] - ylim.secon[1])/b + 3.2

colors = c("Long-term unemployment \n rate (left scale)" = "red",
           "Total unemployment \n rate (left scale)" = "lightblue")
fills = c("Long-term unemployed \n people (right scale)" = "red",
          "Rest of unemployed \n people (right scale)" = "lightblue")
size = 0.8
alpha = 0.3
```

png(“3.Unemployment in US.png”, units=“in”, width=7, height=5, res=1080)

``` r
ggplot(data, aes(x = date)) +
  geom_line(aes(y = longterm_rate, color = colors[1] %>% names()), size = size) +
  geom_line(aes(y = unemployment_rate, color = colors[2] %>% names()), size = size) +
  geom_area(aes(y = a + b*num_longterm, fill = fills[1] %>% names()), alpha = alpha) +
  geom_area(aes(y = a + b*num_total, fill = fills[2] %>% names()), alpha = alpha) +
  geom_vline(xintercept = dmy("01-12-2020"), linetype = "dashed") +
  scale_color_manual(name = "", values = colors) +
  scale_fill_manual(name = "", values = fills) +
  scale_y_continuous(breaks = seq(0, 20, by = 4), limits = c(0, 20),
                     sec.axis = sec_axis(~ (.-a)/b, 
                                         name = "Millions of People")) +
  labs(title = "Chart 3: Unemployment rates in the United States",
       subtitle = "Monthly data, seasonally adjusted",
       x = "", y = "%", caption = "Last observation: FEB 2021") +
  theme(legend.position = "bottom", plot.caption.position = "plot")
```

![](Plots_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

# 10a. Employment by Industry

``` r
data = read.csv("Employment_by_Industry.csv", stringsAsFactors = FALSE)
```

``` r
industries = c("Accommodation and food services [72]",
               "Information, culture and recreation [51, 71]",
               "Other services (except public administration) [81]",
               "Agriculture [111-112, 1100, 1151-1152]",
               "Forestry, fishing, mining, quarrying, oil and gas [21, 113-114, 1153, 2100]",
               "Utilities [22]",
               "Construction [23]",
               "Manufacturing [31-33]",
               "Transportation and warehousing [48-49]",
               "Educational services [61]",
               "Health care and social assistance [62]",
               "Public administration [91]",
               "Wholesale and retail trade [41, 44-45]",
               "Finance, insurance, real estate, rental and leasing [52-53]",
               "Professional, scientific and technical services [54]")
```

``` r
data = data %>%
  rename(date = REF_DATE,
         industry = North.American.Industry.Classification.System..NAICS.,
         stat_type = Statistics,
         data_type = Data.type,
         value = VALUE) %>%
  filter(GEO == "Canada",
         industry %in% industries,
         stat_type == "Estimate",
         data_type == "Seasonally adjusted") %>%
  mutate(date = as.yearmon(date)) %>%
  filter(date %in% as.yearmon(c("Dec 2020", "Feb 2021")))
```

``` r
data %>% group_by(industry) %>%
  summarise(change = diff(value))
```

    ## # A tibble: 15 x 2
    ##    industry                                                               change
    ##    <chr>                                                                   <dbl>
    ##  1 Accommodation and food services [72]                                   -10   
    ##  2 Agriculture [111-112, 1100, 1151-1152]                                 -15.1 
    ##  3 Construction [23]                                                       45.5 
    ##  4 Educational services [61]                                               16   
    ##  5 Finance, insurance, real estate, rental and leasing [52-53]              9.90
    ##  6 Forestry, fishing, mining, quarrying, oil and gas [21, 113-114, 1153,~  -3   
    ##  7 Health care and social assistance [62]                                  22.2 
    ##  8 Information, culture and recreation [51, 71]                           -19.2 
    ##  9 Manufacturing [31-33]                                                   -4   
    ## 10 Other services (except public administration) [81]                      19.3 
    ## 11 Professional, scientific and technical services [54]                    14.5 
    ## 12 Public administration [91]                                               7.60
    ## 13 Transportation and warehousing [48-49]                                   9.80
    ## 14 Utilities [22]                                                           3.20
    ## 15 Wholesale and retail trade [41, 44-45]                                 -45.3

``` r
industry_levels = c("Accomodation and food services",
               "Information, culture and recreation",
               "Other services",
               "Agriculture, natural resources and utilities",
               "Construction",
               "Manufacturing, transportation and warehousing",
               "Education, health and public administration",
               "Wholesale and retail trade",
               "Financial and professional services")
```

``` r
plot_data = as.data.frame(cbind(
  industry = industry_levels,
  change = c(-10.0, -19.2, 19.3, -15.1-3.0+3.2, 45.5,
             -4.0+9.8, 16.0+22.2+7.6, -45.3, 9.9+14.5)),
  stringsAsFactors = FALSE) %>%
  mutate(change = as.numeric(change),
         industry = factor(industry, levels = industry_levels))
```

png(“10a.Change in Employment.png”, units=“in”, width=7, height=5,
res=1080)

``` r
ggplot(plot_data, aes(x = reorder(industry, desc(industry)), y = change)) +
  geom_bar(stat = "identity", aes(fill = change > 0)) +
  geom_hline(yintercept = 0) +
  labs(y = "Thousands of people", x = "") +
  scale_y_continuous(breaks = seq(-50, 50, by = 10)) +
  scale_fill_manual(values = c("red", "lightblue")) +
  labs(title = "Chart 10a:",
       subtitle = "Change in employment between DEC 2020 and FEB 2021,
                   seasonally adjusted, monthly") +
  coord_flip() +
  theme(legend.position = "None")
```

![](Plots_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

# 8. Lending

``` r
data = read.csv("lending.csv", stringsAsFactors = FALSE)
```

``` r
data = data %>%
  rename(date = `ï..date`, price = SLOS_BUS_LEND_PC,
         non_price = SLOS_BUS_LEND_NP) %>%
  mutate(date = as.yearqtr(date)) %>%
  filter(date >= as.yearqtr("2006Q1"))
```

png(“8.Lending.png”, units=“in”, width=7, height=5, res=1080)

``` r
colors = c("Price" = "lightblue", "Non-Price" = "red")
size = 0.8

ggplot(data, aes(x = date)) +
  geom_line(aes(y = price, color = colors[1] %>% names()), size = size) +
  geom_line(aes(y = non_price, color = colors[2] %>% names()), size = size) +
  geom_hline(yintercept = 0) +
  geom_vline(xintercept = as.yearqtr("2020Q3"), linetype = "dashed") +
  scale_color_manual(name = "", values = colors) +
  scale_x_continuous(breaks = seq(2006, 2020, by=2)) +
  scale_y_continuous(position = "right", breaks = seq(-60, 100, by = 20)) +
  labs(title = "Chart 8:",
       subtitle = "Senior Loan Officer Survey business lending conditions, quarterly data",
       x = "", y = "%", caption = "Last observation: 2020 Q4") +
  theme(legend.position = "bottom", plot.caption.position = "plot")
```

![](Plots_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

# 4. Commodity Prices

``` r
data = read.csv("commodity.csv")
```

``` r
data = data %>% 
  rename(date = `ï..date`) %>%
  mutate(date = dmy(date)) %>%
  filter(date >= "2020-01-01")

data = data  %>%
  mutate(W.MTLS = W.MTLS/data$W.MTLS[1]*100,
         W.FOPR = W.FOPR/data$W.FOPR[1]*100,
         W.AGRI = W.AGRI/data$W.AGRI[1]*100,
         W.ENER = W.ENER/data$W.ENER[1]*100)
```

``` r
# scaling for double axes plot
primary = data$W.MTLS
secondary = data$W.FOPR
ylim.prim = c(floor(min(primary)), ceiling(max(primary)))
ylim.secon = c(min(secondary), max(secondary))
b = diff(ylim.prim)/diff(ylim.secon)
a = (ylim.prim[1] - ylim.secon[1]) - 83

colors = c("Base Metals \n (left scale)" = "lightblue", 
           "Forestry Products \n (right scale)" = "orange",
           "Agricultural Products \n (left scale)" = "green", 
           "Energy \n (right scale)" = "red")
size = 0.8
```

png(“4.Commodity Prices.png”, units=“in”, width=7, height=5, res=1080)

``` r
ggplot(data, aes(x = date)) +
  geom_line(aes(y = W.MTLS, color = colors[1] %>% names()), size = size) +
  geom_line(aes(y = W.AGRI, color = colors[3] %>% names()), size = size) +
  geom_line(aes(y = -a + b*W.FOPR, color = colors[2] %>% names()), size = size) +
  geom_line(aes(y = -a + b*W.ENER, color = colors[4] %>% names()), size = size) +
  geom_hline(yintercept = 100) +
  geom_vline(xintercept = ymd("2021-01-15"), linetype = "dashed") +
  scale_color_manual(name = "Commodity", values = colors) +
  scale_y_continuous(sec.axis = sec_axis(~ (.+a)/b, name = "Index")) +
  labs(title = "Chart 4: Commodity Prices", 
       subtitle = "Index: January 01, 2020 = 100, weekly data",
       y = "Index", x = "", caption = "Last observation: 24 MAR 2021") +
  theme(legend.position = "bottom", plot.caption.position = "plot")
```

![](Plots_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

# 2. Equity Prices + Exchange Rate

``` r
sptsx = read.csv("a_SPTSX.csv", stringsAsFactors = FALSE)
sp500 = read.csv("b_SP500.csv", stringsAsFactors = FALSE)
stoxx = read.csv("c_STOXX50.csv", stringsAsFactors = FALSE)
sse = read.csv("d_SSE.csv", stringsAsFactors = FALSE)
msci = read.csv("e_MSCI.csv", stringsAsFactors = FALSE)
```

Choosing standard variables

``` r
sptsx = sptsx %>% 
  select(Date, Close) %>%
  rename(date = Date, value = Close) %>%
  mutate(date = as_datetime(date),
         value = as.numeric(value))

sp500 = sp500 %>%
  rename(date = DATE, value = SP500) %>%
  mutate(date = as_datetime(date),
         value = as.numeric(value))
```

    ## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion

``` r
stoxx = stoxx %>%
  rename(date = `ï..Date`, value = Price) %>%
  select(date, value) %>%
  mutate(date = gsub(",", "", date) %>% mdy(),
         value = gsub(",", "", value) %>% as.numeric())

sse = sse %>%
  select(Date, Close) %>%
  rename(date = Date, value = Close) %>%
  mutate(date = as_datetime(date),
         value = as.numeric(value))

msci = msci %>%
  rename(date = `ï..Date`, value = Price) %>%
  select(date, value) %>%
  mutate(date = gsub(",", "", date) %>% mdy(),
         value = gsub(",", "", value) %>% as.numeric())
```

``` r
data = inner_join(sptsx, sp500, by = c("date")) %>%
  inner_join(., stoxx, by = c("date")) %>%
  inner_join(., sse, by = c("date")) %>%
  inner_join(., msci, by = c("date")) %>%
  filter(date >= "2020-01-03")

colnames(data) = c("date", "sptsx", "sp500", "stoxx", "sse", "msci")

data = data %>% mutate(sptsx = sptsx/data$sptsx[1]*100,
                sp500 = sp500/data$sp500[1]*100,
                stoxx = stoxx/data$stoxx[1]*100,
                sse = sse/data$sse[1]*100,
                msci = msci/data$msci[1]*100)
```

png(“2a.Equity Prices.png”, units=“in”, width=7, height=5, res=1080)

``` r
colors = c("CA: S&P/TSX composite" = "RED", 
           "US: S&P 500" = "LIGHTBLUE", 
           "EU: STOXX 50" = "GREEN",
           "CHN: SSE composite" = "ORANGE", 
           "Emerging Mkt.: MSCI" = "PURPLE")
size = 0.8

ggplot(data = data, aes(x = date)) +
  geom_line(aes(y = sptsx, color = colors[1] %>% names()), size = size) +
  geom_line(aes(y = sp500, color = colors[2] %>% names()), size = size) +
  geom_line(aes(y = stoxx, color = colors[3] %>% names()), size = size) +
  geom_line(aes(y = sse, color = colors[4] %>% names()), size = size) +
  geom_line(aes(y = msci, color = colors[5] %>% names()), size = size) +
  geom_hline(yintercept = 100) +
  geom_vline(xintercept = as_datetime("2021-01-15"), linetype = "dashed") +
  scale_y_continuous(position = "right") +
  labs(title = "Equity Prices", subtitle = "Index: January 3, 2020 = 100",
       x = "", y = "Index", caption = "Last observation: 29 MAR 2021") +
  scale_color_manual(name = "Equity", values = colors) +
  theme(plot.caption.position = "plot", 
        axis.title.y = element_text(hjust = 0))
```

![](Plots_files/figure-gfm/unnamed-chunk-34-1.png)<!-- -->

``` r
usd_cad = read.csv("USD-CAD.csv", stringsAsFactors = FALSE)
ceer = read.csv("CEER.csv", stringsAsFactors = FALSE)
```

``` r
usd_cad = usd_cad %>%
  rename(date = `ï..date`, usd_cad = FXUSDCAD) %>%
  mutate(date = as_datetime(date %>% dmy()),
         usd_cad = 1/usd_cad)

ceer = ceer %>%
  rename(date = `ï..date`, ceer = CEER,
         ceer_no_us = CEER_noUS) %>%
  mutate(date = as_datetime(date %>% dmy()))
```

``` r
data = inner_join(usd_cad, ceer, by = c("date")) %>%
  filter(date >= "2020-01-01")
```

``` r
# scaling for double axes plot
primary = data$ceer
secondary = data$usd_cad
ylim.prim = c(floor(min(primary)), ceiling(max(primary)))
ylim.secon = c(min(secondary), max(secondary))
b = diff(ylim.prim)/diff(ylim.secon)/1.03
a = (ylim.prim[1] - ylim.secon[1])/b + 27

colors = c("CEER" = "red", "CEER excl. US" = "lightblue",
           "USD/CAD" = "green")
size = 0.8
```

png(“2b.Exchange Rates.png”, units=“in”, width=7, height=5, res=1080)

``` r
ggplot(data = data, aes(x = date)) +
  geom_line(aes(y = ceer, color = colors[1] %>% names()), size = size) +
  geom_line(aes(y = ceer_no_us, color = colors[2] %>% names()), size = size) +
  geom_line(aes(y = a + b*usd_cad, color = colors[3] %>% names()), size = size) +
  geom_vline(xintercept = as_datetime("2021-01-15"), linetype = "dashed") +
  labs(title = "Canadian Dollar", x = "", caption = "Last observation: 30 MAR 2021") +
  scale_y_continuous("CEER Index", 
                     sec.axis = sec_axis(~ (.-a)/b, name = "USD")) +
  scale_color_manual(name = "Exchange Rates", values = colors) +
  theme(legend.position = "bottom", plot.caption.position = "plot")
```

![](Plots_files/figure-gfm/unnamed-chunk-39-1.png)<!-- -->

# 1. COVID - Advanced Economies

``` r
covid_data = read.csv("owid-covid-data.csv", stringsAsFactors = FALSE)
```

Check region

``` r
countries = c("Canada", "United Kingdom", "Japan",
              "United States", "European Union")
countries %in% covid_data$location
```

    ## [1] TRUE TRUE TRUE TRUE TRUE

Check starting-ending dates by region

``` r
for (country in countries){
  subdata = covid_data %>%
    filter(location == country) %>%
    select(date)
  print(paste0(country, ": from ", first(subdata$date), 
                        ", to ", last(subdata$date)))
}
```

    ## [1] "Canada: from 2020-01-26, to 2021-03-29"
    ## [1] "United Kingdom: from 2020-01-31, to 2021-03-29"
    ## [1] "Japan: from 2020-01-22, to 2021-03-29"
    ## [1] "United States: from 2020-01-22, to 2021-03-29"
    ## [1] "European Union: from 2020-01-23, to 2021-03-29"

``` r
plot_data = covid_data %>%
  filter(location %in% countries) %>%
  select(c(date, location, new_cases_per_million)) %>%
  mutate(date = as_date(date),
         cases_pm = zoo::rollmean(new_cases_per_million, 7,
                                  fill = NA)) %>%
  na.omit()

plot_data_merged = plot_data %>%
  filter(location %in% countries[1]) %>%
  select(-c(location, new_cases_per_million))

for (i in 2:length(countries)){
  new = plot_data[plot_data$location == countries[i],]
  plot_data_merged = plot_data_merged %>%
    inner_join(new[,c("date", "cases_pm")], by = c("date"))
}

colnames(plot_data_merged) = c("date", countries)
```

png(“1.COVID-Advanced Economies.png”, units=“in”, width=7, height=5,
res=1080)

``` r
colors = c("Canada" = "RED", "United Kingdom" = "PURPLE", "Japan" = "YELLOW", 
           "United States" = "LIGHTBLUE", "European Union" = "GREEN")
size = 0.8

plot = ggplot(data = plot_data_merged, aes(x = date))
plot = plot + geom_line(aes(y = Canada, color = countries[1]), size = size)
plot = plot + geom_line(aes(y = `United Kingdom`, color = countries[2]), size = size)
plot = plot + geom_line(aes(y = Japan, color = countries[3]), size = size)
plot = plot + geom_line(aes(y = `United States`, color = countries[4]), size = size)
plot = plot + geom_line(aes(y = `European Union`, color = countries[5]), size = size)
plot = plot + geom_vline(xintercept = as_date("2021-01-13"),linetype = "dashed")
plot = plot + labs(title = "Chart 1: Advanced Economies", 
                   subtitle = "Daily new cases per million people, 7-day moving average",
                   x = "", y = "Number of Cases", caption = "Last observation: 26 MAR 2021") +
  scale_color_manual(name = "Economies", values = colors) +
  scale_y_continuous(position = "right") +
  theme(plot.caption.position = "plot")
print(plot)
```

![](Plots_files/figure-gfm/unnamed-chunk-44-1.png)<!-- -->
