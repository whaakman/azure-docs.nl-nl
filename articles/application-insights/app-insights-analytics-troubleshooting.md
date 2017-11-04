---
title: Problemen met analyses in Azure Application Insights | Microsoft Docs
description: 'Problemen met Application Insights analytics? Begin hier. '
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/11/2016
ms.author: mbullwin
ms.openlocfilehash: 3ef9150cad62e9f9fc43f7afcbbbc01d89884a12
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="troubleshoot-analytics-in-application-insights"></a>Problemen met analyses in Application Insights oplossen
Problemen met [Application Insights Analytics](app-insights-analytics.md)? Begin hier. Analytics is een krachtige zoekprogramma van Azure Application Insights.

## <a name="limits"></a>Limieten
* Queryresultaten zijn momenteel beperkt tot alleen via een week van afgelopen gegevens.
* We testen op browsers: meest recente edities van Chrome, rand en Internet Explorer.

## <a name="known-incompatible-browser-extensions"></a>Bekende incompatibele browser-extensies
* Ghostery

De uitbreiding uitschakelen of een andere browser gebruikt.

## <a name="e-a"></a>'Fout'
![Onverwachte foutscherm](./media/app-insights-analytics-troubleshooting/010.png)

Er is een interne fout opgetreden tijdens de runtime portal: niet-verwerkte uitzondering.

* Opschonen van de browser-cache. 

## <a name="e-b"></a>403... Probeer het opnieuw laden
![403... Probeer het opnieuw laden](./media/app-insights-analytics-troubleshooting/020.png)

Er is een fout opgetreden die te maken heeft met het verificatieproces (tijdens de verificatie of tijdens het genereren van een toegangstoken). De portal kan hebben geen manier om te herstellen zonder browserinstellingen te wijzigen.

* Controleer of [cookies van derden zijn ingeschakeld](#cookies) in de browser. 

## <a name="authentication"></a>403... beveiligingszone controleren
![403.. .verify beveiligingszone](./media/app-insights-analytics-troubleshooting/030.png)

Er is een fout opgetreden die te maken heeft met het verificatieproces (tijdens de verificatie of tijdens het genereren van een toegangstoken). De portal kan hebben geen manier om te herstellen zonder browserinstellingen te wijzigen.

1. Controleer of [cookies van derden zijn ingeschakeld](#cookies) in de browser. 
2. Hebt u een favoriet, bladwijzer of opgeslagen koppeling gebruikt om de portal van Analytics te openen? Hebt u zich aangemeld met andere referenties dan die u hebt gebruikt bij het opslaan van de koppeling?
3. Probeer het met een privé-venster/incognito browservenster (nadat u alle vensters hebt gesloten). U moet dan uw referenties opgeven. 
4. Open een andere (gewone) browservenster en gaat u naar [Azure](https://portal.azure.com). Meld u af. Open vervolgens de koppeling en meld u met de juiste referenties aan.
5. Gebruikers van Edge en Internet Explorer kunnen deze fout ook krijgen als instellingen voor vertrouwde zones niet worden ondersteund.
   
    Controleer of beide [Analytics-portal](https://analytics.applicationinsights.io) en [Azure Active Directory-portal](https://portal.azure.com) zijn in de zone met dezelfde beveiliging:
   
   * Open in Internet Explorer, **Internetopties**, **beveiliging**, **vertrouwde websites**, **Sites**:
     
     ![Dialoogvenster Internet-opties, een site toe te voegen aan vertrouwde websites](./media/app-insights-analytics-troubleshooting/033.png)
     
     Als de lijst met websites een van de volgende URL's bevat, moet u de andere sites ook opnemen in de lijst:
     
     https://analytics.applicationinsights.io<br/>
     https://login.microsoftonline.com<br/>
     https://login.windows.net

## <a name="e-d"></a>404 ... Kan de resource niet vinden
![404... bron is niet gevonden](./media/app-insights-analytics-troubleshooting/040.png)

Bron van toepassing is verwijderd uit de Application Insights en is niet langer beschikbaar. Dit kan gebeuren als u de URL naar de pagina Analytics opgeslagen.

## <a name="e-e"></a>403 ... Geen autorisatie
![403... niet geautoriseerd](./media/app-insights-analytics-troubleshooting/050.png)

U bent niet gemachtigd voor het openen van deze toepassing in Analytics.

* Krijgt u de koppeling van iemand anders? Vraag om ervoor te zorgen dat u zich in de [lezers of inzenders voor deze resourcegroep](app-insights-resources-roles-access-control.md).
* U de koppeling met andere referenties opslaan? Open de [Azure-portal](https://portal.azure.com), meldt u zich af en probeer deze koppeling weer, de juiste referenties opgeeft.

## <a name="html-storage"></a>403 ... HTML5-opslag
Onze portal maakt gebruik van HTML5 localStorage en sessionStorage.

* Chrome: Instellingen, privacy, instellingen van de inhoud.
* Internet Explorer: Internet-opties, tabblad Geavanceerd, beveiliging, de DOM-opslag inschakelen

![403... probeert in te schakelen HTML5-opslag](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404 ... Abonnement is niet gevonden
![404 ... Abonnement is niet gevonden](./media/app-insights-analytics-troubleshooting/070.png)

De URL is ongeldig. 

* Open de app-resource in [Application Insights-portal](https://portal.azure.com). Gebruik vervolgens de Analytics-knop.

## <a name="e-h"></a>404... pagina bestaat niet
![404 ... Pagina bestaat niet](./media/app-insights-analytics-troubleshooting/080.png)

De URL is ongeldig.

* Open de app-resource in [Application Insights-portal](https://portal.azure.com). Gebruik vervolgens de Analytics-knop.

## <a name="cookies"></a>Cookies van derden inschakelen
  Zie [cookies van derden uitschakelen](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), maar we moeten kennisgeving **inschakelen** ze.


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

