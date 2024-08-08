# TL;DR
This folder contains the code relevant to data extraction.  
The data are stored locally and are not shared in this repo.  

# Overview
We extract data from two sources:  
- market data, i.e. minute-by-minute betting odds [Betfair](https://www.betfair.com).
- game data, including the timing of goals, from [Footystats](https://footystats.org/).

# Market data
The market data set starts in May, 2015, the earliest date available at the Betfair Historical Data service, and ends in January, 2020, immediately before the pandemic-related soccer league suspensions.  
The files are provided in bulk for all matches of all competitions for which the market was available in a country, so we filter relevant matches *a posteriori* using the game data set.  
The betting odds are “last traded prices”, before and during the match. These are market odds of actual transactions, as opposed to offered odds. There is no information on volume, thus market liquidity is not observed directly in the data.  
Historical data are not available for 100% of the matches for which the market was active.  
Betfair informed us that data recording issues can be the result of: data recorder downtime, data processor/processing errors, or technical matters with the website. These issues are reasonably assumed to be not related to features of the match.    
The raw data are extracted via Betfair's API and are available in compressed JSON files.    
The variables available in the market data set include the country where the match took place, team names, a timestamp and last traded prices for “Home”, “Away” and “Draw”.   A more comprehensive list is found in section A.2. As the market data set does not relate directly to the soccer match itself, there are no timestamped data for the kick-off or goals. The assumed kick-off time is the moment at which the market turns in-play.  
The code for market data extraction is on notebook `betfair_api.ipynb`.  

# Game data
The game data set was gathered for domestic leagues from 17 countries.  
Raw data were provided in a set with one csv file per league per year.  
The data set includes information on goals, cards, shots and other game variables. Among the game variables, only goals are timed.  
The timings are recorded as “match minutes”, as opposed to the market data set that records a Unix timestamp. Thus, a goal at minute 50 in the game data set may have occurred, for instance, 67 minutes after kick-off, considering injury time in the end of the first half and a 15-minute long half-time. 
The extraction was performed by downloading the csv files from Footystats' website. 
The code for game data consolidation is on notebook `footystats.ipynb`.
