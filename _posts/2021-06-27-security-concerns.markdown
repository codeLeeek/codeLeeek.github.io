---
layout: post
title: "The forgotten stakeholder."
date: 2021-06-27 00:00:00 +1000
description: "Should we be concerned about indirect stakeholders of our system?"
img: 2021-06-27/security-concerns.jpg
---
I've been in the IT industry for around eight years now, the last two working with Security. Looking back with a Security lense I begin to realise I've only ever really considered two major groups when it comes to security stakeholders, the users of the product and the corporate assets. To be fair I'm relatively new to security but of all the blogs, articles, podcasts, security standards and even legal requirements I've encountered over the last couple of years very few tend to consider the seemingly forgotten stakeholder, everyone else. 

But lets take a step a back. What do I mean by "everyone else" as a security stakeholder?

### Analogy time. 
Think about buying a car. Before you decide on which car you want, you will likely consider several things: 

**Where will you store it?** : In a Garage/Carport? Shared car pack? On the street? When asking these questions you're essentially considering how to protect your investment. These are your company or corporate security concerns. 

**How safe is it?**: Does it have airbags? Seat-belts? Stability Control? Crumple zones? By asking these questions you're considering how to protect the driver and passengers of the car. These are primarily your user security concerns.

But what about pedestrians and other road users? The people that will never user your car but can still be very much affected if something goes wrong. More importantly, have you taken them into consideration when buying a car? 

### Enough with the self-aggrandising nonsense!
Alright, ok, please forgive my over indulgence. Essentially what I'm trying to say is how often do we consider the people that aren't directly involved with the service or product we work on? In my career I've only really ever thought about security stakeholders as the company, staff, clients, service providers and users; not so much the indirect stakeholders who may just be visiting or simply perceive the brand as legitimate. 

 To be clear I'm not suggesting this is a new revelation, certainly not my idea. In fact I only started thinking about this when I came across [Troy Hunt's blog post](https://www.troyhunt.com/heres-why-your-static-website-needs-https/) around the importance of https.

### So how do we think about security for everyone else?
That's a good question, and it isn't so much about what new things do we have to do to protect the indirect stakeholders but rather, when assessing and remediating risk, simply thinking about how the identified vulnerability can affect the indirect stakeholders as well as our key identified stakeholders.

To see how this can work in practice let's take a look at some common vulnerabilities through the lense of how it can affect the indirect stakeholders that aren't our immediate user base and how it could affect them.

#### HTTPS certificates
I'm sure by now all the security conscious among us have encountered [Troy Hunt's blog post](https://www.troyhunt.com/heres-why-your-static-website-needs-https/) on the importance and simplicity of securing your site with a HTTPS certificate. In a nutshell Troy demonstrates how a website without a HTTPS certificate could be exploited to extract sensitive data from the visitor through various man-in-the-middle attacks even if the site itself does not explicitly store any of said victims information. In essence abusing the trust the visitors have in the compromised site to extract sensitive information from them. Troy's blog post covers the subject better than I ever could, as such I won't go into specifics but I highly recommend the read if you haven't already. It is a great entry into thinking about and expanding the scope of affected users of a breach or attack and, as mentioned above, is the inciting incident for my own post.

#### Reflected cross site scripting (XSS)
Anyone that has worked with web development or web security will likely have heard of Cross Site Scripting (XSS) and the common flavours it comes in; Stored and Reflected XSS.
To give a brief overview of each type:
- Stored: Malicious script is saved to a persistent data-store which can then be triggered by potential, and possibly multiple, victims outside of the initial page the script was injected.
- Reflected: Malicious script is included within the HTTP Request which is then triggered when the victim view the webpage.
When triggered the malicious script can the be used extract information via the DOM or insecure Cookies or it could be used to trick the victim into giving out more information via a fabricated login page or redirect.

In my experience Stored XSS is generally perceived as the more significant threat due to the potential for privilege escalation from the attack within the current system, but if we think about XSS through the lense of protecting everyone Reflected XSS is still a significant threat. Even if the site doesn't store any user data that can be stolen, a clever attacker could exploit a reflected XSS vulnerability, and the sites reputation, to craft a fallacious SSO login redirect (think Google or Facebook) to extract the victims login credentials cause the victim serious harm.   

#### Email spoofing & phishing
Domain-based Message Authentication, Reporting, and Conformance (DMARC), Sender Policy Framework (SPF) and DomainKeys Identified Mail (DKIM) are a series of protocols and mechanisms designed to mitigate and prevent email domain spoofing with the aim of reducing the efficacy of spam and phishing attacks. In a more real-world scenario adding in these protective measure helps to ensure malicious actors can't fake official or valid email headers and exploit the trust the victim may have with a given domain.

This is something that has come up in my short time in Security but the conversations tend to focus on protecting users and internal stakeholders who would be expecting legitimate emails alongside brand reputation concerns. If we look at spoofing through the lense of protecting everyone though, phishing doesn't necessarily just impact users and internal stakeholders. Even if a victim has no direct involvement with a brand, an official looking email with the appearance of legitimate "from" address could be enough to entice them to "sign-up" and give away sensitive information.        

#### Sub-domain takeovers
When I first encountered a sub-domain takeover vulnerability I was unceremoniously giddy with excitement, as a developer at the time I loved to see how seemingly benign little mistakes could be exploited in clever ways. Looking back on it now though, when assessing the impacts and more importantly who could be affected I was only looking at it from an internal business perspective, thinking something along the lines of "Wow, someone could put some pretty questionable content here and make us look really bad". At the time it never crossed my mind about how an attacker could use this as a way to exploit the users of our system and even those who may only have had a passing knowledge of the brand.

To get a clearer picture of what a Sub-domain Takeover is I recommend checking out [HackerOne's post](https://www.hackerone.com/blog/Guide-Subdomain-Takeovers) on the subject.

In general sub-domain takeovers can be quite harmful to a brand, not only with regards to the content that can be displayed, it can also be used to take advantage of other potential vulnerabilities in the form of say cookie scopes and cross site request forgery attacks and like spoofing and phishing provides an attacker a means of leaching off of a brands credibility to trick unsuspecting victims. Like email phishing this doesn't always have to mean users or internal stakeholders, if a brand is well-known, that may be enough to lower the guard of indirect stakeholders resulting in them giving up information that wouldn't give to just anyone and unlike XSS the attacker isn't limited to the constraints of the existing code to inject their malicious scripts and are now free to craft whatever harmful code they can dream up.

#### Security Response Headers
> Security headers are a lot like seatbelts; they aren’t sexy or exciting, they aren’t difficult to use or time consuming, and if you create a habit of using them, they can save you in an emergency” -[@shehackspurple](https://twitter.com/shehackspurple)

I've always found security headers to be fascinating concept, oft forgotten and by themselves not a guarantor of security but like the quote above mentions, without them the impact of other vulnerabilities can be significantly worse. In essence security headers instruct the visitors browser on the actions that are and are not allowed to happen for legitimate uses of the current site. 

There are a number of security headers but for the purposes of this though experiment of cherry-picked a few to see if we can apply the lense of the indirect stakeholder and see how the lack of these headers could impact them. 

##### HTTP Strict Transport Security (HSTS)
The Strict-Transport-Security header instructs the browser to only load the current site via HTTPS and specifies how long this rule is in effect. In conjunction with a valid HTTPS certificate this header aims to mitigate the risk of a man-in-the-middle attack by limiting the possibility of a visitor from unintentionally viewing the HTTP version of a site. For direct users and identified stakeholders this would likely manifest itself when entering the URL into the browser, I know I make this mistake often and don't specify HTTPS. If this were to occur on a public network without the HSTS header the visitor could then have the request intercepted and modified by a malicious attacker.

For indirect stakeholder the attack vector would likely be different. An attacker could attempt to phish the victim with a link specifically targeting the non-secure HTTP version. It's worth pointing the limitation of the HSTS header in that only protects subsequent visits to a website and *does not* by default protect the first request, which would be the more likely scenario for an indirect user. To better manage this for indirect stakeholders it is worthwhile [pre-loading](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security#preloading_strict_transport_security) your HSTS header.


##### Cross-Origin Resource Sharing (CORS)
The Access-Control-Allow-Origin header instructs the browser as to which URI's requests from the current page can be sent to. Ideally from a security perspective you want this to be as restrictive as possible, i.e. please don't put "*" if you can avoid it. Fundamentally misconfiguration of this header impacts users and indirect stakeholders in a similar way, allowing an attacker to exfiltrate data obtained from the page to an external source via an ajax request. If we focus solely on returning users that may have cookie and session data to steal behind authentication, it is possible to miss pages that may not be behind authentication but still vulnerable to reflected XSS that an indirect user would be vulnerable to, such as a corporate site or informational wiki.

##### Content Security Policy (CSP)
The Content-Security-Policy header instructs the browser where resources such as fonts, images, css and javascript for the current page can be loaded from. One of the key attack vectors CSP aims to address is XSS, specifically XSS injected via references to external script files without size limitations, as opposed to injecting raw javascript that is restricted to the maximum length of the url or input field. Like CORS, misconfiguration of this header impacts users and indirect stakeholders in a very similar way, though I would argue indirect stakeholders, with a limited understanding of the normal behaviour of the website are more susceptible to complex XSS attacks that attempt to imitate authentic sites and require loading larger external resources.  

*__note__: Before you grab your pitchforks :P I know I don't include the Strict-Transport-Security header, among others, within this blog. It is a [known issue](https://github.com/isaacs/github/issues/1249) with GitHub pages and I'm looking to migrate away from this as soon as I can*.

### Final thoughts

Thanks for bearing with me while I attempted to explain what is probably a common and well understood concept. In this post I have attempted to show and start a discussion around thinking about security in terms of everyone who could be impacted by a vulnerability and not just in terms of those who are regular users or maintainers. 

While I have attempted to show a number of different types of attack and how they might affect indirect stakeholders I recognise that most of what I have just written boils down to *"Attackers can exploit a victims knowledge and trust of a given brand to phish them into giving up more sensitive data"* and not the overly nuanced thought piece I was hoping for. With that I'd like to hear your input on the topic. Can you think of any scenarios or attacks in which someone who has never visited a particular site could be affected by vulnerability? 

Please leave a comment with your thoughts, i'd love to hear them and start a more in-dept discussion within the Security and Web-Development community.

