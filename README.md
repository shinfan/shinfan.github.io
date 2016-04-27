Google Cloud Java Pub/Sub Client
================================

Java idiomatic client for [Google Cloud Pub/Sub](https://cloud.google.com/pubsub/) services.

-  [Home](http://shinfan.github.io/)
-  [API Documentation](http://shinfan.github.io/api/)

This client supports the following Google Cloud Platform services:

- [Publisher API](http://shinfan.github.io/api/index.html?com/google/cloud/pubsub/spi/v1/PublisherApi.html)
- [Subscriber API](http://shinfan.github.io/api/index.html?com/google/cloud/pubsub/spi/v1/SubscriberApi.html)

Prerequisites
----------

- [Java 7](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html)
- [Apache Maven](https://maven.apache.org/)

Installation
----------
Currently the latest pub/sub artifact has been published to our internal repo.
To install it, use this [pom file](http://shinfan.github.io/pom.xml) as a quick starting point.
##### Note
Replace "{ Main class }" with your own main class path.

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

```java
import com.google.cloud.pubsub.spi.v1.PublisherApi;
import com.google.pubsub.v1.Topic;

public class PubSubSample {

  public static void main(String[] args) throws Exception {

    try (PublisherApi publisher = PublisherApi.defaultInstance()) {
      String project = "{Your project name}";
      String topic = "my-first-topic";

      String topicName = PublisherApi.formatTopicName(project, topic);
      Topic createdTopic = publisher.createTopic(topicName);
      System.out.println(String.format("created topic with name %s", createdTopic.getName()));
    }
  }
}
```

To see more sample usages, please read through the [API reference](http://shinfan.github.io/api/index.html?com/google/cloud/pubsub/spi/v1/package-summary.html).

The documentation for each API method includes simple examples.


Execution
--------------

To execute your client app from the command line, you need to add your main class path to the pom file
(see the Installation section).
Once done, run the following commands:

```
$ mvn compile
$ mvn -e exec:java
```

Note that you need to re-run `mvn compile` every time you make a change before executing `mvn -e exec:java` again.

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
