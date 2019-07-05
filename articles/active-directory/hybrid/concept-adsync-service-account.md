---
title: 'Azure AD Connect: ADSync-service-account | Microsoft Docs'
description: In dit onderwerp vindt het account van de service ADSync en aanbevolen procedures met betrekking tot het account.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 06/27/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f228da5afc5998d8fa59ce2d720cec4c9f955b67
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478717"
---
# <a name="adsync-service-account"></a>ADSync-serviceaccount
Azure AD Connect installeert een on-premises-service die wordt georganiseerd synchronisatie tussen Active Directory en Azure Active Directory.  De synchronisatieservice van Microsoft Azure AD Sync (ADSync) wordt uitgevoerd op een server in uw on-premises omgeving.  De referenties voor de service worden ingesteld in de Express-installaties standaard maar kunnen worden aangepast om te voldoen aan de beveiligingsvereisten van uw organisatie.  Deze referenties worden niet gebruikt om verbinding maken met uw on-premises-forests of Azure Active Directory.

De ADSync kiezen is-serviceaccount een belangrijke planning beslissing voordat u Azure AD Connect installeert.  Elke poging de referenties wijzigen nadat de installatie van de service niet kan worden gestart, zal leiden verlies van toegang tot de synchronisatiedatabase en niet kan worden geverifieerd met uw verbonden mappen (Azure- en AD DS).  Er is geen synchronisatie wordt uitgevoerd totdat de oorspronkelijke referenties zijn hersteld.

## <a name="the-default-adsync-service-account"></a>Het standaardaccount ADSync-service

Wanneer uitvoert op een lidserver, is de service AdSync wordt uitgevoerd in de context van een Virtual Service Account (leverancierspecifiek Kenmerk).  Een aangepaste service-account is vanwege een productbeperking gemaakt wanneer op een domeincontroller is geïnstalleerd.  Als het serviceaccount van de Express-instellingen niet aan de beveiligingsvereisten van uw organisatie, kunt u Azure AD Connect implementeren door de optie aanpassen te kiezen.  Kies vervolgens de optie voor service-account die voldoet aan de vereisten van uw organisatie.

>[!NOTE]
>Het standaard serviceaccount wanneer geïnstalleerd op een domeincontroller is van het formulier Domain\AAD_InstallationIdentifier.  Het wachtwoord voor dit account wordt willekeurig gegenereerd en geeft de grote uitdagingen voor de rotatie van herstel en het wachtwoord.  Microsoft raadt u aan het aanpassen van de serviceaccount tijdens de initiële installatie op een domeincontroller als een zelfstandig gebruiken of een groep beheerd serviceaccount (sMSA / gMSA)

|Azure AD Connect-locatie|Service-account gemaakt|
|-----|-----|
|Lidserver|NT-SERVICE\ADSync|
|Domeincontroller|Domain\AAD_74dc30c01e80 (see note)|

## <a name="custom-adsync-service-accounts"></a>Aangepaste ADSync-service-accounts
Microsoft adviseert het uitvoeren van de ADSync-service in de context van een Virtual Service Account of een zelfstandige of beheerd serviceaccount.  De domeinbeheerder mogelijk ook voor kiezen om te maken van een service-account ingericht om te voldoen aan de beveiligingsvereisten voor uw specifieke organisatie.   Voor het aanpassen van het serviceaccount dat tijdens de installatie gebruikt, kiest u de optie aanpassen op de pagina van de Express-instellingen hieronder.   De volgende opties zijn beschikbaar:

- Standaard-account: Azure AD Connect wordt ingericht op het serviceaccount, zoals hierboven beschreven
- beheerd serviceaccount: gebruik van een zelfstandige of groepen beheerde Serviceaccounts die zijn ingericht door uw beheerder
- domeinaccount: gebruik een domein-serviceaccount die door de beheerder is ingericht

![](media/concept-adsync-service-account/adsync1.png)

![](media/concept-adsync-service-account/adsync2.png)

## <a name="diagnosing-adsync-service-account-changes"></a>Accountwijzigingen ADSync-service vaststellen
De referenties voor de service ADSync wijzigen na installatie tot de service niet starten leiden zal, verliezen toegang tot de synchronisatiedatabase en niet kan worden geverifieerd met uw verbonden mappen (Azure- en AD DS).  Verlenen toegang tot de database naar de nieuwe ADSync-service-account is onvoldoende om van dit probleem te herstellen. Er is geen synchronisatie wordt uitgevoerd totdat de oorspronkelijke referenties zijn hersteld.

De ADSync-service geeft een niveau foutbericht wordt weergegeven in het gebeurtenislogboek wanneer deze kan niet worden gestart.  De inhoud van het bericht varieert afhankelijk van of de ingebouwde database (localdb) of het volledige SQL in gebruik is.  Hier volgen enkele voorbeelden van de logboekgegevens van gebeurtenissen die mogelijk aanwezig zijn.

### <a name="example-1"></a>Voorbeeld 1

De versleutelingssleutels AdSync-service kunnen niet worden gevonden en is opnieuw gemaakt.  Synchronisatie wordt niet uitgevoerd totdat dit probleem is opgelost.

Het oplossen van dit probleem het Microsoft Azure AD Sync versleutelingssleutels niet meer toegankelijk als de AdSync-service aanmelden referenties worden gewijzigd.  Als de referenties zijn gewijzigd, gebruikt u de Services-toepassing te wijzigen van het account aanmelden terug naar de oorspronkelijk geconfigureerde waarde (ex. NT-SERVICE\AdSync) en de service opnieuw.  Hiermee wordt onmiddellijk juiste werking van de service AdSync hersteld.

Raadpleeg de volgende [artikel](https://go.microsoft.com/fwlink/?linkid=2086764) voor meer informatie.

### <a name="example-2"></a>Voorbeeld 2

De service is niet starten omdat een verbinding met de lokale database (localdb) kan niet worden gemaakt.

Het oplossen van dit probleem de Microsoft Azure AD Sync-service, verliest machtiging voor toegang tot de provider van de lokale database als service AdSync aanmelden referenties worden gewijzigd.  Als de referenties zijn gewijzigd de Services-toepassing gebruiken om te wijzigen van het account aanmelden terug naar de oorspronkelijk geconfigureerde waarde (ex. NT-SERVICE\AdSync) en de service opnieuw.  Hiermee wordt onmiddellijk juiste werking van de service AdSync hersteld.

Raadpleeg de volgende [artikel](https://go.microsoft.com/fwlink/?linkid=2086764) voor meer informatie.

Aanvullende Details van die de gegevens van de volgende fout is geretourneerd door de provider:
 

``` 
OriginalError=0x80004005 OLEDB Provider error(s): 
Description  = 'Login timeout expired'
Failure Code = 0x80004005
Minor Number = 0 
Description  = 'A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections. For more information see SQL Server Books Online.'
```
## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).