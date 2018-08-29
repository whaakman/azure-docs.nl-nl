---
title: 'Snelstartgids: Herkennen gesproken tekst in Java (Windows of Linux)'
titleSuffix: Microsoft Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in Java (Windows of Linux)
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 08/16/2018
ms.author: fmegen
ms.openlocfilehash: 923ab3378d5e2d833e11c5111d4dd9964fea6dc4
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126610"
---
# <a name="quickstart-recognize-speech-in-java-windows-or-linux"></a>Snelstartgids: Herkennen gesproken tekst in Java (Windows of Linux)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dit document wordt beschreven hoe u een op basis van een Java-consoletoepassing maken voor Java Runtime Environment (JRE) die wordt gebruik gemaakt van de spraak-SDK.
De toepassing is gebaseerd op de Microsoft Cognitive Services SDK Maven-pakket.
We Eclipse gebruiken als een Integrated Development Environment (IDE).

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [de spraakservice gratis uitproberen](get-started.md).
* Een PC (Windows x64, Ubuntu 16.04 x64) in staat om het uitvoeren van Eclipse, met een werkende microfoon.
* 64-bits JRE/JDK voor Java 8.
* Versie 4.8 van [Eclipse](https://www.eclipse.org), 64-bits versie.
* Voer de volgende opdrachten voor de installatie van de vereiste pakketten op Ubuntu-16.04:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="create-and-configure-your-project"></a>Maken en configureren uw project

1. Start Eclipse.

1. Voer in het startprogramma voor Eclipse, de naam van een nieuwe map in de **werkruimte** veld.
   Klik vervolgens op **starten**.

   ![Eclipse-werkruimte maken](media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Het hoofdvenster van de Eclipse IDE wordt na een tijdje worden weergegeven.
   Als er een welkomstscherm wordt weergegeven in deze, sluit u deze.

1. Selecteer **bestand** \> **nieuwe** \> **Project**.

1. In de **nieuw Project** wizard die wordt weergegeven selecteert **Java-Project**, en klikt u op **volgende**.

   ![Selecteer een wizard](media/sdk/qs-java-jre-02-select-wizard.png)

1. Voer in het volgende venster **snelstartgids** als een project de naam en kies **JavaSE 1.8** (of omhoog) als uitvoeringsomgeving.
   Klik op **Voltooien**.

   ![Selecteer een wizard](media/sdk/qs-java-jre-03-create-java-project.png)

1. Als een venster met de titel **Open Associated Perspective?** pop up Selecteer **Open perspectief**.

1. In de **Package explorer**, met de rechtermuisknop op de **snelstartgids** project en selecteer **configureren** \> **converteren naar Maven-Project**.

   ![Converteren naar Maven-project](media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. Voer in het venster dat verschijnt, **com.microsoft.cognitiveservices.speech.samples** als **groeps-Id** en **snelstartgids** als **artefact-Id**. Selecteer **Voltooien**.

   ![Maven POM configureren](media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. Bewerk de **pom.xml** bestand.

  * Aan het einde van het bestand, voordat de eindcode `</project>`, maakt u een sectie opslagplaatsen met een verwijzing naar de Maven-opslagplaats voor de spraak-SDK:

    [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#repositories)]

  * Bovendien toevoegen daarna een gedeelte met afhankelijkheden met de spraak-SDK versie 0.6.0 als een afhankelijkheid:

    [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#dependencies)]

  * Sla de wijzigingen op.

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Selecteer **bestand** \> **nieuw** \> **klasse** een nieuwe lege klasse toevoegen aan uw Java-project.

1. In het venster **nieuwe Java-klasse** Voer **speechsdk.quickstart** in de **pakket** veld en **Main** in de **naam**  veld.

   ![Het maken van een Main-klasse](media/sdk/qs-java-jre-06-create-main-java.png)

1. Vervang alle code in `Main.java` door het volgende codefragment:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` met de abonnementssleutel van uw.

1. Vervang de tekenreeks `YourServiceRegion` met de [regio](regions.md) die zijn gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen aan het project.

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

Druk op F11, of selecteer **uitvoeren** \> **Debug**.
De volgende 15 seconden van spraakinvoer van de microfoon worden herkend en vastgelegd in het consolevenster.

![Console-uitvoer na geslaagde opname](media/sdk/qs-java-jre-07-console-output.png)

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek in dit voorbeeld in de `quickstart/java-jre` map.

## <a name="next-steps"></a>Volgende stappen

* [Onze voorbeelden ophalen](speech-sdk.md#get-the-samples)
