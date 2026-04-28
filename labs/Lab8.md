#### SSL Decryption
1. On Fortiauthenticator navigate to Certificate Management > Certificate Authorities > Local CAs and create new
  - Certificate ID: ssld_fortiacme_net
  - Intermediate CA: int_fortiacme_net
  - Name(CN) : ssld.fortiacme.net
  - Validity period : 365 days

2. Export key and cert
  - set password to : fortinet1

3. On Fortimanager Navagate to Policy & Objects > Advanced > Dynamic Local Certificate ( likely under ... ), Create New
  - Name ssld_cert
  - Create Per Device mapping
    - Mapped Device : FGTBr01
    - Click Import Certificate and Import the CA you created via pks12
    - click ok
    - Local Certificate : Select ssld_fortiacme_net from dropdown
    - Click Ok
  - Click Ok, put in change notes

4. Navigate to Policy & Packages > Security Profiles > SSL/SSH Inspection, Create new
  - Name : SSLD Inspection
  - Inspection Method : Full SSL Inspection
  - CA Certificate : ssld_cert
  - OK and add changenote

5. Navagate to Policy & Objects > Policy Packages > FGTBranch Policy and edit one of your internet boud SSID's ( I suggest 802.1x )
  - SSL/SSH Inspection : SSLD Inspection
  - Ok and changenotes


6. 

Add it to fortimanager and Gate

Add it to a new SSL Inspection profile

apply app virua and SSL Insp

Push

Profit

