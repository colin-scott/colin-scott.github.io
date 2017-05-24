---
layout: post
title: "Do Toll-Free Phone Lines Violate Network Neutrality?"
bibliography: bib.bib
date: 2017-05-17 00:01:12
comments: true
categories:
---

The network neutrality debate focuses primarily on supply side issues, such as whether content providers should be
allowed to pay Internet service providers for "fast lanes", and whether allowing them to do so would stifle innovation.

It seems fruitful to also look at network neutrality through a
different lens. In this post I explore the following question:

>  What implications do network neutrality regulations have on equality of access to the Internet?

### The Moral Dilemma

I've had a recurring debate with my friends who support network neutrality. Most of them agree with the following premises:

- Society should strive to provide equal *opportunities* to all citizens.

- Access to the Internet provides opportunities.

- There are over a billion people in the world who cannot afford the costs of Internet data.[^2]

- Therefore, something should be done (immediately) to provide more
  affordable access to the Internet.

But when I try to take the argument further (as follows), the debate gets heated:

- Some content providers (most notably: Facebook and the other members of the Free Basics program)
  are willing to pay Internet providers for the costs that consumers incur by accessing their content.[^3]

- Preventing these content providers from subsidizing Internet access (on
  network neutrality grounds[^9]) *prolongs* inequalities in society.

### Who Will Pay?

Although my net neutrality friends may agree that we should not
prolong inequalities in society, they disagree that we should allow content providers
to pay for data costs. According to them, the negative implications that might
result outweigh the benefits of improved access.

But who else will pay?

One answer: the government. The government is (in most of the world[^4]) a
democratically elected body, accountable to the people. They could in theory
implement an affordable Internet subsidy while
maintaining network neutrality.

Personally, I'm skeptical that government-lead data subsidies will ever become
widespread, particularly in countries where
access inequalities are the most stark. Governments are slow to move,
struggle to sustain projects that span multiple election cycles,
and are not without their own conflicts of interest (e.g., consider that
governments sell spectrum rights for billions of dollars,[^5] and may even seek
to maintain high data costs as a form of censorship[^6])

Clearly this is a complex issue. In lieu of a solution to the moral dilemma, I'd like pursue
another line of argumentation with my net neutrality friends:

### The Definitional Dilemma

Before the World Wide Web (and still to this day!), phone calls were a very common way for
people to access information. In the same way that billing works on the Internet, the consumer of voice
information (the caller) needs to pay the phone provider for the costs of
transporting their phone calls.

Some *voice* content providers (customer service
centers, crime reporting centers, suicide help lines, etc.) decided that they
would like to allow consumers of their services to call in for free.
Instead of having the consumer pay, the content providers pay the phone provider for the costs of
maintaining their toll-free phone line.

The parallels to the Internet are striking. Many of the arguments against Internet content providers also seem to apply to voice content providers.
(What if a small innovative startup cannot afford to pay for a toll-free phone line? etc.).

Yet, toll-free phone lines aren't bothersome to my net neutrality
friends. I think it would be valuable for us to understand why!

I suspect that the *content* being served (and the companies that are doing the serving) is the real source of discomfort
for people who oppose Free Basics. The same people don't have much of an issue with toll-free phone
lines because toll-free phone lines are commonly used by governments, non-profits and companies with ostensibly charitable intentions.

Yet there are plenty of governments, non-profits, and companies with charitable intentions who are eager to
pay to deliver *Internet* content to consumers who can't afford it. Which
brings us to an actionable takeaway from this whole discussion:

### Toll-Free Data via Voice Calls

Remember the good old days of dial-up modems? Dial-up modems transferred
Internet data over *voice* lines, by encoding data as audio signals.

There's no reason we can't do the same over cellular to deliver data to a
mobile phone. We just need software (a "modem") running on the
phone to decode the audio signals. In fact, this idea has already been proposed
before, in a slightly different context.[^7]

Now, if a well meaning company wants to provide data to consumers free of
charge, they could pay for a toll-free phone line (or a return-your-missed-call phone line[^11]) for consumers to call, and run a server that transfers data over that
voice line whenever it receives a call. An application running on the phone could periodically make calls (without
the user needing to intervene) to the toll-free phone number to retrieve small
amounts of data.

What's neat about this scheme is that we can implement it right now, without needing to change business models or wait on regulatory decisions. And it's not fundamentally any different than toll-free phone lines, or even a regular return-your-missed-call phone system.[^10]

Anyone have spare cycles to build the modem and server infrastructure? I've already got some ideas for great applications we could build
on top of toll-free data.

---

Thanks to Bill Thies, Aurojit Panda, and Sachin Gaur for helping me shape these thoughts.

---

[^2]: [Internet.org State of Connectivity Report](https://info.internet.org/en/wp-content/uploads/sites/4/2016/07/state-of-connectivity-2015-2016-02-21-final.pdf)

[^3]: In fact there are two startups, [Jana](http://www.jana.com/home) and [Movivo](http://www.movivo.com/), who are in the business of making it easy for content providers to reimburse consumers for the costs of their data usage, by sending mobile top-ups to the consumer after they have consumed the data. In India, Airtel rolled out a [business model](https://en.wikipedia.org/wiki/Airtel_Zero) that allowed content providers to pay for data, but this has come under substantial criticism from network neutrality advocates.

[^4]: [Historical data](https://ourworldindata.org/democracy/) on the fraction of the world population living in democratic countries.

[^5]: [India ends spectrum auction for 9.5 billion dollars](http://www.livemint.com/Industry/xt5r4Zs5RmzjdwuLUdwJMI/Spectrum-auction-ends-after-lukewarm-response-from-telcos.html)

[^6]: As in Jordan or Eritrea.

[^7]: [Hermes: Data Transmission over Unknown Voice Channels](https://www.cs.nyu.edu/~jchen/publications/com31a-dhananjay.pdf).

[^9]: As in the [ban of Free Basics within India](https://www.theguardian.com/technology/2016/may/12/facebook-free-basics-india-zuckerberg).

[^10]: Which does not require a toll-free number, you can run it over a normal phone number.

[^11]: With the (sole?) exception of the USA, phone calls are billed only to the caller, not the receiver. By giving a missed call (for free), the client can signal to the server to call them back, such that the server bears the cost of the phone call rather than the client.
