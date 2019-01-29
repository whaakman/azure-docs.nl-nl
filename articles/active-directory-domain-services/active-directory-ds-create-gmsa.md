---
title: 'Azure Active Directory Domain Services: Maak een groep beheerd serviceaccount | Microsoft Docs'
description: Azure Active Directory Domain Services beheerde domeinen beheren
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: e6faeddd-ef9e-4e23-84d6-c9b3f7d16567
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: ergreenl
ms.openlocfilehash: ef21383db52d45d996f2c6c1d6900ebba2349635
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203076"
---
# <a name="create-a-group-managed-service-account-gmsa-on-an-azure-ad-domain-services-managed-domain"></a>Een groep beheerd serviceaccount (gMSA) op een beheerd domein van Azure AD Domain Services maken
Dit artikel laat u het maken van beheerde serviceaccounts in een Azure AD Domain Services beheerde domein.

## <a name="managed-service-accounts"></a>Beheerde serviceaccounts
Een zelfstandig beheerd serviceaccount (sMSA) is een beheerd domeinaccount waarvan het wachtwoord wordt automatisch beheerd. Dit vereenvoudigt het beheer van service principal name (SPN) en kunnen gedelegeerd beheer aan andere beheerders. Dit type beheerd serviceaccount (MSA) werd geïntroduceerd in Windows Server 2008 R2 en Windows 7.

De groep beheerde serviceaccount (gMSA) biedt de dezelfde voordelen voor veel servers in het domein. Alle exemplaren van een service die wordt gehost op een server-farm moeten de dezelfde service-principal voor wederzijdse verificatieprotocollen gebruiken om te werken. Wanneer een gMSA wordt gebruikt als service-principal, beheert het Windows-besturingssysteem van de account wachtwoord in plaats van de beheerder.

**Meer informatie:**
- [Overzicht van de groep beheerde Service-Accounts](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Aan de slag met beheerde serviceaccounts voor groepen](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)


## <a name="using-service-accounts-in-azure-ad-domain-services"></a>Met behulp van service-accounts in Azure AD Domain services
Azure AD Domain Services beheerde domeinen zijn vergrendeld en beheerd door Microsoft. Er zijn een aantal belangrijke overwegingen bij het gebruik van service-accounts met Azure AD Domain Services.

### <a name="create-service-accounts-within-custom-organizational-units-ou-on-the-managed-domain"></a>Service-accounts in aangepaste organisatie-eenheden (OU) maken in het beheerde domein
U kunt een service-account kan niet maken in de ingebouwde 'AADDC gebruikers' of 'AADDC Computers' organisatie-eenheden. [Maak een aangepaste organisatie-eenheid](active-directory-ds-admin-guide-create-ou.md) op uw beheerde domein en maak vervolgens de service-accounts in aangepaste organisatie-eenheid.

### <a name="the-key-distribution-services-kds-root-key-is-already-pre-created"></a>De hoofdsleutel van de distributie (KDS-hoofdsleutel) is al vooraf gemaakt
De hoofdsleutel van de distributie (KDS-hoofdsleutel) is vooraf gemaakte op een beheerd domein van Azure AD Domain Services. U hoeft te maken van de hoofdmap van een KDS-sleutel en niet over bevoegdheden beschikken om u te doen dus. U weergeven ofwel de KDS-basissleutel in het beheerde domein niet.

## <a name="sample---create-a-gmsa-using-powershell"></a>-Het maken van een gMSA met behulp van PowerShell
Het volgende voorbeeld toont het maken van een aangepaste organisatie-eenheid met behulp van PowerShell. Vervolgens kunt u een gMSA binnen de organisatie-eenheid maken met behulp van de ```-Path``` parameter opgeven voor de organisatie-eenheid.

```powershell
# Create a new custom OU on the managed domain
New-ADOrganizationalUnit -Name "MyNewOU" -Path "DC=CONTOSO100,DC=COM"

# Create a service account 'WebFarmSvc' within the custom OU.
New-ADServiceAccount -Name WebFarmSvc  `
-DNSHostName ` WebFarmSvc.contoso100.com  `
-Path "OU=MYNEWOU,DC=CONTOSO100,DC=com"  `
-KerberosEncryptionType AES128, AES256  ` -ManagedPasswordIntervalInDays 30  `
-ServicePrincipalNames http/WebFarmSvc.contoso100.com/contoso100.com, `
http/WebFarmSvc.contoso100.com/contoso100,  `
http/WebFarmSvc/contoso100.com, http/WebFarmSvc/contoso100  `
-PrincipalsAllowedToRetrieveManagedPassword CONTOSO-SERVER$
```

**Voor PowerShell-cmdlets:**
- [Nieuwe ADOrganizationalUnit cmdlet](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit)
- [Cmdlet nieuwe-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/New-ADServiceAccount)


## <a name="next-steps"></a>Volgende stappen
- [Een aangepaste organisatie-eenheid maken in een beheerd domein](active-directory-ds-admin-guide-create-ou.md)
- [Overzicht van de groep beheerde Service-Accounts](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Aan de slag met beheerde serviceaccounts voor groepen](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)
