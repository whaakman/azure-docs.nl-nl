---
title: Een Azure-functie maken met Java en IntelliJ | Microsoft Docs
description: Informatie over het maken en publiceren van een eenvoudige HTTP-geactiveerde, serverloze Apps op Azure met Java en IntelliJ.
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: Azure functions, functies, gebeurtenisverwerking, berekenen, architectuur zonder server, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: eb8499ef6c0f872a0761f7be606e058387947b2b
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319877"
---
# <a name="create-your-first-azure-function-with-java-and-intellij-preview"></a>Uw eerste Azure-functie maken met Java en IntelliJ (preview)

> [!NOTE]
> Java voor Azure Functions is momenteel in preview.

In dit artikel wordt beschreven:
- Over het maken van een [serverloze](https://azure.microsoft.com/overview/serverless-computing/) function-project met IntelliJ IDEA en Apache Maven
- Stappen voor het testen en foutopsporing van de functie in de geïntegreerde ontwikkelomgeving (IDE) op uw eigen computer
- Instructies voor het implementeren van de function-project naar Azure Functions

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

Voor het ontwikkelen van een functie met een Java- en IntelliJ, moet u de volgende software installeren:

- [Java Developer Kit](https://www.azul.com/downloads/zulu/) (JDK), versie 8
- [Apache Maven](https://maven.apache.org), versie 3.0 of hoger
- [IntelliJ IDEA](https://www.jetbrains.com/idea/download), Community of Ultimate-versies met Maven
- [Azure-CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT]
> De omgevingsvariabele JAVA_HOME moet worden ingesteld op de locatie waar de JDK is geïnstalleerd om de stappen in dit artikel te voltooien.

 Het is raadzaam dat u installeert [versie 2 van Azure Functions Core Tools](functions-run-local.md#v2). Het biedt een lokale ontwikkelingsomgeving voor schrijven, uitvoeren en debuggen van Azure Functions.

## <a name="create-a-functions-project"></a>Een Functions-project maken

1. Selecteer in IntelliJ IDEA, **nieuw Project maken**.  
1. In de **nieuw Project** venster **Maven** in het linkerdeelvenster.
1. Selecteer de **maken archetype** selectievakje en selecteer vervolgens **toevoegen Archetype** voor de [azure-functions-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
1. In de **Archetype toevoegen** venster, vul de velden als volgt:
    - _Groeps-id_: com.microsoft.azure
    - _ArtifactId_: azure-functions-archetype
    - _Versie_: Gebruik de nieuwste versie van [de centrale opslagplaats](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![een Maven-project maken vanuit archetype in IntelliJ IDEA](media/functions-create-first-java-intellij/functions-create-intellij.png)  
1. Selecteer **OK**, en selecteer vervolgens **volgende**.
1. Voer de details in voor de huidige project en selecteer **voltooien**.

Maven maakt de projectbestanden in een nieuwe map met dezelfde naam als de _ArtifactId_ waarde. De gegenereerde code van het project is een eenvoudige [HTTP-geactiveerde](/azure/azure-functions/functions-bindings-http-webhook) -functie die kan de hoofdtekst van de activerende HTTP-aanvraag.

## <a name="run-functions-locally-in-the-ide"></a>Functies lokaal worden uitgevoerd in de IDE

> [!NOTE]
> Als u wilt uitvoeren en fouten opsporen in functies lokaal, zorg ervoor dat u hebt geïnstalleerd [versie 2 van Azure Functions Core Tools](functions-run-local.md#v2).

1. In- of wijzigingen handmatig importeren [automatisch importeren](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html).
1. Open de **Maven-projecten** werkbalk.
1. Vouw **Lifecycle**, en open vervolgens **pakket**. De oplossing is gebouwd en verpakt in een nieuwe doeldirectory.
1. Vouw **invoegtoepassingen** > **azure-functies** en open **azure-functies: uitvoeren** starten van de lokale Azure Functions-runtime.  
  ![Maven-werkbalk voor Azure Functions](media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

1. Het dialoogvenster uitvoeren wanneer u bent met klaar testen van uw functie. Slechts één functie host kan actieve en actieve lokaal op een tijdstip zijn.

## <a name="debug-the-function-in-intellij"></a>Fouten opsporen in de functie in IntelliJ

1. Voor het starten van de functie-host in de foutopsporingsmodus, toevoegen **- DenableDebug** als het argument tijdens het uitvoeren van uw functie. U kunt de configuratie in ofwel wijzigen [maven doelstellingen](https://www.jetbrains.com/help/idea/maven-support.html#run_goal) of Voer de volgende opdracht uit in een terminalvenster:  

   ```
   mvn azure-functions:run -DenableDebug
   ```

   Met deze opdracht zorgt ervoor dat de functie-host aan een poort voor foutopsporing op 5005 openen.

1. Op de **uitvoeren** in het menu **configuraties bewerken**.
1. Selecteer **(+)** om toe te voegen een **externe**.
1. Voltooi de _naam_ en _instellingen_ velden en selecteer vervolgens **OK** aan de configuratie op te slaan.
1. Nadat Setup is voltooid, selecteert u **< naam van externe configuratie > fouten opsporen in** of druk op Shift + F9 op het toetsenbord foutopsporing te starten.

   ![Functies debuggen in IntelliJ](media/functions-create-first-java-intellij/debug-configuration-intellij.PNG)

1. Wanneer u klaar bent, stopt u het foutopsporingsprogramma en het proces dat wordt uitgevoerd. Slechts één functie host kan actieve en actieve lokaal op een tijdstip zijn.

## <a name="deploy-the-function-to-azure"></a>De functie implementeren in Azure

1. Voordat u uw functie naar Azure implementeren kunt, moet u [Meld u aan met behulp van de Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

   ``` azurecli
   az login
   ```

1. Implementeer de code in een nieuwe functie met behulp van de `azure-functions:deploy` Maven-target. U kunt ook selecteren de **azure-functies: implementeren** optie in het venster Maven-projecten.

   ```
   mvn azure-functions:deploy
   ```

1. Zoek de URL voor uw functie in de Azure CLI-uitvoer nadat de functie is geïmplementeerd.

   ``` output
   [INFO] Successfully deployed Function App with package.
   [INFO] Deleting deployment package from Azure Storage...
   [INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
   [INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   ```

## <a name="next-steps"></a>Volgende stappen

- Neem de [Azure Functions Java-handleiding voor ontwikkelaars](functions-reference-java.md) door voor meer informatie over het ontwikkelen van Java-functies. Deze handleiding is vooralsnog door een vertaalmachine vertaald.
- Extra functies met verschillende triggers toevoegen aan uw project met behulp van de `azure-functions:add` Maven-target.
