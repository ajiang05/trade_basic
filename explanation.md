# SAC_trade Trading Algorithm Setup Explanation

Welcome to your first trading algorithm project! Here is a detailed breakdown of what was done to get the project started.

## What Was Executed

To get the environment up and running, we ran a sequence of commands on your terminal from the `SAC_trade` directory. Here is the exact command segment that was executed:

```bash
python3 -m venv venv && source venv/bin/activate && pip install --upgrade pip && pip install pandas numpy yfinance matplotlib jupyter
```

### Breakdown of the command:

1. **`python3 -m venv venv`**:
   - This command creates a "virtual environment" folder named `venv` inside the project folder. A virtual environment is an isolated space where python packages can be installed safely without affecting the rest of your computer or other Python projects you might have.
2. **`source venv/bin/activate`**:
   - This "activates" the virtual environment we just created. It tells your shell that any packages installed after this command takes effect should go directly into the `venv` folder instead of globally.
3. **`pip install --upgrade pip`**:
   - This ensures the Python package manager (`pip`) is up-to-date and won't hit any installation bugs. 
4. **`pip install pandas numpy yfinance matplotlib jupyter`**:
   - We invoked `pip` to fetch essential libraries from the internet for algorithmic trading:
     - **`pandas`**: The industry standard for handling data sets in tables (similar to an Excel spreadsheet, but driven by code).
     - **`numpy`**: Provides intense mathematical functions and array operations.
     - **`yfinance`**: A powerful open-source library that connects to Yahoo Finance to easily download historical stock price data for free.
     - **`matplotlib`**: A plotting library we use to draw charts, graphs (e.g., historical closing prices), and indicators so you can visually verify your strategy.
     - **`jupyter`**: The web-based application (and computing environment) that enables you to run Python block-by-block in interactive `.ipynb` notebooks.

## The Starter Code (`01_data_fetching.ipynb`)

I created a starter file called `01_data_fetching.ipynb` in the `SAC_trade` directory. Jupyter notebooks structure code into blocks called "cells" which can contain either explanatory Markdown text or executable Python code. 

1. **Imports Cell**:
   - `import yfinance as yf`
   - `import pandas as pd`
   - `import matplotlib.pyplot as plt`
   - This tells Python to load the libraries we just installed into your notebook's memory so we can use their commands.
2. **Data Download Cell**:
   - `df = yf.Ticker(ticker).history(start=start_date, end=end_date)`
   - This line reaches out to Yahoo Finance, pulls daily historical price records for an asset ("SPY" - the S&P 500 ETF) over the last two years, and sets it to a "dataframe" variable called `df`. 
3. **Visualization Cell**:
   - Uses `matplotlib` (`plt`) to plot `df.index` (which is the date/time) against `df['Close']` (the actual price).

## Strategy Design (`02_strategy_design.ipynb`)

Now we have added a second notebook that acts as the real "brain" of the operation. This outlines the **Moving Average Crossover** method:

### How The Strategy Works (The Concept)
Daily stock prices are chaotic and jittery. To find the "true trend", traders calculate average prices over a set timeframe to smooth out the noise.
- **The Fast Line (50-Day Average):** Reacts quickly to new information. It tracks short-term momentum.
- **The Slow Line (200-Day Average):** Moves sluggishly. It tracks the overarching long-term trend.

**The Golden Rule:** 
- When the Fast Line crosses *above* the Slow Line (a "Golden Cross"), it means short-term momentum is accelerating upwards—a strong **BUY** signal. 
- When the Fast Line drops *below* the Slow Line (a "Death Cross"), momentum is dying—a strong **SELL** signal.

### The Code Implementation

1. **Mathematical Averaging (`SMA_50` and `SMA_200`)**:
   - We use `.rolling(window=X).mean()` to scan over the last 50 days (and 200 days) to create two smoothed average lines out of the chaotic daily price chart.
2. **Generating the 'Buy' and 'Sell' Flags (`numpy.where`)**:
   - We scan back through history using `np.where()`. Every day where the 50-day average is *higher* than the 200-day average, we record a `1` meaning "Hold". Otherwise, we record a `0` meaning "Don't Hold".
3. **Plotting the Signals (`Position == 1`)**:
   - We calculate the *moment* those columns change (`df['Signal'].diff()`). A transition from 0 to 1 generates a visual **Green Arrow** (Buy trigger). A transition from 1 to 0 generates a visual **Red Arrow** (Sell trigger).
