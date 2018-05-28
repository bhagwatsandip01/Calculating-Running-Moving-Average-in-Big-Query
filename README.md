# Calculating-Running-Moving-Average-in-Big-Query


Sometimes you'll want to compute a running average over a selection of rows for the past N number of time periods. The running average is also called "moving average" or "rolling average".

The reason to use a running average is to smooth out the highs and lows of the data set and get a feel for the trends in the data.

Let's consider quarterly revenues for the years 2001 to 2008 in a table "[project.dataset.table]":

![alt text](https://drive.google.com/uc?id=1mPhZiHuY6KFXjNIVfVEMpeHP9BdGqdFL)

When we plot the revenue as a timeseries, as expected, we see huge revenue jumps in the fourth quarter of the year (due to some reasion.)


![alt text](https://drive.google.com/uc?id=1GICzgaWityMCpvRKorCpmizMcZBx5G-X)

To smooth out these huge jumps in revenue, we can compute a moving average that averages the previous three periods:

Now,we are going to calculate a 4-period moving average. A simple way to compute the sum up the number of periods, (4+3+2+1=10)

1) Biq Query row_number to number the rows

  syntax: row_number() over ()
   
     We will first number the rows of the table,
     
 #### Example:

select quarter,revenue,row_number() over (Quarter) as row_number

from [project.dataset.table]   

![alt text](https://drive.google.com/uc?id=1hGtFca73lmZUSzZWSkBR70RvSYVgkHlt)

we want to compute a moving average that averages the previous three periods:


#### Example:-

select quarter,revenue,
AVG(revenue) OVER (ORDER BY row_num RANGE BETWEEN 3 PRECEDING AND CURRENT ROW) AS quarter_moving_avg

from [project.dataset.table]


This is the plot for revenue and moving average.


![alt text](https://drive.google.com/uc?id=1uj2t2qDcfxvCm0cx0hMUrHRI_o69lvSD)

