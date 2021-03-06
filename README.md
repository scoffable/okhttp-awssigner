# OkHttp AWSv4 Signer interceptor

  [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.github.esiqveland.okhttp3/aws-interceptor/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.github.esiqveland.okhttp3/aws-interceptor/)
  [![Build Status](https://circleci.com/gh/esiqveland/okhttp-awssigner.svg?&style=shield)](https://circleci.com/gh/esiqveland/okhttp-awssigner)
  [![Coverage Status](https://coveralls.io/repos/github/esiqveland/okhttp-awssigner/badge.svg?branch=master)](https://coveralls.io/github/esiqveland/okhttp-awssigner?branch=master)
  
## What is it?

An interceptor for OkHttpClient from Square to sign requests for AWS services that require signatures.

This project aims to follow the AWSv4 signature spec described here: https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html


## Motivation

I could not find a signing interceptor that did not depend on the entire AWS SDK, so I made this one.
My goal is to reduce the number of dependencies, so as to make it very easy to include in any Java project.


## Usage

Interceptor should be included late in the interceptor chain, so that all headers (including `Host`) has been set by OkHttp,
before signing is invoked.

```xml
<dependency>
    <groupId>com.github.esiqveland.okhttp3</groupId>
    <artifactId>aws-interceptor</artifactId>
    <version>0.9.2</version>
</dependency>
```


```java

String accessKey = "AKIDEXAMPLE";
String secretKey = "wJalrXUtnFEMI/K7MDENG+bPxRfiCYEXAMPLEKEY";
String regionName = "us-east-1";
String serviceName = "iam";


AwsConfiguration cfg = new AwsConfiguration(
        accessKey,
        secretKey,
        regionName,
        serviceName
);


Interceptor awsInterceptor = new AwsSigningInterceptor(cfg);


OkHttpClient client = new OkHttpClient.Builder()
    // NetworkInterceptor is invoked after Host header is set by OkHttpClient, so use this
    .addNetworkInterceptor(awsInterceptor)
    .build();

```

## TODO
 - [X] remove commons-lang dependency
 - [X] add more tests from the examples of signing requests
    - [X] support duplicate header keys: `get-header-key-duplicate`
    - [X] support canonical header value trim: `get-header-value-trim`
    - [X] url with spaces: `get-space`
    - [X] urls with redundant path: `get-slash`
    - [ ] multiline-header value: `get-header-value-multiline`
      - not supported by OkHttp, see test `testMultiLineHeader`
 - [ ] support temporary credentials from AWS Security Token Service? See folder `post-sts-token` for testdata.
 - [ ] provide some form of debug logging?

## Credits

The official AWSv4 signature documentation.

Apache [jclouds](https://github.com/jclouds/jclouds) for query parameter parsing and sorting.

## License

Copyright (c) 2017 Eivind Larsen

This library is licensed under the Apache License, Version 2.0.

See http://www.apache.org/licenses/LICENSE-2.0.html or the LICENSE file in this repository for the full license text.


