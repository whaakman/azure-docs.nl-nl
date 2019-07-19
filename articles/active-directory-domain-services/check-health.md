---
title: 'Azure AD Domain Services: Controleer de status van uw beheerde domein | Microsoft Docs'
description: Controleer de status van uw beheerde domein met behulp van de pagina status in de Azure Portal.
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: 6b808126fe4366d3ca3cc19c674b489ec3055665
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234160"
---
# <a name="check-the-health-of-an-azure-ad-domain-services-managed-domain"></a>De status controleren van een door Azure AD Domain Services beheerd domein

## <a name="overview-of-the-health-page"></a>Overzicht van de status pagina
Op de pagina status op uw Azure Portal kunt u op de hoogte blijven van wat er gebeurt met uw beheerde domein. In dit artikel worden de elementen van de status pagina besproken.

### <a name="how-to-view-the-health-of-your-managed-domain"></a>De status van uw beheerde domein weer geven
1. Ga naar de [pagina Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in het Azure Portal.
2. Klik op het domein waarvan u de status wilt weer geven.
3. Klik in het navigatie deel venster aan de linkerkant op **status**.

In de volgende afbeelding ziet u een voor beeld van een status pagina: ![Voor beeld van status pagina](./media/active-directory-domain-services-alerts/health-page.png)

>[!NOTE]
> De status van uw beheerde domein wordt elk uur geëvalueerd. Nadat u de wijzigingen hebt aangebracht in uw beheerde domein, wacht u tot de volgende evaluatie cyclus om de bijgewerkte status van uw beheerde domein weer te geven. De ' laatst geëvalueerde ' tijds tempel in de rechter bovenhoek geeft aan wanneer de status van uw beheerde domein voor het laatst is geëvalueerd.
>

### <a name="status-of-your-managed-domain"></a>Status van uw beheerde domein
De status in de rechter bovenhoek van uw status pagina geeft de algemene status van uw beheerde domein aan. De status factoren in alle bestaande waarschuwingen in uw domein. U kunt ook de status van uw domein weer geven op de pagina overzicht van Azure AD Domain Services.

| Status | Pictogram | Uitleg |
| --- | :----: | --- |
| Wordt uitgevoerd | <img src= "./media/active-directory-domain-services-alerts/running-icon.png" width = "15" alt="Green check mark for running"> | Uw beheerde domein werkt probleemloos en heeft geen kritieke of waarschuwings meldingen. Dit domein heeft mogelijk informatieve waarschuwingen. |
| Aandacht vereist (waarschuwing) | <img src= "./media/active-directory-domain-services-alerts/warning-icon.png" width = "15" alt="Yellow exclamation mark for warning"> | Er zijn geen kritieke waarschuwingen voor uw beheerde domein, maar er zijn een of meer waarschuwingen die moeten worden behandeld. |
| Aandacht vereist (kritiek) | <img src= "./media/active-directory-domain-services-alerts/critical-icon.png" width = "15" alt="Red exclamation mark for critical"> | Er zijn een of meer kritieke waarschuwingen voor uw beheerde domein. Mogelijk hebt u ook waarschuwings-en/of informatieve waarschuwingen. |
| Installeert | <img src= "./media/active-directory-domain-services-alerts/deploying-icon.png" width = "15" alt="Blue circular arrows for deploying"> | Uw domein wordt geïmplementeerd. |

## <a name="monitors"></a>Monitors
Monitors zijn aspecten van uw beheerde domein die regel matig Azure AD Domain Services bewaakt. De beste manier om uw monitors in een goede staat te houden is het oplossen van actieve waarschuwingen voor uw beheerde domein.

Azure AD Domain Services controleert momenteel het volgende:
 - Backup
 - Synchronisatie met Azure AD

### <a name="the-backup-monitor"></a>De monitor backup
Hiermee wordt gecontroleerd of er regel matig back-ups van uw beheerde domein worden uitgevoerd. In de volgende tabel wordt uitgelegd wat u kunt verwachten in de kolom Details van de back-upmonitor:

| Detail waarde | Uitleg |
| --- | --- |
|' Nooit een back-up maken ' | Deze status is normaal voor een nieuw beheerd domein. Over het algemeen wordt de eerste back-up 24 uur na het inrichten van het beheerde domein gemaakt. [Neem contact op met de ondersteuning](contact-us.md)als uw beheerde domein niet nieuw is gemaakt of als u deze status gedurende een abnormale periode ziet. |
| De laatste back-up is 1 tot 14 dagen geleden genomen | Over het algemeen wordt deze waarde verwacht voor de back-upmonitor. |
| De laatste back-up is meer dan 14 dagen geleden gemaakt. | Een tijd van meer dan twee weken is een ongebruikelijk lange tijd sinds uw laatste back-up. Actieve kritieke waarschuwingen kunnen verhinderen dat uw beheerde domein regel matig een back-up maakt. Los eerst eventuele actieve waarschuwingen voor uw beheerde domein [op en neem contact op met de ondersteuning](contact-us.md)als het probleem zich blijft voordoen. |


### <a name="the-synchronization-with-azure-ad-monitor"></a>De monitor synchronisatie met Azure AD
Micro soft controleert hoe vaak uw beheerde domein is gesynchroniseerd met Azure Active Directory. Het aantal objecten (gebruikers & groepen) en het aantal wijzigingen dat sinds de laatste synchronisatie in uw Azure AD-adres lijst is aangebracht, kunnen van invloed zijn op hoe lang een synchronisatie periode kan duren. [Neem contact op met de ondersteuning](contact-us.md)als uw beheerde domein voor het laatst drie dagen geleden is gesynchroniseerd.

## <a name="alerts"></a>Waarschuwingen
Er worden waarschuwingen gegenereerd voor problemen op uw beheerde domein die moeten worden opgelost om Azure AD Domain Services uit te voeren. Elke waarschuwing legt het probleem af en biedt een omzettings-URL die specifieke stappen bevat om het probleem op te lossen. Ga naar het artikel [problemen met waarschuwingen oplossen](troubleshoot-alerts.md) om alle waarschuwingen en hun oplossingen weer te geven.

### <a name="alert-severity"></a>Ernst van waarschuwing
Waarschuwingen worden ingedeeld in drie verschillende niveaus van Ernst: kritiek, waarschuwing en informatief.

 * **Kritieke waarschuwingen** zijn problemen die het beheerde domein aanzienlijk beïnvloeden. Deze waarschuwingen moeten onmiddellijk worden opgelost, omdat micro soft uw beheerde domein niet kan controleren, beheren, patchen en synchroniseren. 
 * **Waarschuwingen** over problemen die in de toekomst mogelijk van invloed zijn op uw beheerde domein. Deze waarschuwingen bieden aanbevelingen voor het beveiligen van uw beheerde domein.
 * **Informatieve waarschuwingen** zijn meldingen die geen negatieve invloed hebben op uw domein. Informatieve waarschuwingen zijn ontworpen om u op de hoogte te blijven van wat er gebeurt in uw domein en Azure AD Domain Services.

## <a name="next-steps"></a>Volgende stappen
- [Waarschuwingen voor uw beheerde domein oplossen](troubleshoot-alerts.md)
- [Meer informatie over Azure AD Domain Services](overview.md)
- [Neem contact op met het product team](contact-us.md)
