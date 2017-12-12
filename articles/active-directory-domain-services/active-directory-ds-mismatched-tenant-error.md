---
title: Los de fouten niet-overeenkomende directory voor bestaande Azure AD Domain Services beheerde domeinen | Microsoft Docs
description: Begrijpen en oplossen van fouten van niet-overeenkomende directory voor bestaande beheerde domeinen van Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 40eb75b7-827e-4d30-af6c-ca3c2af915c7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: maheshu
ms.openlocfilehash: 24e11769e9b403bc00157e3f60869effa6a9633f
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="resolve-mismatched-directory-errors-for-existing-azure-ad-domain-services-managed-domains"></a>Los de fouten niet-overeenkomende directory voor bestaande beheerde domeinen van Azure AD Domain Services
U hebt een bestaande Azure AD Domain Services beheerd domein. Wanneer u gaat u naar de Azure-portal en het beheerde domein, ziet u het volgende foutbericht weergegeven:

![Fout tijdens het niet-overeenkomende directory](.\media\getting-started\mismatched-tenant-error.png)

U kunt dit beheerde domein niet beheren totdat de fout is opgelost.


## <a name="whats-causing-this-error"></a>Wat de oorzaak van deze fout?
Deze fout wordt veroorzaakt als uw beheerde domein en het virtuele netwerk is ingeschakeld in deel uitmaken van twee verschillende Azure AD-tenants. Bijvoorbeeld, u hebt een beheerd domein 'contoso.com' genoemd en deze is ingeschakeld voor van Contoso Azure AD-tenant. Echter, de Azure-netwerk waarin het beheerde domein is ingeschakeld hoort bij Fabrikam - een andere Azure AD-tenant.

De nieuwe Azure portal (en specifiek de extensie Azure AD Domain Services) is gebouwd op Azure Resource Manager. In de moderne Azure Resource Manager-omgeving, worden bepaalde beperkingen afgedwongen voor het leveren betere beveiliging en voor toegang op basis van rollen (RBAC) tot resources beheren. Inschakelen van Azure AD Domain Services voor een Azure AD-tenant is een gevoelige bewerking omdat het ervoor zorgt referentie-hashes dat kunnen worden gesynchroniseerd met het beheerde domein. Deze bewerking moet u een beheerder tenant voor de map. Bovendien moet u beheerdersrechten hebben via het virtuele netwerk waarin u het beheerde domein inschakelen. Het beheerde domein en het virtuele netwerk voor RBAC controleert het consistent worden gewerkt, moeten behoren tot dezelfde Azure AD-tenant.

Kortom, kan u een beheerd domein inschakelen voor een Azure AD-tenant 'contoso.com' in een virtueel netwerk die horen bij een Azure-abonnement die eigendom zijn van een andere Azure AD-tenant 'fabrikam.com'. 

**Geldige configuratie**: In dit implementatiescenario het beheerde domein van Contoso is ingeschakeld voor de Contoso Azure AD-tenant. Het beheerde domein komt tot uiting in een virtueel netwerk die horen bij een Azure-abonnement die eigendom zijn van de Contoso Azure AD-tenant. Daarom behoren zowel het beheerde domein als het virtuele netwerk tot dezelfde Azure AD-tenant. Deze configuratie is ongeldig en volledig wordt ondersteund.

![Geldige tenantconfiguratie](./media/getting-started/valid-tenant-config.png)

**Niet-overeenkomende tenantconfiguratie**: In dit implementatiescenario het beheerde domein van Contoso is ingeschakeld voor de Contoso Azure AD-tenant. Het beheerde domein is echter beschikbaar zijn in een virtueel netwerk dat bij een Azure-abonnement die eigendom zijn van de Fabrikam Azure AD-tenant hoort. Daarom het beheerde domein en het virtuele netwerk deel uitmaken van twee verschillende Azure AD-tenants. Deze configuratie is de niet-overeenkomende tenantconfiguratie en wordt niet ondersteund. Het virtuele netwerk moet worden verplaatst naar de dezelfde Azure AD-tenant (dat wil zeggen, Contoso) als het beheerde domein. Zie de [resolutie](#resolution) sectie voor meer informatie.

![Niet-overeenkomende tenantconfiguratie](./media/getting-started/mismatched-tenant-config.png)

Daarom wanneer het beheerde domein en het virtuele netwerk is ingeschakeld in deel uitmaken van twee verschillende Azure AD-tenants u deze fout te zien.

Er gelden de volgende regels in de Resource Manager-omgeving:
- Een Azure AD-directory mogelijk meerdere Azure-abonnementen.
- Een Azure-abonnement mogelijk meerdere resources, zoals virtuele netwerken.
- Een beheerd domein van één Azure AD Domain Services is ingeschakeld voor een Azure Active directory.
- Een beheerd domein van Azure AD Domain Services kan worden ingeschakeld op een virtueel netwerk die horen bij een van de Azure-abonnementen binnen dezelfde Azure AD-tenant.


## <a name="resolution"></a>Oplossing
Hebt u twee opties de niet-overeenkomende directory-fout op te lossen. U mag:

- Klik op de **verwijderen** knop om te verwijderen van de bestaande beheerd domein. Opnieuw maken met behulp van de [Azure-portal](https://portal.azure.com), zodat het beheerde domein en het virtuele netwerk beschikbaar in de is behoren tot de Azure AD-directory. Voeg alle machines die eerder zijn toegevoegd aan het verwijderde domein aan het zojuist gemaakte beheerde domein.

- Verplaats het Azure-abonnement met het virtuele netwerk naar de Azure AD-directory waartoe uw beheerde domein behoort. Volg de stappen in de [eigendom van een Azure-abonnement overdragen aan een ander account](../billing/billing-subscription-transfer.md) artikel.


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Problemen oplossen met - Azure AD Domain Services](active-directory-ds-troubleshooting.md)
