---
title: 'Azure AD Connect-synchronisatie: Onopzettelijke verwijderingen voorkomen | Microsoft Docs'
description: Dit onderwerp beschrijft de functie te voorkomen dat onopzettelijke verwijderingen (onopzettelijke verwijderingen voorkomen) in Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 25bc3d0c0517278f20cba4fd4beb2fc4d82b66f1
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464150"
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect-synchronisatie: Onopzettelijke verwijderingen voorkomen
Dit onderwerp beschrijft de functie te voorkomen dat onopzettelijke verwijderingen (onopzettelijke verwijderingen voorkomen) in Azure AD Connect.

Bij het installeren van Azure AD Connect, te voorkomen dat per ongeluk is verwijderd standaard ingeschakeld en geconfigureerd voor het exporteren van een met meer dan 500 verwijderingen niet toestaan. Deze functie is ontworpen om u te beschermen tegen onbedoelde configuratiewijzigingen en wijzigingen aan uw on-premises directory die invloed zouden hebben op veel gebruikers en andere objecten.

## <a name="what-is-prevent-accidental-deletes"></a>Wat is onopzettelijke verwijderingen voorkomen
Algemene scenario's, wanneer er veel verwijderingen opnemen:

* Wijzigingen in [filteren](how-to-connect-sync-configure-filtering.md) waar een volledige [organisatie-eenheid](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) of [domein](how-to-connect-sync-configure-filtering.md#domain-based-filtering) niet is geselecteerd.
* alle objecten in een organisatie-eenheid worden verwijderd;
* de naam van een organisatie-eenheid is gewijzigd, zodat alle objecten in die eenheid als buiten het synchronisatiebereik worden beschouwd.

De standaardwaarde van 500 objecten kan worden gewijzigd met PowerShell met behulp van `Enable-ADSyncExportDeletionThreshold`, deze maakt deel uit van de synchronisatie van de AD-module geïnstalleerd met Azure Active Directory Connect. U moet deze waarde aan de grootte van uw organisatie configureren. Omdat de synchronisatieplanning wordt elke 30 minuten uitgevoerd, is de waarde het aantal verwijderingen gezien binnen 30 minuten.

Als er te veel verwijderingen voorbereid om te worden geëxporteerd naar Azure AD, wordt het exporteren is geblokkeerd en u ontvangt een e-mailbericht als volgt:

![E-mailadres onopzettelijke verwijderingen voorkomen](./media/how-to-connect-sync-feature-prevent-accidental-deletes/email.png)

> *Hallo (technische contactpersoon). (Tijd) de Identity synchronization service gedetecteerd dat het aantal verwijderingen het verwijderen van de geconfigureerde drempelwaarde voor (organisatienaam) overschreden. Een totaal van (getal) objecten zijn in deze identiteitssynchronisatie uitvoeren voor verwijdering verzonden. Dit bereikt of overschrijdt de drempelwaarde voor de geconfigureerde verwijderen van objecten (getal). U moet bevestigen dat deze verwijderingen moeten worden verwerkt voordat we wordt voortgezet. Raadpleeg het voorkomen dat onbedoelde verwijderingen voor meer informatie over de fout die worden vermeld in dit e-mailbericht.*
>
> 

U ziet ook de status `stopped-deletion-threshold-exceeded` wanneer u zoeken in de **Synchronization Service Manager** gebruikersinterface voor het profiel exporteren.
![Synchronisatie van Service Manager-UI van onopzettelijke verwijderingen voorkomen](./media/how-to-connect-sync-feature-prevent-accidental-deletes/syncservicemanager.png)

Als dit werd niet verwacht, onderzoeken en corrigerende maatregelen. Als u wilt zien welke objecten zijn op het punt te worden verwijderd, het volgende doen:

1. Start **Synchronization Service** vanuit het startmenu.
2. Ga naar **Connectors**.
3. Selecteer de Connector met het type **Azure Active Directory**.
4. Onder **acties** aan de rechterkant, selecteer **Search Connector Space**.
5. In het pop-upvenster onder **bereik**, selecteer **verbroken omdat** en kies een tijd in het verleden. Klik op **Zoeken**. Deze pagina bevat een overzicht van alle objecten op het punt te worden verwijderd. Door te klikken op elk item, krijgt u meer informatie over het object. U kunt ook klikken op **kolominstelling** om toe te voegen extra kenmerken voor weergave in het raster.

![Connectorgebied zoeken](./media/how-to-connect-sync-feature-prevent-accidental-deletes/searchcs.png)

Als alle verwijderingen worden gewenst is, klikt u vervolgens het volgende doen:

1. Als u wilt ophalen van de huidige drempelwaarde verwijderen, voert u de PowerShell-cmdlet `Get-ADSyncExportDeletionThreshold`. Geef een account voor globale beheerders van Azure AD en het wachtwoord. De standaardwaarde is 500.
2. Tijdelijk deze beveiliging uitschakelen en kunt deze verwijderingen doorlopen, voert u de PowerShell-cmdlet: `Disable-ADSyncExportDeletionThreshold`. Geef een account voor globale beheerders van Azure AD en het wachtwoord.
   ![Referenties](./media/how-to-connect-sync-feature-prevent-accidental-deletes/credentials.png)
3. Met de Azure Active Directory-Connector nog steeds is geselecteerd, selecteert u de actie **uitvoeren** en selecteer **exporteren**.
4. Als u wilt de beveiliging opnieuw inschakelt, voert u de PowerShell-cmdlet: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`. 500 vervangen door de waarde die u al opgemerkt bij het ophalen van de huidige drempelwaarde voor verwijderen. Geef een account voor globale beheerders van Azure AD en het wachtwoord.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
