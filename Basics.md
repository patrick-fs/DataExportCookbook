# Basic Recipes

Keep two things in mind when analyzing your Data Export data:

1. Rows are individual events created during sessions
2. Distinct sessions can be found by using a compound key of `sessionid` + `userid`

## Total session count
```sql
select count(*) as "total recorded session count for your org"
from 
(select sessionid, userid
from fsexport 
group by sessionid, userid)
```
![image](https://user-images.githubusercontent.com/45576380/50705505-0b40a400-1029-11e9-8c74-4b489abbe0d4.png)

## Most active sessions
You can construct session replay URLs following the pattern in this example. You can find `your org id` by watching a session in FullStory and copying it out of the session reply URL. Your org id will be a short alpha-numeric string, for example: `1ENq`.
```sql
select count(sessionid) as "events per session",
'https://app.fullstory.com/ui/' || 'your org id' || '/session/' || userid || ':' || sessionid as "session replay URL"
from fsexport 
group by sessionid, userid
having "events per session" > 35
order by "events per session" desc
```
![image](https://user-images.githubusercontent.com/45576380/50705678-90c45400-1029-11e9-8c06-8b9bfa10f354.png)

## Sessions by length
[Narrative description here]
```sql
select trunc(extract(second from (Max(eventstart) - Min(eventstart)))/60, 2) as "session length (minutes)",
count(sessionid) as "events per session",
'https://app.fullstory.com/ui/' || 'your org id' || '/session/' || userid || ':' || sessionid as "session replay URL"
from fsexport 
group by sessionid, userid
order by "session length (minutes)" desc
```
![image](https://user-images.githubusercontent.com/45576380/50705875-295ad400-102a-11e9-9e5a-a79b9159b26a.png)
