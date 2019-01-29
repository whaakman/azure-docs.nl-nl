---
title: Configureren van DNS voor een beheerd domein met behulp van LDAPS via internet toegang krijgen tot | Microsoft Docs
description: Configureren van DNS voor toegang tot een Azure AD Domain Services beheerde domein met LDAPS via internet
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: a47f0f3e-2578-422a-a421-034f66de38f5
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: ergreenl
ms.openlocfilehash: 122282d168246e34aaa4a6369f7433b167355887
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55152991"
---
# <a name="configure-dns-to-access-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Configureren van DNS voor toegang tot een Azure AD Domain Services beheerde domein met behulp van veilige LDAP (LDAPS)

## <a name="before-you-begin"></a>Voordat u begint
Volledige [taak 3: secure LDAP inschakelen voor het beheerde domein met de Azure-portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)

## <a name="task-4-configure-dns-to-access-the-managed-domain-from-the-internet"></a>Taak 4: Configureren van DNS voor toegang tot het beheerde domein via internet
> [!TIP]
> **Optionele taak** : als u niet van plan bent voor toegang tot het beheerde domein met behulp van LDAPS via internet, overslaan van deze taak voor de configuratie.
>
>

Voordat u deze taak uitvoert, complete de stappen die worden beschreven in [taak 3](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md).

Nadat u de toegang van secure LDAP via internet hebt ingeschakeld, moet u DNS bijwerken zodat clientcomputers dit beheerde domein vindt. Ziet u een extern IP-adres op de **eigenschappen** tabblad **externe IP-adres voor LDAPS toegang**.

Uw externe DNS-provider zo configureren dat de DNS-naam van het beheerde domein (bijvoorbeeld ' ldaps.contoso100.com') naar deze externe IP-adres verwijst. Bijvoorbeeld, de volgende DNS-vermelding maken:

    ldaps.contoso100.com  -> 52.165.38.113

Dat is alles. U kunt nu verbinding maken met het beheerde domein met behulp van secure LDAP via internet.

> [!WARNING]
> Houd er rekening mee dat de verlener van het LDAPS-certificaat als u wilt verbinding maken met succes met het beheerde domein LDAPS wordt gebruikt door clientcomputers moeten vertrouwen. Als u een openbaar vertrouwde certificeringsinstantie (CA) gebruikt, hoeft u niet verder niets te doen omdat clientcomputers deze certificaatverleners vertrouwen. Als u een zelfondertekend certificaat gebruikt, installeert u het openbare deel van de zelf-ondertekend certificaat in het vertrouwde certificaatarchief op de clientcomputer.
>
>

## <a name="next-step"></a>Volgende stap
[Taak 5: verbinding maken met het beheerde domein en de Secure LDAP-toegang vergrendelen](active-directory-ds-ldaps-bind-lockdown.md)
