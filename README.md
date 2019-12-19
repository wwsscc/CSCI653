# CSCI653 Pattern Recognition in Day Trading
# Introduction

As pattern recognition in stock time series has attracted the attention, technical analysts and traders believe that patterns and shapes could be used to find profitable trading opportunities. Generally, these trading strategies have been used for long term investments, but in this project, I will be applying these concepts on day trading. Day trading is speculation in securities, specifically buying and selling financial instruments within the same trading day. 

In this project, I plan to use this investment strategies on day trading. All stocking price data are real market price. Based on the five special patterns, Main focus is to recognize patterns and determine the neckline stock time series data. This time series data is per minute stock prices for seven (7) days. In this project, I use an novel approach of using genetic algorithm to get this window size and combine both template and rule based approaches. 

# Methods

Genetic Algorithm
Perceptually Important Points

# Rule-based Matching 

Besides defining the preferred patterns visually as pattern templates, rules can be defined to describe the shape of the preferred patterns. One of the advantages of applying rule-based pattern matching over the template-based approach is that the relationship between the points is hard to define explicitly in the template-based approach. For example, in a head-and-shoulder pattern, the general guidelines are that the two shoulders in the pattern must be lower than the head while having a similar degree of amplitude (within 15% in average). In such a case, although we can plot a pattern template according to these requirements, such kinds of rules cannot be guaranteed during the pattern-matching process. Patterns with similar shapes as compared to the query pattern but violating certain rules may still be identified. Therefore, the rule-based approach gives yet another method to identify patterns. 
Based on the definitions of technical patterns by Lo et al. (2000), we described the one reversal technical patterns in rule format. According to the template-based approach, it is assumed that 7 PIPs, from sp1 to sp7, will be identified first for the pattern matching process. Therefore, the rules for describing the relationships and constraints among these 7 PIPs are defined. The corresponding definition of the inverse head-and-shoulder pattern is as following: 

Rule set 1 (inverse head-and-shoulder) 
1.	sp4 < sp2 and sp6
2.	sp2 < sp1 and sp3
3.	sp6 < sp5 and sp7
4.	sp3 < sp1
5.	sp5 < sp7
6.	diff(sp2,sp6) < 15%
7.	diff(sp3, sp5) < 15%

where diff(spx,spy) denotes the difference between data points spx and spy, ‘‘spx < spy and spz’’ denotes that spx must be smaller than spy and spz. 
With the defined rules, the sequences can then be evaluated. First, seven PIPs are identified from the sequences. Then, those sequences which can validate all the above rules are identified as a matching result. 

# Optimal Segmentation of Time-Series 
Given a large time-series data, we can expect that there might exist multiple patterns of different widths in the time-series. Although the previous section provides a way of matching patterns in a given time-series, we might want to find patterns of varying widths in different segments of the data. Genetic Algorithm offers a solution to this problem. 

# Genetic Algorithm 
Genetic Algorithm (GA) is an algorithm inspired by the process of natural selection. A population of individuals are first initialized, of which each individual represents the solution to the optimization problem through a predefined encoding. Next, the individuals will go through a series of crossovers and mutations, and on each generation, the best individuals will be chosen through a process called selection. After a number of generations, the best individual will be chosen as the solution to the optimization problem. 

# Problem Representation in GA 
# Individual 

Each individual is represented as a sequence of numbers,  which are the splitting points in the time-series data. For example, in a time-series with 1000 data points, an individual with value {200,600,800} will have 4 segments 1-200, 201-500, 601-800 and 801-1000. 
Initialization of Population 
A value dlen (desired length) is given by the user, which is the average length of a segment that the user expects to get. The first individual will then be the time-series split by the segments of the length dlen. 

# Crossover 
Crossover is the operation to exchange the characteristics of two individuals with each other. For our instance of GA, single point crossover is used. 
Given two individuals A, B, where A = {10,35,70,120}
B = {25,40,90,140} 
We generate two random numbers to represent the crossover points in each individual. Assuming we generated two random numbers: (1,2), crossover point for A is 1, while crossover point for B is 2. 
Therefore, after crossover, the two individuals are A’ = {10,90,140}
B’ = {25,40,35,70,120} 
We will then sort the two individuals to allow easier processing. Take note that it is possible for the individuals to change in size after the crossover operation. 

# Mutation 

Mutation is used to introduce new characteristics into the population. Without mutation, it would not be possible to get out of local minima in the optimization process. 
Given an individual A, mutation will either add or remove a random point in the individual. Currently, the probability of adding or removing is equal (0.5 probability). The new point generated cannot be currently in the individual. The new point to be added also must not generate a segment that is smaller than the pattern template size, or no pattern can be matched in the segment. For removal, all points can be removed, unless the individual has no splitting point, which mean that there is only one segment, which is the entire time-series. 
Adding new Point 
1.	Generate range of allowed points that can be added (no points currently in individual, or 
within fixed distance from individual). 
2.	Randomly choose a point from the list of allowed points. 
3.	Add into the individual. 
4.	Sort the individual. 
Removing Point
1. Randomly pick a point to delete, or fail if individual has no points. 
2. Delete point. Selection 
Selection is the process of filtering the individuals such that only the “strongest” individuals with the best fitness scores will survive to the next generation. While we can simply pick the individuals with the best fitness scores, the individuals with low fitness scores still need to have non-zero probability of surviving to the next generation, in order to give diversity to the population. 
For our optimization problem, lower fitness scores are better. Hence, we use the tournament selection scheme. The tournament selection scheme will run k times to select k individuals. On every iteration, it will randomly pick n individuals from the population, and select the best individual for the next generation. Hence, we can adjust n tournament size to be larger to give more selection pressure to choose the best individuals, or smaller n to give higher probability for weaker individuals to survive to the next generation. 
Evaluation of Fitness 
The fitness value is calculated using the template-based method mentioned in the earlier section. Rule-based matching only gives a binary match/not match result, which is not useful to the implementation of GA. Template-based method gives a distance measure (DM), in which a smaller value will represent a closer match between chosen template and PIP points. Hence, the GA algorithm is implemented with a minimization objective. 
When matching with different templates for a given segment, the lowest DM value for all template matchings is used (since it is taken to be the correct matching). Since a given individual can have different number of segments, we also use the average DM value for all segments as the fitness score of the individual.  


# Five Patterns
The five templates used in the template matching are shown below: 
![Image of Patterns](https://github.com/wwsscc/CSCI653/blob/master/WechatIMG3.jpeg)
![Image of Pattern](https://github.com/wwsscc/CSCI653/blob/master/WechatIMG4.jpeg)

# Tested
To test the genetic algorithm, we generate a series of synthetic patterns and concatenate them into a time-series data. The lengths of the patterns are preset, and are different for each pattern. 

1. Test accuracy for single pattern
![Image of Patterns](https://github.com/wwsscc/CSCI653/blob/master/WechatIMG5.jpeg)
2. Test multiple patterns
![Image of Patterns](https://github.com/wwsscc/CSCI653/blob/master/WechatIMG6.jpeg)
![Image of Patterns](https://github.com/wwsscc/CSCI653/blob/master/WechatIMG7.jpeg)

For this test, it has been discovered that the GA algorithm can sometimes find a lower optimal value for the fitness function by combining the last three patterns into a segment. The PIP algorithm tends to erase the features of the time-series when computing the PIPs, and in that sense the last three patterns can easily be considered as an inverse triple top. A solution that contains less segments also tend to have a higher chance of giving better fitness values. While such an interpretation might not be considered wrong in the case of real world data, it is not the ground truth in our synthetic experiment. 
For our synthetic data generation, it has been discovered that 100 generations is enough to get the correct optimal value. However, we still stick to the 1000 generations that was mentioned in the paper by Chung, F.-L., et al.(2004) to use in the backtesting of real world data. 

# Backtesting 

Backtesting is the process of testing a trading strategy on relevant historical data to ensure its viability before the trader risks any actual capital. 
We first run the genetic algorithm to break down the time-series into smaller segments that should have higher probabilities of containing patterns, before running the rule-based pattern recognition algorithm to detect patterns in the time-series. For this testing, we only run the rule-based pattern for detecting inverse head and shoulder pattern. 
Given the inverse head and shoulders pattern, we can draw a neckline (line connecting the highest price and the second highest price in the pattern). The theory states that the price should rise for some time after the neckline has been crossed (Lo, Andrew, et al., 2000). 
The period for testing is from 24 November to 8 December 2017. The test is conducted on 1951 1 
stocks from Nasdaq stock market. The stock data is obtained from the Alpha Vantage web API . We get the optimal segments from the genetic algorithm and only look for inverse head and shoulder pattern. We use three (3) different investment strategies. In all these strategies, a stock is purchased as soon as the current price rises above the neckline. We will invest $150 in each stock. 
Out of the 1951 stocks, we detected inverse head and shoulder pattern in the following 9 stocks. 

# Expected Results

1. Purchasing stock when price above neckline and sell at the end, expected results will show loss.
2. Purchasing stock when price above neckline but sell when the price of stock falls below the highest. Expected results will show profit.

# References
Alpha Vantage. “Alpha Vantage - Free APIs for Realtime and Historical Financial Data, 
Technical Analysis, Charting, and More!”, 11 Dec. 2017, www.alphavantage.co/. Chung, F.-L., et al. “An Evolutionary Approach to Pattern-Based Time Series Segmentation.” 

IEEE Transactions on Evolutionary Computation, vol. 8, no. 5, 2004, pp. 471–489., 
doi:10.1109/tevc.2004.832863.

F.-A. Fortin, F.-M. De Rainville, M.-A. Gardner, M. Parizeau, and C. Gagné. DEAP: Evolutionary 
Algorithms Made Easy. Journal of Machine Learning Research, 13:2171--2175, 2012. Fu, Tak-Chung, et al. “Stock Time Series Visualization Based on Data Point Importance.” 

Engineering Applications of Artificial Intelligence, vol. 21, no. 8, 2008, pp. 1217–1232., 
doi:10.1016/j.engappai.2008.01.005.

Fu, Tak-Chung, et al. “Stock Time Series Pattern Matching: Template-Based vs. Rule-Based 
Approaches.” Engineering Applications of Artificial Intelligence, vol. 20, no. 3, 2007, pp. 
347–364., doi:10.1016/j.engappai.2006.07.003.

Lo, Andrew, et al. “Foundations of Technical Analysis: Computational Algorithms, Statistical 
Inference, and Empirical Implementation.” 2000, doi:10.3386/w7613. 

