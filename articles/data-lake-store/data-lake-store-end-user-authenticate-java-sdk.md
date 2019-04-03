---
title: 'Verificatie van eindgebruikers: Java met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van eindgebruikersverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory met behulp van Java
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 8b558fca964f33d47d331e007329d1bae2626877
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58881253"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-java"></a>Verificatie van de eindgebruiker met Azure Data Lake Storage Gen1 met behulp van Java
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK gebruiken](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-end-user-authenticate-python.md)
> * [REST API gebruiken](data-lake-store-end-user-authenticate-rest-api.md)
> 
>   

In dit artikel leert u over het gebruik van de Java-SDK voor verificatie van eindgebruikers met Azure Data Lake Storage Gen1 doen. Zie voor service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Java-SDK, [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Java](data-lake-store-service-to-service-authenticate-java.md).

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory-toepassing voor 'Native'**. U moet zijn voltooid de stappen in [eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

* [Maven](https://maven.apache.org/install.html). In deze zelfstudie wordt Maven gebruikt voor build- en projectafhankelijkheden. Hoewel het mogelijk is te ontwikkelen zonder een buildsysteem als Maven of Gradle, maken deze systemen het veel eenvoudiger om afhankelijkheden te beheren.

* (Optioneel) Een IDE zoals [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) of vergelijkbaar.

## <a name="end-user-authentication"></a>Verificatie van de eindgebruiker
1. Maak een Maven-project met behulp van [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) vanaf de opdrachtregel of met behulp van een IDE. [Hier](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) vindt u instructies over het maken van een Java-project met behulp van IntelliJ. [Hier](https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) vindt u instructies over het maken van een Java-project met behulp van Eclipse.

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
   
    De eerste afhankelijkheid is het gebruik van de Data Lake Storage-SDK met Gen1 (`azure-data-lake-store-sdk`) vanuit de maven-opslagplaats. De tweede afhankelijkheid dient om op te geven welk framework voor logboekregistratie (`slf4j-nop`) voor deze toepassing moet worden gebruikt. Maakt gebruik van de Data Lake Storage-SDK met Gen1 [slf4j](https://www.slf4j.org/) logboekregistratie gevel, kunt u kiezen uit een aantal populaire logboekregistratie frameworks als log4j, logback, enz., logboekregistratie Java of niet vastleggen. In dit voorbeeld wordt logboekregistratie uitgeschakeld. Daarom wordt de binding **slf4j-nop** gebruikt. [Hier](https://www.slf4j.org/manual.html#projectDep) vindt u andere opties voor logboekregistratie voor uw toepassing.

3. Voeg de volgende importinstructies toe aan uw toepassing.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.DeviceCodeTokenProvider;

4. Het volgende codefragment in uw Java-toepassing gebruiken om op te halen van het token voor de Active Directory systeemeigen toepassing die u hebt gemaakt met eerdere de `DeviceCodeTokenProvider`. Vervang **Fill-in-HERE** met de werkelijke waarden voor de systeemeigen Azure Active Directory-toepassing.

        private static String nativeAppId = "FILL-IN-HERE";
            
        AccessTokenProvider provider = new DeviceCodeTokenProvider(nativeAppId);   

De Data Lake Storage Gen1 SDK biedt handige methoden waarmee u kunnen de beveiligingstokens die nodig zijn om te communiceren met het Gen1 van Data Lake Storage-account beheren. Dit zijn echter niet de enige methoden die met SDK kunnen worden gebruikt. U kunt elke andere methode voor het verkrijgen van een token gebruiken. Zo kunt u de [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java) gebruiken, of uw persoonlijke code.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u verificatie van eindgebruikers om u te verifiëren met Azure Data Lake Storage Gen1 met behulp van Java-SDK. U kunt nu de volgende artikelen die bespreken hoe u de Java SDK gebruiken om te werken met Azure Data Lake Storage Gen1 kijken.

* [Bewerkingen van de gegevens in Data Lake Storage Gen1 met behulp van Java-SDK](data-lake-store-get-started-java-sdk.md)


