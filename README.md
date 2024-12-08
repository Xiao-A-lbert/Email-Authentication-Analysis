# Email Authentication Analysis

<h2>Description</h2>
In this email authentication analysis, I exam a suspicious email's spf check, sender domain, and DKIM public key using various tools. 
<br />


<h2>Languages and Utilities Used</h2>

- <b>Linux CLI</b>
- <b>Sublime Text Editor</b>
- <b>mxtoolbox</b>
- <b>message header analyzer tool</b>

<h2>Environments Used </h2>

- <b>Unbuntu</b> 

<br />
<br />
Suspicious email wtih a sense of urgency claiming their domain to be namecheap.com. 

![1) phishing email](https://github.com/user-attachments/assets/d13836d2-995e-4dd5-a54c-5f3365e8175a)

<br />
<br />
Examining the received header, I identified the sender's mail server (o22.mailservice.namecheap.com), their ip of 149.72.142.11, and the time + date of when this email was sent. I also noticed that they passed google's spf check.   

![2)tcp connection made from sus mail server to google passing google spf](https://github.com/user-attachments/assets/588718e8-a4f9-4f1e-b2a1-2650cf2e6ada)

<br />
<br />
To manually check the spf records, I used a dig command to look for text records from the sender "mailserviceemailout1.namecheck.com" and grepped out spf. The results showed all records including records from "sendgrid.net." 
Another dig into "sendgrid.net" showed a range of authorized ip senders including 149.72.0.0/16. 
In summary, the email originiated from o22.mailservice.namecheap.com with an ip of 149.72.142.11 which was authorized to send emails from mailserviceemailout1.namecheap.com.
The spd record for mailserviceemailout1 includes sendgrid.net, indicating emails can be sent through sendgrid's infrastructure. 
Google's email server verified this and allowed the email to pass through.      

![3) dig txt records from email server grep spf, dig sendgrid to find ip address](https://github.com/user-attachments/assets/987c9e6f-738a-4c48-84ed-b8c9c3c2e0d3)

<br />
<br />
Examining the DKIM-Signature header, we can see the domain and selector. I used these two variables to find the associated public key.   

![4)check domain and selector used to locate public key](https://github.com/user-attachments/assets/37fd4fc5-46eb-46f2-a210-7368d0c06f80)

<br />
<br />
Here I used nslookup to find the public key for namecheap.com. from sendgrid.net 

![5) using nslookup in terminal with syntaxs to find public key for namecheap](https://github.com/user-attachments/assets/b77fd57e-b363-475a-8e7b-205817907f07)

<br />
<br />
Using a took like mxtoolbox also showed me the public key. 

![6)mxtool domain n selector lookup comfirms public key](https://github.com/user-attachments/assets/2791b3a0-2098-4a54-8fc8-568a54e58ff6)

<br />
<br />
Using mxtoolbox's email header analyzer tool, it showed all of the DKIM checks that were performed. Ignore the body hash value not matching for integrity in this example email header. 

![7) mxtools email header analyzer dkim check ](https://github.com/user-attachments/assets/89756550-ac39-4cc7-b574-696aa9a971f1)

<br />
<br />
Examining the authentication-results header for the passing dkim for namecheap.com & sendgrid, dmarc pass, and google's spf pass matching my own authentication of spf and dkim allows me to conclude that this email is most likely not a phishing attempt but futher analysis is needed beyond an authentication check. I do understand that passing these authentication checks is a positive but does not guarantee that an email is safe or genuine since attackers can register a lookalike domain and pass these checks themselves. 

![8) after analysis, this email is legitimate](https://github.com/user-attachments/assets/02cc61e2-b368-46ec-a5b3-fe2204031843)
<br />
<br />
