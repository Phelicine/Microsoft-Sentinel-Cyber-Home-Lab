## Lessons Learned

- Exposing RDP publicly results in immediate brute-force attempts
- LogonType 3 was observed more frequently than LogonType 10
- Proper NSG rules are critical to prevent automated attacks
- Sentinel KQL queries must match exact column names
- After exposing your honeypot to the internet, it may take while before it starts to get targeted(this may vary of course)
