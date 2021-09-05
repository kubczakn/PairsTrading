# Pairs Trading
A simple and versatile pairs trading framework.
# Setup
Clone the repository:
```bash
git clone Pairs-Trading
cd Pairs-Trading
```
Create and activate virtual environment:
```bash
$ python3 -m venv venv
$ source venv/bin/activate
```
Update pip and install wheel:
```bash
(venv)$ pip3 install --upgrade pip
(venv)$ pip3 install wheel
```
Install dependencies:
```bash
(venv)$ pip install -r requirements.txt
```

# Usage
This framework requires an Excel Worksheet that contains a column of tickers labeled 
"Ticker" as input.  
  
At a high level, the framework produces a pairs trading strategy using this ticker data through a series of pipeline steps with the provided notebooks.  
  
A summary of the pipeline is as follows:  
  
1. Price-Getter
    1. Process tickers and return historical price data
2. Pairs-Finder
    1.  Find candidate pairs
    2. See "Strategy" for details on how valid pairs are found
3. Pairs-Optimizer
    1.  Optimize entry / exit thresholds for valid pairs using Bayesian Optimization.
4. Pairs-Tester
    1.  Create strategy using optimal thresholds for pairs
    2.  Test strategy on out of sample data and compare to a benchmark of taking a long position on securities.  
  
For each notebook in the pipeline, the "chosen_dataset" variable will need to be renamed based on the name of provided tickers worksheet.

# Strategy
## Finding Pairs
The logic for finding valid pairs is handled by Finder objects. Finder objects take 
in historical price data and output candidate pairs. Candidate pairs are first found by performing the Engle-Granger cointegration test on the `log(price)` all possible pairs. From the candidate pairs gathered here, we further filter the candidate pairs by performing the Augmented Dickey-Fuller test on the spread of the candidate pairs, where the spread is defined as:
```
spread = log(stockA prices) - n * log(stockB prices)
```
where `n = dynamic beta hedge ratios` from the pair. In both filtering rounds we only accept pairs that have a p-value of less than 0.01.

## Optimizing Pairs
The upper and lower bounds for each pair are optimized using Bayesian Optimization.
## Testing Pairs
The pairs are built using stock data from 2012-2019, and are backtested on data from 2019-present.
