# Applying Brinson Model to a portfolio in 2022 with Python!

One of my classmate in Fudan told me that he had a great performance in HK stock market in 2022, is it true?
Let's find out with Brinson Model.

Before we start coding, let me briefly introduce the detail of Brinson Model.

In the model, it supposed a performance of a portfolio can be attributed to 3 dimensions
- Allocation Effect: you are good at picking sectors/ asset classes
- Selection Effect: you are good at picking stocks in the sector/ asset class
- Interaction Effect: you are good at doing other things, such as market timing

If we have to calculate this, we must have a portfolio and a benchmark; and gather 4 types of data
- r_benchmark: The rate of return of each field in the benchmark
- w_benchmark: The weight of each field in the benchmark
- r_portfolio: The rate of return of each field in the portfolio
- w_portfolio: The weight of each field in the portfolio

Alright, now let's get started!

Before we started calculation, we have picked Heng Seng Index as our benchmark(cuz our portfolio traded at HK stock market).
We also 
