# This is the COVID Pass REDCap project and supporting External Modules

REDCap COVID Pass is not a substitute for professional medical advice, diagnosis, or treatment. It is only for screening related to COVID-19.

This screening aid is based on recommendations from the Centers for Disease Control and Prevention, the Massachusetts Department of Public Health, and Partners HealthCare experts. Screening criteria in other states may be different.

Always consult a medical professional when needed.**

**Please read the license agreement before proceeding**.
---

The project contains

***Project Structure***
- XML structure of the REDCap COVID Pass project (REDCap’s ODM file)
You can (and probably should) modify the structure to suit your needs
- A TXT file with the final survey message. This message has some HTML in it that the REDCap user interface usually strips some things, so you have to use the “Source Code” view in the text editor for that field

The XML file should contain the Alerts and Notifications needed – you may need to go through those as well to edit text and URL Links

***The External Modules***
- **PHS LDAP Survey Login** – this is the module that, then enabled will provide a login screen on the public survey URL.
A few things to note here:
  - It only works if your system is already using LDAP / Active Directory. IF you are NOT – let me know and we can think of an alternative
  - You have to do some quick configuration:
  - You can provide Custom Instructions for the login screen – simple HTML accepted here
  - You can provide a custom ERROR message (if login fails) – simple HTML accepted
  - Generate new record after login AND Reserve new record – These are explained on the bottom
  - Then map First Name, Last Name, Username, Email, Display name. NOTE: if you don’t want any field mapped, just leave it empty and it will be ignored

- **PHS Custom Survey Termination Screen**
- **PHS Get Day of Week**
 
--- 
## Notes and General Comments

The “details” for the PHS login:
We ran into an issue whereby people completing surveys at round-about the same second caused records to be double-written. This is a REDCap bug. Simple example of the issue:
- User A and User B open the site at the exact same moment (roughly). They are both assigned record ID 10 by REDcap. Then, because of how short the survey is they submit it almost at the same time. REDCap doesn’t react fast enough and it doubles-up the entries for record 10. I.e. when you look at the field history of a field you see both entries… and the users get mixed-messages in the end.

TO SOLVE THE ABOVE I introduced two checkboxes that do:
- After someone authenticates, we generate a rather overkill record ID – something like XXXXX_XX_XXXXX where X is a random number between 1 and 99999 (and 1 and 99 in the middle) (ex. 2312_22_23125). REDCap allows us to put this even in an auto-numbered project – so it’s not a problem. It will just look very strange in the Dashboard.
- After that random ID is generated, which is all-bug-guaranteed to be unique in the project, we can do one of two things:
  - ONE: (checkbox starting with “IF checked, a new record will be generated after login […]) the record will be created, the user data saved (with incomplete status) and then the user directed to the public survey URL
  - TWO: (checkbox starting with “This works together with the one above[…]”) – both have to be checked in order for this to work – but in this case the record will NOT be created. I.e. the unique ID will be determined, a survey hash will be reserved, but the record itself will NOT be created. The person will then be directed to the dedicated HASH for their record and their information will be piped in the URL (which will populate the necessary fields). IF you go the route of pre-creating the records, I would opt for this one because it won’t create unnecessary records in your project.
 


---
This was developed in:
REDCap 9.1.22
Tested in 9.1.22 and 9.5.23
