---
title: AMQP 1.0 gebruiken met de Java Service Bus-API | Microsoft Docs
description: Het gebruik van de Java Message Service (JMS) met Azure Service Bus en Message Queuing-Protodol AMQP 1.0 (Advanced).
services: service-bus-messaging
documentationcenter: java
author: spelluru
manager: timlt
editor: ''
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2018
ms.author: spelluru
ms.openlocfilehash: 9a223c67e0c1f2e71d2953be63924a114e7420af
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018227"
---
# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>De Java Message Service (JMS) API gebruiken met Service Bus en AMQP 1.0
Het Advanced Message Queuing Protocol (AMQP 1.0) is een efficiënte, betrouwbare, wire-level berichtenprotocol dat u gebruiken kunt om robuuste, platformonafhankelijke berichten toepassingen te bouwen.

Ondersteuning voor AMQP 1.0 in Service Bus betekent dat u kunt het in de wachtrij gebruiken en publiceren/abonneren brokered messaging-onderdelen uit een scala aan platformen met behulp van een doeltreffende binaire-protocol. Bovendien kunt u toepassingen bestaat uit onderdelen die zijn gebouwd met behulp van een combinatie van programmeertalen, frameworks en besturingssystemen bouwen.

Dit artikel wordt uitgelegd hoe u Service Bus-berichtfuncties (wachtrijen en onderwerpen publiceren/abonneren op onderwerpen) uit Java-toepassingen met behulp van de populaire Java Message Service (JMS) API standaard. Er is een [companion-artikel](service-bus-amqp-dotnet.md) waarin wordt uitgelegd hoe u doet hetzelfde met behulp van de Service Bus .NET API. U kunt deze twee handleidingen samen gebruiken voor meer informatie over platformonafhankelijke berichten met behulp van AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Aan de slag met Service Bus
Deze handleiding wordt ervan uitgegaan dat u al een Service Bus-naamruimte met een wachtrij met de naam **basicqueue**. Als u dit niet doet, dan u kunt [maken van de naamruimte en wachtrij](service-bus-create-namespace-portal.md) met behulp van de [Azure-portal](https://portal.azure.com). Zie voor meer informatie over het maken van Service Bus-naamruimten en -wachtrijen [aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Gepartitioneerde wachtrijen en onderwerpen bieden ook ondersteuning voor AMQP. Zie voor meer informatie, [gepartitioneerde berichtentiteiten](service-bus-partitioning.md) en [AMQP 1.0-ondersteuning voor Service Bus gepartitioneerde wachtrijen en onderwerpen](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-the-amqp-10-jms-client-library"></a>De clientbibliotheek van AMQP 1.0 JMS downloaden
Voor informatie over het downloaden van de meest recente versie van de Apache Qpid JMS AMQP 1.0-clientbibliotheek, gaat u naar [ https://qpid.apache.org/download.html ](https://qpid.apache.org/download.html).

U moet de volgende vier JAR-bestanden uit het archief van de Apache Qpid JMS AMQP 1.0-distributiepunt toevoegen aan het KLASSEPAD Java bij het bouwen en uitvoeren van JMS toepassingen met Service Bus:

* geronimo jms\_1.1\_spec 1.0.jar
* qpid-jms - client-[versie] JAR

> ! [OPMERKING] JMS JAR namen en versies is mogelijk gewijzigd. Zie voor meer informatie, [Qpid JMS - AMQP 1.0](https://qpid.apache.org/maven.html#qpid-jms-amqp-10).

## <a name="coding-java-applications"></a>Codering van Java-toepassingen
### <a name="java-naming-and-directory-interface-jndi"></a>Naamgeving van Java en Directory-Interface (JNDI)
JMS gebruikt de naamgeving van Java en Directory-Interface (JNDI) voor een scheiding tussen de namen van logische en fysieke. Twee typen JMS objecten worden opgelost met JNDI: ConnectionFactory- en doelserver. JNDI maakt gebruik van een providermodel waarin u andere directoryservices voor het afhandelen van de naam van het probleem zou moeten rechten kunt aansluiten. De Apache Qpid JMS AMQP 1.0-bibliotheek wordt geleverd met een eenvoudige eigenschappen bestandsgebaseerde JNDI Provider die is geconfigureerd met behulp van een eigenschappenbestand van de volgende indeling:

```
# servicebus.properties - sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="setup-jndi-context-and-configure-the-connectionfactory"></a>JNDI context installeren en configureren van de ConnectionFactory

De **ConnectionString** waarnaar wordt verwezen in de versie in de 'gedeeld toegangsbeleid' in de [Azure Portal](https://portal.azure.com) onder **Primary Connection String**
```
// The connection string builder is the only part of the azure-servicebus SDK library
// we use in this JMS sample and for the purpose of robustly parsing the Service Bus 
// connection string. 
ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);
        
// set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");

// Look up queue
Destination queue = (Destination) context.lookup("QUEUE");
```

#### <a name="configure-producer-and-consumer-destination-queues"></a>Producent en consument bestemming wachtrijen configureren
De vermelding die wordt gebruikt voor het definiëren van een bestemming in de Qpid eigenschappen bestand JNDI provider is de volgende indeling hebben:

De doelwachtrij maken voor de producent- 
```
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create Producer
MessageProducer producer = session.createProducer(queue);
```

Wachtrijen maken voor de consument- 
```
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create Consumer
MessageConsumer consumer = session.createConsumer(queue);
```

### <a name="write-the-jms-application"></a>De toepassing JMS schrijven
Er zijn geen speciale API's of -opties die vereist als u de JMS voor Service Bus. Er zijn echter enkele beperkingen die later worden behandeld. Net als bij elke toepassing JMS eerst te beginnen de vereiste configuratie van de omgeving JNDI, moeten kunnen omzetten van een **ConnectionFactory** en bestemmingen.

#### <a name="configure-the-jndi-initialcontext"></a>De InitialContext JNDI configureren
De JNDI-omgeving is geconfigureerd door een hashtabel van configuratie-informatie wordt doorgegeven aan de constructor van de klasse javax.naming.InitialContext. De twee vereiste elementen in de hashtabel zijn de naam van de klasse van de eerste Context Factory en de URL van de Provider. De volgende code laat zien hoe het configureren van de omgeving JNDI voor het gebruik van de eigenschappen op basis van bestanden JNDI Provider met een eigenschappenbestand met de naam Qpid **servicebus.properties**.

```java
// set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + \
"?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Een eenvoudige JMS-toepassing met behulp van een Service Bus-wachtrij
Het volgende voorbeeldprogramma JMS TextMessages verzendt naar een Service Bus-wachtrij met de logische naam JNDI van wachtrij en ontvangt de berichten weer.

U kunt alle toegang tot alle de bron-code en configuratie-informatie van de [Azure Service Bus-voorbeelden JMS wachtrij snel starten](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/qpid-jms-client/JmsQueueQuickstart)

```java
// Copyright (c) Microsoft. All rights reserved.
// Licensed under the MIT license. See LICENSE file in the project root for full license information.

package com.microsoft.azure.servicebus.samples.jmsqueuequickstart;

import com.microsoft.azure.servicebus.primitives.ConnectionStringBuilder;
import org.apache.commons.cli.*;
import org.apache.log4j.*;

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.util.Hashtable;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.function.Function;

/**
 * This sample demonstrates how to send messages from a JMS Queue producer into
 * an Azure Service Bus Queue, and receive them with a JMS message consumer.
 * JMS Queue. 
 */
public class JmsQueueQuickstart {

    // Number of messages to send
    private static int totalSend = 10;
    //Tracking counter for how many messages have been received; used as termination condition
    private static AtomicInteger totalReceived = new AtomicInteger(0);
    // log4j logger 
    private static Logger logger = Logger.getRootLogger();

    public void run(String connectionString) throws Exception {

        // The connection string builder is the only part of the azure-servicebus SDK library
        // we use in this JMS sample and for the purpose of robustly parsing the Service Bus 
        // connection string. 
        ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);
        
        // set up JNDI context
        Hashtable<String, String> hashtable = new Hashtable<>();
        hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
        hashtable.put("queue.QUEUE", "BasicQueue");
        hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
        Context context = new InitialContext(hashtable);
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        
        // Look up queue
        Destination queue = (Destination) context.lookup("QUEUE");

        // we create a scope here so we can use the same set of local variables cleanly 
        // again to show the receive side separately with minimal clutter
        {
            // Create Connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            // Create Session, no transaction, client ack
            Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

            // Create producer
            MessageProducer producer = session.createProducer(queue);

            // Send messages
            for (int i = 0; i < totalSend; i++) {
                BytesMessage message = session.createBytesMessage();
                message.writeBytes(String.valueOf(i).getBytes());
                producer.send(message);
                System.out.printf("Sent message %d.\n", i + 1);
            }

            producer.close();
            session.close();
            connection.stop();
            connection.close();
        }

        {
            // Create Connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            connection.start();
            // Create Session, no transaction, client ack
            Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            // Create consumer
            MessageConsumer consumer = session.createConsumer(queue);
            // create a listener callback to receive the messages
            consumer.setMessageListener(message -> {
                try {
                    // receives message is passed to callback
                    System.out.printf("Received message %d with sq#: %s\n",
                            totalReceived.incrementAndGet(), // increments the tracking counter
                            message.getJMSMessageID());
                    message.acknowledge();
                } catch (Exception e) {
                    logger.error(e);
                }
            });

            // wait on the main thread until all sent messages have been received
            while (totalReceived.get() < totalSend) {
                Thread.sleep(1000);
            }
            consumer.close();
            session.close();
            connection.stop();
            connection.close();
        }

        System.out.printf("Received all messages, exiting the sample.\n");
        System.out.printf("Closing queue client.\n");
    }

    public static void main(String[] args) {

        System.exit(runApp(args, (connectionString) -> {
            JmsQueueQuickstart app = new JmsQueueQuickstart();
            try {
                app.run(connectionString);
                return 0;
            } catch (Exception e) {
                System.out.printf("%s", e.toString());
                return 1;
            }
        }));
    }

    static final String SB_SAMPLES_CONNECTIONSTRING = "SB_SAMPLES_CONNECTIONSTRING";

    public static int runApp(String[] args, Function<String, Integer> run) {
        try {

            String connectionString = null;

            // parse connection string from command line
            Options options = new Options();
            options.addOption(new Option("c", true, "Connection string"));
            CommandLineParser clp = new DefaultParser();
            CommandLine cl = clp.parse(options, args);
            if (cl.getOptionValue("c") != null) {
                connectionString = cl.getOptionValue("c");
            }

            // get overrides from the environment
            String env = System.getenv(SB_SAMPLES_CONNECTIONSTRING);
            if (env != null) {
                connectionString = env;
            }

            if (connectionString == null) {
                HelpFormatter formatter = new HelpFormatter();
                formatter.printHelp("run jar with", "", options, "", true);
                return 2;
            }
            return run.apply(connectionString);
        } catch (Exception e) {
            System.out.printf("%s", e.toString());
            return 3;
        }
    }
}
```

### <a name="run-the-application"></a>De toepassing uitvoeren
Geeft de **Connection String** van het beleid voor gedeelde controleren zodat ze de toepassing uit te voeren.
Hieronder volgt de uitvoer van het formulier door de toepassing wordt uitgevoerd:

```
> mvn clean package
>java -jar ./target/jmsqueuequickstart-1.0.0-jar-with-dependencies.jar -c "<CONNECTION_STRING>"

Sent message 1.
Sent message 2.
Sent message 3.
Sent message 4.
Sent message 5.
Sent message 6.
Sent message 7.
Sent message 8.
Sent message 9.
Sent message 10.
Received message 1 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-1
Received message 2 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-2
Received message 3 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-3
Received message 4 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-4
Received message 5 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-5
Received message 6 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-6
Received message 7 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-7
Received message 8 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-8
Received message 9 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-9
Received message 10 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-10
Received all messages, exiting the sample.
Closing queue client.

```

## <a name="amqp-disposition-and-service-bus-operation-mapping"></a>AMQP toestand en toewijzing van Service Bus-bewerking
Hier ziet u hoe een AMQP-bestemming wordt omgezet in een Service Bus-bewerking:

```
ACCEPTED = 1; -> Complete()
REJECTED = 2; -> DeadLetter()
RELEASED = 3; (just unlock the message in service bus, will then get redelivered)
MODIFIED_FAILED = 4; -> Abandon() which increases delivery count
MODIFIED_FAILED_UNDELIVERABLE = 5; -> Defer()
```


## <a name="unsupported-features-and-restrictions"></a>Niet-ondersteunde functies en -beperkingen
De volgende beperkingen zijn bij het gebruik van JMS via AMQP 1.0 met Service Bus, namelijk:

* Slechts één **MessageProducer** of **MessageConsumer** is toegestaan **sessie**. Als u wilt maken van meerdere **MessageProducers** of **MessageConsumers** in een toepassing, maakt u een toegewezen **sessie** voor elk van deze.
* Vluchtige onderwerpabonnementen worden momenteel niet ondersteund.
* **MessageSelectors** worden momenteel niet ondersteund.
* Transactionele sessies en gedistribueerde transacties worden niet ondersteund.

Bovendien Azure Service Bus splitst het besturingselement vlak van het gegevensvlak en daarom biedt geen ondersteuning voor diverse JMS van dynamische topologie functies:

| Niet-ondersteunde methode          | vervangen                                                                             |
|-----------------------------|------------------------------------------------------------------------------------------|
| createDurableSubscriber     | maken van een onderwerpabonnement overzetten van de bericht-kiezer                                 |
| createDurableConsumer       | maken van een onderwerpabonnement overzetten van de bericht-kiezer                                 |
| createSharedConsumer        | Service Bus-onderwerpen zijn altijd deelbaar, Zie hierboven                                       |
| createSharedDurableConsumer | Service Bus-onderwerpen zijn altijd deelbaar, Zie hierboven                                       |
| createTemporaryTopic        | maken van een onderwerp via management API/tools/portal met *AutoDeleteOnIdle* ingesteld op een vervalperiode |
| createTopic                 | een onderwerp via management tools-API-portal maken                                           |
| afmelden                 | verwijderen van het onderwerp management tools-API-portal                                             |
| createBrowser               | niet ondersteund. Gebruik de Peek()-functionaliteit van de Service Bus-API                         |
| createQueue                 | een wachtrij via management tools-API-portal maken                                           | 
| createTemporaryQueue        | maken van een wachtrij via management API/tools/portal met *AutoDeleteOnIdle* ingesteld op een vervalperiode |

## <a name="summary"></a>Samenvatting
In deze gebruiksaanwijzing laten zien hoe u Service Bus brokered messaging functies (wachtrijen en onderwerpen publiceren/abonneren op onderwerpen) uit Java met behulp van de populaire JMS API en de AMQP 1.0.

U kunt ook Service Bus AMQP 1.0 in andere talen, waaronder .NET, C, Python en PHP. Onderdelen die zijn gebouwd met behulp van deze verschillende talen kunnen op betrouwbare wijze berichten uitwisselen en ondersteuning op vol vertrouwen met behulp van AMQP 1.0 in Service Bus.

## <a name="next-steps"></a>Volgende stappen
* [Ondersteuning van AMQP 1.0 in Azure Service Bus](service-bus-amqp-overview.md)
* [AMQP 1.0 gebruiken met de Service Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)
* [Service Bus AMQP 1.0 handleiding voor ontwikkelaars](service-bus-amqp-dotnet.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Java Developer Center](https://azure.microsoft.com/develop/java/)

