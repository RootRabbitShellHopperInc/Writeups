Greetings fellow Shell Hoppers ! We've got a new assignment in the form of a flag hunt that has been assigned to Shell Hoppers Inc. from TryHackMe ! We've been tasked to find a flag and escape the corridor inside of a webpage. 

The assignment reads as below:

![](../Pasted%20image%2020251029161057.png)

Okay Shell Hoppers, let's get to it !

According to the description of this assignment, we should be on the lookout for an IDOR vulnerability. The definition is given as follows:  

Insecure direct object references (IDOR) are a type of access control vulnerability that arises when an application uses user-supplied input to access objects directly. The term IDOR was popularized by its appearance in the OWASP 2007 Top Ten.

One way this can manifest is by appending things to the URL to try to get a different result or access places not intended for the end user.

Upon visiting the website, we are greeted with white corridor with six doors on either side and a final door at the end of the hallway.

![](../Pasted%20image%2020251029162252.png)

First things first, let's take a look at the source code and see if we have any freebies.

![](../Pasted%20image%2020251029162419.png)

Hmm, so far nothing exactly jumps out at me as a freebie. 

Let's see what happens when we try to open a door.

![](../Pasted%20image%2020251029163457.png)

We're greeted with an empty room and a dead end. Interestingly enough, our the page URL has added the first string ***c4ca4238a0b923820dcc509a6f75849b*** of the list we noticed when we checked the HTML source code. Let's back out and check out the next room down. 

We're met with another dead end resembling the last room. However, our URL has changed again with the ending of ***c81e728d9d4c2f636f067f89cc14862c*** this time. 

Interesting... taking a look at the hint provided by THM, these strings resemble hashes. Now that is interesting ! Let's see if we can get lucky and try one with https://crackstation.net/. Let's paste the first hash in and see what we get. 

![](../Pasted%20image%2020251029164851.png)

Bingo, we've got a match ! That's an MD5 hash for the number "1". 
Let's try the hash we got from the second door and see what we get. 

![](../Pasted%20image%2020251029165133.png)

We get the value for the number "2" for this hash. Let's add the rest of the hashes we get to https://crackstation.net and see what comes of it. 

![](../Pasted%20image%2020251029165648.png)

So here we see the numbers go from 1 to 13. Keeping an IDOR mindset, let's to produce an MD5 hash for the number "14" and see if that leads anywhere. For this, let's use https://gchq.github.io/CyberChef/ to produce an MD5 hash. 

![](../Pasted%20image%2020251029165853.png)

We get ***aab3238922bcc25a6f606eb525ffdc56*** as a hash for "14". Let's try to append that to our corridor URL and see if it leads anywhere. 

![](../Pasted%20image%2020251029170014.png)

Hmm it appears that page doesn't exist. Let's try the opposite direction and append the hash for the number "0". Let's head to CyberChef and produce that hash value. 

![](../Pasted%20image%2020251029170137.png)

We have a hash value of ***cfcd208495d565ef66e7dff9f98764da***, let's append that and see what happens. 

![](../Pasted%20image%2020251029170230.png)

Success ! We are rewarded with a flag. Great job out there Shell Hoppers, and as always, Hoppy Hacking !