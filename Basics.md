# Basic Recipes

## Total session count
[Narrative description here]
```sql
select count(*) as "total recorded session count for your org"
from 
(select sessionid, userid
from fsexport 
group by sessionid, userid)
```
[screengrab of results here]

## Most active sessions
[Narrative description here]
```sql
select sessionid, userid,
count(userid) as "events per session",
'https://app.fullstory.com/ui/' || 'your org id here' || '/session/' || userid || ':' || sessionid as "session replay URL"
from fsexport 
group by sessionid, userid
having "events per session" > 9
order by "events per session" desc
```
[screengrab of results here]

## Sessions by length
[Narrative description here]
```sql
select sessionid, userid,
trunc(extract(second from (Max(eventstart) - Min(eventstart)))/60, 2) as "session length (minutes)"
from fsexport 
group by sessionid, userid
order by "session length (minutes)" desc
```
[screengrab of results here]
