---
title: Los de fouten niet-overeenkomende map voor bestaande Azure AD Domain Services beheerde domeinen | Microsoft Docs
description: Begrijpen en oplossen van niet-overeenkomende map voor bestaande Azure AD Domain Services beheerde domeinen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 40eb75b7-827e-4d30-af6c-ca3c2af915c7
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/11/2017
ms.author: ergreenl
ms.openlocfilehash: 7f69214304161c091a4550aff770b6333656972c
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54844208"
---
# <a name="resolve-mismatched-directory-errors-for-existing-azure-ad-domain-services-managed-domains"></a>Los de fouten niet-overeenkomende map voor bestaande Azure AD Domain Services beheerde domeinen
Hebt u een bestaande Azure AD Domain Services beheerde domein. Wanneer u gaat u naar de Azure-portal en het beheerde domein, ziet u de volgende strekking weergegeven:

![Niet-overeenkomende map fout](./media/getting-started/mismatched-tenant-error.png)

U kunt dit beheerde domein niet beheren tot de fout is opgelost.


## <a name="whats-causing-this-error"></a>Wat deze fout wordt veroorzaakt?
Deze fout wordt veroorzaakt wanneer uw beheerde domein en het virtuele netwerk is ingeschakeld in deel uitmaken van twee verschillende Azure AD-tenants. Bijvoorbeeld, hebt u een beheerd domein 'contoso.com' genoemd en is ingeschakeld voor het Contoso Azure AD-tenant. Echter, de Azure-netwerk waarin het beheerde domein is ingeschakeld hoort bij Fabrikam - een andere Azure AD-tenant.

De nieuwe Azure-portal (en specifiek de extensie Azure AD Domain Services) is gebouwd op Azure Resource Manager. In de moderne Azure Resource Manager-omgeving, worden bepaalde beperkingen afgedwongen om toegang te bieden betere beveiliging en voor toegang op basis van rollen (RBAC) tot resources beheren. Als u Azure AD Domain Services voor een Azure AD-tenant inschakelt, is een gevoelige bewerking omdat het ervoor zorgt dat referentie-hashes worden gesynchroniseerd met het beheerde domein. Met deze bewerking moet u een tenant-beheerder voor de map. Bovendien moet u beheerdersrechten hebben via het virtuele netwerk waarin u het beheerde domein inschakelen. Het beheerde domein en het virtuele netwerk voor RBAC controleert het consistent worden gewerkt, moeten behoren tot dezelfde Azure AD-tenant.

Kort gezegd, kan u een beheerd domein inschakelen voor een Azure AD-tenant 'contoso.com' in een virtueel netwerk die behoren tot een Azure-abonnement eigendom zijn van een andere Azure AD-tenant 'fabrikam.com'. 

**Geldige configuratie**: In dit implementatiescenario worden het beheerde domein Contoso is ingeschakeld voor de Contoso Azure AD-tenant. Het beheerde domein wordt weergegeven in een virtueel netwerk die behoren tot een Azure-abonnement eigendom zijn van de Contoso Azure AD-tenant. Daarom behoren zowel het beheerde domein als het virtuele netwerk tot dezelfde Azure AD-tenant. Deze configuratie is geldig en volledig wordt ondersteund.

![Geldige tenantconfiguratie](./media/getting-started/valid-tenant-config.png)

**Niet-overeenkomende tenantconfiguratie**: In dit implementatiescenario worden het beheerde domein Contoso is ingeschakeld voor de Contoso Azure AD-tenant. Het beheerde domein wordt echter weergegeven in een virtueel netwerk die deel uitmaakt van een Azure-abonnement eigendom zijn van de Fabrikam Azure AD-tenant. Daarom het beheerde domein en het virtuele netwerk horen bij twee verschillende Azure AD-tenants. Deze configuratie is de configuratie van de niet-overeenkomende tenants en wordt niet ondersteund. Het virtuele netwerk moet worden verplaatst naar dezelfde Azure AD-tenant (dat wil zeggen, Contoso) als het beheerde domein. Zie de [resolutie](#resolution) sectie voor meer informatie.

![Configuratie van niet-overeenkomende tenants](./media/getting-started/mismatched-tenant-config.png)

Daarom wanneer het beheerde domein en het virtuele netwerk is ingeschakeld in deel uitmaken van twee verschillende Azure AD-tenants deze fout wordt weergegeven.

Er gelden de volgende regels in de Resource Manager-omgeving:
- Een Azure AD-directory mogelijk meerdere Azure-abonnementen.
- Een Azure-abonnement hebben mogelijk meerdere resources, zoals virtuele netwerken.
- Één Azure AD Domain Services beheerde domein is ingeschakeld voor een Azure AD-directory.
- Een beheerd domein van Azure AD Domain Services kan worden ingeschakeld in een virtueel netwerk die behoren tot een van de Azure-abonnementen binnen dezelfde Azure AD-tenant.


## <a name="resolution"></a>Oplossing
U hebt twee opties om op te lossen de niet-overeenkomende map-fout. U kunt:

- Klik op de **verwijderen** knop verwijderen van de bestaande beheerde domein. Opnieuw maken met behulp van de [Azure-portal](https://portal.azure.com), zodat het beheerde domein en het virtuele netwerk beschikbaar in de is deel uitmaken van de Azure AD-directory. Neem deel aan alle machines die eerder zijn toegevoegd aan het verwijderde domein met de zojuist gemaakte beheerde domein.

- Verplaats het Azure-abonnement met het virtuele netwerk met de Azure AD-directory, die uw beheerde domein behoort. Volg de stappen in de [eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md) artikel.


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Problemen oplossen - Azure AD Domain Services met](active-directory-ds-troubleshooting.md)
