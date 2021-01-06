# Create a "Message of the Day" banner

The Aruba Provision is a little different than Cisco. You need to go into "non-Interactive mode" first.
```
session interactive-mode disable

banner motd "                ******************************************\n                *     Unauthorized access prohibited     *\n                ****************************************** \n                            Customer Name\n\n                You have reached a Customer name device.\n                **************Warning ********************\nThis system is for the use of authorized clients only. Individuals using the\ncomputer network system without authorization, or in excess of their\nauthorization, are subject to having all their activity on this computer \nnetwork system monitored and recorded by system personnel. B To protect the \ncomputer network system from unauthorized use and to ensure the computer \nnetwork systems is functioning properly, system administrators monitor this \nsystem. B Anyone using this computer network system expressly consents to such \nmonitoring and is advised that if such monitoring reveals possible conduct of \ncriminal activity, system personnel may provide the evidence of such activity \nto law enforcement officers.\n \nAccess is restricted to authorized users only.\nUnauthorized access, attempted access, or use of any computing system is a \nviolation of Section 502 of the California Penal code and/or applicable Federal\nLaw, and is subject to prosecution.\n"

session interactive-mode enable

 ```
