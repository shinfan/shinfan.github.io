Google Cloud Java Pub/Sub Client
================================

Java idiomatic client for [Google Cloud Pub/Sub](https://cloud.google.com/pubsub/) services.
This client supports the following Google Cloud Platform services:

- [Publisher API](http://shinfan.github.io/api/index.html?com/google/cloud/pubsub/spi/v1/PublisherApi.html)
- [Subscriber API](http://shinfan.github.io/api/index.html?com/google/cloud/pubsub/spi/v1/SubscriberApi.html)

Prerequisites
----------

- [Java 7](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html)
- [Apache Maven](https://maven.apache.org/)

Installation
----------

To install, it is sufficient to copy this [pom.xml](http://shinfan.github.io/pom.xml) into your project directory.

Authentication
--------------

To authenticate all your API calls, first install and setup the [Google Cloud SDK](https://cloud.google.com/sdk/).
After that is installed, run the following command in your terminal:

```
$ gcloud auth login
```
At this point, you are now authenticated to make calls to Pub/Sub and other Google Cloud services.

Examples
-------------

The [documentation](http://shinfan.github.io/api/index.html?com/google/cloud/pubsub/spi/v1/package-summary.html)
includes simple examples for every API method. Please read it through for more usage samples.


```java
package com.google.pubsub.client.app;

import com.google.cloud.pubsub.spi.v1.PublisherApi;
import com.google.pubsub.v1.Topic;

import java.util.logging.Level;
import java.util.logging.Logger;

public class PubSubSample {

  public static void main(String[] args) throws Exception {
    Logger.getLogger("").setLevel(Level.WARNING);

    try (PublisherApi publisher = PublisherApi.createWithDefaults()) {
      String project = "{Your project name}";
      String topic = "my-first-topic";

      // See API documentation for usage samples of other methods:
      // http://shinfan.github.io/api/index.html?com/google/cloud/pubsub/spi/v1/PublisherApi.html
      String topicName = PublisherApi.formatTopicName(project, topic);
      Topic createdTopic = publisher.createTopic(topicName);
      System.out.println(String.format("created topic with name %s", createdTopic.getName()));
    }
  }
}
```

Place the code above in a file at the location:

```
src/main/java/com/google/pubsub/client/app/PubSubSample.java
```

Execution
--------------

To execute your client app from the command line, run the following commands (which assume that
you put your app in the suggested location):

```
$ mvn package
$ java -XX:-PrintWarnings -jar target/pubsub-client-app-0.0.0-jar-with-dependencies.jar
```

Note that you need to re-run `mvn package` every time you make a change before executing your sample app again.

You can also execute your app in your IDEs. Note there is [a known compatibility issue](https://github.com/trustin/os-maven-plugin#issues-with-eclipse-m2e-or-other-ides)
with Eclipse and maven-os-plugin.


Troubleshooting
-------------

##### "Failed to transport ... {os.detected.classifier} ..." error from my IDE.

It is a known compatibility issue. You can either execute your app in your terminal as a work-around, or fix the problem by following the instructions [here](https://github.com/trustin/os-maven-plugin#issues-with-eclipse-m2e-or-other-ides).

##### "Jetty ALPN/NPN has not been properly configured."

This error is caused by the missing dependency of netty-tcnative-boringssl-static.
There are three options to work-around this issue:

- Execute your app via the command line (see the instructions in the Execution section).
- Follow the instructions [here](https://github.com/trustin/os-maven-plugin#issues-with-eclipse-m2e-or-other-ides)
  to add the plugin to your IDE.
- Manually replace ${os.detected.classifier} in the [pom file](http://shinfan.github.io/pom.xml)
  with the correct value. You can find the correct value by inspecting the output of a call to 'mvn compile'.

##### API call returns status of PERMISSION_DENIED.

Check your project name. You will see this error if anyone has ever created a project with the same name before.
