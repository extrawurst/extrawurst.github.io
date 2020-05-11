---
title:  "Email forwarding madness"
date:   2020-03-01 12:00:00
categories: [general]
---

![email]({{ site.url }}/assets/email.jpg)

<span class="cap-char">E</span>mail — this age old tool that will not die. You need email to register accounts, have a point of contact for your customers and business contacts. It is the only really standardised communication channel.

<span class="cap-char">M</span>any people like to have custom domains for their email accounts. For a long time I am managing those for my family and many of the domains/apps that I own. All those are just supposed to be forwarded to a few gmail accounts that actually host the email account.

<span class="cap-char">A</span>ll these forwards were handled using mailgun before, easy setup even for complicated routing like one catch-all address that is forwarded to multiple accounts. And most of all it was* free.  
*Yes it is not free anymore! Even using mailgun just for that simple use case will cost $35 a month now.

<span class="cap-char">I</span> was searching forever to find an alternative: Amazon SES (too much maintainance), Google Domains (not available in europe), Mailjet (does not support receiving), Pobox and ForwardMX (not free) and [many more](https://woorkup.com/email-forwarding-service). Unfortunately almost all had some downside compared to mailgun.

<span class="cap-char">L</span>et’s look at the problem more closely: 1) we want to have a *custom domain* that we can use for receiving and sending email 2) we don’t want an actual email account for each of these because all are supposed to just *forward* to gmail (or something similar). 3) Setup and maintainance shall be *easy* and 4) it should be *free*.

# The solution

For the sending part it turns out you do not even need a SMTP server associated with your domain. You can make gmail simply send from your custom domains: [https://forwardemail.net/en/faq#how-to-send-mail-as-using-gmail](https://forwardemail.net/en/faq#how-to-send-mail-as-using-gmail)

For the receiving part I end up using two different approaches:

1. For the simple cases I moved domains from my previous Registrar to namecheap.com. Here you get great rates (actually cheaper in my case) and email forwarding is included (even with complicated routing).
2. For my domains/apps this approach does not work because it requires using route53 on AWS and namecheap.com only supports email fowarding when using their own DNS.

In those cases we are using forwardemail.net which allows for free forwarding routes. Why am I not using this everywhere? Because they require the route setup to be publicly visible in DNS-TXT entries.

# Further reading

* [https://github.com/Mailu/Mailu](https://github.com/Mailu/Mailu) (dockerized email server)  
If I would host my own solution I’d probably go with this one
