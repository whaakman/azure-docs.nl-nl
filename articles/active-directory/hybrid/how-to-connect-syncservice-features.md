---
title: Functies van Azure AD Connect sync-service en configuratie | Microsoft Docs
description: Beschrijving van service naast functies voor Azure AD Connect sync-service.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: fa7d0f98f8119ad34e0764423e99af4564f20387
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46311925"
---
# <a name="azure-ad-connect-sync-service-features"></a>Functies van Azure AD Connect sync-service
De synchronisatiefunctie van Azure AD Connect bestaat uit twee onderdelen:

* Het on-premises-onderdeel met de naam **Azure AD Connect-synchronisatie**, ook wel genoemd **synchronisatie-engine**.
* De service die zich bevinden in Azure AD ook wel bekend als **Azure AD Connect sync-service**

Dit onderwerp wordt uitgelegd hoe u de volgende functies van de **Azure AD Connect-synchronisatieservice** werken en hoe kunt u ze configureren met behulp van Windows PowerShell.

Deze instellingen zijn geconfigureerd door de [Azure Active Directory-Module voor Windows PowerShell](https://aka.ms/aadposh). Download en installeer deze afzonderlijk van Azure AD Connect. De cmdlets die zijn beschreven in dit onderwerp zijn geïntroduceerd in de [2016 maart-release (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Als u niet de cmdlets die in dit onderwerp wordt beschreven hoeft of doen ze niet hetzelfde resultaat opleveren, Controleer of dat u de nieuwste versie uitvoeren.

Als u wilt zien van de configuratie in uw Azure AD-directory, `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures resultaat](./media/how-to-connect-syncservice-features/getmsoldirsyncfeatures.png)

Veel van deze instellingen kunnen alleen worden gewijzigd door Azure AD Connect.

De volgende instellingen kunnen worden geconfigureerd door `Set-MsolDirSyncFeature`:

| DirSyncFeature | Opmerking |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Hiermee kunt objecten die u wilt deelnemen aan op userPrincipalName naast het primaire SMTP-adres. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |Kan de synchronisatie-engine om bij te werken van het kenmerk userPrincipalName voor gebruikers van beheerde/licentie (niet-gefedereerde). |

Nadat u een functie hebt ingeschakeld, kunnen deze kan niet worden uitgeschakeld.

> [!NOTE]
> Vanaf 24 augustus 2016 de functie *tolerantie van dubbele kenmerken* is standaard ingeschakeld voor nieuwe Azure AD-mappen. Deze functie wordt ook worden geïmplementeerd en ingeschakeld op de mappen die zijn gemaakt vóór deze datum. U ontvangt een e-mailmelding wanneer uw directory is opeens van deze functie is ingeschakeld.
> 
> 

De volgende instellingen zijn geconfigureerd door Azure AD Connect en kan niet worden gewijzigd door `Set-MsolDirSyncFeature`:

| DirSyncFeature | Opmerking |
| --- | --- |
| DeviceWriteback |[Azure AD Connect: Apparaat terugschrijven inschakelen](how-to-connect-device-writeback.md) |
| DirectoryExtensions |[Azure AD Connect-synchronisatie: Directory-extensies](how-to-connect-sync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Hiermee kunt een kenmerk in quarantaine geplaatst wanneer dit is een duplicaat van een ander object in plaats van het gehele object mislukt tijdens het exporteren. |
| Wachtwoordhashsynchronisatie |[Implementatie van wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie](how-to-connect-password-hash-synchronization.md) |
|Pass-through-verificatie|[Aanmelden van gebruikers met Azure Active Directory Pass through-verificatie](how-to-connect-pta.md)|
| UnifiedGroupWriteback |[Voorbeeld: Groep terugschrijven](how-to-connect-preview.md#group-writeback) |
| UserWriteback |Momenteel niet ondersteund. |

## <a name="duplicate-attribute-resiliency"></a>Tolerantie van dubbele kenmerken
In plaats van mislukt voor het inrichten van objecten met dubbele UPN's / proxyAddresses, dubbel kenmerk 'quarantaine' en een tijdelijke waarde is toegewezen. Wanneer het conflict is opgelost, wordt de tijdelijke UPN automatisch gewijzigd in de juiste waarde. Zie voor meer informatie, [duplicatiekenmerken identificeren voor synchronisatie- en](how-to-connect-syncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName zachte match
Wanneer deze functie is ingeschakeld, zachte match is ingeschakeld voor de UPN in aanvulling op de [primaire SMTP-adres](https://support.microsoft.com/kb/2641663), dat altijd is ingeschakeld. Zachte match wordt zodat deze overeenkomen met bestaande cloudgebruikers in Azure AD met on-premises gebruikers gebruikt.

Als u wilt vergelijken on-premises AD-accounts met bestaande accounts die zijn gemaakt in de cloud en u geen gebruikmaakt van Exchange Online, wordt deze functie is handig. In dit scenario hebben niet u over het algemeen een reden voor het instellen van de SMTP-kenmerk in de cloud.

Deze functie is standaard ingeschakeld voor zojuist gemaakt Azure AD-mappen. U kunt zien of deze functie is ingeschakeld voor u door te voeren:  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Als deze functie is niet ingeschakeld voor uw Azure AD-directory, kunt klikt u vervolgens u dit inschakelen door te voeren:  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>UserPrincipalName-updates synchroniseren
In het verleden updates voor het kenmerk UserPrincipalName met behulp van de synchronisatieservice van on-premises is geblokkeerd, tenzij beide volgende voorwaarden wordt voldaan:

* De gebruiker wordt beheerd (niet-gefedereerde).
* De gebruiker heeft geen licentie is toegewezen.

Zie voor meer informatie, [gebruikersnamen in Office 365, Azure of Intune komen niet overeen met de on-premises UPN of alternatieve aanmeldings-ID](https://support.microsoft.com/kb/2523192).

Inschakelen van deze functie kunt de synchronisatie-engine bijwerken van de userPrincipalName wanneer het gewijzigde on-premises is en u synchronisatie van wachtwoordhashes gebruiken. Als u Federatie, wordt deze functie wordt niet ondersteund.

Deze functie is standaard ingeschakeld voor zojuist gemaakt Azure AD-mappen. U kunt zien of deze functie is ingeschakeld voor u door te voeren:  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Als deze functie is niet ingeschakeld voor uw Azure AD-directory, kunt klikt u vervolgens u dit inschakelen door te voeren:  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Nadat deze functie is ingeschakeld, blijven bestaande userPrincipalName waarden als-is. Op de volgende wijziging van de userPrincipalName-kenmerk on-premises, wordt de normale Deltasynchronisatie op gebruikers de UPN bijgewerkt.  

## <a name="see-also"></a>Zie ook
* [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md).

