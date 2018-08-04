---
title: Azure AD Domain Services - Controleer de status van uw beheerde domein | Microsoft Docs
description: Controleer de status van uw beheerde domein met behulp van de health-pagina in Azure portal.
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/12/2018
ms.author: ergreenl
ms.openlocfilehash: a9248e2310e509bbc4f527608246b47543a03fc4
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502174"
---
# <a name="check-the-health-of-an-azure-ad-domain-services-managed-domain"></a>Controleer de status van een Azure AD Domain Services beheerde domein

## <a name="overview-of-the-health-page"></a>Overzicht van de health-pagina
Met behulp van de health-pagina in uw Azure-portal, bent u kunnen ervoor zorgen dat op de hoogte van wat in uw beheerde domein gebeurt er. Dit artikel helpt bij de elementen van de health-pagina.

### <a name="how-to-view-the-health-of-your-managed-domain"></a>De status van uw beheerde domein weergeven
1. Navigeer naar de [pagina Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in Azure portal.
2. Klik op het domein dat u wilt weergeven van de status van.
3. Klik in het navigatiedeelvenster links op **Health**.

De volgende afbeelding ziet u een voorbeeld van health-pagina: ![voorbeeld van de health-pagina](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Status van uw beheerde domein wordt elk uur worden geëvalueerd. Na het aanbrengen van wijzigingen aan uw beheerde domein, wachten tot de volgende beoordelingscyclus om de bijgewerkte status van uw beheerde domein weer te geven. De tijdstempel 'Laatst geëvalueerd' in de rechterbovenhoek ziet u wanneer de status van uw beheerde domein voor het laatst is geëvalueerd.
>

### <a name="status-of-your-managed-domain"></a>Status van uw beheerde domein
De status in de rechterbovenhoek van status van uw pagina geeft aan dat de algehele status van uw beheerde domein. De status van de rekening wordt gehouden met alle van de bestaande waarschuwingen in uw domein. U kunt ook de status van uw domein weergeven op de overzichtspagina van Azure AD Domain Services.

| Status | Pictogram | Uitleg |
| --- | :----: | --- |
| In uitvoering | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | Uw beheerde domein soepel en hoeft niet alle waarschuwingen voor kritieke of waarschuwingsstatus. Dit domein mogelijk informatieve waarschuwingen. |
| Aandacht is vereist (waarschuwing) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | Er zijn geen kritieke waarschuwingen op uw beheerde domein, maar er zijn een of meer waarschuwingen die moeten worden opgelost. |
| Aandacht is vereist (Kritiek) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | Er zijn een of meer kritieke waarschuwingen op uw beheerde domein. Misschien hebt u ook de waarschuwing en/of informatieve waarschuwingen. |
| Implementeren | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | Uw domein wordt momenteel geïmplementeerd. |

## <a name="monitors"></a>Monitors
Monitors zijn aspecten van uw beheerde domein die Azure AD Domain Services regelmatig controleert. De beste manier om uw monitors in een status in orde houden is om op te lossen actieve waarschuwingen voor uw beheerde domein.

Azure AD Domain Services controleert op dit moment het volgende:
 - Backup
 - Synchronisatie met Azure AD

### <a name="the-backup-monitor"></a>De monitor 'back-up'
Hiermee wordt bewaakt of regelmatige back-ups van uw beheerde domein worden uitgevoerd. De volgende tabel wordt uitgelegd wat u kunt verwachten in de detailkolom van de back-monitor:

| Waarde voor rapportagedetail opgeven | Uitleg |
| --- | --- |
|'Nooit een back-up' | Deze status is normaal dat een nieuwe beheerde domein. De eerste back-up gemaakt in het algemeen, 24 uur nadat uw beheerde domein is ingericht. Als uw beheerde domein niet onlangs is gemaakt of u deze status voor een abnormaal tijdsduur ziet [contact op met ondersteuning](active-directory-ds-contact-us.md). |
| Laatste back-up is gehaald 1 tot en met 14 dagen geleden | In het algemeen wordt deze waarde verwacht voor de back-monitor. |
| Laatste back-up werd gehouden met meer dan 14 dagen geleden. | Elk gewenst moment meer dan twee weken is een buitengewoon lang sinds uw laatste back-up. Actieve kritieke waarschuwingen kunnen voorkomen dat uw beheerde domein back-up op gezette tijden. Eerste, los eventuele actieve waarschuwingen voor uw beheerde domein, en vervolgens als het probleem zich blijft voordoen, [contact op met ondersteuning](active-directory-ds-contact-us.md). |


### <a name="the-synchronization-with-azure-ad-monitor"></a>De monitor 'synchronisatie met Azure AD'
Microsoft controleert hoe vaak uw beheerde domein is gesynchroniseerd met Azure Active Directory. Het aantal objecten (gebruikers en groepen) en het aantal wijzigingen aangebracht in uw Azure AD-directory sinds de laatste synchronisatie kan beide invloed hebben op hoe lang een synchronisatieperiode kan duren. Als uw beheerde domein het laatst is gesynchroniseerd meer dan drie dagen geleden, [contact op met ondersteuning](active-directory-ds-contact-us.md).

## <a name="alerts"></a>Waarschuwingen
Waarschuwingen worden gegenereerd voor problemen in uw beheerde domein die moeten worden behandeld in de order voor Azure AD Domain Services om uit te voeren. Elke waarschuwing legt het probleem en biedt een oplossing-URL die geeft een overzicht van specifieke stappen om het probleem te verhelpen. Als u wilt weergeven van alle waarschuwingen en hun oplossingen, gaat u naar de [waarschuwingen oplossen](active-directory-ds-troubleshoot-alerts.md) artikel.

### <a name="alert-severity"></a>Ernst van waarschuwingen
Waarschuwingen worden onderverdeeld in drie verschillende niveaus van ernst: kritiek, waarschuwing en ter informatie.

 * **Kritieke waarschuwingen** zijn problemen die grote invloed is op uw beheerde domein. Deze waarschuwingen moeten onmiddellijk worden opgelost omdat Microsoft niet controleren, beheren, patch en synchroniseren van uw beheerde domein. 
 * **Waarschuwingsmeldingen** op de hoogte stellen van problemen die mogelijk van invloed op uw beheerde domein in de toekomst. Deze waarschuwingen bieden aanbevelingen voor het beveiligen van uw beheerde domein.
 * **Informatieve waarschuwingen** zijn meldingen dat zijn geen negatieve invloed heeft op uw domein. Informatieve waarschuwingen zijn ontworpen om te voorkomen dat u kennis over wat in uw domein en Azure AD Domain Services gebeurt er.

## <a name="next-steps"></a>Volgende stappen
- [Los waarschuwingen op uw beheerde domein](active-directory-ds-troubleshoot-alerts.md)
- [Lees meer over Azure AD Domain Services](active-directory-ds-overview.md)
- [Neem contact op met het productteam](active-directory-ds-contact-us.md)
