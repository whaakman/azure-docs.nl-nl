---
title: 'Azure AD Connect-synchronisatie: onopzettelijke verwijderingen voorkomen | Microsoft Docs'
description: Dit onderwerp beschrijft de functie verhinderen onopzettelijk verwijderen (onopzettelijke verwijderingen voorkomen) in Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 57ce7b2fcece751b1386ef1d57762ad8d1c27c62
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect-synchronisatie: onopzettelijke verwijderingen voorkomen
Dit onderwerp beschrijft de functie verhinderen onopzettelijk verwijderen (onopzettelijke verwijderingen voorkomen) in Azure AD Connect.

Wanneer de installatie van Azure AD Connect te voorkomen dat per ongeluk is verwijderd standaard ingeschakeld en geconfigureerd voor het exporteren van een met meer dan 500 verwijderingen niet toestaan. Deze functie is ontworpen om u te beschermen tegen onbedoelde configuratiewijzigingen en wijzigingen aan uw on-premises directory die veel gebruikers en andere objecten zou beïnvloeden.

## <a name="what-is-prevent-accidental-deletes"></a>Wat is onopzettelijke verwijderingen voorkomen
Algemene scenario's, wanneer er veel verwijderingen omvatten:

* Wijzigingen in [filteren](active-directory-aadconnectsync-configure-filtering.md) waar een volledige [organisatie-eenheid](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) of [domein](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) is uitgeschakeld.
* Alle objecten in een organisatie-eenheid worden verwijderd.
* De naam van een organisatie-eenheid wordt gewijzigd zodat alle objecten in deze worden beschouwd als niet binnen het bereik voor synchronisatie.

De standaardwaarde van 500 objecten kan worden gewijzigd met PowerShell met `Enable-ADSyncExportDeletionThreshold`, die deel uitmaakt van de module AD Sync is geïnstalleerd met Azure Active Directory Connect. U moet deze waarde aan de grootte van uw organisatie configureren. Aangezien de sync scheduler wordt elke 30 minuten uitgevoerd, is de waarde het aantal verwijderingen gezien binnen 30 minuten.

Als er te veel verwijderingen voorbereid om te worden geëxporteerd naar Azure AD, stopt de export en u ontvangt een e-mail als volgt:

![E-mailadres onopzettelijke verwijderingen voorkomen](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hello (technische contactpersoon). (Tijd) de synchronisatieservice identiteit gedetecteerd dat het aantal verwijderingen de geconfigureerde exportverwijderingsdrempel instellen voor (organisatienaam) overschreden. Een totaal van (getal) objecten zijn in deze identiteitssynchronisatie uitvoeren voor verwijdering verzonden. Dit voldaan aan of groter dan de drempelwaarde voor de geconfigureerde verwijdering van objecten (getal). Moet je bevestigen dat deze verwijderingen worden verwerkt voordat er wordt voortgezet. Zie de voorkomen onopzettelijke verwijderingen voor meer informatie over de fout weergegeven in dit e-mailbericht.*
>
> 

U ziet ook de status `stopped-deletion-threshold-exceeded` wanneer u zoeken in de **Synchronization Service Manager** gebruikersinterface voor het profiel exporteren.
![Onopzettelijke verwijderingen Sync Service Manager UI voorkomen](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Als dit werd niet verwacht, onderzoek en corrigerende acties uitvoeren. Als u wilt zien welke objecten zijn verwijderd, het volgende doen:

1. Start **synchronisatieservice** vanuit het startmenu.
2. Ga naar **Connectors**.
3. Selecteer de Connector met het type **Azure Active Directory**.
4. Onder **acties** aan de rechterkant, selecteer **Connectorgebied zoeken**.
5. In het pop-upvenster onder **bereik**, selecteer **verbroken omdat** en kies een tijdstip in het verleden. Klik op **Search**. Deze pagina bevat een overzicht van alle objecten verwijderd. Door te klikken op elk item, kunt u aanvullende informatie over het object ophalen. U kunt ook klikken op **kolominstelling** extra kenmerken voor weergave in het raster toevoegen.

![Connectorgebied zoeken](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Als alle verwijderingen aanbrengen wilt, klikt u vervolgens het volgende doen:

1. Voor het ophalen van de huidige exportverwijderingsdrempel instellen, voert u de PowerShell-cmdlet `Get-ADSyncExportDeletionThreshold`. Geef een globale beheerder van Azure AD-account en wachtwoord. De standaardwaarde is 500.
2. Tijdelijk uitschakelen van deze beveiliging en kunt deze verwijdert doorlopen, voert u de PowerShell-cmdlet: `Disable-ADSyncExportDeletionThreshold`. Geef een globale beheerder van Azure AD-account en wachtwoord.
   ![Referenties](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
3. Met de Azure Active Directory-Connector nog steeds is geselecteerd, selecteert u de actie **uitvoeren** en selecteer **exporteren**.
4. Als u wilt de beveiliging opnieuw inschakelen, voert u de PowerShell-cmdlet: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`. 500 vervangen door de waarde die u bij het ophalen van de huidige verwijdering drempelwaarde opgemerkt. Geef een globale beheerder van Azure AD-account en wachtwoord.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
