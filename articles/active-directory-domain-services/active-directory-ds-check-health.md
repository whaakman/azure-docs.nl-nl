---
title: Azure AD Domain Services - Controleer de status van uw beheerde domein | Microsoft Docs
description: Controleer de status van uw beheerde domein met behulp van de health-pagina in de Azure portal.
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: ergreenl
ms.openlocfilehash: 5be6a2c36dfec8c8543f6dcfe252ce01b4c28e50
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34587235"
---
# <a name="check-the-health-of-an-azure-ad-domain-services-managed-domain"></a>Controleer de status van een beheerd domein van Azure AD Domain Services

## <a name="overview-of-the-health-page"></a>Overzicht van de health-pagina
Met behulp van de health-pagina in uw Azure-portal, zich u aan op wat er in uw beheerde domein gebeurt up-to-date te houden. Dit artikel helpt bij de elementen van de health-pagina.

### <a name="how-to-view-the-health-of-your-managed-domain"></a>De status van uw beheerde domein weergeven
1. Navigeer naar de [Azure AD Domain Services-pagina](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in de Azure portal.
2. Klik op het domein dat u wilt weergeven van de status van.
3. Klik in het navigatiedeelvenster links op **Health**.

De volgende afbeelding ziet u een voorbeeldpagina health: ![voorbeeld health-pagina](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Status van uw beheerde domein wordt elk uur worden geëvalueerd. Nadat u deze wijzigingen naar uw beheerde domein, wachten tot de volgende beoordelingscyclus om de bijgewerkte status van uw beheerde domein weer te geven. De tijdstempel 'Het laatst is geëvalueerd' in de rechterbovenhoek geeft aan wanneer de status van uw beheerde domein voor het laatst is geëvalueerd.
>

### <a name="status-of-your-managed-domain"></a>Status van uw beheerde domein
De status in de rechterbovenhoek van de gezondheid van uw pagina geeft aan dat de algehele status van uw beheerde domein. Status van de factoren in alle van de bestaande waarschuwingen in uw domein. U kunt ook de status van uw domein weergeven op de overzichtspagina van Azure AD Domain Services.

| Status | Pictogram | Uitleg |
| --- | :----: | --- |
| In uitvoering | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | Uw beheerde domein soepel wordt uitgevoerd en heeft geen geen waarschuwingen voor kritieke of waarschuwingsstatus. Dit domein misschien informatieve waarschuwingen. |
| Aandacht (waarschuwing) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | Er zijn geen kritieke waarschuwingen op uw beheerde domein, maar er zijn een of meer waarschuwingen die moeten worden opgelost. |
| Aandacht (Kritiek) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | Er zijn een of meer kritieke waarschuwingen op uw beheerde domein. Mogelijk hebt u ook de waarschuwing en/of informatieve waarschuwingen. |
| Implementeren | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | Uw domein is bezig te worden geïmplementeerd. |

## <a name="monitors"></a>Monitors
Monitors zijn aspecten van uw beheerde domein die Azure AD Domain Services op regelmatig gecontroleerd. De beste manier om uw monitors houden in orde is geen actieve waarschuwingen voor uw beheerde domein omzetten.

Azure AD Domain Services bewaakt momenteel het volgende:
 - Backup
 - Synchronisatie met Azure AD

### <a name="the-backup-monitor"></a>De monitor 'back-up'
Hiermee wordt bewaakt of regelmatige back-ups van uw beheerde domein worden uitgevoerd. De volgende tabel wordt uitgelegd wat u kunt verwachten in de detailkolom van de back-monitor:

| Waarde voor rapportagedetail opgeven | Uitleg |
| --- | --- |
|"Nooit een back-up" | Deze status is normaal voor een nieuwe beheerde domein. De eerste back-up gemaakt in het algemeen 24 uur nadat uw beheerde domein is ingericht. Als uw beheerde domein niet nieuw gemaakt is of u deze status voor een abnormale tijdsduur ziet [contact op met ondersteuning](active-directory-ds-contact-us.md). |
| Laatste back-up is gehaald 1 tot en met 14 dagen geleden | In het algemeen wordt deze waarde verwacht voor de back-monitor. |
| Laatste back-up is langer dan 14 dagen geleden gehaald. | Elk gewenst moment meer dan twee weken is buitengewoon lang sinds de laatste back-up. Actieve kritieke waarschuwingen kunnen voorkomen dat uw beheerde domein wordt een back-up op regelmatige basis. Eerste, los eventuele actieve waarschuwingen voor uw beheerde domein en vervolgens als het probleem zich blijft voordoen, [contact op met ondersteuning](active-directory-ds-contact-us.md). |


### <a name="the-synchronization-with-azure-ad-monitor"></a>De monitor 'synchronisatie met Azure AD'
Microsoft controleert hoe vaak uw beheerde domein is gesynchroniseerd met Azure Active Directory. Het aantal objecten (gebruikers en groepen) en het aantal wijzigingen aangebracht in uw Azure AD-directory sinds de laatste synchronisatie kan beide beïnvloeden hoe lang een synchronisatieperiode kan duren. Als uw beheerde domein het laatst is gesynchroniseerd meer dan drie dagen geleden, [contact op met ondersteuning](active-directory-ds-contact-us.md).

## <a name="alerts"></a>Waarschuwingen
Waarschuwingen worden gegenereerd voor problemen in uw beheerde domein die moeten worden opgelost om Azure AD Domain Services worden uitgevoerd. Elke waarschuwing wordt het probleem en vindt u een resolutie-URL bevat een overzicht van specifieke stappen uitvoeren om het probleem te verhelpen. Als u wilt weergeven van alle waarschuwingen en hun oplossingen, gaat u naar de [-waarschuwingen oplossen](active-directory-ds-troubleshoot-alerts.md) artikel.

### <a name="alert-severity"></a>Ernst van waarschuwingen
Waarschuwingen worden onderverdeeld in drie verschillende niveaus van ernst: essentiële, waarschuwingen en informatieve.

 * **Kritieke waarschuwingen** zijn problemen die grote invloed is op uw beheerde domein. Deze waarschuwingen moeten onmiddellijk worden opgelost omdat Microsoft niet bewaken, beheren, patch en synchroniseren van uw beheerde domein. 
 * **Waarschuwingen** zullen u informeren over problemen die mogelijk van invloed op uw beheerde domein in de toekomst. Deze waarschuwingen bieden aanbevelingen voor het beveiligen van uw beheerde domein.
 * **Informatieve meldingen** zijn meldingen die van invloed zijn geen negatieve op uw domein. Informatieve waarschuwingen zijn ontworpen om te voorkomen dat u kennis over wat er in uw domein en de Azure AD Domain Services gebeurt.

## <a name="next-steps"></a>Volgende stappen
- [Waarschuwingen op uw beheerde domein oplossen](active-directory-ds-troubleshoot-alerts.md)
- [Meer informatie over Azure AD Domain Services](active-directory-ds-overview.md)
- [Neem contact op met het productteam](active-directory-ds-contact-us.md)
