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

Let's start by enumerating this machine some more to find hidden pages associated with the host website. I'll start by using the following command in gobuster: `gobuster dir -u http://10.201.83.59 -b 404`. We'll add the -b 404 to filter out 404 status codes, as we will likely have many of them. 

## Hunt for the first flag

We get a hit here with the directory "hidden".

![](../Pasted%20image%2020251103164406.png)

Let's jump to the "hidden" page and see what they have for us. 

We're greeted with a simple page that says "Welcome to CTF". Checking the source code there doesn't appear to be anything of interest there. 

![](../Pasted%20image%2020251103164649.png)
![](../Pasted%20image%2020251103164743.png)

So let's try to enumerate this directory and see what we can find using the same command. Let's just be sure that we change the url to reflect the hidden directory. 

![](../Pasted%20image%2020251103165029.png)

We're greeted with a page that reads "dead end". Poking around in the source code for this page reveals something interesting.

![](../Pasted%20image%2020251103165047.png)

Beside hidden, there appears to be some type of encoded line or hash. Let's check it out in CyberChef and see if anything interesting happens. 

![](../Pasted%20image%2020251103165127.png)

### First Flag

Bingo ! We got a hit in CyberChef with a Base64 decode that reveals **flag{f1rs7_fl4g}**

![](../Pasted%20image%2020251103165258.png)

## Hunt for the second flag

![](../Pasted%20image%2020251103173029.png)

*Note: I switched to my kali OS rather than the AttackBox at this time for faster response times whilst enumerating, so the target IP will differ from here on to 10.201.66.137.*

Here I was stuck for a bit and felt like I was out of options until I took a look back at my nmap scan. I noticed that on port 65524 there was a robots.txt file that I hadn't noticed previously. 

![](../Pasted%20image%2020251103190936.png)

So I hop to the URL and find an interesting user-agent: a18672860d0510e5ab6699730763b250

![](../Pasted%20image%2020251103191110.png)

Let's once again try to input this into CyberChef and see if anything comes of it.
CyberChef unfortunately wasn't able to work its magic for us this time.

I thought this could be some type of hash and went to https://crackstation.net/ to see if I could get lucky. 

![](../Pasted%20image%2020251103191523.png)

Unfortunately Crack Station couldn't help here either. We'll most likely need a more powerful hash cracking tool to progress further. After a bit of research online, I discovered a website called https://md5hashing.net/ that may be able to help. 

![](../Pasted%20image%2020251103192115.png)

And presto, we have our second flag ! **flag{1m_s3c0nd_fl4g}**
![](../Pasted%20image%2020251103192202.png)

## Hunt for the third flag

Next I decided to take a look at http://10.201.66.137:65524/ itself and see if I could get any other leads. ![](../Pasted%20image%2020251103192541.png)
![](../Pasted%20image%2020251103192557.png)

Nothing Immediately stood out to me. Let's take a look at the source code and see if any hints are left for us. There was an interesting possibly base encoded string, based off the hint, that was left for us, **ObsJmP173N2X6dOrAgEAL0Vu** let's take a look at that a bit later.

![](../Pasted%20image%2020251103195419.png)

Thankfully, flag 3 is right out there in the open up for grabs ! **flag{9fdafbd64c47471a8f54cd3fc64cd312}**

![](../Pasted%20image%2020251103192748.png)

As usual, I'm going to pop it into CyberChef and see if it can decipher this for us using the hint that it could be base encoded.

![](../Pasted%20image%2020251103193048.png)

![](../Pasted%20image%2020251103195738.png)

Jackpot ! This was base62 encoded and it seems to have given us a hidden directory named: **/n0th1ng3ls3m4tt3r**. So I appended this to the URL http://10.201.66.137:65524/n0th1ng3ls3m4tt3r/ and get the following: 

![](../Pasted%20image%2020251103195930.png)

It seems we have entered the matrix lol.

First things first, we take a look at the source code.

![](../Pasted%20image%2020251103200013.png)

We find an interesting string inside the paragraph tag. **940d71e8655ac41efb5f8ab850668505b86dd64186a66e57d1483e7f5fe6fd81**

We're told we need to use the provided wordlist to crack this hash, so let's pull out one of my favorite password cracking tools, John the Ripper !

First lets save this hash into a txt file so JTR is able to read it. 

![](../Pasted%20image%2020251103201246.png)

The hint given by THM appears to tell us that this is a GOST hash. 

![](../Pasted%20image%2020251103201751.png)

Let's finally crack this password by invoking the following command:
`john --format=gost --wordlist=/home/rootrabbit/Downloads/easypeasy.txt ephash.txt`

![](../Pasted%20image%2020251103202107.png)

Ladies and gentlemen, we have our password !

## SSH Into the Machine

Next we are tasked with logging in remotely to the target machine.
Let's start by enumerating our newest directory and see what comes up.

![](../Pasted%20image%2020251103202600.png)

We get a ton for code 403s meaning these files exist, however we don't have permission to access them. 

