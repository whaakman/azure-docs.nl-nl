---
title: 'Snelstartgids: spraak herkennen in Java (Windows of Linux)'
titleSuffix: Azure Cognitive Services
description: Meer informatie over spraak herkennen in Java (Windows of Linux)
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 10/12/2018
ms.author: fmegen
ms.openlocfilehash: 80ddef79392acb677555ed795bf429f5ec0266a0
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49467256"
---
# <a name="quickstart-recognize-speech-in-java-on-windows-or-linux-by-using-the-speech-service-sdk"></a>Snelstart: Spraak herkennen in Java (Windows of Linux) met behulp van de Speech Service-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een Java-consoletoepassing met behulp van de [Speech Service-SDK](speech-sdk.md). Doel is om realtime spraak naar tekst om te zetten vanuit de microfoon van uw pc. De toepassing wordt gebouwd met het Speech SDK Maven-pakket en de Eclipse Java IDE (v4.8) op 64-bit Windows of Ubuntu Linux 16.04. De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

> [!NOTE]
> Zie [Speech Devices SDK](speech-devices-sdk.md) voor de Speech Devices-SDK en het Roobo-apparaat.

## <a name="prerequisites"></a>Vereisten

U hebt een abonnementssleutel voor de Speech-service nodig om deze snelstartgids te doorlopen. U kunt er gratis een krijgen. Zie [Speech Service gratis uitproberen](get-started.md) voor de details.


## <a name="create-and-configure-project"></a>Project maken en configureren

Als u Ubuntu 16.04 gebruikt, voer dan vóór het starten van Eclipse de volgende opdrachten uit om ervoor te zorgen dat de vereiste pakketten geïnstalleerd zijn.

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

1. Start Eclipse.

1. Voer in de Eclipse Launcher, in het veld **Workspace**, de naam van een nieuwe werkruimtemap in. Selecteer vervolgens **Starten**.

   ![Schermopname van Eclipse Launcher](media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Binnen enkele ogenblikken verschijnt het hoofdvenster van de Eclipse-IDE. Sluit het welkomstscherm als dit verschijnt.

1. Maak via de Eclipse-menubalk een nieuw project aan door **Bestand** > **Nieuw** > **Project** te kiezen.

1. Het dialoogvenster **Nieuw project** wordt weergegeven. Selecteer **Java-project** en vervolgens **Volgende**.

   ![Schermopname van het dialoogvenster Nieuw project, met Java-project gemarkeerd](media/sdk/qs-java-jre-02-select-wizard.png)

1. De wizard Nieuw Java-project wordt gestart. Voer in het veld **Projectnaam** **quickstart** in en kies **JavaSE-1.8** als uitvoeringsomgeving. Selecteer **Voltooien**.

   ![Schermopname van de wizard Nieuw Java-project](media/sdk/qs-java-jre-03-create-java-project.png)

1. Als het venster **Gekoppeld perspectief openen?** wordt weergegeven, selecteert u **Perspectief openen**.

1. Klik in de **Package explorer** met de rechtermuisknop op het **quickstart**-project. Kies in het contextmenu **Configureren** > **Naar Maven-project converteren**.

   ![Schermopname van Package explorer](media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. Het venster **Nieuw POM maken** wordt weergegeven. Voer in het veld **Groeps-ID** **com.microsoft.cognitiveservices.speech.samples** in, en voer in het veld **Artefact-ID** **quickstart** in. Selecteer vervolgens **Voltooien**.

   ![Schermopname van het venster Nieuw POM maken](media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. Open het bestand **pom.xml** en bewerk dit.

   * Maak aan het einde van het bestand, vóór de afsluitende tag `</project>`, een `repositories`-element met een verwijzing naar de Maven-opslagplaats voor de Speech-SDK, zoals hier getoond:

     [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#repositories)]

  * Voeg ook een `dependencies`-element toe met Speech SDK versie 1.0.1 als afhankelijkheid:

     [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#dependencies)]

   * Sla de wijzigingen op.

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Om een nieuwe lege klasse aan uw Java-project toe te voegen, selecteert u **Bestand** > **Nieuw** > **Klasse**.

1. Voer in het venster **Nieuwe Java-klasse**, in het veld **Pakket**, **speechsdk.quickstart** in en voer in het veld **Naam** **Main** in.

   ![Schermopname van het venster Nieuwe Java-klasse](media/sdk/qs-java-jre-06-create-main-java.png)

1. Vervang alle code in `Main.java` door het volgende codefragment:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

Druk op F11 of selecteer **Uitvoeren** > **Fouten opsporen**.
De volgende 15 seconden aan spraakinvoer vanuit uw microfoon worden herkend en geregistreerd in het consolevenster.

![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-java-jre-07-console-output.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek naar dit voorbeeld in de map `quickstart/java-jre`.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Intenties van gesproken inhoud herkennen met behulp van de Speech-SDK voor Java](how-to-recognize-intents-from-speech-java.md)

## <a name="see-also"></a>Zie ook

- [Spraak vertalen](how-to-translate-speech-csharp.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
