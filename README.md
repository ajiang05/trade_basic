# SAC Trade Analyst Tool

## Overview
This is just a basic overview. This repository contains a foundational algorithmic trading and security analysis project. The primary objective of this system is to ingest historical market data, apply quantitative mathematical models (such as moving average crossovers), and output actionable trading recommendations. It is designed to act as an analytical assistant rather than an automated execution bot.

## Project Structure

- `01_data_fetching.ipynb`
  This notebook is responsible for connecting to financial data streams (Yahoo Finance) to download, clean, and visualize historical daily price data for specified assets.

- `02_strategy_design.ipynb`
  This notebook contains the computational logic of the trading algorithm. It calculates technical indicators (e.g., 50-day and 200-day Simple Moving Averages) and processes the data to generate binary Buy or Sell signals based on momentum crossovers.

- `explanation.md`
  A detailed documentation file providing a conceptual breakdown of the mathematics used in the algorithm, as well as a record of the environment setup commands.

## Setup Instructions
1. Activate the isolated virtual environment: `source venv/bin/activate`
2. Ensure the required packages (pandas, numpy, yfinance, matplotlib, jupyter) are installed via pip.
3. Open the `.ipynb` files using a compatible editor (like VS Code or Cursor) utilizing the `venv` Jupyter kernel.