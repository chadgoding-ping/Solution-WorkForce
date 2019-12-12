## Active Directory configuration
It's pretty difficult to demonstrate Ping's WF capabilities without an AD environment. Customers are so invested in the Microsoft stack that they are understandably concerned that non-MS services play properly with that. 
  
This Server Profile includes a sequence of Powershell commands that can be executed to create a new ActiveDirectory Forest with a Domain Controller and a Certificate Authority.
    
The first collection is to create the AD Forest, Add AD Domain Services (i.e. a Domain Controller), DNS Server and adds a set of users:  
* `cn=pingfederate,ou=ServiceAccounts,...` -- Used by PF for it's Admin calls to AD
* `cn=pinguser1,ou=UserAccounts,...` -- User accounts to authenticate with
  
Passwords for Users are `2FederateM0re`

Note: these users are created with the commands in:  
* [11-Create-PingServiceUser.ps](ActiveDirectory/00-Install-and-Configure-Domain-Controller/11-Create-PingServiceUser.ps)
* [15-Create-PingUsers.ps](ActiveDirectory/00-Install-and-Configure-Domain-Controller/15-Create-PingUsers.ps)
  
The Certificate Authority is used by the Domain Controller to get certificates used by LDAPS, and future use cases for Win10 Devices or User Certs (i.e. Smart Cards)

An implementation of ActiveDirectory with these commands has been performed:  

* Forest Name: `ping-demos.com`
* Root DN: `dc=ping-demos,dc=com`
* NetBIOS Name: `pingdemos`
* Domain Controller DNS: `int-ad.solution-wf.ping-eng.com`

**Service Principal Name**  
A SPN has been added to the `pingfederate` account to allow Kerberos from `workforce-pf.ping-demos.com`

This is configured in:
* [11-Create-PingServiceUser.ps](ActiveDirectory/00-Install-and-Configure-Domain-Controller/11-Create-PingServiceUser.ps)

**LDAPS Certificate for PingFed**  
In order for PingFed to be able to connect over LDAPS, you will need to export the certificate.  

I use Apache Directory Studio to connect via LDAPS, store the certificate for this Session, and Export it from there (Preferences --> Connections --> Certificate Validation --> Temporary Trusted).   
  
The certificate should be the *untitled* one  
  
Export this as a `.DER` format and you can import into PingFed.  

If you are connecting to this ActiveDirectory instance, the cert has already been exported and stored here:  
* [AD LDAPS Cert](ActiveDirectory/solution-wf-ad-cert.crt)
