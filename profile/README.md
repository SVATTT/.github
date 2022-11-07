# Orgnizations for organize SVATTT 

## Network & Rules
- Network: https://drive.google.com/file/d/1j0ZDi2i5kKpNb9Wxphu6w2x4YbMJ0esU/view?usp=sharing
- Rule: https://docs.google.com/document/d/1c670gXXjvSHH1ipW8kM_3twW6EYNSU4k_WKlRAKNtqA/view?usp=sharing

## Some Problems
- The scoreboard is not quite beautiful
  - Need someone do css this
- One team can only attack another team through gateway 
  - Set iptables and verify with curl that one proxy cannot connect directly to other chall and other proxy.
- One team can not defense by forwarding traffic to another team. 
  - The task need to add a secret to each team. And that checker verify that it's connecting to the correct team.

## How to submit flag in attack defense mode
- In attack defense mode, you need to call a PUT api to internal ip address of scoreboard (http://10.254.0.253:8080/flags) to submit flag. 
- Each team have a secret X-Team-Token. 
- We will send it to each team. 
- Here is the sample curl: 
```
curl -i -s -k -X $'PUT'
-H $'Host: 10.254.0.253:8080' -H $'Accept-Encoding: gzip, deflate' -H $'Accept: /' -H $'Connection: close' -H $'X-Team-Token: xxxxxxxxxxxxxxxx' -H $'Content-Length: 36' -H $'Content-Type: application/json'
--data-binary $'["WHUZIXPMV4F0GWGDN6BV1GBHM4X3S7H="]'
$'http://10.254.0.253:8080/flags'
```

## Attack & Defense Flag Format
Pay attention that the format of flag in attack defense mode is a base64 encoded string (for example: WHUZIXPMV4F0GWGDN6BV1GBHM4X3S7H= )

## Attack & Defend Challenges
- web (port 4001): https://drive.google.com/file/d/1jvvBax-RuBUGl4oIqwj7Pvk2KVbGKX1F/view?usp=sharing
- pwn1 (port 4002) https://drive.google.com/file/d/1c-edc1Um91-6axkzwzTAq2CNZL1pjwEV/view?usp=share_link
- pwn2 (port 4003) https://drive.google.com/file/d/1keHWyZR7IgXWRJkgsn1g8pLiahZHRk5B/view?usp=share_link

## Forbidden Actions in attack defense mode
The following actions are forbidden:
- Try to use automate tools to send too many requests lead to dos another team. You don't need bruteforce to get the flag.
- Forward traffic from your proxy server to another team challenge server to defense.

## Round Update
- Each round is 5 minutes. 
- Each round will have a new flag. 
- The service status of each round is updated after the health check. 
- If the health check is failed, the failed status was show through all the current round. It's not REALTIME.

## Attack & Defense Guide
### Attack guide
- Try to find and exploit the easy bug first and then find the bug that is hard to prevent.
- Write a script to automate the exploit and submit flag to scoreboard. Don't manual, it's too slow.
- Keep persistence in system to easy get flag later.
### Defense guide
- Sniff traffic to detect exploit payload, example tool: tcpdump
- Develop a proxy to intercept and drop exploit payload
   - Be careful if you use iptables, you can DROP ALL YOUR TRAFFIC (try iptables-save, iptables-restore to backup rules)
   - If you don't have custom one, our teammate provides a good example tool here: https://github.com/Q5Ca/simple-portforwarder
- Exploit yourself system to remove all persistence backdoor.

## Flag for service V-store (4001)
- The lastest flag of current round is always appended at the end of file /flag or table flags
