Hello fellow Shell Hoppers ! A new assignment has come into Shell Hopper Inc. We're tasked with using nmap to scope out a website and later compromising the machine to find the flags.

## Enumeration through nmap

In this particular case, we're going to be checking out IP 10.201.83.59.

There are a series of 3 preliminary questions that we're given: 

### Questions
A. How many open ports ?
B. What is the version of nginix ?
C. What is running on the highest port ?

We can tackle these three questions using nmap. I used the following command to obtain the answers: `nmap -sS -p- -vv -A 10.201.83.59 -oN espcan.txt` This command will provide us with the following information.

![](../Pasted%20image%2020251103162351.png)

### Answers
A. 3
B. 1.16.1
C. Apache

## Compromising the Machine

