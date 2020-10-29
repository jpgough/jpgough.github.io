---
layout: post
title: "JDConf Day 3: Keeping Your Java Applications Secure"
date: 2020-10-29
---

#### Keeping Your Java Applications Secure - [@speakjava](https://twitter.com/speakjava)

##### OpenJDK Release Model

We are seeing that the ability to add new features to Java in a controlled way is a real benefit to the Java ecosystem.
Incubator and preview features are a really powerful way to outreach to users.
OpenJDK development is now much more agile and features are only included when ready. 
Rather than having releases driven by features (which historically caused delays).

Long term support for all releases would just not be practical. 
This only applies to binary distributions of OpenJDK. 

![Speak Java](/assets/images/blog/jdconf/speakjava.png)

JDK 8 was classified as Long Term Support Release to January 2019 for commercial users.
JDK 11 was the next long term release, Oracle introduced a new licence for Java 11.
You can use OracleJDK for personal use, oracle product, oracle cloud infrastructure - otherwise you need a Java SE licence. 
JDK8u202 and earlier is indefinitely for free, JDK8u210 and beyond are subject to the new licence.

##### Vulnerabilities 

These are classified as:

* CVE Common vulnerabilities and exposures
* CVSS common vulnerabilities scoring system, each CVE has a CVSS

April 2019 (above the line) has a significant vulnerability that could impact your system on windows. 
There are a fair number of medium vulnerabilities, it is important to keep your platform up to date to ensure security.  

![How Important are Updates](/assets/images/blog/jdconf/java-updates.png)

##### Standards Conformance 

How do you know what you are getting as a binary distribution matches a valid Java release. 
This is driven by the JSR which is governed by the JCP. 
The JSR consists of three parts:
* The specification
* The reference implementation (proving the specification is possible)
* The TCK (Test Compatibility Kit) has over 150,000 tests and is used to test Java binary matches the standard

Zulu (produced for Microsoft Azure Cloud) is a compliant TCK release. 
Azul support a wide variety of JVM releases, back to JDK 6. 
Azul provides two versions of each update, Security only update (CPU) and a full update (PSU).
CPU allows for a quick roll out with minimum testing.
PSU is the full update, security testing, bug fixes and everything. 
As shown in the slide below CPU releases allow you to remain secure and carry significantly less risk. 

![Security Updates Carry Less Risk](/assets/images/blog/jdconf/cpu-vs-psu.png)