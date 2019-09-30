

#  SSLHandshakeExceptionsun.security.ssl.Alerts in getSSLException!

I got this exception when I am trying to call a third party REST api from by spring boot application using apache HttpClient.

## Steps to fix this.

- Try hitting the endpoint in the browser. If it is a ubuntu or linux machine use `curl -v <endpoint>` to see the SSL is present.
- When I hit from my MAC, I could see the SSL status is OK. Still I am getting this exception when try to make a call from my spring boot application.
- However in my Ubuntu machine, I could see the SSL is not OK. So we decided to download the certificate from the provider and add it to the root certificates.
   > [https://askubuntu.com/questions/645818/how-to-install-certificates-for-command-line](https://askubuntu.com/questions/645818/how-to-install-certificates-for-command-line)

- Now we could see the SSL status is OK in the ubuntu as well but the exception from my application was not resolved.
- Now we decided to add the certificate in the project jre certificates folder. But `JAVA_HOME` is not set in my mac. Now set the `JAVA_HOME` before adding the certificates to the java security folder.
  > To set java_Home in mac:
echo export "JAVA_HOME=\$(/usr/libexec/java_home)" >> ~/.bash_profile

  > https://stackoverflow.com/questions/22842743/setting-java-home-environment-variable-on-mac-osx-10-9

- We then added the downloaded certificate to the jre's security folder.
  > `sudo keytool -import -trustcacerts -file ~/Downloads/DigiCertSHA2ExtendedValidationServerCA.crt -keystore cacerts`

- Now we could able to hit the endpoint from my spring boot application without this excpetion.
