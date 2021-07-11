## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/cenkt/cenkt.github.io/edit/master/README.md) to maintain and preview the content for your website in Markdown files.
https://github.com/cenkt/cenkt.github.io/blob/master/MerkezFonlama%20(1).html
Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List



1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```
[press](https://github.com/cenkt/cenkt.github.io/blob/master/Untitled1.ipynb)

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/cenkt/cenkt.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

# Lab Session Notebook - Returns

## From Prices to Returns

In this lab we'll work the very basics of Returns - computing returns, and compounding a sequence of returns.

Let's start with a set of prices for a stock "A", in a python list:


```python
prices_a = [8.70, 8.91, 8.71]
```

Recall that the return from time $t$ to time ${t+1} is given by:

$$ R_{t,t+1} = \frac{P_{t+1}-P_{t}}{P_{t}} $$

or alternately

$$ R_{t,t+1} = \frac{P_{t+1}}{P_{t}} - 1 $$

If you come from R or another language that supports vectors, you might expect something like this to work:

```python
returns_a = prices_a[:-1]/prices_a[1:] - 1
```

However, since Python lists do not operate as vectors, that will not work, generating an error about "/" not working for lists.



```python
# WILL NOT WORK - THIS WILL GENERATE AN ERROR!
# prices_a[1:]/prices_a[:-1] -1
```

Instead, we can convert them to a `numpy` array. Numpy arrays _do_ behave like vectors, so this works:


```python
import numpy as np

prices_a = np.array([8.70, 8.91, 8.71])
prices_a
```




    array([8.7 , 8.91, 8.71])




```python
prices_a[1:]/prices_a[:-1] - 1
```




    array([ 0.02413793, -0.02244669])



Now, let's add a few more days of prices and introduce a second stock. Let's call these two stocks "BLUE" and "ORANGE". Instead of using raw numpy arrays, we are going to use the far more powerful Pandas DataFrame, which wraps the functionality of numpy into a very convenient and easy to use data structure called a DataFrame. Note how the DtaFrame has two nicely indexed columns as well as a row index that by default runs from 0 to 4.


```python
import pandas as pd

prices = pd.DataFrame({"BLUE": [8.70, 8.91, 8.71, 8.43, 8.73],
                       "ORANGE": [10.66, 11.08, 10.71, 11.59, 12.11]})
```


```python
prices
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8.70</td>
      <td>10.66</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8.91</td>
      <td>11.08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.71</td>
      <td>10.71</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.43</td>
      <td>11.59</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8.73</td>
      <td>12.11</td>
    </tr>
  </tbody>
</table>
</div>



**WARNING**

However, because Pandas DataFrames will align the row index (in this case: 0, 1, 2, 3, 4) the exact same code fragment will not work as you might expect.  (see the section on row alignment in the "Crash Course" videos if this is unclear to you)


```python
prices.iloc[1:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>8.91</td>
      <td>11.08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.71</td>
      <td>10.71</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.43</td>
      <td>11.59</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8.73</td>
      <td>12.11</td>
    </tr>
  </tbody>
</table>
</div>




```python
prices.iloc[:-1]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8.70</td>
      <td>10.66</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8.91</td>
      <td>11.08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.71</td>
      <td>10.71</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.43</td>
      <td>11.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
prices.iloc[1:]/prices.iloc[:-1] - 1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



We can fix this in one of several ways. First, we can extract the values of the DataFrame column which returns a numpy array, so that the DataFrame does not try and align the rows.


```python
prices.iloc[1:].values/prices.iloc[:-1] - 1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.024138</td>
      <td>0.039400</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.022447</td>
      <td>-0.033394</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.032147</td>
      <td>0.082166</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.035587</td>
      <td>0.044866</td>
    </tr>
  </tbody>
</table>
</div>



You could have also used the values in the denominator:


```python
prices.iloc[1:]/prices.iloc[:-1].values - 1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.024138</td>
      <td>0.039400</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.022447</td>
      <td>-0.033394</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.032147</td>
      <td>0.082166</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.035587</td>
      <td>0.044866</td>
    </tr>
  </tbody>
</table>
</div>



However, there are a couple of ways to do this without extracting the values, and these are probably a bit cleaner and more readable. The first option is to use the `.shift()` method on the array, which realigns the indices.


```python
prices
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8.70</td>
      <td>10.66</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8.91</td>
      <td>11.08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.71</td>
      <td>10.71</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.43</td>
      <td>11.59</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8.73</td>
      <td>12.11</td>
    </tr>
  </tbody>
</table>
</div>



Since we want to get the row at index 0 (8.84 and 10.66) to line up with the row at index 1 (8.54 and 10.30) so we can divide the 2nd row (at index 1) by the first row (at index 0) we want to shift the rows in the denominator by 1 ... which we do with `.shift(1)`


```python
prices.shift(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8.70</td>
      <td>10.66</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.91</td>
      <td>11.08</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.71</td>
      <td>10.71</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8.43</td>
      <td>11.59</td>
    </tr>
  </tbody>
</table>
</div>



So, now we can obtain the returns on each day as follows:


```python
returns = prices/prices.shift(1) - 1
returns
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.024138</td>
      <td>0.039400</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.022447</td>
      <td>-0.033394</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.032147</td>
      <td>0.082166</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.035587</td>
      <td>0.044866</td>
    </tr>
  </tbody>
</table>
</div>



Note how we cannot compute returns for the first day, because we dont have the closing price for the previous day. In general, we lose one data point when we go from prices to returns.

Finally, there is a built-in method in DataFrame that computes the percent change from one row to another. Since that is exactly what a return is (the percent change in price) we can just use this method to compute the return.



```python
returns = prices.pct_change()
returns
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.024138</td>
      <td>0.039400</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.022447</td>
      <td>-0.033394</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.032147</td>
      <td>0.082166</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.035587</td>
      <td>0.044866</td>
    </tr>
  </tbody>
</table>
</div>



## Reading data from a CSV file
Since typing in returns is tedious, let's read the data in from a file. Pandas provides a convenient and simple way to read in a CSV file of the returns.


```python
prices = pd.read_csv('data/sample_prices.csv')
prices
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8.7000</td>
      <td>10.6600</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8.9055</td>
      <td>11.0828</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.7113</td>
      <td>10.7100</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.4346</td>
      <td>11.5907</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8.7254</td>
      <td>12.1070</td>
    </tr>
    <tr>
      <th>5</th>
      <td>9.0551</td>
      <td>11.7876</td>
    </tr>
    <tr>
      <th>6</th>
      <td>8.9514</td>
      <td>11.2078</td>
    </tr>
    <tr>
      <th>7</th>
      <td>9.2439</td>
      <td>12.5192</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9.1276</td>
      <td>13.3624</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9.3976</td>
      <td>14.4080</td>
    </tr>
    <tr>
      <th>10</th>
      <td>9.4554</td>
      <td>11.9837</td>
    </tr>
    <tr>
      <th>11</th>
      <td>9.5704</td>
      <td>12.2718</td>
    </tr>
    <tr>
      <th>12</th>
      <td>9.7728</td>
      <td>11.5892</td>
    </tr>
  </tbody>
</table>
</div>




```python
returns = prices.pct_change()
returns
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.023621</td>
      <td>0.039662</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.021807</td>
      <td>-0.033638</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.031763</td>
      <td>0.082232</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.034477</td>
      <td>0.044544</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.037786</td>
      <td>-0.026381</td>
    </tr>
    <tr>
      <th>6</th>
      <td>-0.011452</td>
      <td>-0.049187</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.032676</td>
      <td>0.117008</td>
    </tr>
    <tr>
      <th>8</th>
      <td>-0.012581</td>
      <td>0.067353</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.029581</td>
      <td>0.078249</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.006151</td>
      <td>-0.168261</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.012162</td>
      <td>0.024041</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.021149</td>
      <td>-0.055623</td>
    </tr>
  </tbody>
</table>
</div>




```python
returns.mean()
```




    BLUE      0.01
    ORANGE    0.01
    dtype: float64




```python
returns.std()
```




    BLUE      0.023977
    ORANGE    0.079601
    dtype: float64




```python
returns.plot.bar()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1187c10f0>




```python
prices.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x118b53d30>




    
![png](lab_101_files/lab_101_32_1.png)
    


## Compounding Returns

Now that we have a series of 12 monthly returns, we can produce the compounded return by multiplying the individual period returns, as long as the returns are expressed as growth rates in what I call "1+R" format.

To compound the returns, all we need to do is add 1 to each return and then multiply them. The result is itself in "1+R" format, so we need to subtract 1.

Let's compute the compounded return of our two series. 


```python
returns + 1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BLUE</th>
      <th>ORANGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.023621</td>
      <td>1.039662</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.978193</td>
      <td>0.966362</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.968237</td>
      <td>1.082232</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.034477</td>
      <td>1.044544</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.037786</td>
      <td>0.973619</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.988548</td>
      <td>0.950813</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1.032676</td>
      <td>1.117008</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.987419</td>
      <td>1.067353</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1.029581</td>
      <td>1.078249</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1.006151</td>
      <td>0.831739</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1.012162</td>
      <td>1.024041</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1.021149</td>
      <td>0.944377</td>
    </tr>
  </tbody>
</table>
</div>




```python
np.prod(returns+1)
```




    BLUE      1.123310
    ORANGE    1.087167
    dtype: float64




```python
(returns+1).prod()
```




    BLUE      1.123310
    ORANGE    1.087167
    dtype: float64




```python
(returns+1).prod()-1
```




    BLUE      0.123310
    ORANGE    0.087167
    dtype: float64




```python
(((returns+1).prod()-1)*100).round(2)
```




    BLUE      12.33
    ORANGE     8.72
    dtype: float64



## Annualizing Returns

To annualize a return for a period, you compound the return for as many times as there are periods in a year. For instance, to annualize a monthly return you compund that return 12 times. The formula to annualize a monthly return $R_m$ is:

$$ (1+R_m)^{12} - 1$$

To annualize a quarterly return $R_q$ you would get:

$$ (1+R_q)^{4} - 1$$

And finally, to annualize a daily return $R_d$ you would get:

$$ (1+R_d)^{252} - 1$$

For example, to annualize a 1% monthly, and 4% quarterly and a 0.01% daily return you would do:


```python
rm = 0.01
(1+rm)**12 - 1
```




    0.12682503013196977




```python
rq = 0.04
(1+rq)**4 - 1
```




    0.1698585600000002




```python

```
