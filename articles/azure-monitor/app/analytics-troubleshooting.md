---
title: Analyses in Azure Application Insights oplossen | Microsoft Docs
description: 'Problemen met Application Insights analytics? Begin hier. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/11/2016
ms.author: mbullwin
ms.openlocfilehash: 1dc312598fa9bbacedc085480b0771adeb2d182a
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731164"
---
# <a name="troubleshoot-analytics-in-application-insights"></a>Problemen met analyses in Application Insights oplossen
Problemen met [Application Insights Analytics](analytics.md)? Begin hier. Analytics is de krachtige zoekfunctie van Azure Application Insights.

## <a name="limits"></a>Limieten
* Op dit moment zijn queryresultaten beperkt tot alleen gedurende een week van oude gegevens.
* We testen op browsers: meest recente versies van Chrome, Microsoft Edge en Internet Explorer.

## <a name="known-incompatible-browser-extensions"></a>Bekende niet-compatibele browser-extensies
* Ghostery

De uitbreiding uitschakelen of gebruik een andere browser.

## <a name="e-a"></a> "Fout"
![Onverwachte foutscherm](media/analytics-troubleshooting/010.png)

Er is een interne fout opgetreden tijdens het portal runtime niet-verwerkte uitzondering.

* Opschonen van de browser-cache. 

## <a name="e-b"></a>403... Probeer het opnieuw te laden
![403... Probeer het opnieuw te laden](media/analytics-troubleshooting/020.png)

Er is een fout opgetreden die te maken heeft met het verificatieproces (tijdens de verificatie of tijdens het genereren van een toegangstoken). De portal mogelijk worden hersteld door browserinstellingen te wijzigen.

* Controleer of [cookies van derden zijn ingeschakeld](#cookies) in de browser. 

## <a name="authentication"></a>403... beveiligingszone controleren
![403.. .verify beveiligingszone](media/analytics-troubleshooting/030.png)

Er is een fout opgetreden die te maken heeft met het verificatieproces (tijdens de verificatie of tijdens het genereren van een toegangstoken). De portal mogelijk worden hersteld door browserinstellingen te wijzigen.

1. Controleer of [cookies van derden zijn ingeschakeld](#cookies) in de browser. 
2. Hebt u een favoriet, bladwijzer of opgeslagen koppeling gebruikt om de portal van Analytics te openen? Hebt u zich aangemeld met andere referenties dan die u hebt gebruikt bij het opslaan van de koppeling?
3. Probeer het met een privé-venster/incognito browservenster (nadat u alle vensters hebt gesloten). U moet dan uw referenties opgeven. 
4. Open een ander (gewoon) browservenster en Ga naar [Azure](https://portal.azure.com). Meld u af. Open vervolgens de koppeling en meld u met de juiste referenties aan.
5. Gebruikers van Edge en Internet Explorer kunnen deze fout ook krijgen als instellingen voor vertrouwde zones niet worden ondersteund.
   
    Controleer of beide [analyseportal](https://portal.azure.com) en [Azure Active Directory-portal](https://portal.azure.com) zich in dezelfde beveiligingszone:
   
   * Open in Internet Explorer, **Internetopties**, **Security**, **vertrouwde sites**, **Sites**:
     
     ![Dialoogvenster Internetopties, een site toe te voegen aan vertrouwde websites](media/analytics-troubleshooting/033.png)
     
     Als de lijst met websites een van de volgende URL's bevat, moet u de andere sites ook opnemen in de lijst:
     
     https://analytics.applicationinsights.io<br/>
     https://login.microsoftonline.com<br/>
     https://login.windows.net

## <a name="e-d"></a>404 ... Kan de resource niet vinden
![404... resource is niet gevonden](media/analytics-troubleshooting/040.png)

Bron van toepassing is verwijderd uit de Application Insights en is t beschikbaar meer. Dit kan gebeuren als u de URL naar de pagina Analytics hebt opgeslagen.

## <a name="e-e"></a>403 ... Er is geen autorisatie
![403... niet gemachtigd](media/analytics-troubleshooting/050.png)

U bent niet gemachtigd voor het openen van deze toepassing in Analytics.

* Krijgt u de koppeling van iemand anders? Vraag deze om te controleren of u zich in de [lezers of bijdragers voor deze resourcegroep](../../application-insights/app-insights-resources-roles-access-control.md).
* Hebt u de koppeling met andere referenties opgeslagen? Open de [Azure-portal](https://portal.azure.com), meld u af en probeer het vervolgens deze koppeling weer de juiste referenties op te geven.

## <a name="html-storage"></a>403 ... HTML5-opslag
Onze portal maakt gebruik van HTML5 localStorage en sessionStorage.

* Chrome: Instellingen, privacy, instellingen van de inhoud.
* Internet Explorer: Internet-opties, tabblad Geavanceerd, beveiliging, DOM-opslag inschakelen

![403... probeert in te schakelen HTML5-opslag](media/analytics-troubleshooting/060.png)

## <a name="e-g"></a>404 ... Kan het abonnement niet vinden
![404 ... Kan het abonnement niet vinden](media/analytics-troubleshooting/070.png)

De URL is ongeldig. 

* Open de app-resource in [Application Insights-portal](https://portal.azure.com). Gebruik vervolgens de knop Analytics.

## <a name="e-h"></a>404... pagina bestaat niet
![404 ... Pagina bestaat niet](media/analytics-troubleshooting/080.png)

De URL is ongeldig.

* Open de app-resource in [Application Insights-portal](https://portal.azure.com). Gebruik vervolgens de knop Analytics.

## <a name="cookies"></a>Cookies van derden inschakelen
  Zie [cookies van derden uitschakelen](https://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), maar u ziet dat we moeten **inschakelen** ze.


[!INCLUDE [app-insights-analytics-footer](../../../includes/app-insights-analytics-footer.md)]

