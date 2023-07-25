# Shibboleth IdP Version 4 Installer

## Overview
The AAF Shibboleth IdP Installer is designed to automate the install of version 4 for the [Shibboleth IdP](https://shibboleth.atlassian.net/wiki/spaces/IDP4/overview) on a dedicated with one of the following supported operating systems;
* Rocky Linux 8 or 9
* CentOS 7, Stream 8 or Stream 9
* RedHAT 7, 8 or 9
* ORACLE Linux 7, 8 or 9
* Ubuntu 20.04 (Focal Fossa) or 22.04 (Jammy Jellyfish)

For a full set of documentation and guidance on upgrading from Shibboleth version 3 please refer to the [AAF IdPv4 Installer Knowledge base](https://aaf.freshdesk.com/support/solutions/articles/19000120020-shibboleth-idpv4-installer).

## Status
This software is actively being developed and maintained. It is ready for use in production environments.

This version now enables the technical connection to eduGAIN, the global federation of federations.

This release is for Shibboleth version 4.3.0 running on Jetty 9.4.50

## License
Apache License Version 2.0, January 2004

# AAF Rapid IdP

Need a managed, secure and highly available cloud Identity Provider solution?

[Contact the AAF about Rapid IdP today.](https://aaf.edu.au/rapid/)

[<img src="https://aaf.edu.au/images/Rapid-IdP.png"  width="500"/>](https://aaf.edu.au/rapid/)

# Juns notes on how to run shibboleth-idp4-installer on CentOS 7 with Vagrant (VirtualBox)
pre: make sure this field is unqiue: ENTITY_ID=https://idp.example.edu/idp/shibbolethv4 in bootstrap-v4.ini:10

0. $ copy the Vagrant file from here: shibboleth-idp4-installer/Vagrantfile
1. $ vagrant up
2. $ vagrant scp ../openldap_server openldap_server
3. $ vagrant ssh
4. $ sudo su - & cd /home/vagrant/openldap_server
5. $ cp ansible_hosts.dist ansible_hosts
6. $ vim ansible_hosts, and add `127.0.0.1 ansible_connection=local` (details here: How to run an Ansible playbook locally
 https://gist.github.com/alces/caa3e7e5f46f9595f715f0f55eef65c1)
7. $ ansible-playbook -i ansible_hosts site.yml
8. $ ldapsearch -H ldap://localhost:389 -D "cn=Manager,dc=example,dc=edu" -W -b "ou=people,dc=example,dc=edu" (password is "password")
9. step 8 should return some results

10. $ vagrant scp . shibboleth-idp4-installer
11. $ vagrant ssh
12. $ sudo su -
13. $ cd /home/vagrant/shibboleth-idp4-installer
14. $ ./bootstrap-v4.sh (fix issues if something breaks)
15. Bootstrap finished!

To make your IdP functional follow these steps:

1. Register your IdP in Federation Registry:
   https://manager.test.aaf.edu.au/federationregistry/registration/idp

   - For 'Step 3. SAML Configuration' we suggest using the "Easy registration
     using defaults" with the value 'https://idp.example.edu'

   - For 'Step 4. Attribute Scope' use 'example.edu'.

   - For 'Step 5. Cryptography'

       * For the 'Signing Certificate' paste the contents of /opt/shibboleth/shibboleth-idp/current/credentials/idp-signing.crt
       * For the 'Backchannel Certificate' paste the contents of /opt/shibboleth/shibboleth-idp/current/credentials/idp-backchannel.crt
       * For the 'Encryption Certificate' paste the contents of /opt/shibboleth/shibboleth-idp/current/credentials/idp-encryption.crt

   - For 'Step 6. Supported Attributes' select the following:
       * auEduPersonSharedToken
       * commonName
       * displayName
       * eduPersonAffiliation
       * eduPersonAssurance
       * eduPersonScopedAffiliation
       * eduPersonTargetedID
       * email
       * organizationName
       * surname
       * givenName
       * eduPersonOrcid
       * eduPersonPrincipalName
       * homeOrganization
       * homeOrganizationType

   After completing this form, you will receive an email from the federation
   indicating your IdP is pending.

   You should now continue with the installation steps documented at
   https://aaf.freshdesk.com/a/solutions/articles/19000119755
