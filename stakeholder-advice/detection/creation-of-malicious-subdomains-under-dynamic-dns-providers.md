<!--
---
title: "DNS Abuse Detection: Creation of Malicious Subdomains Under Dynamic DNS Providers"
...
-->
# DNS Abuse Detection: Creation of Malicious Subdomains Under Dynamic DNS Providers

## Definition

Malicious registration of domains is the creation of new domains by threat actors online.

This technique is when someone who is going to do a bad thing registers a domain themselves. It is distinguished from an actor stealing, compromising, taking over, or otherwise hijacking DNS resources.

Before attacking a victim, adversaries purchase or create domains from an entity other than a registrar or registry that provides subdomains under domains they own and control. See also the [entry on Wikipedia](https://en.wikipedia.org/wiki/Dynamic_DNS).

[Dynamic DNS](https://www.cloudflare.com/en-gb/learning/dns/glossary/dynamic-dns/) is a legitimate online service for many end users. However, because the IP addresses are dynamic and the dynamic DNS services are free or relatively cheap, dynamic DNS services are commonly used to enable other attacks such as phishing, fast-flux, or malware command and control.

Many online resources, such as file servers, APIs, or web servers, run on internet connections that have their IP addresses changed frequently. This creates a problem if the operators of those endpoints want to give a hosted resource a specific domain name. Dynamic DNS works by keeping the DNS updated with the correct IP address.

For example, if a web administrator is operating a website with a domain name of www.example.com and an IP address of 192.0.2.0, any time a user enters www.example.com into their browser, the DNS will direct them to the server at 192.0.2.0. If the server changes its IP address (e.g. if it was updated by the ISP), a dynamic DNS service can automatically update the DNS record to reflect this change.

Dynamic DNS enables the threat actors to launch phishing, malware etc campaigns without registering for a domain name with an entity covered by, for example, [the ICANN terms of use](https://www.icann.org/en/contracted-parties/accredited-registrars/resources/agreements-and-policies). 

Dynamic DNS providers are one source of domains for threat actors. Like some registrars, dynamic DNS providers often have an API to programmatically generate subdomains to make it easy and efficient to create many domains. Threat actors use this capability to launch campaigns with highly scalable numbers of fully qualified domains.

## Advice

In the case of malicious registration of dynamic DNS domains, the dynamic DNS provider is the authoritative resolver and therefore acts similar to the registry and registrar of the created domain. This means that the same advice is applicable to them as with the creation of the more usual effective second-level domains.

It is more challenging for dynamic DNS providers to perform these anti-abuse checks given their profit model and access to data used in, e.g., KYC checks. However, detecting abuse is an important part of maintaining a usable service. Therefore, detection and mitigation of malicious subdomains should be a regular part of a provider’s operations.

A domain in a dynamic DNS zone is itself a potential indicator of abuse because of the challenges faced by the dynamic DNS provider. Therefore, detection of malicious subdomains falls more heavily on stakeholders other than the provider itself. These stakeholders can look for signals that indicate malicious intent, such as:

* Signs of typosquatting  
* Known malicious infrastructure  
* Use of known bulletproof hosting providers  
* IP address reputation, including history of blocklists mentioning the IP  
* How long ago the domain was registered

This list is illustrative and is not exhaustive.

When a domain is reported as malicious, it should be put on blocklists enforced at the recursive/resolver level. Blocklist information can be shared via cyber threat intelligence channels.

One of the primary purposes of protective DNS is to ingest blocklists and domain information and perform correlations to determine domain reputation scores across indicators of compromise. Protective DNS then will block malicious domains as they are detected.

## Examples

Remote Access Trojans such as LuminosityLink, NJRAT, and ImminentMonitor register and use subdomains under dynamic DNS providers.

* [The DarkComet RAT uses DDNS providers for command and control servers](https://www.hyas.com/blog/the-prevalence-of-darkcomet-in-dynamic-dns).  
* [Scattered Spider: Still Hunting for Victims in 2025](https://www.silentpush.com/blog/scattered-spider-2025/#h-2025-key-findings)  
* [Fraudsters Abuse Dynamic DNS Subdomains For Phishing](https://alluresecurity.com/fraudsters-abuse-dynamic-dns-subdomains/)

Phishing campaigns commonly register and use dynamic DNS providers.

## Potential Resources

* Passive DNS \- For example: After analysis and detecting that a malicious subdomain is pointing at an IP address, searching for the IP address in a Passive DNS dataset can provide other malicious domain names on the same network infrastructure  
* [The Public Suffix List](https://publicsuffix.org/) is the de facto standard list of zones that are managed to directly register domains. A section of the list ([labeled “Private Domains”](https://publicsuffix.org/list/public_suffix_list.dat#:~:text=//%20%3D%3D%3DBEGIN%20PRIVATE%20DOMAINS%3D%3D%3D)) is a list of potential dynamic DNS zones.  
* Some known lists of dynamic DNS providers:  
  * [https://github.com/alexandrosmagos/dyn-dns-list](https://github.com/alexandrosmagos/dyn-dns-list)  
  * [https://help.dyn.com/list-of-dyn-dns-pro-remote-access-domain-names/](https://help.dyn.com/list-of-dyn-dns-pro-remote-access-domain-names/)  
  * [https://gist.github.com/jkeychan/cd6172ec00f76d55476cf2e21a094c74](https://gist.github.com/jkeychan/cd6172ec00f76d55476cf2e21a094c74)

