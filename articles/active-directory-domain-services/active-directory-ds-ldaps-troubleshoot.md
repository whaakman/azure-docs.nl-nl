---
title: Secure LDAP (LDAPS) in Azure AD Domain Services oplossen | Microsoft Docs
description: Secure LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services oplossen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 445c60da-e115-447b-841d-96739975bdf6
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: ergreenl
ms.openlocfilehash: a055c73ea868ba40065638d90fadf66f5204276c
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54848747"
---
# <a name="troubleshoot-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Secure LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services oplossen

## <a name="connection-issues"></a>Verbindingsproblemen
Als u problemen ondervindt bij het verbinding maken met het beheerde domein met behulp van secure LDAP:

* De keten van de verlener van het certificaat voor secure LDAP moet worden vertrouwd op de client. U kunt de basiscertificeringsinstantie toevoegen aan het certificaatarchief Vertrouwde basiscertificeringsinstanties op de client de vertrouwensrelatie tot stand brengen.
* Controleer of dat de LDAP-client (bijvoorbeeld ldp.exe) maakt verbinding met de beveiligde LDAP-eindpunt met behulp van een DNS-naam, niet het IP-adres.
* Controleer de DNS-naam die van de LDAP-client maakt verbinding met. Het moet worden omgezet naar het openbare IP-adres voor secure LDAP in het beheerde domein.
* Controleer of dat het certificaat voor secure LDAP voor uw beheerde domein heeft de DNS-naam in het onderwerp of de alternatieve namen voor onderwerpen-kenmerk.
* De instellingen van de Netwerkbeveiligingsgroep voor het virtuele netwerk moeten het verkeer op poort 636 toestaan vanaf het internet. Deze stap geldt alleen als u de toegang van secure LDAP via internet hebt ingeschakeld.


## <a name="need-help"></a>Hulp nodig?
Als u nog geen verbinding met het beheerde domein met behulp van secure LDAP [Neem contact op met het productteam](active-directory-ds-contact-us.md) voor hulp. Voeg de volgende informatie om u te helpen bij het vaststellen van het probleem beter:
* Een schermafbeelding van ldp.exe gebruikt en mislukt de verbinding.
* Uw Azure AD-tenant-ID en de DNS-domeinnaam van uw beheerde domein.
* Exacte gebruikersnaam die u probeert te koppelen als.


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Grondbeginselen van de LDAP-query](https://technet.microsoft.com/library/aa996205.aspx)
* [Groepsbeleid in een Azure AD Domain Services beheerde domein beheren](active-directory-ds-admin-guide-administer-group-policy.md)
* [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md)
* [Een Netwerkbeveiligingsgroep maken](../virtual-network/tutorial-filter-network-traffic.md)
