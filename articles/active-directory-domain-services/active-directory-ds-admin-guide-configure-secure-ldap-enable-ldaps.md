---
title: Secure LDAP (LDAPS) in Azure AD Domain Services inschakelen | Microsoft Docs
description: Secure LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services inschakelen
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
ms.date: 08/01/2018
ms.author: ergreenl
ms.openlocfilehash: df189e405dcd5277c1ccbd94e9d5d302660be79b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55171419"
---
# <a name="enable-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Veilige LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services inschakelen

## <a name="before-you-begin"></a>Voordat u begint
Volledige [taak 2: het certificaat voor secure LDAP om te exporteren een. PFX-bestand](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).


## <a name="task-3-enable-secure-ldap-for-the-managed-domain-using-the-azure-portal"></a>Taak 3: Secure LDAP inschakelen voor het beheerde domein met de Azure-portal
Voor secure LDAP inschakelen, moet u de volgende configuratiestappen uitvoeren:

1. Navigeer naar de  **[Azure-portal](https://portal.azure.com)**.

2. Zoek op 'domain services' in de **zoeken naar resources** zoekvak. Selecteer **Azure AD Domain Services** uit de zoekresultaten. De **Azure AD Domain Services** pagina geeft een lijst van uw beheerde domein.

    ![Zoeken naar beheerde domein wordt ingericht](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Klik op de naam van het beheerde domein (bijvoorbeeld ' contoso100.com') voor meer informatie over het domein.

    ![Domain Services - Inrichtingsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. Klik op **Secure LDAP** in het navigatiedeelvenster.

    ![Domain Services - pagina voor Secure LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. Toegang van secure LDAP tot uw beheerde domein is standaard uitgeschakeld. In-/ uitschakelen **beveiligde LDAP** naar **inschakelen**.

    ![Secure LDAP inschakelen](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. Toegang van secure LDAP tot uw beheerde domein via internet is standaard uitgeschakeld. In-/ uitschakelen **toegang van secure LDAP via internet toestaan** naar **inschakelen**, als u wilt.

    > [!WARNING]
    > Wanneer u toegang van secure LDAP via internet inschakelt, is het domein is vatbaar voor wachtwoord brute force-aanvallen via internet. Daarom raden we aan het instellen van een NSG aan het vergrendelen van toegang tot de vereiste bron-IP-adresbereiken. Zie de instructies voor [het vergrendelen van LDAPS toegang tot uw beheerde domein via internet](active-directory-ds-ldaps-bind-lockdown.md#task-6-lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet).
    >

6. Klikt u op de map pictogram **. PFX-bestand met certificaat voor secure LDAP**. Geef het pad naar het PFX-bestand met het certificaat voor secure LDAP toegang tot het beheerde domein.

7. Geef de **wachtwoord voor het ontsleutelen. PFX-bestand**. U hebt gebruikt bij het exporteren van het certificaat naar het PFX-bestand hetzelfde wachtwoord opgeven.

8. Wanneer u klaar bent, klikt u op de **opslaan** knop.

9. U ziet een melding dat informeert dat u beveiligde LDAP wordt geconfigureerd voor het beheerde domein. Totdat deze bewerking voltooid is, kunt u andere instellingen voor het domein niet wijzigen.

    ![Configureren van secure LDAP voor het beheerde domein](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> Het duurt ongeveer 10 tot 15 minuten voor secure LDAP inschakelen voor uw beheerde domein. Als het opgegeven certificaat voor secure LDAP komt niet overeen met de vereiste criteria, secure LDAP is niet ingeschakeld voor uw directory en er een fout. Bijvoorbeeld, de domeinnaam is onjuist, het certificaat al is verlopen of verloopt binnenkort. In dit geval, probeer het opnieuw met een geldig certificaat.
>
>

## <a name="next-step"></a>Volgende stap
[Taak 4: DNS configureren voor toegang tot het beheerde domein vanaf het internet](active-directory-ds-ldaps-configure-dns.md)
