# WHAT IS A THREAT MODEL?
A threat model is a tool that allows you to be aware of any privacy risks that come with a system or service. It is highly dependent on one's circumstances and what services and systems one uses. For example:

- A woman seeking an abortion in a red state would consider whether or not her area's state or local police employ cell-site simulators, whether or not her social media stores identifying data for longer than necessary, and whether her ISP has a history of working with law enforcement.
- A protester in Iran would consider that surveillance of VoIP, Email, SMS, and other communications are all legal; and that services that look trustworthy still were allowed through the content block.

# HOW DO I MAKE ONE?
https://www.linddun.org/linddun
WIP

# WHAT IS LINDDUN?
LINDDUN Is a system used to evaluate the services in your threat model. It assesses the threat posed by the service by focusing on seven threat categories. Those categories are as follows:

## Linkability
Linkability is the ability to determine if two Items of Interest (IOIs) are from the same user, without necessarily knowing who that user is.

###### Example: 
>If you regularly frequent an internet café at a similar time each day, logs are likely kept of your connections and queries to the café's modem. These separate data points could be linked as coming from the same person.
>
>If your Friend is being investigated, and there's a picture on social media showing the two of you together at the internet café, then an adversary might think to check the café for any logs from when you took your friend there.

Linkable data increases the risk of **identification**, being **singled out**, and having your **behaviors inferred**. 

## Identifiability
Much of the data you put out into the web carries with it clues as to your identity. If that data is linkable to anonymous data, an adversary would then know who the anonymous data belongs to.

###### Example:
>Websites, Ad domains and Content Delivery Networks are all used to uniquely identify your device, allowing an adversary to associate multiple data points directly to your device. And if one of those happens to be a social media account with your real name and face on it, then that data gets linked to *you*.

**Identified data requires stricter security measures, and can result in unawareness and non-compliance issues.**

## Non-repudiation
Data that you cannot deny is dangerous, as you cannot deny something you know, said, or done.

###### Example:
>If you made a post about something, you cannot deny such activities as filing a complaint with a service, accessing a website, or making a post on social media.

*Identifiable* and *Linkable* data increases the risk of Non-repudiation

## Detectability
Detectability is the ability to distinguish whether an IOI exists or not. This alone can lead to Inference.

###### Example:
>If you know that someone has a record in a rehabilitation data center, you can likely infer that they have an addiction.

Detectability can lead to the Deduction and identification of the Data subject, and increases linkability.

## Disclosure of Information
Trust is a tricky thing. Certain services are required to disclose data under certain circumstances. Stay away from these services.

###### Example:
>Internet Service Providers in Iran are required by law to disclose any requested data to the Iranian Government.

## Unawareness
Unawareness occurs when the data subject is unaware of how their data is processed.

###### Example:
>Being unable to access/rectify your own data, or not being notified of 3rd party sharing.

Unawareness leaves you open to having your rights violated.

## Noncompliance
Just because a company says they "only comply with lawful warrants" that doesn't mean they're telling the truth. Nor does it mean that they take every conceivable measure to verify the lawfulness of the warrants they are given. It could mean that the service doesn't comply with data protection principles such as:
- only collecting data for a pre-determined purpose
- collecting the minimum amount of data they need to run the service
- only store data the required amount of time for the service.

###### Example:
>Amazon complies with Law Enforcement in the United states when they request footage taken by RING doorbells.
>
>Before the Snowden Leaks, the FBI would issue Exigent letters that claimed a state of emergency to bypass the process of obtaining a warrant.
