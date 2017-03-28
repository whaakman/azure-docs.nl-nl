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
ms.date: 03/17/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 4969b43831a3813a4e76c6447c252a9c458f371a
ms.lasthandoff: 03/17/2017


---
# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Wachtwoordsynchronisatie inschakelen voor Azure AD Domain Services
In de voorgaande taken hebt u Azure AD Domain Services ingeschakeld voor uw Azure AD-tenant. Voor de volgende taak moet u de synchronisatie van wachtwoorden inschakelen voor Azure AD Domain Services. Wanneer de synchronisatie van referenties is ingesteld, kunnen gebruikers zich aanmelden bij het beheerde domein middels hun zakelijke referenties.

De vereiste stappen verschillen, afhankelijk van of uw organisatie een Azure AD-tenant in de cloud heeft of kan synchroniseren met uw on-premises directory via Azure AD Connect.

<br>

> [!div class="op_single_selector"]
> * [Azure AD-tenant in de cloud](active-directory-ds-getting-started-password-sync.md)
> * [Gesynchroniseerde Azure AD-tenant](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Taak 5: wachtwoordsynchronisatie met AAD Domain Services inschakelen voor een gesynchroniseerde Azure AD-directory
Een gesynchroniseerde Azure AD-tenant wordt zodanig ingesteld dat deze wordt gesynchroniseerd met de on-premises directory van uw organisatie via Azure AD Connect. Azure AD Connect synchroniseert niet standaard NTLM- en Kerberos-referentie-hashes met Azure AD. Als u Azure AD Domain Services wilt gebruiken, moet u Azure AD Connect configureren zodat de referentie-hashes die vereist zijn voor NTLM- en Kerberos-verificatie worden gesynchroniseerd. 

> [!WARNING]
> U MOET wachtwoordsynchronisatie met AAD Domain Services inschakelen telkens wanneer u Azure AD Domain Services inschakelt. Mogelijk hebt u Azure AD Domain Services al eerder ingeschakeld voor uw Azure AD-directory en dit vervolgens later uitgeschakeld. U moet echter wachtwoordsynchronisatie toch inschakelen wanneer u Azure AD Domain Services de volgende keer inschakelt voor de directory.
>
>

In de volgende stappen schakelt u de synchronisatie van de vereiste referentie-hashes met uw Azure AD-tenant in.

### <a name="install-or-update-azure-ad-connect"></a>Azure AD Connect installeren of bijwerken
Installeer de meest recente aanbevolen versie van Azure AD Connect op een computer die lid is van het domein. Als u een bestaand exemplaar van de Azure AD Connect-installatie hebt, moet u deze bijwerken om de nieuwste versie van Azure AD Connect te verkrijgen. Zorg ervoor dat u over de nieuwste versie van Azure AD Connect beschikt om te voorkomen dat bekende problemen/fouten optreden die mogelijk al zijn opgelost.

**[Azure AD Connect downloaden](http://www.microsoft.com/download/details.aspx?id=47594)**

Aanbevolen versie: **1.1.281.0** - gepubliceerd op 7 september 2016.

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

