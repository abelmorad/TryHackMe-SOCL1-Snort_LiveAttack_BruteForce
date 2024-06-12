# Documentation: TryHackMe SOC L1 - Snort Live Attack Challenge - Brute-Force

## 1. Introduction

- The room invites you to a challenge where you will investigate a series of traffic data and stop malicious activity work with Snort to analyze live and captured traffic
### Objective
- Analyze the traffic with Snort and identify the anomaly first. Then you can create a rule to stop the brute-force attack and capture the flag

## 2. Setup 
### Prerequisites

- Basic knowledge of network security concepts
- Access to the TryHackMe platform

## 3. Procedures

- **Start Snort in sniffer mode and try to figure out the attack source, service, and port.**
  - In the CLI enter `snort -vde` to display the packet data, headers as well as the data link layer headers
  - Analyze the packet and look for anomalies
    - This IP keeps coming up in the packets
      - ![snortliveattack2](https://github.com/abelmorad/TryHackMe-SOC1-Snort_LiveAttack/assets/110463619/b22ebf3c-25c5-40ca-8a89-b2ae43e2cfeb)
    - It seems suspicious let us find out how often this IP comes up. Enter `snort -vde | grep 10.10.245.36`
      - ![snortliveattack4](https://github.com/abelmorad/TryHackMe-SOC1-Snort_LiveAttack/assets/110463619/1cb8f16c-79a4-4598-b37b-1c6dc45d9e9a)
    - We figured that the TCP/22 is under attack by the source of that IP address. Let's create an IPS rule in Snort to stop the attack
    - Gain root access using `sudo su` then in the root directory and navigate to the rules directory. Enter `cd /etc/snort/rules`
    - In the rules directory there are various rule sets available we need to open local.rules and add our rule. Enter `nano local.rule`
    - Let's add our rule. Here we are rejecting any incoming traffic coming from this IP address
      - ![image](https://github.com/abelmorad/TryHackMe-SOC1-Snort_LiveAttack/assets/110463619/7088bd18-851d-4411-8f7c-4fc2a070dad2)
    - To run Snort in IPS mode. Enter `snort -c /etc/snort/rules/local.rules` so we can use the rule we created
    - Since we are successful in rejecting the packets. We captured the flag!
      - ![snortliveattack9](https://github.com/abelmorad/TryHackMe-SOC1-Snort_LiveAttack/assets/110463619/227e8368-cb9d-4b30-bcf6-3737c5934f60)
      - ![snortliveattack10](https://github.com/abelmorad/TryHackMe-SOC1-Snort_LiveAttack/assets/110463619/b01f16f2-2cf6-4cf1-961b-0de856f29bdd)
        
## 4. Attack Detection 

- Observe the console output or log files for alerts generated by the attack.
- Note down alert details like timestamp, source IP, destination IP, type of alert, and ports.

## 5. Conclusion

- We successfully blocked the attacker's IP preventing the attack and captured the flag. Snorts IPS capability is an effective tool to prevent such attacks.
- Another effective solution to prevent brute-force attacks is to have a strong password or Use a CAPTCHA to prevent automated attacks

## 6. References

- [Snort Documentation](http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node4.html)
- [Blocking Brute Force Attacks by OWASP](https://owasp.org/www-community/controls/Blocking_Brute_Force_Attacks#:~:text=The%20most%20obvious%20way%20to,manually%20unlocked%20by%20an%20administrator.)



