---
title: 'Azure AD Domain Services: wachtwoordsynchronisatie inschakelen | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 947ea3c9d789ecf5a754001aafcda6f8bcd41047
ms.contentlocale: nl-nl
ms.lasthandoff: 07/08/2017


---
# <a name="enable-password-synchronization-to-azure-active-directory-domain-services"></a>Wachtwoordsynchronisatie inschakelen voor Azure Active Directory Domain Services
Tijdens de vorige taken hebt u Azure Active Directory Domain Services ingeschakeld voor uw Azure Active Directory-tenant (Azure AD). De volgende taak bestaat uit het inschakelen dat referentie-hashes voor NTLM- (NT LAN Manager) en Kerberos-verificatie moeten worden gesynchroniseerd met Azure AD Domain Services. Wanneer u de referentiesynchronisatie hebt ingesteld, kunnen gebruikers zich bij het beheerde domein aanmelden met hun zakelijke referenties.

De vereiste stappen voor gebruikersaccounts in de cloud en gebruikersaccounts die worden gesynchroniseerd vanuit uw on-premises map met Azure AD Connect, verschillen. Als uw Azure AD-tenant een combinatie van heeft van cloudgebruikers en gebruikers van uw on-premises AD, moet u beide stappen uitvoeren.

<br>

> [!div class="op_single_selector"]
> * **Cloudgebruikersaccounts**: [synchroniseer de wachtwoorden voor cloudgebruikersaccounts met uw beheerde domein](active-directory-ds-getting-started-password-sync.md)
> * **On-premises gebruikersaccounts**: [synchroniseer de wachtwoorden voor gebruikersaccounts die zijn gesynchroniseerd vanuit uw on-premises AD met uw beheerde domein](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-to-your-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a>Taak 5: wachtwoordsynchronisatie inschakelen met uw beheerde domein voor gebruikersaccounts die zijn gesynchroniseerd met uw on-premises AD
Een gesynchroniseerde Azure AD-tenant wordt zodanig ingesteld dat deze wordt gesynchroniseerd met de on-premises directory van uw organisatie via Azure AD Connect. Azure AD Connect synchroniseert niet standaard NTLM- en Kerberos-referentie-hashes met Azure AD. Als u Azure AD Domain Services wilt gebruiken, moet u Azure AD Connect configureren zodat de referentie-hashes die vereist zijn voor NTLM- en Kerberos-verificatie worden gesynchroniseerd. In de volgende stappen schakelt u de synchronisatie van de vereiste referentie-hashes van uw on-premises map met uw Azure AD-tenant in.

> [!NOTE]
> Als uw organisatie gebruikersaccounts heeft die worden gesynchroniseerd vanuit uw on-premises map, moet u synchronisatie van NTLM- en Kerberos-hashes inschakelen om het beheerde domein te kunnen gebruiken. Een gesynchroniseerd gebruikersaccount is een account dat is gemaakt in uw on-premises map en dat wordt gesynchroniseerd met uw Azure AD-tenant met gebruik van Azure AD Connect.
>
>

### <a name="install-or-update-azure-ad-connect"></a>Azure AD Connect installeren of bijwerken
Installeer de meest recente aanbevolen versie van Azure AD Connect op een computer die lid is van het domein. Als u een bestaand exemplaar van de Azure AD Connect-installatie hebt, moet u deze bijwerken om de nieuwste versie van Azure AD Connect te verkrijgen. Zorg ervoor dat u over de nieuwste versie van Azure AD Connect beschikt om te voorkomen dat bekende problemen/fouten optreden die mogelijk al zijn opgelost.

**[Azure AD Connect downloaden](http://www.microsoft.com/download/details.aspx?id=47594)**

Aanbevolen versie: **1.1.553.0** - gepubliceerd op 27 juni 2017.

> [!WARNING]
> U MOET de meest recente aanbevolen versie van Azure AD Connect installeren om ervoor te zorgen dat oudere wachtwoordreferenties (die zijn vereist voor NTLM- en Kerberos-verificatie) kunnen worden gesynchroniseerd met uw Azure AD-tenant. Deze functionaliteit is niet beschikbaar in eerdere versies van Azure AD Connect of in het oudere DirSync-hulpprogramma.
>
>

Installatie-instructies voor Azure AD Connect zijn beschikbaar in het artikel [Getting started with Azure AD Connect](../active-directory/active-directory-aadconnect.md) (Aan de slag met Azure AD Connect).

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Synchronisatie van referentie-hashes van NTLM en Kerberos naar Azure AD inschakelen
Voer het volgende PowerShell-script uit in elk AD-forest als u volledige wachtwoordsynchronisatie wilt afdwingen en ervoor wilt zorgen dat de referentie-hashes van alle on-premises gebruikers worden gesynchroniseerd met uw Azure AD-tenant. Met dit script stelt u in dat de referentie-hashes die zijn vereist voor NTLM-/Kerebos-verificatie worden gesynchroniseerd met uw Azure AD-tenant.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Afhankelijk van de grootte van de directory (aantal gebruikers, groepen enzovoort), kan de synchronisatie van referentie-hashes naar Azure AD enige tijd duren. De wachtwoorden kunnen in het beheerde domein van Azure AD Domain Services worden gebruikt kort nadat de referentie-hashes zijn gesynchroniseerd naar Azure AD.

<br>

## <a name="related-content"></a>Gerelateerde inhoud
* [Wachtwoordsynchronisatie met AAD Domain Services inschakelen voor een Azure AD-directory die zich alleen in de cloud bevindt](active-directory-ds-getting-started-password-sync.md)
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Een virtuele Windows-computer toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Een virtuele Red Hat Enterprise Linux-computer toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

