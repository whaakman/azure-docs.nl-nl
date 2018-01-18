---
title: Azure AD Connect sync-service-functies en configuratie | Microsoft Docs
description: Beschrijving van service aan clientzijde functies voor Azure AD Connect sync-service.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e939c6a53a1154b23431581e789605d83e849fe2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect sync-service-functies
De synchronisatiefunctie van Azure AD Connect bestaat uit twee onderdelen:

* Lokale onderdeel **Azure AD Connect-synchronisatie**, ook wel **synchronisatie-engine**.
* De service zich in Azure AD, ook wel bekend als **Azure AD Connect sync-service**

Dit onderwerp wordt uitgelegd hoe u de volgende functies van de **Azure AD Connect-synchronisatieservice** werk en hoe kunt u ze configureren met Windows PowerShell.

Deze instellingen zijn geconfigureerd door de [Azure Active Directory-Module voor Windows PowerShell](http://aka.ms/aadposh). Download en installeer deze afzonderlijk van Azure AD Connect. De cmdlets die zijn beschreven in dit onderwerp zijn geïntroduceerd in de [versie van de maart 2016 (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Als u niet de cmdlets in dit onderwerp wordt beschreven hebt of ze geen hetzelfde resultaat genereren, Controleer of dat u de meest recente versie uitvoert.

Als u wilt zien van de configuratie in uw Azure AD-directory, `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures resultaat](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Veel van deze instellingen kunnen alleen worden gewijzigd door Azure AD Connect.

De volgende instellingen kunnen worden geconfigureerd door `Set-MsolDirSyncFeature`:

| DirSyncFeature | Opmerking |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Hiermee kunt objecten die u wilt deelnemen aan op userPrincipalName naast het primaire SMTP-adres. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |Hiermee kunt de synchronisatie-engine voor het bijwerken van het kenmerk userPrincipalName voor gebruikers met beheerde/een licentie (niet-gefedereerde). |

Nadat u een functie hebt ingeschakeld, kunnen deze kan niet worden uitgeschakeld.

> [!NOTE]
> Vanaf 24 augustus 2016 de functie *dubbel kenmerk tolerantie* is standaard ingeschakeld voor de nieuwe Azure AD-mappen. Deze functie wordt ook uitgerold en ingeschakeld op de mappen die zijn gemaakt voor deze datum. U ontvangt een e-mailbericht wanneer uw directory wordt deze functie is ingeschakeld.
> 
> 

De volgende instellingen zijn geconfigureerd voor Azure AD Connect en kan niet worden gewijzigd door `Set-MsolDirSyncFeature`:

| DirSyncFeature | Opmerking |
| --- | --- |
| DeviceWriteback |[Azure AD Connect: Apparaat terugschrijven inschakelen](active-directory-aadconnect-feature-device-writeback.md) |
| DirectoryExtensions |[Azure AD Connect-synchronisatie: Directory-uitbreidingen](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Hiermee kunt een kenmerk in quarantaine geplaatst wanneer dit is een duplicaat van een ander object in plaats van het gehele object mislukt tijdens het exporteren. |
| PasswordSync |[Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie implementeren](active-directory-aadconnectsync-implement-password-synchronization.md) |
| UnifiedGroupWriteback |[Voorbeeld: Groep terugschrijven](active-directory-aadconnect-feature-preview.md#group-writeback) |
| UserWriteback |Momenteel niet ondersteund. |

## <a name="duplicate-attribute-resiliency"></a>Dubbel kenmerk tolerantie
Objecten in plaats van het mislukken te richten met dubbele UPN's / proxyAddresses, het gedupliceerde kenmerk 'quarantaine' en een tijdelijke-waarde is toegewezen. Als het conflict is opgelost, wordt de tijdelijke UPN automatisch gewijzigd in de juiste waarde. Zie voor meer informatie [tolerantie voor synchronisatie en dubbel kenmerk](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName zachte overeen
Wanneer deze functie is ingeschakeld, soft-match is ingeschakeld voor de UPN in aanvulling op de [primaire SMTP-adres](https://support.microsoft.com/kb/2641663), die altijd is ingeschakeld. Soft-match wordt gebruikt voor bestaande cloudgebruikers in Azure AD overeen met on-premises gebruikers.

Als u wilt vergelijken lokale AD-accounts met bestaande accounts die zijn gemaakt in de cloud en u geen gebruikmaakt van Exchange Online en deze functie is handig. In dit scenario wordt doorgaans er geen reden is om de SMTP-kenmerk ingesteld in de cloud.

Deze functie op standaard voor nieuw is Azure AD-mappen. U kunt zien of deze functie is ingeschakeld voor u door te voeren:  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Als deze functie is niet ingeschakeld voor uw Azure AD-directory, kunt klikt u inschakelen door te voeren:  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>UserPrincipalName updates synchroniseren
In het verleden hebben updates voor het kenmerk UserPrincipalName met behulp van de synchronisatieservice van on-premises is geblokkeerd, tenzij beide volgende voorwaarden wordt voldaan:

* De gebruiker wordt beheerd (niet-gefedereerde).
* De gebruiker heeft geen licentie toegewezen.

Zie voor meer informatie [gebruikersnamen in Office 365, Azure of Intune komen niet overeen met de lokale UPN of de alternatieve aanmeldings-ID](https://support.microsoft.com/kb/2523192).

Deze functie inschakelt, kunt de synchronisatie-engine de userPrincipalName bijwerken wanneer het gewijzigde lokale is en u Wachtwoordsynchronisatie gebruiken. Als u federation gebruikt, wordt deze functie wordt niet ondersteund.

Deze functie op standaard voor nieuw is Azure AD-mappen. U kunt zien of deze functie is ingeschakeld voor u door te voeren:  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Als deze functie is niet ingeschakeld voor uw Azure AD-directory, kunt klikt u inschakelen door te voeren:  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Nadat u deze functie inschakelt, bestaande userPrincipalName waarden blijft-is. Op de volgende wijziging van de userPrincipalName kenmerk on-premises, wordt de normale Deltasynchronisatie op gebruikers de UPN bijgewerkt.  

## <a name="see-also"></a>Zie ook
* [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).

