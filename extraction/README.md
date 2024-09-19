# TL;DR
This folder contains the code relevant to data **extraction**.  
The data are stored locally and are not publicly shared in this repo.  

# Overview
We extract data from two sources:  
- market data, i.e. minute-by-minute betting odds from [Betfair](https://www.betfair.com).
- game data, including the timing of goals, from [Footystats](https://footystats.org/).


# Market data
## API
We extract market data from Betfair using their [Historical data API](https://historicdata.betfair.com/). The code is in notebook `betfair_api.ipynb`.  
The files are provided in bulk for all matches of all competitions, so in the [`preprocessing`](../preprocessing) folder we filter relevant matches *a posteriori* using the game data set.
## The data  
The betting odds are “last traded prices”, before and during the match. These are market odds of actual transactions, as opposed to offered odds. There is no information on volume, thus market liquidity is not observed directly in the data.  
Historical data are not available for 100% of the matches for which the market was active.  
Betfair informed us that data recording issues can be the result of: data recorder downtime, data processor/processing errors, or technical matters with the website. These issues are assumed to be not related to features of the match.    
The variables available in the market data set include the country where the match took place, team names, a timestamp and last traded prices for bets “Home”, “Away” and “Draw”.  
As the market data set does not relate directly/explicitly to the soccer match events themselves, there are no timestamped data for the kick-off or goals. The assumed kick-off time is the moment at which the market turns in-play. Goals are identified in the  [`preprocessing`](../preprocessing) chapter by comparing the timing of goals in the game data set with jumps in prices in the market data set.

# Game data
The game data set was gathered for domestic leagues from 17 countries.  
Raw data were provided as one `csv` file per league per year.  
The data set includes information on goals, cards, shots and other game variables. Among the game variables, only goals are timed.  
The timings are recorded as “match minutes”, as opposed to the market data set that records a Unix timestamp. Thus, a goal at minute 50 in the game data set may have occurred, for instance, 67 minutes after kick-off, considering injury time in the end of the first half and a 15-minute long half-time. 
The extraction was performed by downloading the csv files from Footystats' website. 
The code for game data **consolidation** is in notebook `footystats.ipynb`.  

# Dates
The earliest complete month of market data made available by Betfair is May, 2015.  
In the current version of this research, the analyzed data set ends in January, 2020, immediately before the pandemic-related soccer league suspensions.  
As one can see in notebook `betfair_api.ipynb`, we may have access to data for matches later than January, 2020. We intend to extend the analysis to more recent years in a future version of the research.
