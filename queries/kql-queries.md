## below are some sample kql queries you can play around with. If you want to learn more about KQL
## check out https://kc7cyber.com/
SecurityEvent
| where EventID == 4625
| summarize Attempts = count() by countryname
| order by Attempts desc

SecurityEvent
| where EventID == 4625
| project TimeGenerated, Account, Computer, EventID, Activity, IpAddress


let GeoIPDB_FULL = _GetWatchlist("geoip");
SecurityEvent
| where EventID == 4625
| where LogonType == 10
| where isnotempty(IpAddress)
| summarize FailedAttempts = count() by IpAddress
| where FailedAttempts > 5
| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)
| order by FailedAttempts desc


let GeoIPDB_FULL = _GetWatchlist("geoip");
SecurityEvent
| where EventID == 4625
|order by TimeGenerated desc
| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)

let GeoIPDB = _GetWatchlist("geoip");
SecurityEvent
| where EventID == 4625
| where isnotempty(IpAddress)
| where IpAddress !startswith "10."
| where IpAddress !startswith "172."
| where IpAddress !startswith "192.168"
| evaluate ipv4_lookup(GeoIPDB, IpAddress, network)
| extend Country = tostring(countryname)
| extend City = tostring(cityname)
| summarize TotalAttempts = count() by Country
| order by TotalAttempts desc
