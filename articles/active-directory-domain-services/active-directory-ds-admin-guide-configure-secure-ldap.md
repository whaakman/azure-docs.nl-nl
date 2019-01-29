---
title: Secure LDAP (LDAPS) configureren in Azure AD Domain Services | Microsoft Docs
description: Secure LDAP (LDAPS) configureren voor een Azure AD Domain Services beheerde domein
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: ergreenl
ms.openlocfilehash: 66c50d7ec9948b1f2447aadcef4a4a8f321204ef
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55177387"
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Veilige LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services configureren
Dit artikel wordt beschreven hoe u beveiligde Lightweight Directory Access Protocol (LDAPS) kunt inschakelen voor uw Azure AD Domain Services beheerde domein. Secure LDAP wordt ook wel bekend als ' Lightweight Directory Access Protocol (LDAP) via Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Een **dat moet worden gebruikt om in te schakelen van secure LDAP**.

   * **Aanbevolen** -een certificaat verkrijgen van een vertrouwde openbare certificeringsinstantie (CA). Deze configuratieoptie is veiliger.
   * U kunt ook u kunt desgewenst ook [een zelfondertekend certificaat maken](#task-1---obtain-a-certificate-for-secure-ldap) zoals verderop in dit artikel.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Vereisten voor het certificaat voor secure LDAP
Een geldig certificaat per de volgende richtlijnen aan te schaffen voordat u secure LDAP inschakelen. Er optreden fouten als u probeert te secure LDAP inschakelen voor uw beheerde domein met een ongeldige/onjuiste certificaat.

1. **Vertrouwde verlener** -het certificaat moet worden uitgegeven door een instantie die wordt vertrouwd door verbinding te maken met het beheerde domein met behulp van secure LDAP. Deze instantie is mogelijk een openbare certificeringsinstantie (CA) of een CA voor ondernemingen wordt vertrouwd door deze computers.
2. **Levensduur** -het certificaat moet geldig zijn voor ten minste de komende 3-6 maanden. Toegang van Secure LDAP tot uw beheerde domein wordt onderbroken wanneer het certificaat is verlopen.
3. **Naam van het onderwerp** -de naam van de certificaathouder op het certificaat moet uw beheerde domein. Bijvoorbeeld, als uw domein heet 'contoso100.com', moet de onderwerpnaam van het certificaat 'contoso100.com'. De DNS-naam (alternatieve onderwerpnaam) ingesteld op een wildcard-naam voor uw beheerde domein.
4. **Sleutelgebruik** -het certificaat moet worden geconfigureerd voor het volgende gebruikt: digitale handtekeningen en sleutelcodering.
5. **Doel van het certificaat** -het certificaat moet geldig zijn voor SSL-serververificatie.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Taak 1: een certificaat voor secure LDAP verkrijgen
De eerste taak omvat het verkrijgen van een certificaat dat wordt gebruikt voor toegang van secure LDAP aan het beheerde domein. U hebt hiervoor twee opties:

* Een certificaat verkrijgen van een openbare CA of een ondernemings-CA.
* Maak een zelf-ondertekend certificaat.

> [!NOTE]
> Client-computers die verbinding moeten maken met het beheerde domein met behulp van secure LDAP moeten de verlener van het certificaat voor secure LDAP vertrouwen.
>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Optie een (aanbevolen): een certificaat voor secure LDAP verkrijgen van een certificeringsinstantie (CA)
Als uw organisatie de certificaten van een openbare CA verkrijgt, moet u het certificaat voor secure LDAP verkrijgen van die openbare Certificeringsinstantie. Als u een CA voor ondernemingen implementeert, moet u het certificaat voor secure LDAP verkrijgen van de ondernemings-CA.

> [!TIP]
> **Zelfondertekende certificaten gebruiken voor beheerde domeinen met '. onmicrosoft.com' domeinachtervoegsels.**
> Als de DNS-domeinnaam van uw beheerde domein eindigt op '. onmicrosoft.com', kan u niet een certificaat voor secure LDAP verkrijgen van een openbare certificeringsinstantie (CA). Omdat Microsoft eigenaar is van het domein 'onmicrosoft.com', wordt de openbare certificeringsinstanties weigeren om uit te geven u een certificaat voor secure LDAP voor een domein met dit achtervoegsel. In dit scenario wordt een zelfondertekend certificaat maken en gebruiken die voor het configureren van secure LDAP.
>

Zorg ervoor dat het certificaat dat u via de openbare certificeringsinstantie verkrijgen voldoet aan alle vereisten die worden beschreven in [vereisten voor het certificaat voor secure LDAP](#requirements-for-the-secure-ldap-certificate).


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Optie B - maken van een zelfondertekend certificaat voor secure LDAP
Als u niet verwacht dat een certificaat van een openbare certificeringsinstantie (CA) te gebruiken, besluiten kunt u om te maken van een zelfondertekend certificaat voor secure LDAP. Kies deze optie als de DNS-domeinnaam van uw beheerde domein eindigt op '. onmicrosoft.com'.

**Maak een zelf-ondertekend certificaat met behulp van PowerShell**

Open op uw Windows-computer, een nieuwe PowerShell-venster als **beheerder** en typ de volgende opdrachten, een nieuw zelfondertekend certificaat maken.

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.com, contoso100.com
```

In het voorgaande voorbeeld, kunt u 'contoso100.com' vervangen door de DNS-domeinnaam van uw beheerde domein. Bijvoorbeeld, als u een beheerd domein met de naam 'contoso100.onmicrosoft.com' gemaakt, vervangen door 'contoso100.com' in het kenmerk onderwerp 'contoso100.onmicrosoft.com' en '*. contoso100.com' in het kenmerk van de DNS-naam met '*. contoso100.onmicrosoft.com').

![Azure AD-directory selecteren](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Het zojuist gemaakte zelfondertekende certificaat is in het certificaatarchief van de lokale computer geplaatst.


## <a name="next-step"></a>Volgende stap
[Taak 2: het exporteren van het certificaat voor secure LDAP naar een. PFX-bestand](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
