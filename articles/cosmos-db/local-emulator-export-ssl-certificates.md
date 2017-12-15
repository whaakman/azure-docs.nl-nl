---
title: De Azure Cosmos DB Emulator certificaten exporteren | Microsoft Docs
description: Bij het ontwikkelen van in talen en runtimes die geen van de Windows-certificaatarchief gebruikmaken moet u exporteren en de SSL-certificaten te beheren. Dit bericht geeft stapsgewijze instructies.
services: cosmos-db
documentationcenter: 
keywords: Azure Cosmos DB Emulator
author: voellm
manager: jhubbard
editor: 
ms.assetid: ef43deda-c2e9-4193-99e2-7f6a88a0319f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: tvoellm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d3811a9d59e388a1ceaacdefebc17afeda5b07d
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="export-the-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a>Exporteren van de certificaten Azure Cosmos DB Emulator voor gebruiken met Java, Python en Node.js

[**Downloaden van de Emulator**](https://aka.ms/cosmosdb-emulator)

De Azure-Emulator Cosmos DB biedt een lokale omgeving waarin de service Azure Cosmos DB voor ontwikkelingsdoeleinden, met inbegrip van het gebruik van SSL-verbindingen. Dit artikel laat zien hoe u exporteert u het SSL-certificaat voor gebruik met talen en runtimes die niet worden geïntegreerd in de Windows-certificaatarchief zoals Java die gebruikmaakt van een eigen [certificaatarchief](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) en Python dat gebruikmaakt van [socket wrappers](https://docs.python.org/2/library/ssl.html) en Node.js dat gebruikmaakt van [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback). U kunt meer lezen over de emulator in [de Emulator Azure Cosmos DB gebruiken voor ontwikkeling en tests](./local-emulator.md).

Deze zelfstudie bevat de volgende taken:

> [!div class="checklist"]
> * Certificaten draaien
> * SSL-certificaat exporteren
> * Het gebruik van het certificaat in Java, Python en Node.js leren

## <a name="certification-rotation"></a>Rotatie van de certificeringsinstantie

Certificaten in de Azure Cosmos DB lokale Emulator worden de eerste keer die is uitgevoerd op de emulator gegenereerd. Er zijn twee certificaten. Een gebruikt voor het verbinden met de lokale emulator en één voor het beheren van geheimen in de emulator. Het certificaat dat u wilt exporteren, is het verbindingscertificaat met de beschrijvende naam 'DocumentDBEmulatorCertificate'.

Beide certificaten kunnen worden hersteld door te klikken op **gegevens opnieuw** zoals hieronder wordt weergegeven in Azure Cosmos DB-Emulator wordt uitgevoerd in het Windows-systeemvak. Als u de certificaten opnieuw genereren en hebben ze in het certificaatarchief Java hebt geïnstalleerd of gebruikt ze ergens anders moet u deze bijwerken, anders uw toepassing wordt niet meer verbinding maken met de lokale emulator.

![Azure DB Cosmos lokale emulator opnieuw instellen van gegevens](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-to-export-the-azure-cosmos-db-ssl-certificate"></a>Het exporteren van het Azure Cosmos DB SSL-certificaat

1. Start de Windows-certificaatbeheerder door certlm.msc uitgevoerd en navigeer naar de map persoonlijke-> certificaten en het certificaat openen met de beschrijvende naam **DocumentDbEmulatorCertificate**.

    ![Azure DB Cosmos lokale emulator exporteren stap 1](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. Klik op **Details** vervolgens **OK**.

    ![Azure DB Cosmos lokale emulator exporteren stap 2](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. Klik op **kopiëren naar bestand...** .

    ![Azure DB Cosmos lokale emulator exporteren stap 3](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. Klik op **Volgende**.

    ![Azure DB Cosmos lokale emulator exporteren stap 4](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. Klik op **Nee, de persoonlijke sleutel niet exporteren**, klikt u vervolgens op **volgende**.

    ![Azure DB Cosmos lokale emulator exporteren stap 5](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. Klik op **Base-64 gecodeerde X.509 (. CER)** en vervolgens **volgende**.

    ![Azure DB Cosmos lokale emulator stap 6 exporteren](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. Het certificaat een naam geven. In dit geval **documentdbemulatorcert** en klik vervolgens op **volgende**.

    ![Azure DB Cosmos lokale emulator stap 7 exporteren](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. Klik op **Voltooien**.

    ![Azure DB Cosmos lokale emulator stap 8 exporteren](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-to-use-the-certificate-in-java"></a>Het gebruik van het certificaat in Java

Bij het uitvoeren van Java-toepassingen of MongoDB-toepassingen die gebruikmaken van de Java-client is het eenvoudiger om het certificaat installeren in het certificaatarchief van Java standaard dan geven de '-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword= '<password>'vlaggen. Bijvoorbeeld de opgenomen [Java-voorbeeldtoepassing](https://localhost:8081/_explorer/index.html) is afhankelijk van het standaard certificaatarchief.

Volg de instructies in de [een certificaat toevoegen aan de Java CA certificatenarchief](https://docs.microsoft.com/azure/java-add-certificate-ca-store) het X.509-certificaat importeren in het certificaatarchief van de standaard Java. Houd werkt Vergeet niet dat u in de map % JAVA_HOME % wanneer keytool wordt uitgevoerd.

Eenmaal de 'CosmosDBEmulatorCertificate' SSL-certificaat is geïnstalleerd uw toepassing moet kunnen verbinding maken en gebruiken van de lokale Azure Cosmos DB-Emulator. Als u problemen blijft ondervinden kunt u volgen de [SSL/TLS-verbindingen voor foutopsporing](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) artikel. Het is zeer waarschijnlijk dat het certificaat is niet geïnstalleerd in het archief %JAVA_HOME%/jre/lib/security/cacerts. Voor bijvoorbeeld als er meerdere geïnstalleerde versies van uw toepassing Java mogelijk gebruik van een andere cacerts archief dan één die u bijgewerkt.

## <a name="how-to-use-the-certificate-in-python"></a>Het gebruik van het certificaat in Python

Standaard de [Python SDK(version 2.0.0 or higher)](sql-api-sdk-python.md) voor de SQL-API wordt niet proberen en de SSL-certificaat gebruiken bij het verbinden met de lokale emulator. Als maar u wilt gebruiken, SSL-validatie voert u de voorbeelden in de [Python socket wrappers](https://docs.python.org/2/library/ssl.html) documentatie.

## <a name="how-to-use-the-certificate-in-nodejs"></a>Het gebruik van het certificaat in Node.js

Standaard de [Node.js SDK(version 1.10.1 or higher)](sql-api-sdk-node.md) voor de SQL-API wordt niet proberen en de SSL-certificaat gebruiken bij het verbinden met de lokale emulator. Als maar u wilt gebruiken, SSL-validatie voert u de voorbeelden in de [Node.js documentatie](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Gedraaide certificaten
> * Het SSL-certificaat wordt geëxporteerd
> * Geleerd hoe u het certificaat in Java, Python en Node.js

U kunt nu doorgaan met het maken van een HTTP-functies van Azure-trigger met een invoer binding Azure Cosmos DB zelfstudie.

> [!div class="nextstepaction"]
> [Maken van een Azure-functie met invoer van de Azure Cosmos-database](tutorial-functions-http-trigger.md) 
