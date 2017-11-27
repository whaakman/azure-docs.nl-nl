---
title: Azure IoT Suite en Logic Apps | Microsoft Docs
description: Een zelfstudie over het aansluiten van Logic Apps Azure IoT Suite voor bedrijfsproces.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 4a1db86f4b715533dfea545365eaf66de0574c5e
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Zelfstudie: Logic App verbinden met uw Azure IoT Suite Remote Monitoring vooraf geconfigureerde oplossing
De [Microsoft Azure IoT Suite] [ lnk-internetofthings] vooraf geconfigureerde oplossing voor externe controle is een uitstekende manier om snel aan de slag met een end-to-end-functieset die verklaart de IoT-oplossing. Deze zelfstudie leert u hoe u logische App toevoegt aan uw vooraf geconfigureerde oplossing voor externe controle van Microsoft Azure IoT Suite. Deze stappen laten zien hoe u uw IoT-oplossing nog verder door verbinding te maken aan een bedrijfsproces kunt nemen.

*Als u een overzicht over het inrichten van een externe controle vooraf geconfigureerde oplossing zoekt, Zie [zelfstudie: aan de slag met de vooraf geconfigureerde IoT-oplossingen][lnk-getstarted].*

Voordat u deze zelfstudie begint, moet u het volgende doen:

* De vooraf geconfigureerde oplossing voor externe controle in uw Azure-abonnement inrichten.
* Maak een account SendGrid zodat u een e-mailberichten die uw bedrijfsproces activeert. U kunt zich aanmelden voor een gratis proefaccount op [SendGrid](https://sendgrid.com/) door te klikken op **Probeer gratis**. Nadat u hebt geregistreerd voor een gratis proefversie account, moet u maken een [API-sleutel](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) in SendGrid die machtigingen verleent voor het verzenden van e-mail. Verderop in de zelfstudie moet u deze API-sleutel.

Voor deze zelfstudie hebt voltooid, moet u Visual Studio 2015 of Visual Studio 2017 te wijzigen van de acties in de vooraf geconfigureerde oplossing voor back-end.

Ervan uitgaande dat u al hebt ingericht uw externe controle vooraf geconfigureerde oplossing, gaat u naar de resourcegroep voor deze oplossing in de [Azure-portal][lnk-azureportal]. De resourcegroep heeft dezelfde naam als de oplossingsnaam van de u hebt gekozen wanneer u uw oplossing voor externe controle ingericht. In de resourcegroep ziet u alle ingerichte Azure-resources voor uw oplossing. De volgende schermafbeelding ziet u een voorbeeld **resourcegroep** blade voor een externe controle vooraf geconfigureerde oplossing:

![](media/iot-suite-v1-logic-apps-tutorial/resourcegroup.png)

Om te beginnen, de logic app instellen voor gebruik met de vooraf geconfigureerde oplossing.

## <a name="set-up-the-logic-app"></a>De logische App instellen
1. Klik op **toevoegen** boven aan de blade met resourcegroepen in de Azure portal.
2. Zoeken naar **logische App**, selecteert u deze en klik vervolgens op **maken**.
3. Vul de **naam** en gebruik van dezelfde **abonnement** en **resourcegroep** die u hebt gebruikt bij het inrichten van uw oplossing voor externe controle. Klik op **Create**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/createlogicapp.png)
4. Wanneer uw implementatie is voltooid, ziet u dat de logische App wordt vermeld als een resource in de resourcegroep.
5. Klik op de logische App om te navigeren naar de blade logische App, selecteer de **lege logische App** sjabloon openen de **Logic Apps Designer**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappsdesigner.png)
6. Selecteer **aanvragen**. Deze actie geeft aan dat een inkomende HTTP-aanvraag met een specifieke JSON nettolading handelingen als een trigger opgemaakt.
7. Plak de volgende code in de aanvraag hoofdtekst JSON-Schema:
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > Nadat u de logische app opslaan, maar u moet eerst een actie toevoegen, kunt u de URL voor het HTTP POST-protocol kopiëren.
   > 
   > 
8. Klik op **+ een nieuwe stap** onder de handmatige trigger. Klik vervolgens op **een actie toevoegen**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappcode.png)
9. Zoeken naar **SendGrid - e-mailbericht verzenden** en klik erop.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappaction.png)
10. Voer een naam voor de verbinding, zoals **SendGridConnection**, voer de **SendGrid-API-sleutel** u gemaakt wanneer u uw SendGrid-account instellen en klik op **maken**.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridconnection.png)
11. E-mailadressen die u voor beide bezit toevoegen de **van** en **naar** velden. Voeg **externe controle waarschuwing [DeviceId]** naar de **onderwerp** veld. In de **hoofdtekst van de E-mail** veld, het toevoegen van **apparaat [DeviceId] [measurementName] met de waarde [measuredValue] heeft gerapporteerd.** U kunt toevoegen **[DeviceId]**, **[measurementName]**, en **[measuredValue]** door te klikken in de **kunt u gegevens uit de vorige stappen** sectie.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridaction.png)
12. Klik op **opslaan** in het menu bovenaan.
13. Klik op de **aanvragen** trigger en kopieer de **Http Post naar deze URL** waarde. U moet deze URL verderop in deze zelfstudie.

> [!NOTE]
> Logic Apps kunnen u uitvoeren [veel verschillende soorten actie] [ lnk-logic-apps-actions] met inbegrip van acties in Office 365. 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a>De taak van de Web EventProcessor instellen
In deze sectie kunt u uw vooraf geconfigureerde oplossing verbinding met de logische App die u hebt gemaakt. Voor het voltooien van deze taak kunt u de URL voor het activeren van de logische App aan de actie die wordt geactiveerd wanneer de waarde van een apparaat sensor een drempelwaarde overschrijdt toevoegen.

1. De git-client gebruiken voor het klonen van de nieuwste versie van de [azure-iot-remote-monitoring github-opslagplaats][lnk-rmgithub]. Bijvoorbeeld:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. Open in Visual Studio de **RemoteMonitoring.sln** uit de lokale kopie van de opslagplaats.
3. Open de **ActionRepository.cs** bestand de **infrastructuur\\opslagplaats** map.
4. Update de **actionIds** woordenlijst met de **Http Post naar deze URL** hebt genoteerd uit uw logische App als volgt:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. De wijzigingen opslaan in de oplossing en Visual Studio af te sluiten.

## <a name="deploy-from-the-command-line"></a>Implementeren vanaf de opdrachtregel
In deze sectie kunt u de bijgewerkte versie van de oplossing voor externe controle ter vervanging van de versie die momenteel worden uitgevoerd in Azure implementeren.

1. Na de [dev set-up] [ lnk-devsetup] instructies voor het instellen van uw omgeving voor implementatie.
2. Volg voor het lokaal implementeren de [lokale implementatie] [ lnk-localdeploy] instructies.
3. Als u wilt implementeren in de cloud en bijwerken van uw bestaande cloudimplementatie, volgt u de [cloud implementatie] [ lnk-clouddeploy] instructies. De naam van uw oorspronkelijke implementatie gebruiken als de implementatienaam van de. Bijvoorbeeld als de oorspronkelijke implementatie is aangeroepen **demologicapp**, gebruik de volgende opdracht:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   Wanneer het build-script wordt uitgevoerd, moet u de dezelfde Azure-account, abonnement, regio en u hebt gebruikt toen u de oplossing ingericht Active Directory-exemplaar te gebruiken.

## <a name="see-your-logic-app-in-action"></a>Zie uw logische App in actie
De vooraf geconfigureerde oplossing voor externe controle heeft twee regels standaard ingesteld wanneer u een oplossing inricht. Beide regels zijn op de **SampleDevice001** apparaat:

* Temperatuur > 38.00
* Vochtigheid > 48,00

De regel temperatuur triggers de **verhogen Alarm** actie en de vochtigheid regel triggers de **SendMessage** in te grijpen. Ervan uitgaande dat u dezelfde URL voor beide acties gebruikt de **ActionRepository** klasse, uw logische app-triggers voor de regel. Beide regels SendGrid gebruiken voor het verzenden van een e-mail naar de **naar** adres met details van de waarschuwing.

> [!NOTE]
> De logische App blijft om te activeren elke keer dat de drempelwaarde wordt voldaan. Om te voorkomen onnodige e-mailberichten, kunt u de regels in de oplossingsportal uitschakelen of uitschakelen van de logische App in de [Azure-portal][lnk-azureportal].
> 
> 

Naast het e-mailberichten ontvangt, kunt u ook zien wanneer de logische App in de portal wordt uitgevoerd:

![](media/iot-suite-v1-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Volgende stappen
Nu dat u een logische App verbinding maken met de vooraf geconfigureerde oplossing een bedrijfsproces gebruikt hebt, kunt u meer informatie over de opties voor het aanpassen van de vooraf geconfigureerde oplossingen:

* [Dynamische telemetrie gebruiken met de vooraf geconfigureerde oplossing voor externe controle][lnk-dynamic]
* [Metagegevens van apparaten informatie in de vooraf geconfigureerde oplossing voor externe controle][lnk-devinfo]

[lnk-dynamic]: iot-suite-v1-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-v1-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
