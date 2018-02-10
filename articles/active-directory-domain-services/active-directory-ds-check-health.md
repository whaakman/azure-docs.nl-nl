---
title: Azure AD Domain Services - Controleer de status van uw beheerde domein | Microsoft Docs
description: Controleer de status van uw beheerde domein met behulp van de health-pagina in de Azure portal.
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: ergreenl
ms.openlocfilehash: 4adbce0305bdc1a9b261f5cf644fad876d101bc6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-ad-domain-services---check-the-health-of-your-managed-domain"></a>De status van uw beheerde domein van Azure AD Domain Services - controleren

## <a name="your-domains-health"></a>Status van uw domein

Met behulp van de health-pagina in uw Azure-portal, zich u aan op wat er in uw beheerde domein gebeurt up-to-date te houden. Dit artikel stappen via alle elementen van de status pagina en leert u hoe u uw domein is in orde vorm.

### <a name="view-your-health-page"></a>De health-pagina weergeven

1. Navigeer naar de pagina Azure AD Domain Services in de [Azure-portal](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices)
2. Klik op het domein dat u wilt weergeven van de status van.
3. Klik op 'Status' in de linkernavigatiebalk.

De volgende afbeelding is een voorbeeld van de health-pagina.

![Voorbeeld van de health-pagina](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Status van uw domein wordt om elk uur geëvalueerd. Nadat u deze wijzigingen naar uw beheerde domein, moet u wachten tot de volgende beoordelingscyclus om weer te geven van uw domein health bijgewerkt. U kunt controleren wanneer uw domein het laatst is geëvalueerd met behulp van de 'Laatste geëvalueerd' tijdstempel zich bevindt in de rechterbovenhoek.
>

### <a name="status-of-your-managed-domain"></a>Status van uw beheerde domein

De status in de rechterbovenhoek van de gezondheid van uw pagina geeft aan dat de algehele status van uw domein. Status van de factoren in alle van de bestaande waarschuwingen in uw domein. U kunt ook de status van uw domein weergeven op de overzichtspagina van Azure AD Domain Services.

Statussen van een beheerd domein:

| Status | Pictogram | Uitleg |
| --- | :----: | --- |
| Wordt uitgevoerd | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | Uw beheerde domein soepel wordt uitgevoerd en heeft geen geen waarschuwingen voor kritieke of waarschuwingsstatus. Dit domein misschien informatieve waarschuwingen. |
| Aandacht (waarschuwing) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | Er zijn geen kritieke waarschuwingen op uw beheerde domein, maar er zijn een of meer waarschuwingen die moeten worden opgelost. |
| Aandacht (Kritiek) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | Er zijn een of meer kritieke waarschuwingen op uw beheerde domein. Mogelijk hebt u ook de waarschuwing en/of informatieve waarschuwingen. |
| Implementeren | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | Uw domein is bezig te worden geïmplementeerd. |

## <a name="monitors"></a>Monitors

Monitors toegelicht bepaalde aspecten over uw beheerde domein die Azure AD Domain Services houdt toezicht op. De beste manier om uw monitors houden in goede staat is om op te lossen waarschuwingen op uw beheerde domein.

De monitoren die beschikbaar zijn:
 - Back-up maken
 - Synchronisatie met AAD

### <a name="backup"></a>Back-up maken

Hiermee wordt bewaakt hoe vaak naartoe back-ups van uw beheerde domein. Hieronder vindt u een tabel met uitleg over de detailkolom van de back-monitor en welke waarden u moet worden verwacht.

| Waarde voor rapportagedetail opgeven | Uitleg |
| --- | --- |
|"Nooit een back-up" | Deze status is normaal voor een nieuwe domein. De eerste back-up wordt meestal gemaakt na 24 uur. Als uw beheerde domein niet nieuw gemaakt is of als u in deze status voor een abnormale tijdsduur [contact op met ondersteuning](active-directory-ds-contact-us.md). |
| Laatste back-up is gehaald 1 tot en met 14 dagen geleden | Dit is in het algemeen de verwachte waarde van de back-monitor. |
| Laatste back-up is langer dan 14 dagen geleden gehaald. | Elk gewenst moment meer dan twee weken is buitengewoon lang sinds de laatste back-up. Eerste, los eventuele waarschuwingen die worden weergegeven op uw beheerde domein en klik vervolgens als het probleem zich blijft voordoen, [contact op met ondersteuning](active-directory-ds-contact-us.md). |


### <a name="synchronization-with-azure-ad"></a>Synchronisatie met Azure AD

Microsoft houdt van hoe vaak uw beheerde domein is gesynchroniseerd met Azure Active Directory. De hoeveelheid gebruikers in uw beheerde domein, evenals de hoeveelheid wijzigingen die zijn aangebracht sinds de laatste synchronisatie kan beide beïnvloeden hoe lang een synchronisatieperiode kan duren. In het algemeen als het is langer dan drie dagen sinds de laatste synchronisatie, is het raadzaam om te [contact op met ondersteuning](active-directory-ds-contact-us.md).

## <a name="alerts"></a>Waarschuwingen

Waarschuwingen zijn problemen op uw beheerde domein die moeten worden opgelost om Azure AD Domain Services worden uitgevoerd. Elke waarschuwing wordt het probleem en vindt u een URL bevat een overzicht van specifieke stappen voor het oplossen van het probleem. Als u wilt weergeven van alle waarschuwingen en hun oplossingen, gaat u naar de [-waarschuwingen oplossen](active-directory-ds-troubleshoot-alerts.md) artikel.

### <a name="severity"></a>Ernst
Waarschuwingen worden onderverdeeld in drie verschillende niveaus van ernst: essentiële, waarschuwingen en informatieve.

 * **Kritieke waarschuwingen** zijn problemen die grote invloed is op uw beheerde domein. Deze waarschuwingen moeten onmiddellijk worden opgelost omdat Microsoft niet bewaken, beheren, patch en synchroniseren van uw beheerde domein.
 * **Waarschuwingen** problemen die mogelijk van invloed op uw domein in de toekomst, maar zijn niet per se 'op te splitsen' uw service kunnen worden. Deze waarschuwingen aanbevolen procedures voor een overzicht en geef suggesties voor het beveiligen van uw beheerde domein.
 * **Informatieve meldingen** zijn meldingen die van invloed zijn geen negatieve op uw domein. Informatieve waarschuwingen zijn ontworpen om te voorkomen dat u kennis over wat er in uw domein en de Azure AD Domain Services gebeurt.

## <a name="next-steps"></a>Volgende stappen
- [Waarschuwingen op uw beheerde domein oplossen](active-directory-ds-troubleshoot-alerts.md)
- [Meer informatie over Azure AD Domain Services](active-directory-ds-features.md)
- [Contact met ons opnemen](active-directory-ds-contact-us.md)
