---
title: 'Azure AD Connect: Objectsynchronisatie oplossen | Microsoft Docs'
description: Dit onderwerp bevat stappen voor het oplossen van problemen met objectsynchronisatie met behulp van de taak voor het oplossen van problemen.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: ed13014dd5ad559ed3793ff52ae410be9174ae2b
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54461311"
---
# <a name="troubleshoot-object-synchronization-with-azure-ad-connect-sync"></a>Objectsynchronisatie met Azure AD Connect-synchronisatie oplossen
Dit artikel bevat stappen voor het oplossen van problemen met objectsynchronisatie met behulp van de taak voor het oplossen van problemen. Als u wilt zien hoe u problemen oplossen in Azure Active Directory (Azure AD) Connect werkt, Bekijk [in deze korte video](https://aka.ms/AADCTSVideo).

## <a name="troubleshooting-task"></a>Taak voor het oplossen van problemen
Voor Azure AD Connect implementatie met versie 1.1.749.0 of hoger, gebruikt u de taak voor het oplossen van problemen in de wizard om problemen met synchronisatie object te. Voor eerdere versies oplossen handmatig zoals wordt beschreven [hier](tshoot-connect-object-not-syncing.md).

### <a name="run-the-troubleshooting-task-in-the-wizard"></a>Het oplossen van problemen met taak uitvoeren in de wizard
Als u wilt de taak voor het oplossen van problemen in de wizard uitvoert, moet u de volgende stappen uitvoeren:

1.  Open een nieuwe Windows PowerShell-sessie op uw Azure AD Connect-server met het uitvoeren als beheerder optie.
2.  Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.
3.  Start de Azure AD Connect-wizard.
4.  Navigeer naar de pagina extra taken, selecteert u problemen oplossen en klik op volgende.
5.  Klik op starten voor het oplossen van problemen met het menu start in PowerShell op de pagina voor problemen oplossen.
6.  Selecteer in het hoofdmenu van Objectsynchronisatie oplossen.
![](media/tshoot-connect-objectsync/objsynch11.png)

### <a name="troubleshooting-input-parameters"></a>Invoerparameters voor het oplossen van problemen
De volgende invoerparameters nodig zijn voor de taak voor het oplossen van problemen:
1.  **Object-DN-naam** : dit is de DN-naam van het object dat problemen oplossen moet
2.  **Naam voor AD Connector** : dit is de naam van de AD-forest waarin de bovenstaande object zich bevindt.
3.  Azure AD-tenant hoofdbeheerdersreferenties ![](media/tshoot-connect-objectsync/objsynch1.png)

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Inzicht in de resultaten van de taak voor het oplossen van problemen
Het oplossen van problemen met taak voert de volgende controles uit:

1.  UPN incompatibel detecteren of het object is gesynchroniseerd met Azure Active Directory
2.  Controleer als object wordt gefilterd door het domein filteren
3.  Controleer als object vervaldatum wordt gefilterd op OE filteren
4.  Controleert u of de objectsynchronisatie van het is geblokkeerd vanwege een gekoppeld postvak
5. Controleer of object dynamische distributie-groep die mag niet worden gesynchroniseerd

De rest van deze sectie wordt beschreven specifieke resultaten die worden geretourneerd door de taak. In elk geval bevat de taak een analyse die gevolgd door de aanbevolen acties om het probleem te verhelpen.

## <a name="detect-upn-mismatch-if-object-is-synced-to-azure-active-directory"></a>UPN discrepantie gedetecteerd als object is gesynchroniseerd met Azure Active Directory
### <a name="upn-suffix-is-not-verified-with-azure-ad-tenant"></a>UPN-achtervoegsel is niet geverifieerd met Azure AD-Tenant
Wanneer UserPrincipalName (UPN) / achtervoegsel van de alternatieve aanmeldings-ID is niet geverifieerd met de Azure AD-Tenant en Azure Active Directory de UPN-achtervoegsels vervangt door de standaard domein naam 'onmicrosoft.com'.

![](media/tshoot-connect-objectsync/objsynch2.png)

### <a name="changing-upn-suffix-from-one-federated-domain-to-another-federated-domain"></a>Het wijzigen van UPN-achtervoegsel van een federatief domein naar een ander federatief domein
Azure Active Directory is niet toegestaan voor de synchronisatie van UserPrincipalName (UPN) / Alternate Login ID achtervoegsel wijziging ten opzichte van een federatief domein naar een ander federatief domein. Dit geldt voor domeinen die worden geverifieerd met de Azure AD-Tenant en het verificatietype dat wordt als federatieve hebben.

![](media/tshoot-connect-objectsync/objsynch3.png) 

### <a name="azure-ad-tenant-dirsync-feature-synchronizeupnformanagedusers-is-disabled"></a>Azure AD-Tenant DirSync-functie 'SynchronizeUpnForManagedUsers' is uitgeschakeld
Wanneer de DirSync-functie van Azure AD-Tenant 'SynchronizeUpnForManagedUsers' is uitgeschakeld, is Azure Active Directory kan geen updates van de synchronisatie voor UserPrincipalName/alternatieve aanmeldings-ID voor de gelicentieerde gebruikersaccounts met beheerde verificatie.

![](media/tshoot-connect-objectsync/objsynch4.png)

## <a name="object-is-filtered-due-to-domain-filtering"></a>Object is gefilterd door het domein filteren
### <a name="domain-is-not-configured-to-sync"></a>Domein is niet geconfigureerd om te synchroniseren
Er is een object buiten het bereik vanwege domein niet is geconfigureerd. In het volgende voorbeeld wordt het object is niet gesynchroniseerd bereik als het domein dat hoort bij is gefilterd van synchronisatie.

![](media/tshoot-connect-objectsync/objsynch5.png)

### <a name="domain-is-configured-to-sync-but-is-missing-run-profilesrun-steps"></a>Domein is geconfigureerd voor synchroniseren, maar ontbreekt stappen uitvoeren profielen/uitvoeren
Object is buiten het bereik als het domein, ontbreekt uitgevoerd werk profielen/uitvoeren. Het object is niet gesynchroniseerd bereik in het onderstaande voorbeeld als het domein dat hoort bij stappen uitvoeren voor de volledige Import profiel uitvoert ontbreekt.
![](media/tshoot-connect-objectsync/objsynch6.png)

## <a name="object-is-filtered-due-to-ou-filtering"></a>Object wordt vervaldatum gefilterd op OE filteren
Het object is niet gesynchroniseerd bereik vanwege een OE-filters gebruiken om configuratie. In het volgende voorbeeld wordt het object deel uitmaakt van organisatie-eenheid = NoSync, DC = bvtadwbackdc, DC = com.  Deze organisatie-eenheid is niet opgenomen in het bereik van gegevenssynchronisatie.</br>

![OU](./media/tshoot-connect-objectsync/objsynch7.png)

## <a name="linked-mailbox-issue"></a>Gekoppelde Postvak probleem
Een gekoppeld postvak moet worden gekoppeld aan een externe hoofdaccount zich in een ander vertrouwde accountforest. Als er geen dergelijke externe hoofdaccount is, wordt Azure AD Connect zal de gebruiker niet synchroniseren rekening komt overeen met de gekoppelde postvak in het Exchange-forest met de Azure AD-tenant.</br>
![Linked Mailbox](./media/tshoot-connect-objectsync/objsynch12.png)

## <a name="dynamic-distribution-group-issue"></a>Dynamische distributiegroep probleem
Als gevolg van diverse verschillen tussen on-premises synchroniseert Active Directory en Azure Active Directory, Azure AD Connect niet dynamisch distributiegroepen met de Azure AD-tenant.

![Dynamische distributiegroep](./media/tshoot-connect-objectsync/objsynch13.png)

## <a name="html-report"></a>HTML-rapport
Naast het analyseren van het object, genereert de taak voor het oplossen van problemen ook een HTML-rapport dat alles bekend over het object is. Dit HTML-rapport kan worden gedeeld met het ondersteuningsteam om te doen probleemoplossing, indien nodig meer.

![](media/tshoot-connect-objectsync/objsynch8.png)

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).
