---
title: Beheer van meerdere tenants ervaringen met Azure naar
description: Azure-gedelegeerde Resourcemanagement kunt een cross-tenant-beheerervaring.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 15454d4b3f0abad6166c4b163df6c8652669d649
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809902"
---
# <a name="cross-tenant-management-experiences"></a>Ervaringen voor cross-tenant

In dit artikel beschrijft de scenario's die u, als een serviceprovider met gebruiken kunt [Azure Resourcemanagement overgedragen](../concepts/azure-delegated-resource-management.md) voor het beheren van Azure-resources voor meerdere klanten uit uw eigen tenant in de [Azure-portal ](https://portal.azure.com).

> [!NOTE]
> Azure-resource gedelegeerd beheer kan ook worden gebruikt in een onderneming met meerdere tenants van de eigen cross-tenant-beheer te vereenvoudigen.

## <a name="understanding-customer-tenants"></a>Inzicht krijgen in klant-tenants

Een tenant Azure Active Directory (Azure AD) is een weergave van een organisatie. Het is een toegewezen exemplaar van Azure AD dat een organisatie ontvangt wanneer ze een relatie met Microsoft maken door de registratie voor Azure, Microsoft 365 of andere services. Elke Azure AD-tenant is uniek en werkt afzonderlijk van andere Azure AD-tenants, en heeft een eigen tenant-ID (GUID). Zie voor meer informatie, [wat is Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)

Normaal gesproken om Azure-resources beheren voor een klant, serviceproviders zelf zou moeten zich aanmelden bij de Azure-portal met een account dat is gekoppeld aan de betreffende klant-tenant, vereisen een beheerder zijn in de tenant van de klant te maken en beheren van gebruikersaccounts voor de serviceprovider.

Hiermee geeft u gebruikers binnen de serviceprovider-tenant die toegang tot en beheer van abonnementen, resourcegroepen en resources in de tenant van de klant kan met Azure gedelegeerde resource Manager, op het onboarding-proces. Deze gebruikers kunnen vervolgens zich aanmelden bij Azure portal met behulp van hun eigen referenties. In Azure portal, kunnen ze alle klanten waartoe ze toegang hebben tot resources beheren. Dit kan worden gedaan door naar de pagina de [mijn klanten](../how-to/view-manage-customers.md) pagina in de Azure-portal of door te werken rechtstreeks binnen de context van het abonnement van de betreffende klant, in Azure portal of via API's.

Azure-gedelegeerde Resourcemanagement biedt meer flexibiliteit voor het beheren van resources voor meerdere klanten zonder zich aanmeldt bij verschillende accounts in verschillende tenants. Een serviceprovider mogelijk bijvoorbeeld drie klanten, met verschillende verantwoordelijkheden en toegangsniveaus, zoals hier wordt weergegeven:

![Drie klant tenants waarin de verantwoordelijkheden van de service-provider](../media/azure-delegated-resource-management-customer-tenants.jpg)

Met behulp van Azure-gedelegeerde Resourcemanagement, geautoriseerde gebruikers kunnen zich aanmelden bij de tenant van de serviceprovider voor toegang tot deze resources, zoals hier wordt weergegeven:

![Resources van de klant beheerd via een serviceprovider-tenant](../media/azure-delegated-resource-management-service-provider-tenant.jpg)

## <a name="supported-services-and-scenarios"></a>Ondersteunde services en scenario 's

De ervaring tussen tenants ondersteunt momenteel de volgende scenario's met resources van de gedelegeerde klant:

[Azure Automation](https://docs.microsoft.com/azure/automation/):

- Automation-accounts gebruiken voor toegang tot en werken met resources van de gedelegeerde klant

[Azure Backup](https://docs.microsoft.com/azure/backup/):

- Back-up en herstellen van gegevens van de klant in klant-tenants

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com//azure/aks/):

- Gehoste Kubernetes-omgevingen beheren en implementeren en beheren van containertoepassingen in klant-tenants

[Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/):

- Meldingen weergeven voor gemachtigde abonnementen in Azure portal of programmatisch via REST API-aanroepen, de mogelijkheid om waarschuwingen in alle abonnementen weer te geven
- Details van computeractiviteit logboek voor gedelegeerde abonnementen weergeven
- Log analytics: Opvragen van gegevens uit externe klanten toegang tot werkruimten in meerdere tenants

[Azure Policy](https://docs.microsoft.com/azure/governance/policy/):

- Naleving momentopnamen weergeven details voor toegewezen beleidsregels binnen gedelegeerde abonnementen
- Maken en bewerken van de beleidsdefinities binnen een gedelegeerde abonnement
- Toewijzen van beleidsdefinities binnen het abonnement overgedragen door de klant gedefinieerde
- Klanten zien beleidsregels die zijn geschreven door de serviceprovider samen met eventuele beleidsregels voor die ze zelf hebt geschreven
- DeployIfNotExists-toewijzingen in de klant-tenants kunnen herstellen als de klant heeft geconfigureerd dat de beheerde identiteit en *roleDefinitionIds* voor deze beleidstoewijzing

[Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/):

- Bevat nu de tenant-ID in de resultaten van de geretourneerde query, zodat u kunt bepalen of een abonnement bij de klant of serviceprovider-tenant hoort

[Azure Security Center](https://docs.microsoft.com/azure/security-center/):

- Zichtbaarheid van alle tenants
  - Naleving van beveiligingsbeleid bewaken en zorg ervoor dat beveiliging dekking voor alle tenants-resources
  - Continue naleving van regelgeving bewaking over meerdere klanten in één weergave
  - Controleren, sorteren en prioriteren van bruikbare security aanbevelingen met secure berekening van de risicoscore
- Cross-tenant-houding beveiligingsbeheer
  - Beveiligingsbeleid beheren
  - Actie ondernemen voor resources die zich niet meer compatibel met bruikbare beveiligingsaanbevelingen
  - Verzamelen en opslaan van gegevens met betrekking tot beveiliging
- Detectie van bedreigingen voor alle tenants en beveiliging
  - Detectie van bedreigingen voor tenants-resources
  - Geavanceerde threat protection besturingselementen zoals just-in-time (JIT) VM-toegang van toepassing
  - Beveiliging van de netwerkconfiguratie voor beveiliging met adaptieve netwerk beperken
  - Zorg ervoor dat de servers wordt uitgevoerd alleen de toepassingen en processen die ze met besturingselementen voor adaptieve toepassingen moeten
  - Wijzigingen in belangrijke bestanden en registervermeldingen verhelpen met bestand integriteit controleren (FIM) bewaken

[Azure-servicestatus](https://docs.microsoft.com/azure/service-health/):

- Controleer de status van resources van de klant met Azure Resource Health
- Volg de status van de Azure-services die worden gebruikt door uw klanten

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/):

- Opties voor noodherstel voor Azure-machines in klant-tenants (Houd er rekening mee dat u Run as-accounts niet gebruiken voor het kopiëren van de VM-extensies) beheren

[Virtuele Machines van Azure](https://docs.microsoft.com/azure/virtual-machines/):

- Extensies voor virtuele machines gebruiken voor het configuratie- en automatiseringstaken taken na de implementatie op Azure Virtual machines in klant-tenants
- Diagnostische gegevens over opstarten gebruiken voor het oplossen van virtuele Azure-machines in klant-tenants
- Toegang tot virtuele machines met de seriële console in klant-tenants
- Houd er rekening mee dat u Azure Active Directory niet voor externe aanmelding aan een virtuele machine gebruiken en een virtuele machine kan niet worden geïntegreerd met een Key Vault voor wachtwoorden, geheimen en cryptografische sleutels voor versleuteling van schijf

[Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/):

- Virtuele netwerken en virtuele netwerkinterfacekaarten (vNICs) binnen de klant tenants implementeren en beheren

Aanvragen voor ondersteuning:

- Opent ondersteuningsaanvragen voor gedelegeerde resources uit de **Help en ondersteuning** blade in Azure portal (door het ondersteuningsplan die beschikbaar zijn voor het gedelegeerd bereik)

Wees op de hoogte van de volgende huidige beperkingen met alle scenario's:

- Aanvragen die worden verwerkt door Azure Resource Manager kunnen worden uitgevoerd met behulp van Azure-gedelegeerde resourcemanagement. De bewerking URI's voor deze aanvragen worden gestart met `https://management.azure.com`. Echter, aanvragen die worden verwerkt door een exemplaar van een resourcetype (zoals KeyVault-geheimen toegang of toegang tot de gegevens van opslag) worden niet ondersteund met Azure gedelegeerde resource Manager. De bewerking URI's voor deze aanvragen worden doorgaans begint met een adres dat is uniek is voor uw exemplaar, zoals `https://myaccount.blob.core.windows.net` of `https://mykeyvault.vault.azure.net/`. De laatste ook zijn doorgaans gegevensbewerkingen in plaats van beheerbewerkingen. 
- Roltoewijzingen moeten op rollen gebaseerd toegangsbeheer (RBAC) gebruiken [ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles). Alle ingebouwde rollen worden momenteel ondersteund met Azure gedelegeerde resource Manager met uitzondering van de eigenaar of Administrator voor gebruikerstoegang ingebouwde rollen met [DataActions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#dataactions) machtiging. Aangepaste rollen en [klassiek abonnement beheerdersrollen](https://docs.microsoft.com/azure/role-based-access-control/classic-administrators) worden ook niet ondersteund.
- Op dit moment u kan onboarding niet een abonnement (of resourcegroep binnen een abonnement) voor Azure Resourcemanagement overgedragen als het abonnement gebruikmaakt van Azure Databricks. Op dezelfde manier als een abonnement is geregistreerd voor de voorbereiding op met de **Microsoft.ManagedServices** resourceprovider, kunt u zich niet voor het maken van een Databricks-werkruimte voor dat abonnement op dit moment.

## <a name="using-apis-and-management-tools-with-cross-tenant-management"></a>Met behulp van hulpprogramma's voor API's en beheren met beheer van meerdere tenants

Voor de ondersteunde services en de hierboven vermelde scenario, kunt u beheertaken uitvoeren, rechtstreeks in de portal of met behulp van hulpprogramma's voor API's en beheer (zoals Azure CLI en Azure PowerShell). Alle bestaande API's kunnen worden gebruikt bij het werken met gedeelde resources (voor services die worden ondersteund).

Er zijn ook API's specifiek voor het uitvoeren van beheertaken voor Azure-gedelegeerde resource. Zie voor meer informatie de **verwijzing** sectie.


## <a name="next-steps"></a>Volgende stappen

- Onboarding uw klanten naar Azure Resourcemanagement, door een gedelegeerd [met behulp van Azure Resource Manager-sjablonen](../how-to/onboard-customer.md) of door [een particuliere of openbare beheerde services aanbieding publiceren op Azure Marketplace](../how-to/publish-managed-services-offers.md).
- [Weergeven en beheren van klanten](../how-to/view-manage-customers.md) door te gaan naar **mijn klanten** in Azure portal.