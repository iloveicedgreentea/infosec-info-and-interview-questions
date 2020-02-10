# General Info

## Interview Tips

These are generally geared towards remote interviews.

* Be generally prepared
    - Make sure the room is a comfortable temperature. Sweating or shivering will destroy your train of thought.
    - Make sure the room is well lit. Test your camera before the interview. Make sure you can be clearly seen and heard.
    - Clear your area of all distractions. Silence your phone. Close unneeded programs.
    - Make sure you ate enough before.
    - Have water within arms reach.

* Write out your notes and have them in front of you. This will help you convey your thoughts without getting lost.
    - Who you are
    - Non-tech things you do
    - Important talking points
    - Some technical references if needed

* Take time to learn about the company and your interviewers.
    * A new company is a commitment. You are interviewing them as much as they are interviewing you. 
    * Research the company on Linkedin and Glassdoor.
    * Look for mutual interests, talking points, etc. Remember, you are working with real people. It's good to make friends.
    * Look for things that you should ask about - compensation structure, internal process, home office reimbursements, etc
    * Look for red flags. Are there reviews mentioning poor processes? Bad interview questions? [Brain puzzles](https://www.theatlantic.com/business/archive/2013/06/google-finally-admits-that-its-infamous-brainteasers-were-completely-useless-for-hiring/277053/)? 
    * In general, have a list of questions to ask. Even if you know the answers, asking questions will show them you are interested. Companies would generally rather hire an enthusiastic Junior than a Senior who isn't interested in the company. You can teach skills. You can't really teach enthusiasm.

* Don't just give answers; have conversations. 
    * Give context to your answers - why do you think X and not Y? How did you arrive at your opinion?
    * Wrong answers are okay if you frame your opinion
    * Not knowing an answer is fine if you can take an educated guess or talk though it. It looks really bad to simply say you don't know and make no attempt to figure it out.
        * "I don't know X, but it works like this with Y so maybe XYZ" or "I don't know off the top of my head but I can research that after and get back to you"
    * It's okay to not know something. It's 2020 - we have Google. If you don't know how to invert a binary tree, as someone who isn't a software engineer, who cares!? [You can google it.](https://twitter.com/mxcl/status/608682016205344768?lang=en)

## General Security Concepts

### CIA Triad

Confidentiality, Integrity, and Availability. This is the fundamental principle of Infosec. 

* Confidentiality - Don't expose data to people who shouldn't see it. Related concepts are IAM, Encryption, etc

* Integrity - Things should have unintended or unauthorized changes. Things need to be reliable and trustworthy. Related concepts are hashing, signing, auditing, non-repudiation

* Availability - Things need to be up and running. It relates to security through concepts like DOS, destroying backups, ransomware, developers having production access, anything that can cause an outage.

### Principle of Least Privilege (PoLP)

In short, nothing should have more access than the absolute bare minimum needed to accomplish a task. One example is a developer in AWS with an account. Many orgs will give them full admin access or a policy like `EC2:*`. This is dangerous. First, nothing is more dangerous to your company than a developer with admin access, and realistically, any kind of production access. Second, *when* their credentials are exposed or stolen, an attacker will now have over-privileged account.

### Defense In Depth

Security is an onion. You can't expect to be secure with one set of protections. You need many layers - Defense in depth

A good combination would be PoLP in all aspects (IAM, containers, server accounts), firewalls on servers and the perimeter, automated vulnerability scanners for alerting, WAFs on endpoints, VPNs, restricted ACLs and security groups, up to date servers, regular and secured backups, and security training for personnel: You can have all the security in the world but Jim from Accounting is still going to open up that Phishing email.

## General Questions

### Where do you get your news/info from?

In my opinion, these are some good places to get news. It's very important to keep up with all kinds of news - attacks, bugs, new tools, etc. 

* Twitter
* https://reddit.com/r/netsec
* https://www.theregister.co.uk/
* Krebs
* Schneier
* Threatpost
* Troy Hunt
* Slacks (Hangops, other private ones)

You can also set up Feedly with a bunch of resources from the above, and others, to get a single dashboard of info.

### Who do you follow in the industry?

Related to the above, it's important to follow important people/groups in the industry outside of looking at news. This mostly applies to twitter.

### What are your favorite tools?

This question is only helpful to start a conversation and judge your familiarity with the entire space, not the tools themselves. Can you name 5 security tools right now? What are they for and why would you use one over the other?

Don't say Kali. 

