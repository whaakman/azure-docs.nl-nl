---
title: Automatisch schalen Azure-resources op basis van prestatiegegevens of een planning | Microsoft Docs
description: Een instelling voor automatisch schalen voor een app service-abonnement met metrische gegevens en een planning maken
author: anirudhcavale
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: ancav
ms.custom: mvc
ms.openlocfilehash: 7e8d97657e03b0eaff76365d3988f51c773e3b55
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-autoscale-setting-for--azure-resources-based-on-performance-data-or-a-schedule"></a>Een instelling voor automatisch schalen voor Azure-resources op basis van prestatiegegevens of een planning maken

Instellingen voor automatisch schalen kunnen u exemplaren van service op basis van vooraf gedefinieerde voorwaarden toevoegen of verwijderen. Deze instellingen kunnen worden gemaakt via de portal. Deze methode biedt een gebruikersinterface voor het maken en configureren van een instelling voor automatisch schalen op basis van een browser. De stappen in deze zelfstudie via:

1. Maken van een App Service
2. Een instelling voor automatisch schalen configureren
3. Activering van een scale-out-actie
4. Activering van een actie schaal in

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-web-app-and-app-service-plan"></a>Een Web-App en een App Service-abonnement maken
1. Klik op de **nieuw** optie in het navigatiedeelvenster links
2. Zoek en selecteer de *Web-App* item en klik op **maken**
3. Selecteer een appnaam zoals *MyTestScaleWebApp*. Maak een nieuwe resourcegroep * myResourceGroup' en plaats deze in de resourcegroep van uw keuze.
4. Binnen een paar minuten moeten uw resources worden ingericht. We verwijzen naar de Web-App en bijbehorende App Service-Plan die alleen zijn gemaakt in de rest van deze zelfstudie.

    ![Een nieuwe app service maken in de portal](./media/monitor-tutorial-autoscale-performance-schedule/Web-App-Create.png)

## <a name="navigate-to-autoscale-settings"></a>Navigeer naar de instellingen voor automatisch schalen
1. Selecteer in het navigatiedeelvenster links de **Monitor** optie. Zodra de pagina wordt geladen Selecteer de **automatisch schalen** tabblad.
2. Een lijst met de bronnen die ondersteuning bieden voor automatisch schalen in uw abonnement worden hier weergegeven. Identificeren van de App Service-Plan die eerder in de zelfstudie is gemaakt en klik erop.

    ![Navigeer naar de instellingen voor automatisch schalen](./media/monitor-tutorial-autoscale-performance-schedule/monitor-blade-autoscale.png)

3. Klik op de instelling voor automatisch schalen op de **inschakelen voor automatisch schalen** knop

De volgende enkele stappen help dat wordt ingevuld van het scherm automatisch schalen eruitzien zoals de volgende afbeelding:

   ![Opslaan van de instelling voor automatisch schalen](./media/monitor-tutorial-autoscale-performance-schedule/Autoscale-Setting-Save.png)

 ## <a name="configure-default-profile"></a>Standaard-profiel configureren
1. Geef een **naam** voor de instelling voor automatisch schalen
2. In de standaard-profiel, zorg ervoor dat de **Scale modus** is ingesteld op 'Aanpassen aan een specifieke exemplaren'
3. Het aantal exemplaren wordt ingesteld op 1. Deze instelling zorgt ervoor dat wanneer er geen ander profiel actief is, of in feite de standaard-profiel als resultaat het aantal exemplaren op 1 geeft.

  ![Navigeer naar de instellingen voor automatisch schalen](./media/monitor-tutorial-autoscale-performance-schedule/autoscale-setting-profile.png)


## <a name="create-recurrence-profile"></a>Terugkeerpatroon profiel maken

1. Klik op de **een scale-voorwaarde toevoegen** koppeling onder het standaardprofiel

2. Bewerk de **naam** van dit profiel zijn 'Maandag tot vrijdag van profiel'

3. Zorg ervoor dat de **Scale modus** is ingesteld op 'Schalen op basis van een metriek'

4. Voor **exemplaar limieten** ingesteld de **Minimum** als '1', de **maximale** als '2' en de **standaard** als '1'. Dit zorgt ervoor dat dit profiel komt niet voor automatisch schalen die de service-abonnement hebt minder dan 1 exemplaar of meer dan 2 exemplaren. Als het profiel niet voldoende gegevens om een beslissing te nemen heeft, wordt het aantal exemplaren (in dit geval 1).

5. Voor **planning** Selecteer 'Repeat specifieke dagen"

6. Het profiel moet worden herhaald van maandag tot vrijdag van 09:00 PST tot 18:00 PST ingesteld. Dit zorgt ervoor dat dit profiel alleen actieve en toepasselijke 9: 00 uur op 18: 00 uur, maandag tot vrijdag. Het profiel 'Default' is tijdens alle andere tijden het profiel maakt gebruik van de instelling voor automatisch schalen.

## <a name="create-a-scale-out-rule"></a>Een scale-out-regel maken

1. In de 'maandag tot vrijdag profiel'

2. Klik op de **een regel toevoegen** koppeling

3. Stel de **metrische bron** moet 'andere resource'. Stel de **brontype** als de App-Services en de **Resource** als de Web-App eerder in deze zelfstudie hebt gemaakt.

4. Stel de **aggregatie tijd** als 'Totaal' de **metrische naam** als 'Aanvragen', en de **tijd gebruikerssegmentatie statistiek** als 'Sum'

5. Stel de **Operator** als 'Groter dan', de **drempelwaarde** als "10" en de **duur** '5' minuten.

6. Selecteer de **bewerking** als 'Toename count door', de **aantal exemplaar** als '1', en de **Cool omlaag** '5' minuten

7. Klik op de **toevoegen** knop

Deze regel zorgt ervoor dat als uw Web-App meer dan 10 aanvragen binnen 5 minuten ontvangt of minder, een extra exemplaar wordt toegevoegd aan uw App Service-Plan voor het beheren van belasting.

   ![Een scale-out-regel maken](./media/monitor-tutorial-autoscale-performance-schedule/Scale-Out-Rule.png)

## <a name="create-a-scale-in-rule"></a>Maak een regel schaal in
Wij raden u altijd een regel schaal in vergezeld gaan van een regel scale-out hebben. Beide zorgt ervoor dat uw resources niet meer dan zijn ingericht. Ten opzichte van inrichting manier hebt u meer exemplaren met dan nodig is om de huidige belasting te verwerken. 

1. In de 'maandag tot vrijdag profiel'

2. Klik op de **een regel toevoegen** koppeling

3. Stel de **metrische bron** moet 'andere resource'. Stel de **brontype** als de App-Services en de **Resource** als de Web-App eerder in deze zelfstudie hebt gemaakt.

4. Instellen de **tijd aggregatie** als 'Totaal' de **metrische naam** als 'Aanvragen', en de **tijd gebruikerssegmentatie statistiek** als 'Gemiddelde'

5. Stel de **Operator** als 'Kleiner zijn dan', de **drempelwaarde** als '5' en de **duur** '5' minuten.

6. Selecteer de **bewerking** als 'Count verlagen door', de **aantal exemplaar** als '1', en de **Cool omlaag** '5' minuten

7. Klik op de **toevoegen** knop

    ![Maak een regel schaal in](./media/monitor-tutorial-autoscale-performance-schedule/Scale-In-Rule.png)

8. **Sla** instelling voor automatisch schalen

    ![Opslaan van de instelling voor automatisch schalen](./media/monitor-tutorial-autoscale-performance-schedule/Autoscale-Setting-Save.png)

## <a name="trigger-scale-out-action"></a>Scale-out triggeractie
De Web-App moet meer dan 10 aanvragen hebben in minder dan vijf minuten zodat de voorwaarde van de scale-out in de instelling voor automatisch schalen zojuist hebt gemaakt.

1. Open een browservenster en navigeer naar de Web-App die eerder in deze zelfstudie hebt gemaakt. U vindt de URL voor uw Web-App in de Azure Portal door te navigeren naar uw Web-App-resource en te klikken op de **Bladeren** knop op het tabblad 'Overzicht'.

2. Snel achter elkaar laad de pagina opnieuw maximaal 10 keer

3. Selecteer in het navigatiedeelvenster links de **Monitor** optie. Zodra de pagina wordt geladen Selecteer de **automatisch schalen** tabblad.

4. Selecteer de App Service-Plan die in deze zelfstudie wordt gebruikt in de lijst

5. Klik op de instelling voor automatisch schalen op de **Uitvoeringsgeschiedenis** tabblad

6. U ziet een diagram weergeven van het aantal exemplaren van de App Service-abonnement gedurende een periode

7. Het aantal exemplaren moet over een paar minuten oplopen op 1, 2.

8. Onder de grafiek ziet u de activiteit logboekvermeldingen voor elke schaalactie op die door deze instelling voor automatisch schalen.

## <a name="trigger-scale-in-action"></a>Schaal in actie activeren
De schaal in voorwaarde in de instelling van triggers als er minder dan 5 aanvragen naar de Web-App gedurende een periode van tien minuten worden voor automatisch schalen. 

1. Zorg ervoor dat er geen aanvragen worden verzonden naar uw Web-App

2. Laden van de Azure-Portal

3. Selecteer in het navigatiedeelvenster links de **Monitor** optie. Zodra de pagina wordt geladen Selecteer de **automatisch schalen** tabblad.

4. Selecteer de App Service-Plan die in deze zelfstudie wordt gebruikt in de lijst

5. Klik op de instelling voor automatisch schalen op de **Uitvoeringsgeschiedenis** tabblad

6. U ziet een diagram weergeven van het aantal exemplaren van de App Service-abonnement gedurende een bepaalde periode.

7. Het aantal exemplaren moet over een paar minuten verwijderen uit 2, op 1. Het proces duurt ten minste tien minuten.  

8. Onder de grafiek worden de bijbehorende reeks activiteit logboekvermeldingen voor elke schaalactie op die door deze instelling voor automatisch schalen

    ![Schaal in acties weergeven](./media/monitor-tutorial-autoscale-performance-schedule/Scale-In-Chart.png)

## <a name="clean-up-resources"></a>Resources opschonen

1. Klik in het menu links in de Azure portal op **alle resources** en selecteer vervolgens de Web-App gemaakt in deze zelfstudie.

2. Klik op de bronpagina op **verwijderen**, verwijderen bevestigen door op te geven **Ja** in het tekstvak en klik vervolgens op **verwijderen**.

3. Vervolgens selecteert u de resource App Service-Plan en klik op **verwijderen**

4. Verwijderen bevestigen door op te geven **Ja** in het tekstvak en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een eenvoudige Web-App en gemaakt App Service-Plan. U een instelling voor automatisch schalen die u wilt schalen de App Service-Plan op basis van het aantal aanvragen het ontvangen van de Web-App wordt gemaakt. Voor meer informatie over automatisch schalen instellingen door te gaan naar het overzicht voor automatisch schalen.

> [!div class="nextstepaction"]
> [De controlegegevens archiveren](./monitor-tutorial-archive-monitoring-data.md)
