# Design AutoComplete Search (Search Suggestion Service)



## Estimates & System Usecases :
1. How many requests wil be made : 1 Billion requests per day

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
prefix: Whatever the user has typed until the moment the search query is sent.

**markQueryAsTrending(query):**
query: a new unique query which has been searched above a certain threshold will be marked as trending query in the system

