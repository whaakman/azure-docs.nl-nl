---
title: Threat Modeling Tool Releases - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: Waarin u de releaseopmerkingen voor de threat modeling tool
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2019
ms.author: jegeib
ms.openlocfilehash: 8d3d2acb28a82f50f0255701fe6079ad444ce03f
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360276"
---
# <a name="threat-modeling-tool-ga-release-71509112---9122018"></a>Threat Modeling Tool GA-release 7.1.50911.2 - 9/12/2018

We zijn trots kondig dat Microsoft Threat Modeling Tool is nu beschikbaar voor downloaden als een ondersteunde algemeen beschikbaar (GA) versie. Deze release bevat belangrijke privacy en beveiligingsupdates, evenals oplossingen voor problemen, functie-updates en verbeteringen in de stabiliteit. Bestaande gebruikers van de Preview-versie 2017 wordt gevraagd om bij te werken naar de meest recente versie via de ClickOnce-technologie bij het openen van de client. Voor nieuwe gebruikers van het hulpprogramma [Klik hier om te downloaden van de client](https://aka.ms/threatmodelingtool).

Met deze release, we zijn beëindigt de ondersteuning voor de Preview-versie 2017 en alle gebruikers van de Preview-update gaat aanraden aan de GA-versie. Op of na 15 oktober-2018, de minimaal vereiste ClickOnce-versie wordt ingesteld voor de Threat Modeling Tool en alle clients van de Preview-versie is vereist om bij te werken.

De Microsoft Threat Modeling Tool 2016, die beschikbaar via is de [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=49168), blijft tot 1 oktober-2019 voor kritieke alleen beveiligingsproblemen wordt ondersteund.

## <a name="feature-changes"></a>Functiewijzigingen

### <a name="azure-stencil-updates"></a>Azure stencil updates

Extra Azure stencils en hun bijbehorende bedreigingen en oplossingen zijn toegevoegd aan het stencil ingesteld verzenden met deze release. Belangrijke wijzigingen zijn aangebracht in de gebieden van 'Azure App Services', 'Azure-databaseservices' en "Azure-opslag."

![Azure Stencil Updates](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_azure_stencil_update-300x70.png)

### <a name="onedrive-integration-feature-removed"></a>OneDrive-integratiefunctie verwijderd

De functies "Opslaan naar OneDrive", 'Openen vanuit OneDrive' en 'Een koppeling voor delen' van de Preview-versie zijn verwijderd. Gebruikers van OneDrive wordt aangeraden om te gebruiken van Microsoft [OneDrive voor Windows](https://onedrive.live.com/about/en-us/download/) client toegang tot hun bestanden die zijn opgeslagen op OneDrive via de standaard-bestand opslaan en openen van dialoogvensters.

## <a name="notable-fixed-bugs-reported-by-customers"></a>Opmerkelijk fouten opgelost die zijn gerapporteerd door klanten

### <a name="in-tmt-preview-the-tool-crashes-when-using-the-standard-template"></a>TMT Preview-versie, het hulpprogramma loopt vast bij het gebruik van de standaardsjabloon

- Wanneer een algemene stencil (bijvoorbeeld ' algemene gegevensstroom') wordt toegevoegd aan de tekening aanvallen en bedreigingen genereert, wordt het hulpprogramma kan vastlopen. Dit probleem is opgelost.

### <a name="in-tmt-preview-when-i-save-a-report-or-copy-the-threats-the-risk-levels-are-incorrect"></a>Preview-versie TMT wanneer ik een rapport opslaan of kopiëren van de bedreigingen, zijn de risiconiveaus onjuist

- Als een gebruiker het risiconiveau van specifieke bedreigingen wijzigt en vervolgens een rapport wordt opgeslagen of gekopieerd van de risico's, kan het risiconiveau terugkeren naar 'Hoog'. Dit probleem is opgelost.

## <a name="known-issues-and-faq"></a>Bekende problemen en veelgestelde vragen

### <a name="users-of-high-resolution-screens-may-experience-small-text-in-the-threat-properties"></a>Gebruikers van de schermen met hoge resolutie ondervinden kleine tekst in de eigenschappen van de bedreiging

#### <a name="issue"></a>Probleem

In de weergave van de analyse van het hulpprogramma, dat als de gebruiker heeft een hoge resolutie die standaard is ingesteld om te vergroten voor de leesbaarheid in Windows, de sectie 'Mogelijk Mitigation(s)' van een bedreiging wordt weergegeven in kleine letters.

![Bekend probleem met de schermen met hoge resolutie](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_screen_resolution-300x153.png)

#### <a name="workaround"></a>Tijdelijke oplossing

De gebruiker kan klikt u op de tekst van de risicobeperking en de standaard Windows-zoomknop (CTRL-muis Wheel omhoog) gebruiken voor het verhogen van de zoomfactor van deze sectie.

### <a name="files-in-the-recently-opened-models-section-of-the-main-window-may-fail-to-open"></a>Mogelijk mislukt het openen van bestanden in de sectie 'Onlangs geopend modellen' van het hoofdvenster

#### <a name="issue"></a>Probleem

De functie 'Open uit OneDrive' van de Preview-versie is verwijderd. Gebruikers met 'Onlangs geopend modellen' die zijn opgeslagen in OneDrive wordt het volgende foutbericht weergegeven.

![OneDrive-functie die zijn verwijderd](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_save_error-300x131.png)

#### <a name="workaround"></a>Tijdelijke oplossing

Gebruikers van OneDrive wordt aangeraden om te gebruiken van Microsoft [OneDrive voor Windows](https://onedrive.live.com/about/en-us/download/) client toegang heeft tot de bestanden die zijn opgeslagen op OneDrive via de standard- en dialoogvenster 'Een model geopend'.

![OneDrive-functie die zijn verwijderd](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_save_onedrive-300x149.png)

### <a name="my-organization-uses-the-2016-version-of-the-tool-can-i-use-the-azure-stencil-set"></a>Mijn organisatie de 2016-versie van het hulpprogramma gebruikt, kan ik de set Azure stencil gebruiken?

Ja, u kunt! De [Azure stencil set is beschikbaar op github](https://github.com/Microsoft/threat-modeling-templates/), en kan worden geladen in de 2016-versie van het hulpprogramma. Voor het maken van een nieuw model met de set Azure stencil, gebruik het dialoogvenster 'Sjabloon voor nieuwe modellen' op het scherm in het hoofdmenu. TMT 2016 kan niet worden gevonden in de velden 'Mogelijke oplossingen' van de set Azure stencil koppelingen weergegeven, daarom ziet u koppelingen weergegeven als HTML-codes.

![Azure Stencil Updates in 2016-Client](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_azure_stencils-300x212.png)

## <a name="system-requirements"></a>Systeemvereisten

- Ondersteunde besturingssystemen
  - Microsoft Windows 10
- .NET-versie vereist
  - .Net 3.5.2
- Aanvullende vereisten
  - Een internetverbinding is vereist voor het ontvangen van updates voor het hulpprogramma, evenals de sjablonen.

## <a name="documentation-and-feedback"></a>Documentatie en feedback

- Documentatie voor de Threat Modeling Tool bevindt zich op [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool), en informatie gegeven [over het gebruik van het hulpprogramma](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Volgende stappen

Download de nieuwste versie van de [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).