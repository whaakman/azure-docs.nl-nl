---
title: Beveiligde LDAP (LDAPS) in Azure AD Domain Services configureren | Microsoft Docs
description: Beveiligde LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services configureren
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: maheshu
ms.openlocfilehash: 771ca39b37e6fb2d75a86df3ac785bc293b4cd5f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Beveiligde LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services configureren
Dit artikel laat zien hoe u beveiligde Lightweight Directory Access Protocol (LDAPS) kunt inschakelen voor uw beheerde domein van Azure AD Domain Services. Beveiligde LDAP wordt ook wel ' Lightweight Directory Access Protocol (LDAP) via Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).
4. Een **certificaat moet worden gebruikt voor het inschakelen van beveiligde LDAP**.

   * **Aanbevolen** -een certificaat verkrijgen van een betrouwbare, openbare certificeringsinstantie. Deze configuratieoptie is veiliger.
   * U kunt ook u kunt ook [een zelfondertekend certificaat maken](#task-1---obtain-a-certificate-for-secure-ldap) zoals verderop in dit artikel.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Vereisten voor de beveiligde LDAP-certificaat
Een geldig certificaat per de volgende richtlijnen te verkrijgen voordat u beveiligde LDAP inschakelen. Er optreden fouten als u probeert in te schakelen beveiligde LDAP voor uw beheerde domein met een ongeldig/onjuist certificaat.

1. **Vertrouwde uitgevers** -het certificaat moet worden uitgegeven door een instantie wordt vertrouwd door computers verbinding maken met het beheerde domein met behulp van beveiligde LDAP. Deze instantie is mogelijk een openbare certificeringsinstantie (CA) of een CA voor ondernemingen wordt vertrouwd door deze computers.
2. **Levensduur** -het certificaat moet geldig zijn ten minste de komende 3-6 maanden. Beveiligde LDAP-toegang tot uw beheerde domein wordt onderbroken wanneer het certificaat is verlopen.
3. **Onderwerpnaam** -de onderwerpnaam op het certificaat moet een jokerteken voor uw beheerde domein. Bijvoorbeeld, als uw domein met de naam, contoso100.com', de onderwerpnaam van het certificaat moet ' *. contoso100.com'. De DNS-naam (alternatieve onderwerpnaam) ingesteld op deze jokertekennaam.
4. **Sleutelgebruik** -het certificaat moet worden geconfigureerd voor het volgende gebruikt - digitale handtekeningen en sleutelcodering.
5. **Doel van het certificaat** -het certificaat moet geldig zijn voor SSL-serververificatie.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Taak 1: een certificaat verkrijgen voor beveiligde LDAP
De eerste taak omvat het verkrijgen van een certificaat gebruikt voor beveiligde LDAP-toegang tot het beheerde domein. U hebt hiervoor twee opties:

* Een certificaat verkrijgen van een openbare CA of een ondernemings-CA.
* Een zelfondertekend certificaat maken.

> [!NOTE]
> Clientcomputers die verbinding moeten maken met het beheerde domein met behulp van beveiligde LDAP moeten de verlener van het beveiligde LDAP-certificaat vertrouwen.
>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Optie een (aanbevolen) - een beveiligde LDAP-certificaat verkrijgen van een certificeringsinstantie (CA)
Als uw organisatie krijgt de certificaten van een openbare Certificeringsinstantie, moet u de beveiligde LDAP-certificaat verkrijgen van die openbare Certificeringsinstantie. Als u een ondernemings-CA implementeert, moet u de beveiligde LDAP-certificaat verkrijgen van de ondernemings-CA.

> [!TIP]
> **Gebruik zelfondertekende certificaten voor beheerde domeinen met '. onmicrosoft.com' domeinachtervoegsels.**
> Als de DNS-domeinnaam van uw beheerde domein eindigt op ". onmicrosoft.com', u een beveiligde LDAP-certificaat kan niet verkrijgen van een openbare certificeringsinstantie. Omdat Microsoft eigenaar is van het domein 'onmicrosoft.com', weigeren openbare certificeringsinstanties uitgeven van een beveiligde LDAP-certificaat voor u voor een domein met dit achtervoegsel. In dit scenario een zelfondertekend certificaat maken en gebruiken die beveiligde LDAP configureren.
>

Zorg ervoor dat het certificaat dat u via de openbare certificeringsinstantie verkrijgen voldoet aan alle vereisten die worden beschreven in [vereisten voor de LDAP om certificaten veilig](#requirements-for-the-secure-ldap-certificate).


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Optie B - een zelfondertekend certificaat maken voor beveiligde LDAP
Als u niet van plan bent om een certificaat van een openbare certificeringsinstantie te gebruiken, kunt u een zelfondertekend certificaat maken voor beveiligde LDAP. Kies deze optie als de DNS-domeinnaam van uw beheerde domein eindigt op ". onmicrosoft.com'.

**Maak een zelfondertekend certificaat met behulp van PowerShell**

Open op uw Windows-computer als een nieuwe PowerShell-venster **beheerder** en typ de volgende opdrachten, een nieuw zelfondertekend certificaat maken.

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject *.contoso100.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.com
```

Vervang in het voorgaande voorbeeld, '*. contoso100.com' met de DNS-domeinnaam van uw beheerde domein. For example, als u een beheerd domein 'contoso100.onmicrosoft.com' genoemd, vervangen door '*. contoso100.com' in het voorgaande script met ' *. contoso100.onmicrosoft.com').

![Azure AD-directory selecteren](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

De zojuist gemaakte zelf-ondertekend certificaat wordt geplaatst in het certificaatarchief van de lokale machine.


## <a name="next-step"></a>Volgende stap
[Taak 2: het beveiligde LDAP-certificaat te exporteren een. PFX-bestand](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
