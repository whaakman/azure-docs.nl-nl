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
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: bfab0c374e4b20b09167f37363fe0681144426ac
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43699342"
---
# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>De Java Message Service (JMS) API gebruiken met Service Bus en AMQP 1.0
Het Advanced Message Queuing Protocol (AMQP 1.0) is een efficiënte, betrouwbare, wire-level berichtenprotocol dat u gebruiken kunt om robuuste, platformonafhankelijke berichten toepassingen te bouwen.

Ondersteuning voor AMQP 1.0 in Service Bus betekent dat u kunt het in de wachtrij gebruiken en publiceren/abonneren brokered messaging-onderdelen uit een scala aan platformen met behulp van een doeltreffende binaire-protocol. Bovendien kunt u toepassingen bestaat uit onderdelen die zijn gebouwd met behulp van een combinatie van programmeertalen, frameworks en besturingssystemen bouwen.

Dit artikel wordt uitgelegd hoe u Service Bus-berichtfuncties (wachtrijen en onderwerpen publiceren/abonneren op onderwerpen) uit Java-toepassingen met behulp van de populaire Java Message Service (JMS) API standaard. Er is een [companion-artikel](service-bus-amqp-dotnet.md) waarin wordt uitgelegd hoe u doet hetzelfde met behulp van de Service Bus .NET API. U kunt deze twee handleidingen samen gebruiken voor meer informatie over platformonafhankelijke berichten met behulp van AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Aan de slag met Service Bus
Deze handleiding wordt ervan uitgegaan dat u al een Service Bus-naamruimte met een wachtrij met de naam **Wachtrij1**. Als u dit niet doet, dan u kunt [maken van de naamruimte en wachtrij](service-bus-create-namespace-portal.md) met behulp van de [Azure-portal](https://portal.azure.com). Zie voor meer informatie over het maken van Service Bus-naamruimten en -wachtrijen [aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Gepartitioneerde wachtrijen en onderwerpen bieden ook ondersteuning voor AMQP. Zie voor meer informatie, [gepartitioneerde berichtentiteiten](service-bus-partitioning.md) en [AMQP 1.0-ondersteuning voor Service Bus gepartitioneerde wachtrijen en onderwerpen](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-the-amqp-10-jms-client-library"></a>De clientbibliotheek van AMQP 1.0 JMS downloaden
Voor informatie over het downloaden van de meest recente versie van de Apache Qpid JMS AMQP 1.0-clientbibliotheek, gaat u naar [ https://qpid.apache.org/download.html ](https://qpid.apache.org/download.html).

U moet de volgende vier JAR-bestanden uit het archief van de Apache Qpid JMS AMQP 1.0-distributiepunt toevoegen aan het KLASSEPAD Java bij het bouwen en uitvoeren van JMS toepassingen met Service Bus:

* geronimo jms\_1.1\_spec 1.0.jar
* qpid-amqp-1-0-client-[Version].jar
* qpid-amqp-1-0-client-jms-[Version].jar
* qpid-amqp-1-0-Common-[Version].jar

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

#### <a name="configure-the-connectionfactory"></a>De ConnectionFactory configureren
De vermelding die wordt gebruikt voor het definiëren van een **ConnectionFactory** in het bestand van de eigenschappen Qpid JNDI-provider is de volgende indeling hebben:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Waar **[jndi_name]** en **[ConnectionURL]** hebben de volgende betekenis:

* **[jndi_name]** : De logische naam van de ConnectionFactory. Dit is de naam die in de Java-toepassing met behulp van de methode JNDI IntialContext.lookup() wordt opgelost.
* **[ConnectionURL]** : Een URL waarmee de JMS-bibliotheek met de vereiste informatie naar de broker AMQP.

De indeling van de **ConnectionURL** is als volgt:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Waar **[naamruimte]**, **[SASPolicyName]** en **[SASPolicyKey]** hebben de volgende betekenis:

* **[namespace]** : De Service Bus-naamruimte.
* **[SASPolicyName]** : Naam van de wachtrij Shared Access Signature-beleid.
* **[SASPolicyKey]** : De wachtrij Shared Access Signature beleidssleutel.

> [!NOTE]
> Moet u de URL-codering het wachtwoord handmatig. Er is een handig hulpprogramma voor het URL-codering beschikbaar op [ http://www.w3schools.com/tags/ref_urlencode.asp ](http://www.w3schools.com/tags/ref_urlencode.asp).
> 
> 

#### <a name="configure-destinations"></a>Bestemmingen configureren
De vermelding die wordt gebruikt voor het definiëren van een bestemming in de Qpid eigenschappen bestand JNDI provider is de volgende indeling hebben:

```
queue.[jndi_name] = [physical_name]
```

of

```
topic.[jndi_name] = [physical_name]
```

Waar **[jndi\_naam]** en **[fysieke\_naam]** hebben de volgende betekenis:

* **[jndi_name]** : De logische naam van de bestemming. Dit is de naam die in de Java-toepassing met behulp van de methode JNDI IntialContext.lookup() wordt opgelost.
* **[physical_name]** : De naam van de Service Bus-entiteit die de toepassing verzendt of ontvangt berichten.

> [!NOTE]
> Tijdens het ontvangen van een Service Bus-onderwerpabonnement, is de fysieke naam die is opgegeven in JNDI, moet de naam van het onderwerp. Naam van het abonnement wordt opgegeven als het duurzame abonnement is gemaakt in de toepassingscode JMS. De [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md) vindt u meer informatie over het werken met Service Bus-onderwerpen uit JMS.
> 
> 

### <a name="write-the-jms-application"></a>De toepassing JMS schrijven
Er zijn geen speciale API's of -opties die vereist als u de JMS voor Service Bus. Er zijn echter enkele beperkingen die later worden behandeld. Net als bij elke toepassing JMS eerst te beginnen de vereiste configuratie van de omgeving JNDI, moeten kunnen omzetten van een **ConnectionFactory** en bestemmingen.

#### <a name="configure-the-jndi-initialcontext"></a>De InitialContext JNDI configureren
De JNDI-omgeving is geconfigureerd door een hashtabel van configuratie-informatie wordt doorgegeven aan de constructor van de klasse javax.naming.InitialContext. De twee vereiste elementen in de hashtabel zijn de naam van de klasse van de eerste Context Factory en de URL van de Provider. De volgende code laat zien hoe het configureren van de omgeving JNDI voor het gebruik van de eigenschappen op basis van bestanden JNDI Provider met een eigenschappenbestand met de naam Qpid **servicebus.properties**.

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Een eenvoudige JMS-toepassing met behulp van een Service Bus-wachtrij
Het volgende voorbeeldprogramma JMS TextMessages verzendt naar een Service Bus-wachtrij met de logische naam JNDI van wachtrij en ontvangt de berichten weer.

```java
// SimpleSenderReceiver.java

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;

public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();

    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);

        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");

        // Create Connection
        connection = cf.createConnection();

        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);

        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }

    public static void main(String[] args) {
        try {

            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }

            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));

            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }

    public void close() throws JMSException {
        connection.close();
    }

    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}    
```

### <a name="run-the-application"></a>De toepassing uitvoeren
De toepassing wordt uitgevoerd, wordt de uitvoer van het formulier gegenereerd:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.

Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318

Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483

Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Platformonafhankelijke berichtenverzending tussen JMS en .NET
Deze handleiding bleek verzenden en ontvangen van berichten naar en van Service Bus met behulp van JMS. Een van de belangrijkste voordelen van AMQP 1.0 is echter dat deze toepassingen worden gebouwd van onderdelen die zijn geschreven in verschillende talen, met berichten die worden uitgewisseld betrouwbaar en op volledig betrouwbaar kan.

Met behulp van de voorbeeldtoepassing JMS de die hierboven worden beschreven en een vergelijkbare .NET-toepassing die is overgenomen uit een artikel companion [met behulp van Service Bus van .NET met AMQP 1.0](service-bus-amqp-dotnet.md), kunt u berichten tussen .NET en Java uitwisselen. Lees dit artikel voor meer informatie over de details van platformonafhankelijke berichtenverzending met behulp van Service Bus en AMQP 1.0.

### <a name="jms-to-net"></a>JMS voor .NET
Ter illustratie van JMS .NET Messaging:

* Start de .NET-voorbeeld-App zonder opdrachtregelargumenten.
* Start de Java-voorbeeldtoepassing met het opdrachtregelargument 'sendonly'. In deze modus wordt de toepassing ontvangt geen berichten uit de wachtrij wordt alleen verzonden.
* Druk op **Enter** een paar keer in de console Java-toepassing, waardoor berichten worden verzonden.
* Deze berichten worden ontvangen door de .NET-toepassing.

#### <a name="output-from-jms-application"></a>Uitvoer van de toepassing JMS
```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Uitvoer van .NET-toepassing
```
> SimpleSenderReceiver.exe    
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET tot JMS
Ter illustratie van .NET JMS Messaging:

* Start de .NET-voorbeeldtoepassing met het opdrachtregelargument 'sendonly'. In deze modus wordt de toepassing ontvangt geen berichten uit de wachtrij wordt alleen verzonden.
* Start de Java-voorbeeldtoepassing zonder opdrachtregelargumenten.
* Druk op **Enter** een paar keer in de console .NET-toepassing, waardoor berichten worden verzonden.
* Deze berichten worden ontvangen door de Java-toepassing.

#### <a name="output-from-net-application"></a>Uitvoer van .NET-toepassing
```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Uitvoer van de toepassing JMS
```
> java SimpleSenderReceiver    
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Niet-ondersteunde functies en -beperkingen
De volgende beperkingen zijn bij het gebruik van JMS via AMQP 1.0 met Service Bus, namelijk:

* Slechts één **MessageProducer** of **MessageConsumer** is toegestaan **sessie**. Als u wilt maken van meerdere **MessageProducers** of **MessageConsumers** in een toepassing, maakt u een toegewezen **sessie** voor elk van deze.
* Vluchtige onderwerpabonnementen worden momenteel niet ondersteund.
* **MessageSelectors** worden momenteel niet ondersteund.
* Tijdelijke bestemmingen; bijvoorbeeld, **TemporaryQueue**, **TemporaryTopic** worden momenteel niet ondersteund, samen met de **QueueRequestor** en **TopicRequestor**API's die ze gebruiken.
* Transactionele sessies en gedistribueerde transacties worden niet ondersteund.

## <a name="summary"></a>Samenvatting
In deze gebruiksaanwijzing laten zien hoe u Service Bus brokered messaging functies (wachtrijen en onderwerpen publiceren/abonneren op onderwerpen) uit Java met behulp van de populaire JMS API en de AMQP 1.0.

U kunt ook Service Bus AMQP 1.0 in andere talen, waaronder .NET, C, Python en PHP. Onderdelen die zijn gebouwd met behulp van deze verschillende talen kunnen op betrouwbare wijze berichten uitwisselen en ondersteuning op vol vertrouwen met behulp van AMQP 1.0 in Service Bus.

## <a name="next-steps"></a>Volgende stappen
* [Ondersteuning van AMQP 1.0 in Azure Service Bus](service-bus-amqp-overview.md)
* [AMQP 1.0 gebruiken met de Service Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)
* [Service Bus AMQP 1.0 handleiding voor ontwikkelaars](service-bus-amqp-dotnet.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Java Developer Center](https://azure.microsoft.com/develop/java/)

