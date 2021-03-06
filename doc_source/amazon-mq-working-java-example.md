# Working Example of Using Java Message Service \(JMS\) with ActiveMQ<a name="amazon-mq-working-java-example"></a>

The following example Java code connects to a broker, creates a queue, and sends and receives a message\. For a detailed breakdown and explanation of this code, see [Tutorial: Connecting a Java Application to Your Amazon MQ Broker](amazon-mq-connecting-application.md)\.

## Prerequisites<a name="quick-start-prerequisites"></a>

### Enable Inbound Connections<a name="quick-start-allow-inbound-connections"></a>

1. Sign in to the [Amazon MQ console](https://console.aws.amazon.com/amazon-mq/)\.

1. From the broker list, choose the name of your broker \(for example, **MyBroker**\)\.

1. On the ***MyBroker*** page, in the **Connections** section, note the addresses and ports of the broker's ActiveMQ Web Console URL and wire\-level protocols\.

1. In the **Details** section, under **Security and network**, choose the name of your security group or ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazon-mq/latest/developer-guide/images/amazon-mq-tutorials-broker-details-link.png)\.

   The **Security Groups** page of the EC2 Dashboard is displayed\.

1. From the security group list, choose your security group\.

1. At the bottom of the page, choose **Inbound**, and then choose **Edit**\.

1. In the **Edit inbound rules** dialog box, add a rule for every URL or endpoint that you want to be publicly accessible \(the following example shows how to do this for an ActiveMQ Web Console\)\.

   1. Choose **Add Rule**\.

   1. For **Type**, select **Custom TCP**\.

   1. For **Port Range**, type the ActiveMQ Web Console port \(`8162`\)\.

   1. For **Source**, leave **Custom** selected and then type the IP address of the system that you want to be able to access the ActiveMQ Web Console \(for example, `192.0.2.1`\)\.

   1. Choose **Save**\.

      Your broker can now accept inbound connections\.

### Add Java Dependencies<a name="quick-start-java-dependencies"></a>

Ensure that the `activemq-client.jar` and `activemq-pool.jar` packages are in your Java build class path\.

The following example shows these dependencies in a Maven project `pom.xml` file\.

```
<dependencies>
    <dependency>
        <groupId>org.apache.activemq</groupId>
        <artifactId>activemq-client</artifactId>
        <version>5.15.0</version>
    </dependency>
    <dependency>
        <groupId>org.apache.activemq</groupId>
        <artifactId>activemq-pool</artifactId>
        <version>5.15.0</version>
    </dependency>
</dependencies>
```

For more information about `activemq-client.jar`, see [Initial Configuration](http://activemq.apache.org/initial-configuration.html) in the Apache ActiveMQ documentation\.

## AmazonMQExample\.java<a name="working-java-example"></a>

```
/*
 * Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.
 *
 * Licensed under the Apache License, Version 2.0 (the "License").
 * You may not use this file except in compliance with the License.
 * A copy of the License is located at
 *
 *  https://aws.amazon.com/apache2.0
 *
 * or in the "license" file accompanying this file. This file is distributed
 * on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either
 * express or implied. See the License for the specific language governing
 * permissions and limitations under the License.
 *
 */

import org.apache.activemq.ActiveMQConnectionFactory;
import org.apache.activemq.jms.pool.PooledConnectionFactory;

import javax.jms.*;

public class AmazonMQExample {

    public static void main(String[] args) throws JMSException {

        // Specify the connection parameters.
        final String wireLevelEndpoint = "ssl://b-1234a5b6-78cd-901e-2fgh-3i45j6k178l9-1.mq.us-east-2.amazonaws.com:61617";
        final String activeMqUsername = "MyUsername123";
        final String activeMqPassword = "MyPassword456";

        // Create a connection factory.
        final ActiveMQConnectionFactory connectionFactory = new ActiveMQConnectionFactory(wireLevelEndpoint);

        // Pass the username and password.
        connectionFactory.setUserName(activeMqUsername);
        connectionFactory.setPassword(activeMqPassword);

        // Create a pooled connection factory for the producer.
        final PooledConnectionFactory pooledConnectionFactoryProducer = new PooledConnectionFactory();
        pooledConnectionFactoryProducer.setConnectionFactory(connectionFactory);
        pooledConnectionFactoryProducer.setMaxConnections(10);

        // Establish a connection for the producer.
        final Connection producerConnection = pooledConnectionFactoryProducer.createConnection();
        producerConnection.start();

        // Create a session.
        final Session producerSession = producerConnection.createSession(false, Session.AUTO_ACKNOWLEDGE);

        // Create a queue named "MyQueue".
        final Destination producerDestination = producerSession.createQueue("MyQueue");

        // Create a producer from the session to the queue.
        final MessageProducer producer = producerSession.createProducer(producerDestination);
        producer.setDeliveryMode(DeliveryMode.NON_PERSISTENT);

        // Create a message.
        final String text = "Hello from Amazon MQ!";
        final TextMessage producerMessage = producerSession.createTextMessage(text);

        // Send the message.
        producer.send(producerMessage);
        System.out.println("Message sent.");

        // Clean up the producer.
        producer.close();
        producerSession.close();
        producerConnection.close();

        // Create a pooled connection factory for the consumer.
        final PooledConnectionFactory pooledConnectionFactoryConsumer = new PooledConnectionFactory();
        pooledConnectionFactoryConsumer.setConnectionFactory(connectionFactory);
        pooledConnectionFactoryConsumer.setMaxConnections(10);

        // Establish a connection for the consumer.
        final Connection consumerConnection = pooledConnectionFactoryProducer.createConnection();
        consumerConnection.start();

        // Create a session.
        final Session consumerSession = consumerConnection.createSession(false, Session.AUTO_ACKNOWLEDGE);

        // Create a queue named "MyQueue".
        final Destination consumerDestination = consumerSession.createQueue("MyQueue");

        // Create a message consumer from the session to the queue.
        final MessageConsumer consumer = consumerSession.createConsumer(consumerDestination);

        // Begin to wait for messages.
        final Message consumerMessage = consumer.receive(1000);

        // Receive the message when it arrives.
        final TextMessage consumerTextMessage = (TextMessage) consumerMessage;
        System.out.println("Message received: " + consumerTextMessage.getText());

        // Clean up the consumer.
        consumer.close();
        consumerSession.close();
        consumerConnection.close();
        pooledConnectionFactoryConsumer.stop();
    }

}
```