---
title: 'Azure AD Connect: Problemen met synchronisatie van het object | Microsoft Docs'
description: Dit onderwerp bevat stappen voor het oplossen van problemen met synchronisatie van object met de taak voor het oplossen van problemen.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f65e84bff63bbdb781991ff6648b0fb98ca5208f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592310"
---
# <a name="troubleshoot-object-synchronization-with-azure-ad-connect-sync"></a>Problemen met object-synchronisatie met Azure AD Connect-synchronisatie
In dit artikel bevat stappen voor het oplossen van problemen met synchronisatie van object met behulp van de taak voor het oplossen van problemen. Als u wilt zien hoe het oplossen van problemen werkt in Azure Active Directory (Azure AD) verbinding maken, bekijken [in deze korte video](https://aka.ms/AADCTSVideo).

## <a name="troubleshooting-task"></a>Het oplossen van taak
Voor Azure AD implementatie verbinden met versie 1.1.749.0 of hoger, gebruik van de taak voor het oplossen van problemen in de wizard object synchronisatieproblemen kunt oplossen. Voor eerdere versies Neem oplossen handmatig beschreven [hier](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md).

### <a name="run-the-troubleshooting-task-in-the-wizard"></a>Voer de taak voor het oplossen van problemen in de wizard
Als u wilt de taak voor het oplossen van problemen in de wizard uitvoert, moet u de volgende stappen uitvoeren:

1.  Open een nieuwe Windows PowerShell-sessie op uw Azure AD Connect-server met het uitvoeren als beheerder optie.
2.  Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.
3.  Start de Azure AD Connect-wizard.
4.  Navigeer naar de pagina aanvullende taken, selecteer oplossen en klik op volgende.
5.  Klik op starten om de probleemoplossing menu start in PowerShell op de pagina probleemoplossing.
6.  Selecteer in het hoofdmenu Object synchronisatie oplossen.
![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch11.png)

### <a name="troubleshooting-input-parameters"></a>Invoerparameters probleemoplossing
De volgende invoerparameters nodig door de taak voor het oplossen van problemen:
1.  **Object-DN-naam** – dit is de DN-naam van het object dat het oplossen van problemen moet
2.  **Naam voor AD Connector** – dit is de naam van het AD-forest waarin de bovenstaande object zich bevindt.
3.  Azure AD-tenant hoofdbeheerdersreferenties ![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch1.png)

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Inzicht in de resultaten van de taak voor het oplossen van problemen
De taak voor het oplossen van problemen worden de volgende controles uitgevoerd:

1.  UPN verschil detecteren als het object is gesynchroniseerd met Azure Active Directory
2.  Controleer als object wordt gefilterd door het filteren van domein
3.  Controleer als object vanwege wordt gefilterd op de OE-filters
4.  Controleer als object synchronisatie is geblokkeerd vanwege een gekoppeld postvak
5. Controleer of de object dynamische distributie-groep die niet moet worden gesynchroniseerd

De rest van deze sectie beschrijft specifieke resultaten die zijn geretourneerd door de taak. In elk geval biedt de taak een analyse gevolgd door de aanbevolen acties om het probleem te verhelpen.

## <a name="detect-upn-mismatch-if-object-is-synced-to-azure-active-directory"></a>UPN verschil detecteren als object is gesynchroniseerd met Azure Active Directory
### <a name="upn-suffix-is-not-verified-with-azure-ad-tenant"></a>UPN-achtervoegsel is niet geverifieerd bij Azure AD-Tenant
Wanneer UserPrincipalName (UPN) / achtervoegsel Alternate Login ID is niet geverifieerd bij de Azure AD-Tenant en Azure Active Directory de UPN-achtervoegsels vervangt door de standaard domain name "onmicrosoft.com".

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch2.png)

### <a name="changing-upn-suffix-from-one-federated-domain-to-another-federated-domain"></a>Wijzigen van de UPN-achtervoegsel van één federatieve domein naar een ander federatieve domein
Azure Active Directory is niet toegestaan voor de synchronisatie van UserPrincipalName (UPN) / Alternate Login ID achtervoegsel wijziging van het ene federatieve domein naar een andere federatieve domein. Dit geldt voor domeinen die worden geverifieerd met de Azure AD-Tenant en het verificatietype als federatieve hebben.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch3.png) 

### <a name="azure-ad-tenant-dirsync-feature-synchronizeupnformanagedusers-is-disabled"></a>Azure AD-Tenant DirSync-functie 'SynchronizeUpnForManagedUsers' is uitgeschakeld
Als de DirSync-functie van Azure AD-Tenant 'SynchronizeUpnForManagedUsers' is uitgeschakeld, heeft Azure Active Directory niet toestaan dat updates synchronisatie UserPrincipalName/alternatieve aanmeldings-ID voor gelicentieerde gebruikersaccounts met beheerde-verificatie.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch4.png)

## <a name="object-is-filtered-due-to-domain-filtering"></a>Object is gefilterd door het filteren van domein
### <a name="domain-is-not-configured-to-sync"></a>Domein is niet geconfigureerd om te synchroniseren
Er is een object buiten het bereik van vanwege domein niet geconfigureerd. In het onderstaande voorbeeld is het object is niet gesynchroniseerd bereik als het domein dat hoort bij wordt gefilterd van synchronisatie.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch5.png)

### <a name="domain-is-configured-to-sync-but-is-missing-run-profilesrun-steps"></a>Het domein is geconfigureerd om te synchroniseren, maar ontbreekt stappen uitvoeren profielen/uitvoeren
Object is buiten het bereik als het domein ontbreekt profielen/uitvoeren stappen uitvoeren. Het object is niet synchroon bereik in het onderstaande voorbeeld, zoals het domein dat hoort bij uitgevoerde stappen voor de volledige Import profiel uitvoert ontbreekt.
![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch6.png)

## <a name="object-is-filtered-due-to-ou-filtering"></a>Object is gefilterd vanwege op OE-filters
Het object is niet gesynchroniseerd bereik vanwege OE filteren configuratie. Het object in het onderstaande voorbeeld behoort OE = NoSync, DC = bvtadwbackdc, DC = com.  Deze organisatie-eenheid is niet opgenomen in het bereik van de synchronisatie.</br>

![ORGANISATIE-EENHEID](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch7.png)

## <a name="linked-mailbox-issue"></a>Gekoppelde Postvak probleem
Een gekoppeld postvak moet worden gekoppeld aan een externe hoofdaccount in een andere vertrouwde accountforest. Als er geen dergelijke externe hoofdaccount, wordt Azure AD Connect zal de gebruiker niet synchroniseren account komt overeen met het gekoppelde postvak in het Exchange-forest met de Azure AD-tenant.</br>
![Gekoppeld postvak](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch12.png)

## <a name="dynamic-distribution-group-issue"></a>Dynamische distributiegroep probleem
Vanwege diverse verschillen tussen on-premises Active Directory en Azure Active Directory, Azure AD Connect niet wordt gesynchroniseerd dynamische distributiegroepen met de Azure AD-tenant.

![Dynamische distributiegroep](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch13.png)

## <a name="html-report"></a>HTML-rapport
Naast het analyseren van het object, genereert de taak voor het oplossen van problemen ook een HTML-rapport dat alles wat u bekend zijn voor het object is. Dit rapport HTML kan worden gedeeld met het ondersteuningsteam doen verder het oplossen van problemen, indien nodig.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch8.png)

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
