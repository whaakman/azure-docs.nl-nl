---
title: Maak een Azure-functie-app met Java- en IntelliJ | Microsoft Docs
description: Instructies voor het maken en publiceren van een eenvoudige HTTP geactiveerd serverloze app met behulp van Java- en IntelliJ naar Azure Functions.
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
ms.openlocfilehash: 5e265543e2ce5feeed095d89cdb47ede9817bad1
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40002675"
---
# <a name="create-your-first-function-with-java-and-intellij-preview"></a>Uw eerste functie maken met Java en IntelliJ (Preview)

> [!NOTE] 
> Java voor Azure Functions is momenteel in preview.

Dit artikel leest u over het maken van een [serverloze](https://azure.microsoft.com/overview/serverless-computing/) function-project met IntelliJ IDEA en Apache Maven, testen en foutopsporing kunt uitvoeren op uw eigen computer vanuit de IDE en deze implementeren in Azure Functions. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

Voor het ontwikkelen van een Azure functions-app met Java en IntelliJ, hebt u het volgende zijn geïnstalleerd:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), versie 8.
-  [Apache Maven](https://maven.apache.org), versie 3.0 of hoger.
-  [IntelliJ IDEA](https://www.jetbrains.com/idea/download), Community of Ultimate met Maven-hulpprogramma's.
-  [Azure-CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> De omgevingsvariabele JAVA_HOME moet zijn ingesteld op de installatielocatie van de JDK om deze quickstart te kunnen voltooien.

U wordt ten zeerste aanbevolen tevens installeert [versie 2 van Azure Functions Core Tools](functions-run-local.md#v2), die een lokale ontwikkelingsomgeving voor schrijven, uitvoeren en debuggen van Azure Functions bieden. 


## <a name="create-a-functions-project"></a>Een Functions-project maken

1. Klik in IntelliJ IDEA, op **nieuw Project maken**.  
1. Selecteer om te maken op basis van **Maven**
1. Het selectievakje voor het **maken archetype** en **toevoegen Archetype** voor de [azure-functions-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
    - Groeps-id: com.microsoft.azure
    - ArtifactId: azure-functions-archetype
    - Versie: Gebruik meest recente versie van [de centrale opslagplaats](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![IntelliJ Maven maken](media/functions-create-first-java-intellij/functions-create-intellij.png)  
1. Klik op **volgende** en details invoeren voor het huidige project en uiteindelijk **voltooien**.

Maven maakt de projectbestanden in een nieuwe map met de naam _artifactId_. De gegenereerde code in het project is een eenvoudige [HTTP-geactiveerde](/azure/azure-functions/functions-bindings-http-webhook) -functie die kan de hoofdtekst van de activerende HTTP-aanvraag.

## <a name="run-functions-locally-in-the-ide"></a>Functies lokaal worden uitgevoerd in de IDE

> [!NOTE]
> [Azure Functions Core Tools, versie 2](functions-run-local.md#v2) uitvoeren en fouten opsporen in functies lokaal moet worden geïnstalleerd.

1. Selecteer om te importeren van wijzigingen of zorg ervoor dat [automatisch importeren](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html) is ingeschakeld.
1. Open de **Maven-projecten** werkbalk
1. Dubbelklik onder levenscyclus, op **pakket** kunnen worden verpakt en maak de oplossing en maak een doelmap.
1. Onder invoegtoepassingen -> azure-functies dubbelklikken **azure-functies: uitvoeren** starten van de lokale azure functions-runtime.  
  ![Maven-werkbalk voor Azure Functions](media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

Sluit het dialoogvenster uitvoeren als u klaar bent uw functie testen. Slechts één functie host kan actieve en actieve lokaal op een tijdstip zijn.

### <a name="debug-the-function-in-intellij"></a>Fouten opsporen in de functie in IntelliJ
Voor het starten van de functie-host in de foutopsporingsmodus, toevoegen **- DenableDebug** als het argument tijdens het uitvoeren van uw functie. Kan uitvoeren onder vanaf de opdrachtregel in terminal of configureren in [maven doelstellingen](https://www.jetbrains.com/help/idea/maven-support.html#run_goal). Vervolgens wordt de functie host een poort voor foutopsporing geopend op 5005. 

```
mvn azure-functions:run -DenableDebug
```

Voor foutopsporing in IntelliJ, In de **uitvoeren** menu Selecteer **configuraties bewerken**. Klik op ** + ** om toe te voegen een **externe**. Vul in **naam** en **instellingen**, en klik vervolgens op **OK** aan de configuratie op te slaan. Nadat Setup is voltooid, klikt u op **Debug** 'Uw RAS-configuratie-Name' of drukt u op **Shift + F9** foutopsporing te starten.

![Functies debuggen in IntelliJ](media/functions-create-first-java-intellij/debug-configuration-intellij.PNG)

Wanneer u klaar bent stop de foutopsporing en het proces dat wordt uitgevoerd. Slechts één functie host kan worden actieve en actieve lokaal op tegelijk.

## <a name="deploy-the-function-to-azure"></a>De functie implementeren in Azure

Bij het implementeren naar Azure Functions worden accountreferenties uit de Azure CLI gebruikt. [Meld u aan met de Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) voordat u doorgaat met de opdrachtprompt van uw computer.

```azurecli
az login
```

Implementeer de code in een nieuwe functie app via de `azure-functions:deploy` Maven-target of Selecteer de azure-functie: optie in het venster Maven-projecten te implementeren.

```
mvn azure-functions:deploy
```

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
