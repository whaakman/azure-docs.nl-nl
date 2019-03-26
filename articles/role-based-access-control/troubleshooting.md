---
title: RBAC voor Azure-resources oplossen | Microsoft Docs
description: Problemen oplossen met op rollen gebaseerd toegangsbeheer (RBAC) voor Azure-resources.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/24/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: d85c49cc8533b88382de81f8f12fde7116afb69a
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407586"
---
# <a name="troubleshoot-rbac-for-azure-resources"></a>RBAC voor Azure-resources oplossen

In dit artikel vindt u antwoorden op veelgestelde vragen over op rollen gebaseerd toegangsbeheer (RBAC) voor Azure-resources, zodat u wat weet u kunt verwachten wanneer u de rollen in de Azure-portal en kan problemen met toegang.

## <a name="problems-with-rbac-role-assignments"></a>Problemen met RBAC-roltoewijzingen

- Als u zich niet op toe te voegen een roltoewijzing in de Azure-portal **toegangsbeheer (IAM)** omdat de **toevoegen** > **roltoewijzing toevoegen** optie is uitgeschakeld of omdat u krijgt de Machtigingsfout 'de client met object-id is niet gemachtigd om uit te voeren actie', controleert u of u momenteel bent aangemeld bij een gebruiker die een rol heeft die is toegewezen de `Microsoft.Authorization/roleAssignments/write` machtiging zoals [eigenaar](built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) bij het bereik dat u wilt de rol toe te wijzen.
- Als u het foutbericht ' kunnen geen roltoewijzingen meer worden gemaakt (code: RoleAssignmentLimitExceeded) ' wanneer u probeert een rol toewijzen, probeert te verminderen van het aantal roltoewijzingen door rollen toewijzen aan groepen in plaats daarvan. Azure biedt ondersteuning voor maximaal **2000** roltoewijzingen per abonnement.

## <a name="problems-with-custom-roles"></a>Problemen met aangepaste rollen

- Als u stappen voor het maken van een aangepaste rol nodig hebt, raadpleegt u de aangepaste rol zelfstudies met behulp van [Azure PowerShell](tutorial-custom-role-powershell.md) of [Azure CLI](tutorial-custom-role-cli.md).
- Als u zich niet aan een bestaande aangepaste rol bijwerken, controleert u of u momenteel bent aangemeld bij een gebruiker die een rol heeft die is toegewezen de `Microsoft.Authorization/roleDefinition/write` machtiging zoals [eigenaar](built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator).
- Als u niet wilt verwijderen van een aangepaste rol en het foutbericht "Er zijn bestaande roltoewijzingen die verwijzen naar de rol (code: RoleDefinitionHasAssignments) ', worden er nog steeds met behulp van de aangepaste rol roltoewijzingen. Deze roltoewijzingen verwijderen en probeer het verwijderen van de aangepaste rol die het opnieuw.
- Als u het foutbericht 'limiet voor roldefinities is overschreden. Geen roldefinities meer kunnen worden gemaakt (code: RoleDefinitionLimitExceeded) ' wanneer u probeert te maken van een nieuwe aangepaste rol, verwijdert u eventuele aangepaste rollen die niet worden gebruikt. Azure biedt ondersteuning voor maximaal **2000** aangepaste rollen in een tenant.
- Als u een fout optreedt die vergelijkbaar is met het "de client is gemachtigd om uit te voeren actie 'Microsoft.Authorization/roleDefinitions/write' over scope '/ subscriptions / {subscriptionid}', maar het gekoppelde abonnement is niet gevonden" wanneer u probeert een aangepaste rol bijwerken, controleren of een of meer [toewijsbare bereiken](role-definitions.md#assignablescopes) in de tenant is verwijderd. Als het bereik is verwijderd, maakt u een ondersteuningsticket als er geen oplossing selfservice beschikbaar is op dit moment is.

## <a name="recover-rbac-when-subscriptions-are-moved-across-tenants"></a>RBAC herstellen als abonnementen tussen tenants zijn verplaatst

- Als u de stappen voor het overdragen van een abonnement moet naar een andere Azure AD-tenant, Zie [eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md).
- Als u een abonnement overdraagt naar een andere Azure AD-tenant, worden alle roltoewijzingen permanent worden verwijderd uit de bron Azure AD-tenant en worden niet gemigreerd naar de doel-Azure AD-tenant. U moet de roltoewijzingen in de doel-tenant opnieuw maken.
- Als u een Azure AD-hoofdbeheerder en u geen toegang hebt tot een abonnement nadat deze is verplaatst tussen tenants, gebruikt u de **Access management voor Azure-resources** in-/ uitschakelen tijdelijk [toegangsrechten](elevate-access-global-admin.md) toegang te krijgen tot het abonnement.

## <a name="issues-with-service-admins-or-co-admins"></a>Problemen met servicebeheerders of co-beheerders

- Als u problemen met de servicebeheerder of CO-beheerders ondervindt, Zie [toevoegen of wijzigen Azure-abonnementbeheerders](../billing/billing-add-change-azure-subscription-administrator.md) en [klassiek abonnement beheerder functies, Azure RBAC-rollen en Azure AD beheerdersrollen](rbac-and-directory-admin-roles.md).

## <a name="access-denied-or-permission-errors"></a>Toegang geweigerd of machtiging fouten

- Als u de machtigingen voor de foutmelding ' de client met object-id is niet gemachtigd om uit te voeren actie bereik (code: AuthorizationFailed) ' wanneer u probeert om een resource te maken, moet u controleren of u momenteel bent aangemeld bij een gebruiker die een rol schrijfrechten heeft op de resource op de geselecteerde scope heeft die is toegewezen. Bijvoorbeeld, voor het beheren van virtuele machines in een resourcegroep, moet u hebt de [Inzender voor virtuele machines](built-in-roles.md#virtual-machine-contributor) -rol op de resourcegroep (of een overkoepelend bereik). Zie voor een lijst van de machtigingen voor elke ingebouwde rol, [ingebouwde rollen voor Azure-resources](built-in-roles.md).
- Als u de Machtigingsfout 'U bent niet gemachtigd om te maken van een ondersteuningsaanvraag' wanneer u probeert te maken of bijwerken van een ondersteuningsticket, controleert u of u momenteel bent aangemeld bij een gebruiker die een rol heeft die is toegewezen de `Microsoft.Support/supportTickets/write` machtiging, zoals [Ondersteunen bijdrager voor ondersteuningsaanvragen](built-in-roles.md#support-request-contributor).

## <a name="rbac-changes-are-not-being-detected"></a>RBAC wijzigingen worden niet wordt gedetecteerd.

Azure Resource Manager slaat soms configuraties en gegevens om prestaties te verbeteren. Bij het maken of verwijderen van roltoewijzingen, kan het wijzigingen van kracht tot 30 minuten duren voordat. Als u de Azure portal, Azure PowerShell of Azure CLI gebruikt, kunt u een vernieuwing van uw wijzigingen aan toewijzingen van rol forceren door afmelden en aanmelden. Als u wijzigingen aan toewijzingen van rol met REST API-aanroepen, kunt u een vernieuwing afdwingen door het vernieuwen van uw toegangstoken.

## <a name="web-app-features-that-require-write-access"></a>Web-appfuncties waarvoor schrijftoegang is vereist

Als u een gebruiker alleen-lezen toegang tot één web-app toewijst, worden sommige functies zijn uitgeschakeld dat u niet had verwacht. De volgende beheermogelijkheden vereisen **schrijven** toegang tot een web-app (Inzender of eigenaar) en niet beschikbaar zijn in elk scenario alleen-lezen.

* Opdrachten (zoals starten, stoppen, enz.)
* Wijzigen van de instellingen zoals de algemene configuratie, instellingen, back-upinstellingen en controle-instellingen
* Toegang tot publicatiereferenties en andere geheimen zoals app-instellingen en verbindingsreeksen
* Streaminglogboeken
* Configuratie van diagnostische logboeken
* Console (opdrachtprompt)
* Actieve en recente implementaties (voor lokale git continue implementatie)
* Geschatte besteding
* Webtests
* Virtueel netwerk (alleen zichtbaar voor een lezer als er eerder een virtueel netwerk is geconfigureerd door een gebruiker met schrijftoegang).

Als u geen toegang een van deze tegels tot, moet u uw beheerder vragen voor Inzender-toegang tot de web-app.

## <a name="web-app-resources-that-require-write-access"></a>Web app-resources waarvoor de toegang voor schrijven

Web-apps worden door de aanwezigheid van een paar verschillende bronnen die wisselwerking tussen diagrammen ingewikkeld. Hier volgt een typische resourcegroep met een paar van websites:

![Resourcegroep voor web-app](./media/troubleshooting/website-resource-model.png)

Als gevolg hiervan, als u iemand toegang tot alleen de web-app veel van de functies op de websiteblade in Azure portal is uitgeschakeld.

Deze items nodig **schrijven** toegang tot de **App Service-plan** die overeenkomt met uw website:  

* Weergeven van de WebApp de prijscategorie (gratis of Standard)  
* Configuratie van de schaal (aantal exemplaren, de grootte van virtuele machine, de instellingen voor automatisch schalen)  
* Quota's (storage, bandbreedte, CPU)  

Deze items nodig **schrijven** toegang tot het geheel **resourcegroep** waarin uw website:  

* SSL-certificaten en -bindingen (SSL-certificaten kunnen worden gedeeld tussen sites in dezelfde resourcegroep bevinden en geo-locatie)  
* Waarschuwingsregels  
* Instellingen voor automatisch schalen  
* Application Insights-onderdelen  
* Webtests  

## <a name="virtual-machine-features-that-require-write-access"></a>Functies van virtuele machines die toegang voor schrijven vereisen

Net als bij de web-apps, vereisen sommige functies op de blade van de virtuele machine toegang voor schrijven naar de virtuele machine of naar andere resources in de resourcegroep.

Virtuele machines zijn gerelateerd aan het domein namen, virtuele netwerken, opslagaccounts en regels voor waarschuwingen.

Deze items nodig **schrijven** toegang tot de **virtuele machine**:

* Eindpunten  
* IP-adressen  
* Disks  
* Extensies  

Hiervoor is **schrijven** toegang tot zowel de **virtuele machine**, en de **resourcegroep** (samen met de naam van het domein) dat zich in:  

* Beschikbaarheidsset  
* Set met gelijke taakverdeling  
* Waarschuwingsregels  

Als u geen toegang een van deze tegels tot, vraagt u uw beheerder voor Inzender-toegang tot de resourcegroep.

## <a name="azure-functions-and-write-access"></a>Azure Functions en toegang voor schrijven

Sommige functies van [Azure Functions](../azure-functions/functions-overview.md) schrijven toegang nodig hebben. Bijvoorbeeld, als een gebruiker kan de rol van lezer is toegewezen, wordt dat niet meer mogelijk om de functies in een functie-app weer te geven. De portal wordt weergegeven **(geen toegang)**.

![Functie-apps geen toegang](./media/troubleshooting/functionapps-noaccess.png)

Een lezer kunt klikken op de **platformfuncties** tabblad en klik vervolgens op **alle instellingen** om bepaalde instellingen weer te geven met betrekking tot een functie-app (vergelijkbaar met een web-app), maar ze kunnen een van deze instellingen niet wijzigen.

## <a name="next-steps"></a>Volgende stappen
* [Toegang tot Azure-resources beheren met op rollen gebaseerd toegangsbeheer en de Azure-portal](role-assignments-portal.md)
* [Activiteitenlogboeken bekijken voor RBAC-wijzigingen in de Azure-resources](change-history-report.md)

