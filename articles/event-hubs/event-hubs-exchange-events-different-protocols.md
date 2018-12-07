---
title: Exchange-gebeurtenissen tussen apps die gebruikmaken van verschillende protocollen - Azure Event Hubs | Microsoft Docs
description: Dit artikel wordt beschreven hoe consumenten en producenten die gebruikmaken van verschillende protocollen (AMQP-, Apache Kafka- en HTTPS) gebeurtenissen bij het gebruik van Azure Event Hubs kunnen uitwisselen.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: bahariri
ms.openlocfilehash: 1345a5814faefd4074e7d9548d374bd79d977514
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015582"
---
# <a name="exchange-events-between-consumers-and-producers-that-use-different-protocols-amqp-kafka-and-https"></a>Exchange-gebeurtenissen tussen consumenten en producenten die gebruikmaken van verschillende protocollen: AMQP, Kafka en HTTPS
Azure Event Hubs ondersteunt drie protocollen voor consumenten en producenten: AMQP, Kafka en HTTPS. Elk van deze protocollen heeft zijn eigen manier voor het weergeven van een bericht, dus op een natuurlijke manier rijst de vraag van de volgende: als een toepassing gebeurtenissen naar een Event Hub met één protocol verzonden en met een ander protocol worden verbruikt, wat doet de verschillende onderdelen en waarden van de gebeurtenis er als volgt uitzien wanneer ze binnenkomen bij de consument? In dit artikel wordt beschreven aanbevolen procedures voor de producent en consument om ervoor te zorgen dat de waarden in een gebeurtenis correct worden geïnterpreteerd door de betreffende toepassing.

Het advies in dit artikel specifiek betrekking heeft op deze clients met de vermelde versies die worden gebruikt bij het ontwikkelen van de codefragmenten:

* Kafka-Java-client (versie 1.1.1 uit https://www.mvnrepository.com/artifact/org.apache.kafka/kafka-clients)
* Microsoft Azure Event Hubs-Client voor Java (versie 1.1.0 uit https://github.com/Azure/azure-event-hubs-java)
* Microsoft Azure Event Hubs-Client voor .NET (versie 2.1.0 uit https://github.com/Azure/azure-event-hubs-dotnet)
* Microsoft Azure Service Bus (versie 5.0.0 https://www.nuget.org/packages/WindowsAzure.ServiceBus)
* HTTPS (ondersteunt alleen producenten)

Andere clients AMQP mogelijk enigszins anders werken. AMQP gaat gepaard met een goed gedefinieerde typesysteem, maar de details van serialiseren taalspecifieke typen naar en van dat typesysteem is afhankelijk van de client, zoals hoe de client biedt toegang tot de onderdelen van een AMQP-bericht.

## <a name="event-body"></a>Hoofdtekst van de gebeurtenis
Alle Microsoft AMQP-clients staat de hoofdtekst van de gebeurtenis voor een niet-geïnterpreteerde eigenschappenverzameling van bytes. Een producent-toepassing wordt een reeks bytes doorgegeven aan de client en een consumerende toepassing die in dezelfde volgorde van de client ontvangt. De interpretatie van reeks bytes gebeurt binnen de toepassingscode.

Bij het verzenden van een gebeurtenis via HTTPS, is de hoofdtekst van de gebeurtenis de geboekt-inhoud, die ook wordt behandeld als niet-geïnterpreteerde bytes. Het is gemakkelijk om de dezelfde status in een Kafka producer- of consumerschaling bereiken met behulp van de opgegeven ByteArraySerializer en ByteArrayDeserializer zoals wordt weergegeven in de volgende code:

### <a name="kafka-byte-producer"></a>Kafka byte [] producent

```java
final Properties properties = new Properties();
// add other properties
properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, ByteArraySerializer.class.getName());

final KafkaProducer<Long, byte[]> producer = new KafkaProducer<Long, byte[]>(properties);

final byte[] eventBody = new byte[] { 0x01, 0x02, 0x03, 0x04 };
ProducerRecord<Long, byte[]> pr =
    new ProducerRecord<Long, byte[]>(myTopic, myPartitionId, myTimeStamp, eventBody);
```

### <a name="kafka-byte-consumer"></a>Kafka-consument voor byte]
```java
final Properties properties = new Properties();
// add other properties
properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, ByteArrayDeserializer.class.getName());

final KafkaConsumer<Long, byte[]> consumer = new KafkaConsumer<Long, byte[]>(properties);

ConsumerRecord<Long, byte[]> cr = /* receive event */
// cr.value() is a byte[] with values { 0x01, 0x02, 0x03, 0x04 }
```

Met deze code maakt een pijplijn transparante bytes tussen de twee helften van de toepassing en kunt u de ontwikkelaar van de toepassing handmatig serialiseren en deserialiseren op geen enkele manier gewenst is, met inbegrip van deserialisatie beslissingen tijdens runtime, bijvoorbeeld op basis van type of gegevens van de afzender in de eigenschappen van de gebruiker worden ingesteld op de gebeurtenis.

Toepassingen waarvoor een enkele, vaste instantie van het gebeurtenistype mogelijk gebruik van andere Kafka objectserializers en deserializers transparant om gegevens te converteren. Neem bijvoorbeeld een toepassing die gebruikmaakt van JSON. Bouw en interpretatie van de JSON-tekenreeks gebeurt op het toepassingsniveau van de. De hoofdtekst van de gebeurtenis is op het niveau van de Event Hubs, altijd een tekenreeks, een reeks tekens in de UTF-8-codering voor bytes. In dit geval kan de Kafka producer of de consument profiteren van de opgegeven StringSerializer of StringDeserializer zoals wordt weergegeven in de volgende code:

### <a name="kafka-utf-8-string-producer"></a>Producent van Kafka UTF-8-tekenreeks
```java
final Properties properties = new Properties();
// add other properties
properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

final KafkaProducer<Long, String> producer = new KafkaProducer<Long, String>(properties);

final String exampleJson = "{\"name\":\"John\", \"number\":9001}";
ProducerRecord<Long, String> pr =
    new ProducerRecord<Long, String>(myTopic, myPartitionId, myTimeStamp, exampleJson);
```

### <a name="kafka-utf-8-string-consumer"></a>Kafka UTF-8 tekenreeks consumenten
```java
final Properties properties = new Properties();
// add other properties
properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());

final KafkaConsumer<Long, String> consumer = new KafkaConsumer<Long, String>(properties);

ConsumerRecord<Long, Bytes> cr = /* receive event */
final String receivedJson = cr.value();
```

Voor de AMQP-zijde bieden zowel Java als .NET ingebouwde manieren om tekenreeksen te zetten naar en van UTF-8-byte-reeksen. Een klasse met de naam EventData staat gebeurtenissen voor de Microsoft AMQP-clients. De volgende voorbeelden ziet u het serialiseren van een tekenreeks UTF-8 in de hoofdcode van een EventData-gebeurtenis in een AMQP-producent en hoe u een instantie van de gebeurtenis EventData deserialiseren naar een UTF-8-tekenreeks in een AMQP-gebruiker.

### <a name="java-amqp-utf-8-string-producer"></a>Producent van Java AMQP UTF-8-tekenreeks
```java
final String exampleJson = "{\"name\":\"John\", \"number\":9001}";
final EventData ed = EventData.create(exampleJson.getBytes(StandardCharsets.UTF_8));
```

### <a name="java-amqp-utf-8-string-consumer"></a>Java AMQP UTF-8 tekenreeks consumenten
```java
EventData ed = /* receive event */
String receivedJson = new String(ed.getBytes(), StandardCharsets.UTF_8);
```

### <a name="c-net-utf-8-string-producer"></a>C#Producent van .NET UTF-8-tekenreeks
```csharp
string exampleJson = "{\"name\":\"John\", \"number\":9001}";
EventData working = new EventData(Encoding.UTF8.GetBytes(exampleJson));
```

### <a name="c-net-utf-8-string-consumer"></a>C#.NET UTF-8 tekenreeks consumenten
```csharp
EventData ed = /* receive event */

// getting the event body bytes depends on which .NET client is used
byte[] bodyBytes = ed.Body.Array;  // Microsoft Azure Event Hubs Client for .NET
// byte[] bodyBytes = ed.GetBytes(); // Microsoft Azure Service Bus

string receivedJson = Encoding.UTF8.GetString(bodyBytes);
```

Omdat Kafka open-source is, kunt ontwikkelaar van de toepassing inspecteren van de implementatie van een serializer of deserializer en implementeren van code, die u maakt of een compatibel reeks bytes aan de AMQP verbruikt.

## <a name="event-user-properties"></a>Eigenschappen van gebeurtenis-gebruiker

Eigenschappen van de gebruiker instellen kunnen worden ingesteld en opgehaald uit beide AMQP-clients (in de Microsoft AMQP-clients deze heten eigenschappen) en Kafka (waar ze headers genoemd). Afzenders van HTTPS kunnen eigenschappen van de gebruiker ingesteld voor een gebeurtenis door op te geven deze als HTTP-headers in de POST-bewerking. Kafka behandelt echter organen van de gebeurtenis zowel headerwaarden gebeurtenis als byte-reeksen. Terwijl in AMQP-clients eigenschapswaarden typen, die worden verstrekt hebben door de eigenschapswaarden op basis van het AMQP-typesysteem codering.

HTTPS is een speciaal geval. Het verzendt, zijn alle eigenschapswaarden UTF-8-tekst. De Event Hubs-service heeft een beperkte hoeveelheid van interpretatie juiste eigenschapswaarden converteren naar AMQP-gecodeerd 32-bits en 64-bits ondertekende gehele getallen, 64-bits getallen met drijvende komma en Booleaanse waarden. Elke eigenschapswaarde hebben die voldoet niet aan een van deze bestandstypen wordt behandeld als een tekenreeks.

Met een combinatie van deze methoden voor het typen van de eigenschap betekent dat een Kafka-consument de volgorde van ruwe byte AMQP-codering ziet, waaronder informatie over het AMQP. Terwijl een AMQP-consumer de volgorde van de niet-getypeerde byte verzonden door de producent Kafka, die de toepassing interpreteren ziet moet.

Voor Kafka-consumenten die eigenschappen van AMQP of HTTPS producenten ontvangt, gebruikt u de klasse AmqpDeserializer, die is gemodelleerd naar de andere deserializers in de Kafka-ecosystemen. De informatie in de reeksen byte AMQP-gecodeerd voor het deserialiseren van de gegevensbytes in een Java-type geïnterpreteerd.

Als een best practice is het raadzaam dat u een eigenschap in berichten die worden verzonden via AMQP of HTTPS bevatten. De Kafka-consument kunt gebruiken om te bepalen of headerwaarden AMQP deserialisatie nodig. De waarde van de eigenschap is niet belangrijk. Deze moet een bekende naam dat de Kafka-consument kunt in de lijst met kopteksten vinden en het gedrag van het overeenkomstig aanpassen.

### <a name="amqp-to-kafka-part-1-create-and-send-an-event-in-c-net-with-properties"></a>AMQP met Kafka-deel 1: maken en verzenden van een gebeurtenis C# (.NET) met eigenschappen
```csharp
// Create an event with properties "MyStringProperty" and "MyIntegerProperty"
EventData working = new EventData(Encoding.UTF8.GetBytes("an event body"));
working.Properties.Add("MyStringProperty", "hello");
working.Properties.Add("MyIntegerProperty", 1234);

// BEST PRACTICE: include a property which indicates that properties will need AMQP deserialization
working.Properties.Add("AMQPheaders", 0);
```

### <a name="amqp-to-kafka-part-2-use-amqpdeserializer-to-deserialize-those-properties-in-a-kafka-consumer"></a>AMQP met Kafka-deel 2: AmqpDeserializer gebruiken voor het deserialiseren van deze eigenschappen in een Kafka-consument
```java
final AmqpDeserializer amqpDeser = new AmqpDeserializer();

ConsumerRecord<Long, Bytes> cr = /* receive event */
final Header[] headers = cr.headers().toArray();

final Header headerNamedMyStringProperty = /* find header with key "MyStringProperty" */
final Header headerNamedMyIntegerProperty = /* find header with key "MyIntegerProperty" */
final Header headerNamedAMQPheaders = /* find header with key "AMQPheaders", or null if not found */

// BEST PRACTICE: detect whether AMQP deserialization is needed
if (headerNamedAMQPheaders != null) {
    // The deserialize() method requires no prior knowledge of a property's type.
    // It returns Object and the application can check the type and perform a cast later.
    Object propertyOfUnknownType = amqpDeser.deserialize("topicname", headerNamedMyStringProperty.value());
    if (propertyOfUnknownType instanceof String) {
        final String propertyString = (String)propertyOfUnknownType;
        // do work here
    }
    propertyOfUnknownType = amqpDeser.deserialize("topicname", headerNamedMyIntegerProperty.value());
    if (propertyOfUnknownType instanceof Integer) {
        final Integer propertyInt = (Integer)propertyOfUnknownType;
        // do work here
    }
} else {
    /* event sent via Kafka, interpret header values the Kafka way */
}
```

Als de toepassing het verwachte type voor een eigenschap weet, er zijn deserialisatiemethoden die een cast daarna niet meer nodig, maar ze genereert een fout als de eigenschap niet van het verwachte type is.

### <a name="amqp-to-kafka-part-3-a-different-way-of-using-amqpdeserializer-in-a-kafka-consumer"></a>AMQP met Kafka-deel 3: een andere manier van het gebruik van AmqpDeserializer in een Kafka-consument
```java
// BEST PRACTICE: detect whether AMQP deserialization is needed
if (headerNamedAMQPheaders != null) {
    // Property "MyStringProperty" is expected to be of type string.
    try {
        final String propertyString = amqpDeser.deserializeString(headerNamedMyStringProperty.value());
        // do work here
    }
    catch (IllegalArgumentException e) {
        // property was not a string
    }

    // Property "MyIntegerProperty" is expected to be a signed integer type.
    // The method returns long because long can represent the value range of all AMQP signed integer types.
    try {
        final long propertyLong = amqpDeser.deserializeSignedInteger(headerNamedMyIntegerProperty.value());
        // do work here
    }
    catch (IllegalArgumentException e) {
        // property was not a signed integer
    }
} else {
    /* event sent via Kafka, interpret header values the Kafka way */
}
```

De andere richting gaan meer betrokken is, omdat de headers die zijn ingesteld door een Kafka producer altijd door een consument AMQP worden gezien als onbewerkte bytes (org.apache.qpid.proton.amqp.Binary voor de Microsoft Azure Event Hubs-Client voor Java, typt of `System.Byte[]` voor van Microsoft .NET AMQP-clients). De eenvoudigste manier is met een van de objectserializers Kafka opgegeven genereren van het aantal bytes voor de headerwaarden aan de producent Kafka en vervolgens een compatibel deserialisatie-code te schrijven aan de kant van de consument AMQP.

Net als bij AMQP aan Kafka, is de aanbevolen procedure die wij adviseren om op te nemen van een eigenschap in berichten die worden verzonden via Kafka. De consument AMQP kunt de eigenschap gebruiken om te bepalen of headerwaarden deserialisatie moeten. De waarde van de eigenschap is niet belangrijk. Deze moet een bekende naam die de consument AMQP kunt in de lijst met kopteksten vinden en overeenkomstig het gedrag aanpassen. Als de producent Kafka kan niet worden gewijzigd, is het ook mogelijk dat de betreffende toepassing om te controleren of de waarde van de eigenschap van een type binair of byte is en deserialisatie op basis van het type proberen.

### <a name="kafka-to-amqp-part-1-create-and-send-an-event-from-kafka-with-properties"></a>Kafka met AMQP-deel 1: maken en verzenden van een gebeurtenis van Kafka met eigenschappen
```java
final String topicName = /* topic name */
final ProducerRecord<Long, String> pr = new ProducerRecord<Long, String>(topicName, /* other arguments */);
final Headers h = pr.headers();

// Set headers using Kafka serializers
IntegerSerializer intSer = new IntegerSerializer();
h.add("MyIntegerProperty", intSer.serialize(topicName, 1234));

LongSerializer longSer = new LongSerializer();
h.add("MyLongProperty", longSer.serialize(topicName, 5555555555L));

ShortSerializer shortSer = new ShortSerializer();
h.add("MyShortProperty", shortSer.serialize(topicName, (short)22222));

FloatSerializer floatSer = new FloatSerializer();
h.add("MyFloatProperty", floatSer.serialize(topicName, 1.125F));

DoubleSerializer doubleSer = new DoubleSerializer();
h.add("MyDoubleProperty", doubleSer.serialize(topicName, Double.MAX_VALUE));

StringSerializer stringSer = new StringSerializer();
h.add("MyStringProperty", stringSer.serialize(topicName, "hello world"));

// BEST PRACTICE: include a property which indicates that properties will need deserialization
h.add("RawHeaders", intSer.serialize(0));
```

### <a name="kafka-to-amqp-part-2-manually-deserialize-those-properties-in-c-net"></a>Kafka met AMQP-deel 2: handmatig deserialiseren van deze eigenschappen in C# (.NET)
```csharp
EventData ed = /* receive event */

// BEST PRACTICE: detect whether manual deserialization is needed
if (ed.Properties.ContainsKey("RawHeaders"))
{
    // Kafka serializers send bytes in big-endian order, whereas .NET on x86/x64 is little-endian.
    // Therefore it is frequently necessary to reverse the bytes before further deserialization.

    byte[] rawbytes = ed.Properties["MyIntegerProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    int myIntegerProperty = BitConverter.ToInt32(rawbytes, 0);

    rawbytes = ed.Properties["MyLongProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    long myLongProperty = BitConverter.ToInt64(rawbytes, 0);

    rawbytes = ed.Properties["MyShortProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    short myShortProperty = BitConverter.ToInt16(rawbytes, 0);

    rawbytes = ed.Properties["MyFloatProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    float myFloatProperty = BitConverter.ToSingle(rawbytes, 0);

    rawbytes = ed.Properties["MyDoubleProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    double myDoubleProperty = BitConverter.ToDouble(rawbytes, 0);

    rawbytes = ed.Properties["MyStringProperty"] as System.Byte[];
string myStringProperty = Encoding.UTF8.GetString(rawbytes);
}
```

### <a name="kafka-to-amqp-part-3-manually-deserialize-those-properties-in-java"></a>Kafka met AMQP-deel 3: handmatig deserialiseren van deze eigenschappen in Java
```java
final EventData ed = /* receive event */

// BEST PRACTICE: detect whether manual deserialization is needed
if (ed.getProperties().containsKey("RawHeaders")) {
    byte[] rawbytes =
        ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyIntegerProperty")).getArray();
    int myIntegerProperty = 0;
    for (byte b : rawbytes) {
        myIntegerProperty <<= 8;
        myIntegerProperty |= ((int)b & 0x00FF);
    }

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyLongProperty")).getArray();
    long myLongProperty = 0;
    for (byte b : rawbytes) {
        myLongProperty <<= 8;
        myLongProperty |= ((long)b & 0x00FF);
    }

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyShortProperty")).getArray();
    short myShortProperty = (short)rawbytes[0];
    myShortProperty <<= 8;
    myShortProperty |= ((short)rawbytes[1] & 0x00FF);

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyFloatProperty")).getArray();
    int intbits = 0;
    for (byte b : rawbytes) {
        intbits <<= 8;
        intbits |= ((int)b & 0x00FF);
    }
    float myFloatProperty = Float.intBitsToFloat(intbits);

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyDoubleProperty")).getArray();
    long longbits = 0;
    for (byte b : rawbytes) {
        longbits <<= 8;
        longbits |= ((long)b & 0x00FF);
    }
    double myDoubleProperty = Double.longBitsToDouble(longbits);

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyStringProperty")).getArray();
String myStringProperty = new String(rawbytes, StandardCharsets.UTF_8);
}
```

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd u hoe u kunt streamen naar Event Hubs waarvoor Kafka is ingeschakeld zonder uw protocolclients te wijzigen of uw eigen clusters uit te voeren. Zie voor meer informatie over Event Hubs en Event Hubs voor Kafka, de volgende artikelen:  

* [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)
* [Meer informatie over Event Hubs for Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Meer voorbeelden van de Event Hubs for Kafka-GitHub verkennen](https://github.com/Azure/azure-event-hubs-for-kafka)
* Gebruik [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) om [gebeurtenissen van Kafka on-premises te streamen naar Event Hubs waarvoor Kafka is ingeschakeld in de cloud](event-hubs-kafka-mirror-maker-tutorial.md).
* Meer informatie over het streamen naar Kafka ingeschakelde Event Hubs met behulp [systeemeigen Kafka toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md), [Apache Flink](event-hubs-kafka-flink-tutorial.md), of [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md)
