---
title: Maak een Azure-functie-app met Java en Eclipse | Microsoft Docs
description: Instructies voor het maken en publiceren van een eenvoudige HTTP geactiveerd serverloze app met behulp van Java en Eclipse om een Azure Functions.
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: Azure functions, functies, gebeurtenisverwerking, berekenen, architectuur zonder server, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: 373a35ea7b93c7717cd251e276be60b14df0920f
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39400425"
---
# <a name="create-your-first-function-with-java-and-eclipse-preview"></a>Uw eerste functie maken met Java en Eclipse (Preview)

> [!NOTE] 
> Java voor Azure Functions is momenteel in preview.

Dit artikel leest u over het maken van een [serverloze](https://azure.microsoft.com/overview/serverless-computing/) function-project met de Eclipse IDE en Apache Maven, testen en foutopsporing kunt uitvoeren en vervolgens implementeren op Azure Functions. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

Voor het ontwikkelen van een Azure functions-app met Java en Eclipse, hebt u het volgende zijn geïnstalleerd:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), versie 8.
-  [Apache Maven](https://maven.apache.org), versie 3.0 of hoger.
-  [Eclipse](https://www.eclipse.org/downloads/packages/), met Java en Maven ondersteunen.
-  [Azure-CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> De omgevingsvariabele JAVA_HOME moet zijn ingesteld op de installatielocatie van de JDK om deze quickstart te kunnen voltooien.

Het raadzaam ook installeren [versie 2 van Azure Functions Core Tools](functions-run-local.md#v2), die een lokale omgeving voor het uitvoeren en debuggen van Azure Functions bieden. 

## <a name="create-a-functions-project"></a>Een Functions-project maken

1. Selecteer in Eclipse het **bestand** in het menu Selecteer vervolgens **Project**. 
1. Open de **Java-Project** map in de **nieuw Project** venster en selecteer **Maven-Project**en selecteer vervolgens **volgende**.
1. Accepteer de standaardwaarden in de **nieuw Maven-Project** dialoog en selecteer **volgende**.
1. Selecteer **toevoegen Archetype** en toevoegen van de vermeldingen voor de [azure-functions-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
    - Archetype groeps-ID: com.microsoft.azure
    - Archetype artefact-ID: azure-functions-archetype
    - Versie: Gebruik meest recente versie van [de centrale opslagplaats](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![Eclipse Maven maken](media/functions-create-first-java-eclipse/functions-create-eclipse.png)  
1. Klik op **OK** en details invoeren voor het huidige project en uiteindelijk **voltooien**.

Maven maakt de projectbestanden in een nieuwe map met de naam _artifactId_. De gegenereerde code in het project is een eenvoudige [HTTP-geactiveerde](/azure/azure-functions/functions-bindings-http-webhook) -functie die kan de hoofdtekst van de activerende HTTP-aanvraag.

## <a name="run-functions-locally-in-the-ide"></a>Functies lokaal worden uitgevoerd in de IDE

> [!NOTE]
> [Azure Functions Core Tools, versie 2](functions-run-local.md#v2) uitvoeren en fouten opsporen in functies lokaal moet worden geïnstalleerd.

1. Met de rechtermuisknop op het gegenereerde project en kies vervolgens **uitvoeren als** en **Maven build**.
1. In de **-configuratie bewerken** dialoogvenster Enter `package` in de **doelstellingen** en **naam** velden en selecteer vervolgens **uitvoeren**. Dit zal maken en de functiecode aan te geven.
1. Als de build voltooid is, een ander Run-configuratie maken zoals hierboven aangegeven, met behulp van `azure-functions:run` als het doel en de naam. Selecteer **uitvoeren** om uit te voeren van de functie in de IDE.

De runtime in het consolevenster beëindigen wanneer u bent met klaar testen van uw functie. Slechts één functie host kan actieve en actieve lokaal op een tijdstip zijn.

### <a name="debug-the-function-in-eclipse"></a>Fouten opsporen in de functie in Eclipse

Wijzig in het Run As-configuratie instellen in de vorige stap, `azure-functions:run` naar `mvn azure-functions:run -DenableDebug` en voer de bijgewerkte configuratie voor het starten van de functie-app in de foutopsporingsmodus.

Selecteer de **uitvoeren** menu's en open **Debug Configurations**. Kies **Remote Java Application** en een nieuw wachtwoord maken. Geef een naam op voor uw configuratie en vul de instellingen in. De poort moet consistent zijn met de poort voor foutopsporing openen door de functie host, die standaard is `5005`. Nadat Setup is voltooid, klikt u op `Debug` foutopsporing te starten.

![Functies debuggen in Eclipse](media/functions-create-first-java-eclipse/debug-configuration-eclipse.PNG)

Onderbrekingspunten instellen en controleren van objecten in de functie met behulp van de IDE. Wanneer u klaar bent, stop de foutopsporing en de actieve functie host. Slechts één functie host kan worden actieve en actieve lokaal op tegelijk.

## <a name="deploy-the-function-to-azure"></a>De functie implementeren in Azure

Bij het implementeren naar Azure Functions worden accountreferenties uit de Azure CLI gebruikt. [Meld u aan met de Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) voordat u doorgaat met de opdrachtprompt van uw computer.

```azurecli
az login
```

Implementeer de code in een nieuwe functie app via de `azure-functions:deploy` Maven-doel in een nieuwe **uitvoeren als** configuratie.

Als het implementeren is voltooid, ziet u de URL die u kunt gebruiken voor toegang tot de Azure-functie-app:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

## <a name="next-steps"></a>Volgende stappen

- Neem de [Azure Functions Java-handleiding voor ontwikkelaars](functions-reference-java.md) door voor meer informatie over het ontwikkelen van Java-functies. Deze handleiding is vooralsnog door een vertaalmachine vertaald.
- U kunt extra functies met verschillende triggers toevoegen aan uw project met behulp van de Maven-target `azure-functions:add`.
