---
title: Azure IoT Suite en Logic Apps | Microsoft Docs
description: Een zelfstudie over het koppelen van logische Apps met Azure IoT Suite voor zakelijke processen.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 4a1db86f4b715533dfea545365eaf66de0574c5e
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106851"
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Zelfstudie: Een logische App aansluiten op uw Azure IoT Suite Remote Monitoring vooraf geconfigureerde oplossing
De [Microsoft Azure IoT Suite] [ lnk-internetofthings] vooraf geconfigureerde oplossing voor externe controle is een uitstekende manier om snel aan de slag met een end-to-end-functieset die een IoT-oplossing verklaart. In deze zelfstudie leert u hoe u logische App toevoegen aan uw vooraf geconfigureerde oplossing voor externe controle van Microsoft Azure IoT Suite. Deze stappen laten zien hoe u uw IoT-oplossing nog verder verbinding maken met een bedrijfsproces kunt uitvoeren.

*Als u een overzicht van hoe u voor het inrichten van een externe controle vooraf geconfigureerde oplossing zoekt, raadpleegt u [zelfstudie: aan de slag met de vooraf geconfigureerde IoT-oplossingen][lnk-getstarted].*

Voordat u deze zelfstudie begint, moet u:

* Richt de vooraf geconfigureerde oplossing voor externe controle in uw Azure-abonnement.
* Maak een SendGrid-account zodat u kunt een e-mailberichten die uw bedrijfsprocessen wordt geactiveerd. U kunt zich aanmelden voor een gratis proefaccount bij [SendGrid](https://sendgrid.com/) door te klikken op **gratis proberen**. Nadat u hebt geregistreerd voor uw account voor gratis proefversie, moet u maken een [API-sleutel](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) in SendGrid die machtigingen voor het verzenden van e-mail verleent. Deze API-sleutel moet u later in de zelfstudie.

Voor deze zelfstudie, moet u Visual Studio 2015 of Visual Studio 2017, wijzigt u welke acties in de vooraf geconfigureerde oplossing voor back-end.

Ervan uitgaande dat u hebt al ingericht voor uw externe controle vooraf geconfigureerde oplossing, gaat u naar de resourcegroep voor die oplossing in de [Azure-portal][lnk-azureportal]. De resourcegroep heeft dezelfde naam als de naam van de oplossing u hebt gekozen wanneer u uw oplossing voor externe bewaking hebt ingericht. In de resourcegroep ziet u alle ingerichte Azure-resources voor uw oplossing. De volgende schermafbeelding ziet u een voorbeeld **resourcegroep** blade voor een externe controle vooraf geconfigureerde oplossing:

![](media/iot-suite-v1-logic-apps-tutorial/resourcegroup.png)

Als u wilt beginnen, instellen van de logische app voor gebruik met de vooraf geconfigureerde oplossing.

## <a name="set-up-the-logic-app"></a>Instellen van de logische App
1. Klik op **toevoegen** aan de bovenkant van uw resourcegroep-blade in Azure portal.
2. Zoeken naar **logische App**, selecteert u deze en klik vervolgens op **maken**.
3. Vul de **naam** en dezelfde **abonnement** en **resourcegroep** die u hebt gebruikt bij het inrichten van uw oplossing voor externe controle. Klik op **Create**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/createlogicapp.png)
4. Wanneer uw implementatie is voltooid, ziet u dat de logische App wordt vermeld als een resource in de resourcegroep.
5. Klik op de logische App om te navigeren naar de blade logische App, selecteer de **lege logische App** sjabloon openen de **ontwerper van logische Apps**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappsdesigner.png)
6. Selecteer **aanvragen**. Deze actie geeft aan dat een binnenkomende HTTP-aanvraag met een specifieke JSON nettolading besluiten als een trigger opgemaakt.
7. Plak de volgende code in de aanvraag hoofdtekst van de JSON-Schema:
   
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
   > Nadat u de logische app opslaan, maar u moet eerst een actie toevoegen, kunt u de URL voor de HTTP post kopiëren.
   > 
   > 
8. Klik op **+ nieuwe stap** onder de handmatige trigger. Klik vervolgens op **een actie toevoegen**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappcode.png)
9. Zoeken naar **SendGrid - e-mail verzenden** en klik erop.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappaction.png)
10. Voer een naam op voor de verbinding, zoals **SendGridConnection**, voer de **SendGrid-API-sleutel** u hebt gemaakt toen u uw SendGrid-account instellen en op **maken**.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridconnection.png)
11. Scheid e-mailadressen die u voor beide bezit de **van** en **naar** velden. Voeg **externe bewakingswaarschuwing [apparaat-id]** naar de **onderwerp** veld. In de **de hoofdtekst van E-mail** veld, toe te voegen **apparaat [apparaat-id] [measurementName] met de waarde [measuredValue] heeft gerapporteerd.** U kunt toevoegen **[apparaat-id]**, **[measurementName]**, en **[measuredValue]** door te klikken in de **kunt u gegevens uit de vorige stappen invoegen** de sectie.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridaction.png)
12. Klik op **opslaan** in het bovenste menu.
13. Klik op de **aanvragen** trigger en kopieer de **Http Post naar deze URL** waarde. U moet deze URL verderop in deze zelfstudie.

> [!NOTE]
> Logische Apps kunt u om uit te voeren [veel verschillende typen actie] [ lnk-logic-apps-actions] met inbegrip van acties in Office 365. 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a>Instellen van de EventProcessor Web Job
In deze sectie kunt u uw vooraf geconfigureerde oplossing voor verbinding met de logische App die u hebt gemaakt. Als u wilt deze taak hebt voltooid, moet u de URL voor het activeren van de actie die wordt geactiveerd wanneer een apparaat sensor drempelwaarde wordt overschreden met de logische App toevoegen.

1. Uw git-client gebruiken om te klonen van de meest recente versie van de [azure-iot-remote-monitoring github-opslagplaats][lnk-rmgithub]. Bijvoorbeeld:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. Open in Visual Studio, de **RemoteMonitoring.sln** van de lokale kopie van de opslagplaats.
3. Open de **ActionRepository.cs** -bestand in de **infrastructuur\\opslagplaats** map.
4. Update de **actionIds** -woordenlijst met de **Http Post naar deze URL** u als volgt van uw logische App hebt genoteerd:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. Sla de wijzigingen in de oplossing en Visual Studio af te sluiten.

## <a name="deploy-from-the-command-line"></a>Implementeren vanaf de opdrachtregel
In deze sectie maakt implementeren u de bijgewerkte versie van de oplossing voor externe controle te vervangen door de versie die momenteel wordt uitgevoerd in Azure.

1. Na de [dev set-up] [ lnk-devsetup] instructies voor het instellen van uw omgeving voor implementatie.
2. Volg voor het lokaal implementeren de [lokale implementatie] [ lnk-localdeploy] instructies.
3. Als u wilt implementeren in de cloud en de implementatie van uw bestaande cloud bijwerken, gaat u als volgt de [implementatie in de cloud] [ lnk-clouddeploy] instructies. Gebruik de naam van uw oorspronkelijke implementatie als de implementatienaam van de. Bijvoorbeeld als de oorspronkelijke implementatie heette **demologicapp**, gebruikt u de volgende opdracht:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   Wanneer de build-script wordt uitgevoerd, worden zorg ervoor dat u de dezelfde Azure-account, abonnementen, regio en Active Directory-exemplaar die u hebt gebruikt toen u de oplossing hebt ingericht.

## <a name="see-your-logic-app-in-action"></a>Uw logische App in actie zien
De vooraf geconfigureerde oplossing voor externe controle heeft twee regels standaard ingesteld wanneer u een oplossing inricht. Beide regels zijn op de **SampleDevice001** apparaat:

* Temperatuur > 38.00
* Vochtigheid > 48,00

De temperatuur regel wordt geactiveerd de **verhogen Alarm** actie en de vochtigheid regel triggers de **SendMessage** actie. Ervan uitgaande dat u dezelfde URL gebruikt voor beide acties de **ActionRepository** klasse, de triggers voor logische app voor de regel. Beide regels SendGrid gebruiken om een e-mail verzenden naar de **naar** -adres met details van de waarschuwing.

> [!NOTE]
> De logische App blijft activeren telkens wanneer de drempelwaarde wordt voldaan. Als u wilt voorkomen dat onnodige e-mailberichten, kunt u de regels in de oplossingsportal uitschakelen of uitschakelen van de logische App in de [Azure-portal][lnk-azureportal].
> 
> 

Naast het e-mailberichten ontvangt, kunt u ook zien wanneer de logische App wordt uitgevoerd in de portal:

![](media/iot-suite-v1-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Volgende stappen
Nu dat u een logische App de vooraf geconfigureerde oplossing verbinden met een bedrijfsproces gebruikt hebt, kunt u meer leren over de opties voor het aanpassen van de vooraf geconfigureerde oplossingen:

* [Dynamische telemetrie gebruiken bij de vooraf geconfigureerde oplossing voor externe controle][lnk-dynamic]
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
