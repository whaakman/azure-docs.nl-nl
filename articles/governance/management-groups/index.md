---
title: Uw resources organiseren met Azure-beheergroepen
description: Informatie over de managementgroepen en het gebruik daarvan.
author: rthorn17
manager: rithorn
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/18/2018
ms.author: rithorn
ms.openlocfilehash: d031059f9811cedb703fec4920e00fd1b2e3f877
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47045342"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Uw resources organiseren met Azure-beheergroepen

Als uw organisatie veel abonnementen heeft, moet u de toegang, beleidsregels en naleving voor deze abonnementen op een efficiënte manier kunnen beheren. Azure-beheergroepen bieden een scopeniveau boven abonnementen. U ordent abonnementen in containers, zogenaamde 'beheergroepen', en past uw governancevoorwaarden hierop toe. Alle abonnementen in een beheergroep nemen automatisch de voorwaarden over die op de beheergroep zijn toegepast. Beheergroepen bieden u beheer van bedrijfskwaliteit op grote schaal, ongeacht de typen abonnementen die u hebt.

U kunt bijvoorbeeld beleid toepassen op een beheergroep, dat het aantal regio's beperkt dat beschikbaar is voor het maken van virtuele machines (VM’s). Dit beleid wordt dan toegepast op alle beheergroepen, abonnementen en resources onder die beheergroep, doordat het ervoor zorgt dat er alleen VM’s kunnen worden gemaakt in die regio.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hiërarchie van managementgroepen en abonnementen

U kunt een flexibele structuur van managementgroepen en abonnementen bouwen om uw resources in een hiërarchie te ordenen voor uniform beleid en toegangsbeheer. Het volgende diagram laat een voorbeeld zien van hoe een hiërarchie voor governance kan worden gemaakt met behulp van beheergroepen.

![boomstructuur](./media/MG_overview.png)

Als u een hiërarchie maakt zoals die in dit voorbeeld, kunt u een beleid toepassen, bijvoorbeeld VM-locaties in alleen de regio US - west in de groep ‘Beheergroep infrastructuurteam’ om intern beleid voor compliance en beveiliging in te schakelen. Dit beleid wordt overgenomen door beide EA-abonnementen onder deze beheergroep en geldt voor alle VM’s onder deze abonnementen. Omdat dit beleid van de beheergroep wordt doorgegeven aan de abonnementen, kan het beveiligingsbeleid niet worden gewijzigd door de eigenaar van de resource of het abonnement, wat zorgt voor betere governance.

Een ander scenario waarbij u beheergroepen kunt gebruiken, is om gebruikers toegang te geven tot meerdere abonnementen. Als u meerdere abonnementen onder die beheergroep verplaatst, hebt u de mogelijkheid om in de beheergroep één toewijzing voor [op rollen gebaseerd toegangsbeheer](../../role-based-access-control/overview.md) (role-based access control, RBAC) te maken, die de toegang doorgeeft aan alle abonnementen.
Eén toewijzing in de beheergroep kan gebruikers toegang geven tot alles wat ze nodig hebben, zonder dat er scripts moeten worden geschreven voor RBAC-toewijzingen in meerdere abonnementen.

### <a name="important-facts-about-management-groups"></a>Belangrijke feiten over beheergroepen

- In één map kunnen 10.000 beheergroepen worden ondersteund.
- Een beheergroepstructuur ondersteunt tot wel zes niveaus.
  - Deze limiet is exclusief het hoofdniveau of het abonnementsniveau.
- Een beheergroep of abonnement biedt ondersteuning voor slechts één bovenliggend item.
- Elke beheergroep kan meerdere onderliggende items hebben.
- Alle abonnementen en beheergroepen bevinden zich in één hiërarchie in elke map. Zie [Belangrijke feiten over de hoofdbeheergroep](#important-facts-about-the-root-management-group) voor uitzonderingen die optreden tijdens de preview-periode.

## <a name="root-management-group-for-each-directory"></a>Hoofdbeheergroep voor elke map

Elke map krijgt één beheergroep op het hoogste niveau, de 'hoofdbeheergroep'.
Deze hoofdbeheergroep is zo in de hiërarchie ingebouwd dat alle beheergroepen en abonnementen hierin zijn samengevouwen. Met deze hoofdbeheergroep kunt u algemene beleidsregels en RBAC-toewijzingen toepassen op mapniveau. De [Active Directory-beheerder moet zichzelf eerst instellen](../../role-based-access-control/elevate-access-global-admin.md) als eigenaar van deze hoofdgroep. Zodra de beheerder de eigenaar van de groep is, kan hij alle RBAC-rollen toewijzen aan andere Active Directory-gebruikers of -groepen om de hiërarchie te beheren.

### <a name="important-facts-about-the-root-management-group"></a>Belangrijke feiten over de hoofdbeheergroep

- De naam en id van de hoofdbeheergroep worden standaard gegenereerd. De weergavenaam kan te allen tijde worden bijgewerkt, zodat die anders wordt weergegeven in de Azure-portal.
  - De naam wordt ‘Hoofdgroep tenant’.
  - De ID is de Azure Active Directory-id.
- In tegenstelling tot andere beheergroepen kan de hoofdbeheergroep niet worden verplaatst of verwijderd.  
- Alle abonnementen en beheergroepen kunnen worden samengevouwen in deze ene hoofdbeheergroep binnen de map.
  - Alle resources in de map kunnen worden samengevouwen in de hoofdbeheergroep voor algemeen beheer.
  - Wanneer er een nieuw abonnement wordt gemaakt, wordt dit standaard automatisch in de hoofdbeheergroep geplaatst.
- Alle Azure-klanten kunnen de hoofdbeheergroep zien, maar niet alle klanten hebben de mogelijkheid om die hoofdbeheergroep te beheren.
  - Iedereen die toegang heeft tot een abonnement, kan de context zien van waar dat abonnement zich in de hiërarchie bevindt.  
  - Niemand krijgt standaard toegang tot de hoofdbeheergroep. Globale beheerders van Active Directory zijn de enige gebruikers die zichzelf toegang kunnen verschaffen.  Wanneer ze toegang hebben, kunnen beheerders van Active Directory elke RBAC-rol toewijzen aan andere gebruikers, zodat deze het beheer daarover hebben.  

> [!NOTE]
> Als uw map de beheergroepservice voor het eerst gebruikte vóór 25-06-2018, zijn mogelijk niet alle abonnementen ingesteld in de hiërarchie voor uw map. Het managementgroep-team werkt in juli/augustus 2018 met terugwerkende kracht alle mappen bij die voor deze datum begonnen met het gebruik van beheergroepen in de Openbare preview. Van alle abonnementen in de mappen worden onderliggende items gemaakt onder de hoofdbeheergroep.
>
> Als u vragen hebt over de terugwerkende kracht waarmee dit proces werkt, neemt u contact op met: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Eerste installatie van beheergroepen

Wanneer een gebruiker start met het gebruik van beheergroepen, vindt er een proces plaats voor de eerste installatie. Eerst wordt er een hoofdbeheergroep gemaakt in de map. Zodra deze groep klaar is, worden alle bestaande abonnementen die in de map zijn opgenomen gemaakt tot onderliggende items van de hoofdbeheergroep. De reden dat dit proces zo werkt, is om er zeker van te zijn dat er binnen een map slechts één beheergroephiërarchie is. Die ene hiërarchie in de map zorgt ervoor dat klanten met een beheerdersrol globale toegang en beleidsregels kunnen toepassen waar andere klanten in de map niet omheen kunnen. Doordat er één hiërarchie is in de map, zijn alle zaken die in de hoofdmap zijn toegewezen, van toepassing op alle beheergroepen, abonnementen, resourcegroepen en resources in de map.

> [!IMPORTANT]
> Toewijzingen voor gebruikerstoegang of beleidstoewijzingen in de hoofdbeheergroep **zijn van toepassing op alle resources in de map**.
> Daarom moeten alle klanten goed nadenken over de noodzaak om items op dit niveau toe te wijzen.
> Op dit niveau mogen alleen toewijzingen voor gebruikerstoegang of beleid de aanduiding ‘Must have’ (Strikt noodzakelijk) hebben.  
  
## <a name="management-group-access"></a>Toegang tot beheergroepen

Azure-beheergroepen ondersteunen [op rollen gebaseerd toegangsbeheer (RBAC) van Azure](../../role-based-access-control/overview.md) voor alle soorten toegang tot resources en roldefinities.
Deze machtigingen worden overgenomen door onderliggende resources die in de hiërarchie zijn opgenomen. Iedere ingebouwde RBAC-rol kan worden toegewezen aan een beheergroep die door alle onderliggende items in de hiërarchie tot de resources worden overgenomen.
Zo kan de RBAC-rol van VM-inzender aan een beheergroep worden toegewezen. Deze rol heeft geen actie in de beheergroep, maar wordt overgenomen door alle VM's in die beheergroep.

In de volgende tabel staat een lijst met rollen en de acties die worden ondersteund in beheergroepen.

| Naam RBAC-rol             | Maken | Naam wijzigen | Verplaatsen | Verwijderen | Toegang toewijzen | Beleid toewijzen | Lezen  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Eigenaar                       | X      | X      | X    | X      | X             | X             | X     |
|Inzender                 | X      | X      | X    | X      |               |               | X     |
|Beheergroep-inzender*             | X      | X      | X    | X      |               |               | X     |
|Lezer                      |        |        |      |        |               |               | X     |
|Beheergroep-lezer*                  |        |        |      |        |               |               | X     |
|Inzender voor resourcebeleid |        |        |      |        |               | X             |       |
|Beheerder van gebruikerstoegang   |        |        |      |        | X             |               |       |

*: Met de rollen Beheergroep-inzender en Beheergroep-lezer kunnen gebruikers alleen die acties uitvoeren op beheergroep-niveau.  

### <a name="custom-rbac-role-definition-and-assignment"></a>Definitie en toewijzing van aangepaste RBAC-rollen

Op dit moment worden aangepaste RBAC-rollen niet ondersteund in beheergroepen. Ga naar het [feedbackforum voor beheergroepen](https://aka.ms/mgfeedback) om de status van dit item te bekijken.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over beheergroepen gaat u naar:

- [Beheergroepen maken om Azure-resources te ordenen](create.md)
- [Uw beheergroepen wijzigen, verwijderen of beheren](manage.md)
- [De Azure PowerShell-module installeren](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [De REST API-specificaties bekijken](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [De Azure CLI-extensie installeren](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)