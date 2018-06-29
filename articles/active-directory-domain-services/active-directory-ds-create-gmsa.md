---
title: 'Azure Active Directory Domain Services: Maak een groep beheerd serviceaccount | Microsoft Docs'
description: Azure Active Directory Domain Services beheerde domeinen beheren
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: e6faeddd-ef9e-4e23-84d6-c9b3f7d16567
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: maheshu
ms.openlocfilehash: 4b4ce876760d024170020ae3faf618c7e9e76773
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035904"
---
# <a name="create-a-group-managed-service-account-gmsa-on-an-azure-ad-domain-services-managed-domain"></a>Maken van een beheerd serviceaccount (gMSA) op een beheerd domein van Azure AD Domain Services
Dit artikel laat zien hoe u beheerde serviceaccounts maakt op een beheerd domein van Azure AD Domain Services.

## <a name="managed-service-accounts"></a>Beheerde serviceaccounts
Een zelfstandige beheerde serviceaccount (sMSA) is een beheerd domeinaccount waarvan het wachtwoord wordt automatisch beheerd. Vereenvoudigt het beheer van de service principal name (SPN) en schakelt gedelegeerd beheer aan andere beheerders. Dit type beheerd serviceaccount (MSA) is geïntroduceerd in Windows Server 2008 R2 en Windows 7.

De groep beheerde serviceaccount (gMSA) biedt dezelfde voordelen voor veel servers in het domein. Alle exemplaren van een service die wordt gehost op een server-farm moeten dezelfde service-principal voor protocollen voor wederzijdse verificatie gebruiken om te werken. Als een gMSA wordt gebruikt als service-principal, beheert de Windows-besturingssysteem de accountwachtwoord in plaats van door de beheerder.

**Meer informatie:**
- [Groep overzicht beheerde serviceaccounts](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Aan de slag met beheerde serviceaccounts voor groepen](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)


## <a name="using-service-accounts-in-azure-ad-domain-services"></a>Met behulp van de service-accounts in Azure AD Domain services
Azure AD Domain Services beheerde domeinen zijn vergrendeld en beheerd door Microsoft. Er zijn een aantal belangrijke aandachtspunten voor wanneer u service-accounts met Azure AD Domain Services.

### <a name="create-service-accounts-within-custom-organizational-units-ou-on-the-managed-domain"></a>Serviceaccounts binnen aangepaste organisatie-eenheden (OU) maken op het beheerde domein
U kunt een serviceaccount in de ingebouwde 'AADDC gebruikers' of 'AADDC Computers, organisatie-eenheden maken. [Maak een aangepaste organisatie-eenheid](active-directory-ds-admin-guide-create-ou.md) op uw beheerde domein en vervolgens serviceaccounts binnen aangepaste organisatie-eenheid wilt maken.

### <a name="the-key-distribution-services-kds-root-key-is-already-pre-created"></a>De hoofdsleutel voor Key Distribution Services (KDS) is al vooraf gemaakt
De hoofdsleutel voor Key Distribution Services (KDS) is vooraf gemaakte op een beheerd domein van Azure AD Domain Services. U hoeft niet te maken van een toegangspunt KDS sleutel en niet over bevoegdheden beschikken om u te doen zodat. U kunt de KDS-basissleutel niet ofwel weergeven op het beheerde domein.

## <a name="sample---create-a-gmsa-using-powershell"></a>Voorbeeld: een gMSA maken met PowerShell
Het volgende voorbeeld ziet u hoe u een aangepaste organisatie-eenheid met behulp van PowerShell. Vervolgens kunt u een gMSA binnen de organisatie-eenheid maken met behulp van de ```-Path``` parameter om de organisatie-eenheid.

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

**PowerShell-cmdlet-documentatie:**
- [Nieuwe ADOrganizationalUnit cmdlet](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit)
- [Cmdlet nieuwe-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/New-ADServiceAccount)


## <a name="next-steps"></a>Volgende stappen
- [Een aangepaste organisatie-eenheid maken op een beheerd domein](active-directory-ds-admin-guide-create-ou.md)
- [Groep overzicht beheerde serviceaccounts](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Aan de slag met beheerde serviceaccounts voor groepen](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)
