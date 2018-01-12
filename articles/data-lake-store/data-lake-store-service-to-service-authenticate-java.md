---
title: 'Verificatie van de service-naar-service: Java met Data Lake Store met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van authentication service-naar-service met Data Lake Store met Azure Active Directory met Java
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: e537d8a6ea53bf4366168727de8ef95b96281d5b
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="service-to-service-authentication-with-data-lake-store-using-java"></a>Service to service-verificatie met Data Lake Store met Java
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET-SDK gebruiken](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-service-to-service-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

In dit artikel hebt u meer informatie over het gebruik van de Java SDK voor verificatie met Azure Data Lake Store-service-naar-service. Verificatie van de eindgebruiker met Data Lake Store met Java SDK wordt niet ondersteund.

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory 'Web' toepassing**. U moet voltooid van de stappen in [authentication Service-naar-service met Data Lake Store met Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md).

* [Maven](https://maven.apache.org/install.html). In deze zelfstudie wordt Maven gebruikt voor build- en projectafhankelijkheden. Hoewel het mogelijk is te ontwikkelen zonder een buildsysteem als Maven of Gradle, maken deze systemen het veel eenvoudiger om afhankelijkheden te beheren.

* (Optioneel) Een IDE zoals [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) of vergelijkbaar.

## <a name="service-to-service-authentication"></a>Verificatie van service-tot-service
1. Maak een Maven-project met behulp van [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) vanaf de opdrachtregel of met behulp van een IDE. [Hier](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) vindt u instructies over het maken van een Java-project met behulp van IntelliJ. [Hier](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) vindt u instructies over het maken van een Java-project met behulp van Eclipse.

2. Voeg de volgende afhankelijkheden toe aan het **pom.xml**-bestand in Maven. Voeg het volgende codefragment toe vóór de tag **\</project>**:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.2.3</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    De eerste afhankelijkheid is om Data Lake Store SDK (`azure-data-lake-store-sdk`) vanuit de Maven-opslag te gebruiken. De tweede afhankelijkheid dient om op te geven welk framework voor logboekregistratie (`slf4j-nop`) voor deze toepassing moet worden gebruikt. De Data Lake Store SDK gebruikt een [slf4j](http://www.slf4j.org/)-façade voor logboekregistratie, waarmee u uit een aantal populaire frameworks voor logboekregistratie, zoals log4j, Java logging, logback enzovoort, of voor geen logboekregistratie kunt kiezen. In dit voorbeeld wordt logboekregistratie uitgeschakeld. Daarom wordt de binding **slf4j-nop** gebruikt. [Hier](http://www.slf4j.org/manual.html#projectDep) vindt u andere opties voor logboekregistratie voor uw toepassing.

3. Voeg de volgende importinstructies toe aan uw toepassing.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

4. Gebruik het volgende fragment in uw Java-toepassing voor het token voor de Active Directory-webtoepassing hebt gemaakt met een van de subklassen van eerder behulp verkrijgen `AccessTokenProvider` (het volgende voorbeeld wordt `ClientCredsTokenProvider`). De tokenprovider slaat de referenties die worden gebruikt voor het ophalen van het token, op in de cache en vernieuwt het token automatisch als het bijna is verlopen. Het is mogelijk te maken van uw eigen subklassen van `AccessTokenProvider` zodat tokens worden verkregen door de code van uw klant. Nu gaan we gebruiken de vraagcode die is opgegeven in de SDK.

    Vervang **FILL-IN-HERE** met de daadwerkelijke waarden voor de Azure Active Directory-webtoepassing.

        private static String clientId = "FILL-IN-HERE";
        private static String authTokenEndpoint = "FILL-IN-HERE";
        private static String clientKey = "FILL-IN-HERE";
    
        AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);   

De Data Lake Store SDK biedt handige methoden om de beveiligingstokens te beheren die nodig zijn om te communiceren met het Data Lake Store-account. Dit zijn echter niet de enige methoden die met SDK kunnen worden gebruikt. U kunt elke andere methode voor het verkrijgen van een token gebruiken. Zo kunt u de [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java) gebruiken, of uw persoonlijke code.

## <a name="next-steps"></a>Volgende stappen
In dit artikel leert u hebt geleerd hoe eindgebruikers verificatie gebruiken om te verifiëren met Azure Data Lake Store Java SDK gebruiken. U kunt nu de volgende artikelen die uitleggen hoe de Java SDK gebruiken om te werken met Azure Data Lake Store bekijken.

* [Bewerkingen van de gegevens in Data Lake Store met Java SDK](data-lake-store-get-started-java-sdk.md)


