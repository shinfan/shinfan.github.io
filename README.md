Google Cloud Java Pub/Sub Client
================================

Java idiomatic client for [Google Cloud Pub/Sub](https://cloud.google.com/pubsub/) services.

-  [Home](http://shinfan.github.io/)
-  [API Documentation](http://shinfan.github.io/api/)

This client supports the following Google Cloud Platform services:

- Publisher API
- Subscriber API

Installation
----------
Currently the latest pub/sub artifact has been published to our internal repo. To install it add
this section to your pom.xml file:
```xml
<repositories>
    <repository>
      <id>gapi</id>
      <name>Gapi Repository</name>
      <layout>default</layout>
      <url>http://104.197.230.53:8081/nexus/content/repositories/snapshots</url>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
    </repository>
</repositories>
```

Next, add this to your pom.xml file
```xml
<dependency>
  <groupId>com.google.cloud</groupId>
  <artifactId>gcloud-java-pubsub</artifactId>
  <version>99.2.1-SNAPSHOT</version>
</dependency>
<dependency>
<groupId>io.netty</groupId>
  <artifactId>netty-tcnative-boringssl-static</artifactId>
  <version>1.1.33.Fork13</version>
  <classifier>${os.detected.classifier}</classifier>
</dependency>
```

Finally, add those extensions:
```
<extension>
  <groupId>kr.motd.maven</groupId>
  <artifactId>os-maven-plugin</artifactId>
  <version>1.4.0.Final</version>
</extension>
```

And plugins:

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-compiler-plugin</artifactId>
  <version>3.5.1</version>
  <configuration>
    <source>1.7</source>
    <target>1.7</target>
  </configuration>
</plugin>
```
```xml
<plugin>
  <groupId>org.codehaus.mojo</groupId>
  <artifactId>exec-maven-plugin</artifactId>
  <version>1.1</version>
  <executions>
    <execution>
    <goals>
    <goal>exec</goal>
    </goals>
    </execution>
  </executions>
  <configuration>
    <mainClass>Your main class</mainClass>
  </configuration>
</plugin>
```

For more details you could see the [sample pom](http://shinfan.github.io/sample.xml)

Authentication
--------------

To authenticate all your API calls, first install and setup the Google Cloud SDK. Once done, you can then run the following command in your terminal:

```
$ gcloud beta auth application-default login
```

At this point you are all set to continue.

Java Versions
-------------

Java 7 or above is required for using this client.


Examples
-------------

```java
public class PubsubShortSample {

  public static void main(String[] args) throws Exception {

    try (PublisherApi publisher = PublisherApi.defaultInstance();
        SubscriberApi subscriber = SubscriberApi.defaultInstance()) {
      int ackDeadlineSeconds = 10;
      int maxPullMessages = 10;
      String project = "my-project";
      String topic = "my-first-topic";
      String subscription = "my-first-subscription";

      String topicName = PublisherApi.formatTopicName(project, topic);
      String subscriptionName = SubscriberApi.formatSubscriptionName(project, subscription);

      Topic createdTopic = publisher.createTopic(topicName);
      System.out.println(String.format("created topic with name %s", createdTopic.getName()));

      PushConfig pushConfig = PushConfig.newBuilder().build();
      Subscription createdSubscription =
          subscriber.createSubscription(
              subscriptionName, topicName, pushConfig, ackDeadlineSeconds);
      System.out.println(
          String.format("created subscription with name %s", createdSubscription.getName()));

      PubsubMessage message =
          PubsubMessage.newBuilder()
              .setData(ByteString.copyFromUtf8("first message to topic"))
              .build();
      PublishResponse response = publisher.publish(topicName, Arrays.asList(message));
      for (String messageId : response.getMessageIdsList()) {
        System.out.println(String.format(">>> Sent message with id %s", messageId));
      }

      PullRequest pullRequest =
          PullRequest.newBuilder()
              .setSubscription(subscriptionName)
              .setMaxMessages(maxPullMessages)
              .build();
      PullResponse pullResponse = subscriber.pull(pullRequest);
      for (ReceivedMessage receivedMessage : pullResponse.getReceivedMessagesList()) {
        System.out.println(
            String.format("<<< Received message: %s", receivedMessage.getMessage().getData()));
      }
    }
  }
}
```

To see more sample usages, please read through the [API reference](http://shinfan.github.io/api/). The documentation for each API method includes simple examples.
