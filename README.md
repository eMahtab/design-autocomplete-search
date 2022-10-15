# Design AutoComplete Search (Search Suggestion Service)

!["Google Search Prediction"](istanbul.PNG?raw=true)

## Estimates & System Usecases :
1. How many users will be using the system(search service) : 1 Billion users

2. How many suggestions are to be provided for each query : At max 10 suggestions

3. What should be the criteria to rank suggestions : Rank based on how many times (frequency) the given word is searched by users 

4. Should we add trending searches in the search suggestions : We should add the current trending searches in the search suggestions

## Design Expectations :
1. The latency of suggestions must be very less, i.e relevant suggestions must be shown as the user is typing.

2. The search suggestion service must be highly available .

3. We will be handling a large amount of data, hence partition tolerance is necessary. Partition tolerance means that the cluster must continue to work despite any network failure between system components/ nodes in the system. Users must not face any performance or availability failure.

4. It is okay if we don't have strong consistency. Owing to the CAP theorem, as availability and partition tolerance is our priority, eventual consistency is fine. If two people see different suggestions for a certain time span, it is fine. But the system must be available. Otherwise, it will degrade the user experience. In any distributed system Partition Tolerance is must.

5. The search suggestions made by the system  must be relevant to whatever prefix the user has entered.

## System APIs
**getSearchSuggestions(prefix):**

Returns the search suggestions for the given prefix.

**markQueryAsTrending(query):**

Marks a search query as trending query in the system, if its searched above a certain threshold.

## Traffic Estimations :
Assuming 1 billion users will be using the search service everyday.
Lets assume each user makes 5 search queries everyday. And we call the search suggestion service on every character typed in the search box.
And lets assume each search query typed by user is of length 5.

Number of requests made by one search query : 5 (length of search word is 5)
Total Number of requests made by one user in a day : 5 * 5 (each user makes 5 requests in a day)

So from one user the system recieves 25 (5 * 5) requests in a day.
For 1 billions users number of requests made in one day = 1 * 25 = 25 Billion requests in a day

Number of requests made per second =  (25 Billion) / 24 * 3600 = 289,352 = Around 300K requests per sec.

# High level design :

!["High Level Design"](high-level-design.jpg?raw=true)

According to our requirements, our autocomplete must be realtime. i.e. new search queries must be added to our database. Hence not only we have to design a system to give suggestions, but we also have to incorporate popular search queries to our database such that users can also get suggestions based on recent as well as popular searches.

Hence our autocomplete consists of two parts:

1. Giving Suggestions

2. Adding new trending queries to the database

## Suggestions Service :

When a user starts writing in the search box, the get_suggestions() query hits our application servers on every character the user enters. It obtains the top 7 suggestions from our DB and then serves it to the user. Let's keep a service called Suggestions service which is responsible to obtain the top 7 suggestions from the database.

## TrendingQueries Service :

Along with this, if the query is popular, let's say the query has been searched above a certain threshold number, then it has to be added to the DB. Let's keep a service called Trending Queries service which is responsible to add the new trending queries to our database.


# Precompute the suggestions for prefix : top 10 suggestions for each possible prefix



# References :
https://systemdesignprep.com/autocomplete
