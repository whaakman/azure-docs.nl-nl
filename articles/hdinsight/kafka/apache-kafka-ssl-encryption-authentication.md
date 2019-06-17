---
title: Instellen van SSL-codering en -verificatie voor Apache Kafka in Azure HDInsight
description: SSL-codering voor communicatie tussen Kafka-clients en Kafka-brokers ook tussen Kafka-brokers instellen. Instellen van SSL-verificatie van clients.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: hrasheed
ms.openlocfilehash: 5d567074a0038915cc43a585b34c9c71ccf3eb1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65465000"
---
# <a name="set-up-secure-sockets-layer-ssl-encryption-and-authentication-for-apache-kafka-in-azure-hdinsight"></a>Instellen van Secure Sockets Layer (SSL)-codering en -verificatie voor Apache Kafka in Azure HDInsight

In dit artikel leest u hoe u SSL-versleuteling tussen clients van Apache Kafka- en Apache Kafka-brokers instelt. Deze ook wordt beschreven hoe u voor het instellen van verificatie van clients (soms aangeduid als wederzijdse SSL).

> [!Important]
> Er zijn twee clients die u voor Kafka-toepassingen gebruiken kunt: een Java-client en een consoleclient. Alleen de Java-client `ProducerConsumer.java` kunt SSL gebruiken voor zowel produceren en te gebruiken. De producent consoleclient `console-producer.sh` werkt niet met SSL.

## <a name="apache-kafka-broker-setup"></a>Apache Kafka-Broker-instellingen

De installatie van de broker Kafka SSL gebruikt vier VM's voor HDInsight-cluster in de volgende manier:

* hoofdknooppunt 0 - certificeringsinstantie (CA)
* worker-knooppunt 0, 1 en 2 - makelaars

> [!Note] 
> Deze handleiding worden zelfondertekende certificaten gebruikt, maar de veiligste oplossing is het gebruik van certificaten van vertrouwde certificeringsinstanties.

De samenvatting van het broker-installatieproces is als volgt:

1. De volgende stappen worden herhaald op elk van de drie worker-knooppunten:

    1. Genereer een certificaat.
    1. Maak een certificaat aanvraag ondertekenen.
    1. Verzend de aanvraag naar de certificeringsinstantie (CA) ondertekenen certificaat.
    1. Aanmelden bij de CA en ondertekenen van de aanvraag.
    1. SCP het ondertekende certificaat terug naar het worker-knooppunt.
    1. SCP het openbare certificaat van de CA met het werkrolknooppunt.

1. Nadat u hebt alle certificaten, de certificaten in het certificaatarchief te plaatsen.
1. Ga naar Ambari en wijzig de configuraties.

Gebruik de volgende gedetailleerde instructies om de broker-installatie te voltooien:

> [!Important]
> In de volgende codefragmenten wnX is een afkorting van een van de drie worker-knooppunten en moet worden vervangen door `wn0`, `wn1` of `wn2` indien van toepassing. `WorkerNode0_Name` en `HeadNode0_Name` moet worden vervangen door de namen van de betreffende computers, zoals `wn0-abcxyz` of `hn0-abcxyz`.

1. Eerste setup uitvoeren op het hoofdknooppunt 0, waarbij voor HDInsight vol raken de rol van de certificeringsinstantie (CA).

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl
    ```

1. De initiële installatie van dezelfde uitvoeren op elk van de brokers (worker-knooppunten 0, 1 of 2).

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl
    ```

1. Uitvoeren op elk van de worker-knooppunten, de volgende stappen uit met behulp van het onderstaande codefragment.
    1. Een keystore maken en deze vullen met een nieuwe persoonlijke certificaat.
    1. Maak een aanvraag voor Certificaatondertekening.
    1. SCP het ondertekenen van certificaten aanvragen bij de Certificeringsinstantie (headnode0)

    ```bash
    keytool -genkey -keystore kafka.server.keystore.jks -validity 365 -storepass "MyServerPassword123" -keypass "MyServerPassword123" -dname "CN=FQDN_WORKER_NODE" -storetype pkcs12
    keytool -keystore kafka.server.keystore.jks -certreq -file cert-file -storepass "MyServerPassword123" -keypass "MyServerPassword123"
    scp cert-file sshuser@HeadNode0_Name:~/ssl/wnX-cert-sign-request
    ```

1. Wijzigen in de CA-computer en meld u aan alle van de ontvangen certificaat ondertekenen van aanvragen:

    ```bash
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn0-cert-sign-request -out wn0-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn1-cert-sign-request -out wn1-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn2-cert-sign-request -out wn2-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    ```

1. De ondertekende certificaten naar de worker-knooppunten van de Certificeringsinstantie (headnode0) sturen.

    ```bash
    scp wn0-cert-signed sshuser@WorkerNode0_Name:~/ssl/cert-signed
    scp wn1-cert-signed sshuser@WorkerNode1_Name:~/ssl/cert-signed
    scp wn2-cert-signed sshuser@WorkerNode2_Name:~/ssl/cert-signed
    ```

1. Het openbare certificaat van de CA verzenden naar elke worker-knooppunt.

    ```bash
    scp ca-cert sshuser@WorkerNode0_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode1_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode2_Name:~/ssl/ca-cert
    ```

1. Toevoegen op elke worker-knooppunt, het openbare certificaat van de CA's aan de truststore en keystore. Het certificaat van de werknemer van het knooppunt vervolgens toevoegen aan de sleutelopslag

    ```bash
    keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -import -file cert-signed -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt

    ```

## <a name="update-kafka-configuration-to-use-ssl-and-restart-brokers"></a>Kafka-configuratie voor het gebruik van SSL en opnieuw opstarten brokers bijwerken

U hebt nu elke Kafka-broker met een keystore en truststore instellen en de juiste certificaten zijn geïmporteerd. Vervolgens verwante eigenschappen van Kafka-configuratie met behulp van Ambari wijzigen en opnieuw opstarten van het Kafka-brokers.

Als u wilt de wijziging van de configuratie hebt voltooid, moet u de volgende stappen uitvoeren:

1. Meld u aan bij Azure portal en selecteer uw Azure HDInsight Apache Kafka-cluster.
1. Ga naar de UI Ambari door te klikken op **Ambari home** onder **Clusterdashboards**.
1. Onder **Kafka-Broker** stelt de **listeners** eigenschap `PLAINTEXT://localhost:9092,SSL://localhost:9093`
1. Onder **kafka-broker geavanceerde** stelt de **security.inter.broker.protocol** eigenschap `SSL`

    ![Eigenschappen van een Kafka-ssl-configuratie, in Ambari bewerken](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari.png)

1. Onder **aangepaste kafka-broker** stelt de **ssl.client.auth** eigenschap `required`. Deze stap is alleen vereist als het instellen van verificatie en versleuteling.

    ![Eigenschappen van een kafka-ssl-configuratie, in Ambari bewerken](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari2.png)

1. Configuratie-eigenschappen toevoegen aan de Kafka `server.properties` bestand voor het adverteren van IP-adressen in plaats van het volledig FULLY Qualified Domain Name ().

    ```bash
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092,SSL://$IP_ADDRESS:9093" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.password=MyServerPassword123" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.key.password=MyServerPassword123" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.password=MyServerPassword123" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

1. Om te verifiëren of de wijzigingen zijn aangebracht correct desgewenst kunt u controleren dat de volgende regels zijn aanwezig in de Kafka `server.properties` bestand.

    ```bash
    advertised.listeners=PLAINTEXT://10.0.0.11:9092,SSL://10.0.0.11:9093
    ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks
    ssl.keystore.password=MyServerPassword123
    ssl.key.password=MyServerPassword123
    ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks
    ssl.truststore.password=MyServerPassword123
    ```

1. Start opnieuw op alle Kafka-brokers.
1. Start de client beheerder met de producent en consument opties om te controleren dat zowel producenten en consumenten op poort 9093 werkt.

## <a name="client-setup-with-authentication"></a>Clientinstallatie (met behulp van verificatie)

> [!Note]
> De volgende stappen zijn alleen vereist als u beide SSL-versleuteling instelt **en** verificatie. Als u versleuteling alleen instelt, gaat u naar [installatie van de Client zonder verificatie](apache-kafka-ssl-encryption-authentication.md#client-setup-without-authentication)

Voer de volgende stappen uit om de clientinstallatie te voltooien:

1. Aanmelden bij de clientcomputer (hn1).
1. Maak een java keystore en een ondertekend certificaat krijgen voor de broker. Kopieer vervolgens het certificaat naar de virtuele machine waarop de CA wordt uitgevoerd.
1. Schakel over naar de CA-computer (hn0) om te ondertekenen certificaat van de client.
1. Ga naar de clientcomputer (hn1) en navigeer naar de `~/ssl` map. Kopieer het ondertekende certificaat naar de client-computer.

```bash
cd ssl

# Create a java keystore and get a signed certificate for the broker. Then copy the certificate to the VM where the CA is running.

keytool -genkey -keystore kafka.client.keystore.jks -validity 365 -storepass "MyClientPassword123" -keypass "MyClientPassword123" -dname "CN=mylaptop1" -alias my-local-pc1 -storetype pkcs12

keytool -keystore kafka.client.keystore.jks -certreq -file client-cert-sign-request -alias my-local-pc1 -storepass "MyClientPassword123" -keypass "MyClientPassword123"

# Copy the cert to the CA
scp client-cert-sign-request3 sshuser@HeadNode0_Name:~/tmp1/client-cert-sign-request

# Switch to the CA machine (hn0) to sign the client certificate.
cd ssl
openssl x509 -req -CA ca-cert -CAkey ca-key -in /tmp1/client-cert-sign-request -out /tmp1/client-cert-signed -days 365 -CAcreateserial -passin pass:MyServerPassword123

# Return to the client machine (hn1), navigate to ~/ssl folder and copy signed cert from the CA (hn0) to client machine
scp -i ~/kafka-security.pem sshuser@HeadNode0_Name:/tmp1/client-cert-signed

# Import CA cert to trust store
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt

# Import CA cert to key store
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt

# Import signed client (cert client-cert-signed1) to keystore
keytool -keystore kafka.client.keystore.jks -import -file client-cert-signed -alias my-local-pc1 -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt
```

Ten slotte, bekijkt u het bestand `client-ssl-auth.properties` met de opdracht `cat client-ssl-auth.properties`. Het moet beschikken over de volgende regels:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=MyClientPassword123
ssl.keystore.location=/home/sshuser/ssl/kafka.client.keystore.jks
ssl.keystore.password=MyClientPassword123
ssl.key.password=MyClientPassword123
```

## <a name="client-setup-without-authentication"></a>Installatie van de client (zonder verificatie)

Als u geen verificatie nodig hebt, worden de stappen voor het instellen van SSL-versleuteling zijn:

1. Aanmelden bij de clientcomputer (hn1) en navigeer naar de `~/ssl` map
1. Kopieer het ondertekende certificaat naar de client-computer van de CA-computer (wn0).
1. De CA-certificaat importeren in de truststore
1. De CA-certificaat importeren in de sleutelopslag

Deze stappen worden weergegeven in het volgende codefragment.

```bash
cd ssl

# Copy signed cert to client machine
scp -i ~/kafka-security.pem sshuser@wn0-umakaf:/home/sshuser/ssl/ca-cert .

# Import CA cert to truststore
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt

# Import CA cert to keystore
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file cert-signed -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt
```

Ten slotte, bekijkt u het bestand `client-ssl-auth.properties` met de opdracht `cat client-ssl-auth.properties`. Het moet beschikken over de volgende regels:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=MyClientPassword123
```

## <a name="next-steps"></a>Volgende stappen

* [Wat is Apache Kafka in HDInsight?](apache-kafka-introduction.md)
