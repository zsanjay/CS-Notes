
### OWASP Foundation: 

OWASP is an open community dedicated to enabling organizations to conceive, develop, acquire, operate and maintain applications that can be trusted.

- OWASP Foundation came online December 1, 2001.
- Established as not-for-profit charitable organization on April 21, 2004
- Tools, documents, forums, etc. are free to anyone.

### Core Values

##### 1. **OPEN** : Everything at OWASP is radically transparent, from out finances to our code.

##### 2. **INNOVATION** : OWASP encourages and supports innovation and experiments for solutions to software security challenges.

##### 3. GLOBAL : Anyone around the world is encouraged to participate in the OWASP community.

##### 4. INTEGRITY: OWASP is an honest and truthful, vendor-neutral, global community.


###  OWASP Top Ten

### Background

The OWASP Top 10 is a powerful awareness document for web application security. It represents a broad consensus about the most critical security risks to web applications.
Project members include a variety of security experts from around the world who have shared their expertise to produce this list.


### Data Collection

- Formalized the data collection process at the Open Security Summit in 2017.
- Published a call for data through social media channels.
- Asked for data, with no restriction on CWEs
- Received donated data for over 500,000 applications to make this the largest and most comprehensive application security data set
- Focus on the root cause whenever possible, as it's more logical for providing identification and remediation guidance.


### Methodology

- This installment of the Top 10 is more data-driven then ever but not blindly data-driven.
- Selected eight of the ten categories from contributed data and two categories from the survey.
- Previous data collection efforts were focused on a prescribed subset of approximately 30 CWEs.
- Used OWASP Dependency Check and extracted the CVSS Exploit and Impact scores, grouped by CWEs.
- Use data for Exploitability and (Technical) Impact.


### Common Weakness Enumeration (CWE)

**CWE** is a community-developed list of software and hardware weakness types. It serves as a common language, a measuring stick for security tools, and as a baseline for weakness identification, mitigation, and prevention efforts.


#### Authorization and Access

- Authorization is the process where requests to access a particular resource should be granted or denied.
- Authorization is not authentication! These terms and their definitions are frequently confused
  1.  Authentication is providing and validating identity (who are you?)
  2. Authorization includes the rules that determine what functionality and data the authenticated user may access.

- Applications need access controls to allow users ( with varying privileges) to use the application.
- Administrators need to manage the applications access control rules and the granting of permissions or entitlements to users and other entities.


#### Types of access control

- #### Role-Based Access Control (RBAC) :
  Access decisions are based on an individual's roles and responsibilities within the organization or user base. 

- #### Discretionary Access Control (DAC) :
  A means of restricting access to information. This is based on the identity of users and/or membership in certain groups. Information owner typically has discretion (choice or option) to change access rights at any time.

- ### Mandatory Access Control (MAC)
  Ensures that the enforcement of security policy does not rely on user compliance. MAC secures information by assigning sensitivity labels on information and comparing this to the level of sensitivity a user is operating ar

- ### Permission Based Access Control
  The abstraction of application actions into a set of permissions. A permission may be represented simply as a string-based name --- for example, "READ"


![[Pasted image 20241217121739.png]]
  

## 1. Broken Access Control (A01:2021).

Previously number 5 on the list, broken access control—a weakness that allows an attacker to gain access to user accounts—moved to number 1 for 2021. The attacker in this context can function as a user or as an administrator in the system.  

**Example:** An application allows a primary key to be changed, and when this key is changed to another user’s record, that user’s account can be viewed or modified.

**Solution:** An [interactive application security testing](https://www.blackduck.com/interactive-application-security-testing.html) (IAST) solution, such as Seeker®, can help you effortlessly detect cross-site request forgery or insecure storage of your sensitive data. It also pinpoints any bad or missing logic being used to handle JSON Web Tokens. [Penetration testing](https://www.blackduck.com/services/penetration-testing.html) can serve as a manual supplement to IAST activities, helping to detect unintended access controls. Changes in architecture and design may be warranted to create trust boundaries for data access.

![[Pasted image 20241217122029.png]]


## 2. Cryptographic Failures (A02:2021).

Previously in position number 3 and formerly known as sensitive data exposure, this entry was renamed as cryptographic failures to accurately portray it as a root cause, rather than a symptom. Cryptographic failures occur when important stored or transmitted data (such as a social security number) is compromised.

**Example:** A financial institution fails to adequately protect its sensitive data and becomes an easy target for credit card fraud and identity theft.

**Solution:** Seeker’s checkers can scan for both inadequate encryption strength and weak or hardcoded cryptographic keys, and then identify any broken or risky cryptographic algorithms. The Black Duck® cryptography module surfaces the cryptographic methods used in open source software (OSS) so they can be further evaluated for strength. Both [Coverity® static application security testing (SAST)](https://www.blackduck.com/static-analysis-tools-sast/coverity.html) and [Black Duck software composition analysis (SCA)](https://www.blackduck.com/software-composition-analysis-tools/black-duck-sca.html) have checkers that can provide a “point in time” snapshot at the code and component levels. However, supplementing with IAST is critical for providing continuous monitoring and verification to ensure that sensitive data isn’t leaked during integrated testing with other internal and external software components.

## 3. Injection (A03:2021).

Injection moves down from number 1 to number 3, and cross-site scripting is now considered part of this category. Essentially, a code injection occurs when invalid data is sent by an attacker into a web application in order to make the application do something it was not designed to do.

**Example:** An application uses untrusted data when constructing a vulnerable SQL call.

**Solution:** Including SAST and IAST tools in your continuous integration / continuous delivery ([CI/CD](https://www.blackduck.com/glossary/what-is-cicd.html)) pipeline helps identify injection flaws both at the static code level and dynamically during application runtime testing. Modern application security testing (AST) tools such as Seeker can help secure the software application during the various test stages and check for a variety of injection attacks (in addition to SQL injections). For example, it can identify NoSQL injections, command injections, LDAP injections, template injections, and log injections. Seeker is the first tool to provide a new, dedicated checker designed to specifically detect [Log4Shell vulnerabilities](https://www.blackduck.com/blog/mitigating-impact-of-log4j-log4shell.html), determine how Log4J is configured, test how it actually behaves, and validate (or invalidate) those findings with its patented Active Verification engine.


## Insecure Design (A04:2021).

Insecure design is a new category for 2021 that focuses on risks related to design flaws. As organizations continue to “shift left,” threat modeling, secure design patterns and principles, and reference architectures are not enough.

**Example:** A movie theater chain that allows group booking discounts requires a deposit for groups of more than 15 people. Attackers threat model this flow to see if they can book hundreds of seats across various theaters in the chain, thereby causing thousands of dollars in lost income.

**Solution:** Seeker IAST detects vulnerabilities and exposes all the inbound and outbound API, services, and function calls in highly complex web, cloud, and [microservices-based applications](https://www.blackduck.com/glossary/what-are-microservices.html). By providing a visual map of the data flow and endpoints involved, any weaknesses in the design of the app design are made clear, aiding in pen testing and [threat modeling](https://www.blackduck.com/glossary/what-is-threat-modeling.html) efforts.


## 5. Security Misconfiguration (A05:2021).

The former external entities category is now part of this risk category, which moves up from the number 6 spot. Security misconfigurations are design or configuration weaknesses that result from a configuration error or shortcoming.

**Example:** A default account and its original password are still enabled, making the system vulnerable to exploit.

**Solution:** Solutions like Coverity SAST include a checker that identifies the information exposure available through an error message. Dynamic tools like Seeker IAST can detect information disclosure and inappropriate HTTP header configurations during application runtime testing.

## 6. Vulnerable and Outdated Components (A06:2021).

This category moves up from number 9 and relates to components that pose both known and potential security risks, rather than just the former. Components with known vulnerabilities, such as CVEs, should be identified and patched, whereas stale or malicious components should be evaluated for viability and the risk they may introduce.

**Example:** Due to the volume of components used in development, a development team might not know or understand all the components used in their application, and some of those components might be out-of-date and therefore vulnerable to attack.

**Solution:** [Software composition analysis (SCA) tools](https://www.blackduck.com/software-composition-analysis-tools.html) like Black Duck can be used alongside static analysis and IAST to identify and detect outdated and insecure components in an application. IAST and SCA work well together, providing insight into how vulnerable or outdated components are actually being used. Seeker IAST and Black Duck SCA together go beyond identifying a vulnerable component, uncovering details like whether that component is currently loaded by an application under test. Additionally, metrics such as developer activity, contributor reputation, and version history can give users an idea of the potential risk that a stale or malicious component may pose.

## 7. Identification and Authentication Failures (A07:2021).

Previously known as broken authentication, this entry has moved down from number 2 and now includes CWEs related to identification failures. Specifically, functions related to authentication and session management, when implemented incorrectly, allow attackers to compromise passwords, keywords, and sessions, which can lead to stolen user identity and more.

**Example:** A web application allows the use of weak or easy-to-guess passwords (i.e., “password1”).

**Solution:** Multifactor authentication can help reduce the risk of compromised accounts, and automated static analysis is highly useful in finding such flaws, while manual static analysis can add strength when evaluating custom authentication schemes. Coverity SAST includes a checker that specifically identifies broken authentication vulnerabilities. Seeker IAST can detect hardcoded passwords and credentials, as well improper authentication or missing critical steps in authentication.


## 8. Software and Data Integrity Failures (A08:2021).

This is a new category for 2021 that focuses on software updates, critical data, and CI/CD pipelines used without verifying integrity. Also now included in this entry, insecure deserialization is a deserialization flaw that allows an attacker to remotely execute code in the system.

**Example:** An application deserializes attacker-supplied hostile objects, opening itself to vulnerability.

**Solution:** [Application security tools](https://www.blackduck.com/) help detect deserialization flaws, and penetration testing can validate the problem. Seeker IAST can also check for unsafe deserialization and help detect insecure redirects or any tampering with token access algorithms.

## 9. Security Logging and Monitoring Failures (A09:2021).

Formerly known as insufficient logging and monitoring, this entry has moved up from number 10 and has been expanded to include more types of failures. Logging and monitoring are activities that should be performed on a website frequently—failure to do so leaves a site vulnerable to more severe compromising activities.

**Example:** Events that can be audited, like logins, failed logins, and other important activities, are not logged, leading to a vulnerable application.

**Solution:** After performing penetration testing, developers can study test logs to identify possible shortcomings and vulnerabilities. Coverity SAST and Seeker IAST can help identify unlogged security exceptions.

## 10. Server-Side Request Forgery (A10:2021).

A new category this year, a server-side request forgery (SSRF) can happen when a web application fetches a remote resource without validating the user-supplied URL. This allows an attacker to make the application send a crafted request to an unexpected destination, even when the system is protected by a firewall, VPN, or additional network access control list. The severity and incidence of SSRF attacks are increasing due to cloud services and the increased complexity of architectures.

**Example:** If a network architecture is unsegmented, attackers can use connection results or elapsed time to connect or reject SSRF payload connections to map out internal networks and determine if ports are open or closed on internal servers.

**Solution:** Seeker is one of the modern AST tools that can track, monitor, and detect SSRF without the need for additional scanning and triaging. Due to its advanced instrumentation and agent-based technology, Seeker can pick up any potential exploits from SSRF as well.


###  References

https://www.blackduck.com/glossary/what-is-owasp-top-10.html#G

