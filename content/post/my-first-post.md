---
author: "Ashish Nayyar"
date: 2018-02-10
linktitle: Configure SSL in Wiremock 
title: How to use Wiremock with HTTPS
weight: 10
---


## Introduction

Wiremock is an awesome HTTP stubbing librabry which can be ued to mock end points. It is super simple and can be used as a standalone server. The beauty is that it can be used to stub SSL endpoints making it very versatile and easier to use.

## Step 1 - Generate a self signed certificate
```
keytool.exe -genkey -alias wiremock -keyalg RSA -keysize 1024  -validity 365 -keypass password  -keystore wiremockKS.jks -storepass password
```
This will create a file named ```wiremockKS.jks``` with a self signed certificate. Add the correct hostname for certificate to be uesd on host. For Ex the following certificate can be used for ```localhost``` : 

```
What is your first and last name?
  [Unknown]:  localhost
What is the name of your organizational unit?
  [Unknown]:  some unit
What is the name of your organization?
  [Unknown]:  awesome org
What is the name of your City or Locality?
  [Unknown]:  mel
What is the name of your State or Province?
  [Unknown]:  VIC
What is the two-letter country code for this unit?
  [Unknown]:  AU
Is CN=localhost, OU=some unit, O=awesome org, L=mel, ST=VIC, C=AU correct?
  [no]:  yes
```
## Step 2 -  Export the certificate (later to be imported in Java's default key store)

```
keytool -export -keystore wiremockKS.jks -alias wiremock -file wiremock-pub.cer
```

## Step 3 - Import public key in Java's default truststore 

```
keytool -import -file wiremock-pub.cer -alias wiremock -keystore  %JAVA_HOME%\jre\lib\security\cacerts
``` 
```
Enter keystore password:
Owner: CN=localhost, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=AU
Issuer: CN=localhost, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=AU
Serial number: 4c87a4a0
Valid from: Fri Feb 16 13:30:03 AEDT 2018 until: Sat Feb 16 13:30:03 AEDT 2019
Certificate fingerprints:
         MD5:  76:03:32:8F:89:81:34:D6:78:64:5C:AF:02:6C:24:28
         SHA1: 83:53:20:84:88:1A:F4:0F:67:8A:69:51:6D:A7:FA:63:F6:46:7C:B2
         SHA256: 69:81:EE:87:4F:58:AE:A2:40:B8:CF:A5:41:5E:25:7C:DE:77:42:F0:3C:5E:09:83:8D:EB:05:7A:D5:9D:AF:FF
         Signature algorithm name: SHA256withRSA
         Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 11 5D 9E 06 42 B7 2C 9B   AA 97 A7 C9 3C 9F 55 02  .]..B.,.....<.U.
0010: FA FE 13 12                                        ....
]
]

Trust this certificate? [no]:  yes
Certificate was added to keystore
```

Default password for java's keystore is **changeit**

## Step 5 Start wiremock with the newly generated keystore in Step 1

```
java -jar wiremock-standalone-2.6.0.jar --https-port 9099 -v --https-keystore .\wiremockKS.jks
```
## Step 6 - Test with a client

I tested with quick and dirty Groovy script and voila..got the response
```
String postResult
((HttpURLConnection)new URL('https://localhost:9099/test').openConnection()).with {
    requestMethod = 'POST'
    postResult = inputStream.text
}
```
