---
title: Resources organiseren met beheergroepen Azure | Microsoft Docs
description: Meer informatie over de beheergroepen en het gebruik ervan.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/31/2018
ms.author: rithorn
ms.openlocfilehash: edc57d146ccb034ac3fd627386000a1953b0e558
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480319"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Resources organiseren met beheergroepen voor Azure

Als uw organisatie veel abonnementen heeft, moet u de toegang, beleidsregels en naleving voor deze abonnementen op een efficiënte manier kunnen beheren. Een niveau van bereik bovenstaande abonnementen bieden dat Azure-beheergroepen. U abonnementen in containers met de naam "beheergroepen" ordenen en de voorwaarden van uw beheeracties toepassen op de beheergroepen. Alle abonnementen in een beheergroep overnemen automatisch de voorwaarden die aan de beheergroep. Van beheergroepen bieden u zakelijke management op grote schaal, ongeacht welk type abonnementen die u mogelijk hebt.

U kunt bijvoorbeeld beleid toepassen op een beheergroep die de regio's beschikbaar voor het maken van virtuele machine (VM beperkt). Dit beleid zou worden toegepast op alle beheergroepen, abonnementen en resources van die groep management door toe te staan alleen virtuele machines in deze regio worden gemaakt.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hiërarchie van beheergroepen en -abonnementen

U kunt een flexibele structuur van beheergroepen en abonnementen voor het organiseren van uw resources in een hiërarchie voor geïntegreerde policy and access management kunt bouwen.
Het volgende diagram toont een voorbeeld van het maken van een hiërarchie voor beheer met behulp van beheergroepen.

![structuur](media/management-groups/MG_overview.png)

Door het maken van een hiërarchie zoals in dit voorbeeld kunt u een beleid, bijvoorbeeld VM locaties beperkt tot regio VS-West op de groep 'Infrastructure-Team beheergroep' toepassen op de interne naleving inschakelen en beveiligingsbeleid. Dit beleid worden overgenomen door naar beide EA-abonnementen onder deze beheergroep en geldt voor alle virtuele machines onder deze abonnementen. Als dit beleid van de beheergroep naar de abonnementen overneemt, kan dit beveiligingsbeleid kan niet worden gewijzigd door de eigenaar van de resource of abonnement waardoor het verbeterde governance.

Een ander scenario waarin u beheergroepen wilt gebruiken is het bieden van toegang tot meerdere abonnementen voor gebruikers.  Als u meerdere abonnementen voor die beheergroep verplaatst, moet u de mogelijkheid een RBAC-toewijzing maken voor de beheergroep, die worden overgenomen door waarmee de toegang tot alle abonnementen hebben.  Zonder de noodzaak tot script RBAC-toewijzingen voor meerdere abonnementen kunt één toewijzing van de beheergroep die gebruikers toegang hebben tot alle benodigde inschakelen.

### <a name="important-facts-about-management-groups"></a>Belangrijke informatie over de beheergroepen

- 10.000 beheergroepen kunnen in één map worden ondersteund.
- Een boomstructuur van de groep management kan maximaal zes niveaus van diepte ondersteunen.
  - Deze limiet bevatten niet het hoogste niveau doen of het abonnementsniveau.
- Elke beheergroep en het abonnement kan alleen ondersteuning voor één bovenliggend object.
- Elke beheergroep kan meerdere onderliggende items hebben.
- Alle abonnementen en beheergroepen bevinden zich in een enkele hiërarchie in elke map. Zie [belangrijke informatie over de Root management-groep](#important-facts-about-the-root-management-group) voor uitzonderingen tijdens de Preview-versie.

## <a name="root-management-group-for-each-directory"></a>Root management-groep voor elke map

Elke directory is een enkel op het hoogste niveau beheergroep met de naam van de beheergroep "Root" gegeven. Deze hoofdbeheergroep is zo in de hiërarchie ingebouwd dat alle beheergroepen en abonnementen hierin zijn opgevouwen. Deze hoofdmap beheergroep kan globaal beleid en RBAC-toewijzingen moet worden toegepast op het niveau van de map. De [Directory-beheerder moet verhogen zelf](../role-based-access-control/elevate-access-global-admin.md) in eerste instantie moet de eigenaar van deze groep hoofdmap. Zodra de beheerder de eigenaar van de groep is, kunnen ze RBAC-rol toewijzen aan andere directory: gebruikers of groepen voor het beheren van de hiërarchie.  

### <a name="important-facts-about-the-root-management-group"></a>Belangrijke informatie over de Root management-groep

- De naam en ID van de root management-groep krijgen standaard. De weergavenaam kan worden bijgewerkt op elk gewenst moment om verschillende in Azure portal weer te geven.
  - De naam is 'Tenant hoofdgroep'.
  - De ID is de Azure Active Directory-ID.
- De root management-groep kan niet worden verplaatst of verwijderd, in tegenstelling tot andere beheergroepen.  
- Alle abonnementen en beheergroepen fold om aan de beheergroep met één hoofdmap in de map.
  - Alle resources in de directory Vouw tot de root management-groep voor het beheer van globale.
  - Nieuwe abonnementen worden standaard automatisch ingesteld op de root management-groep wanneer gemaakt.
- Alle Azure-klanten kunnen de root management-groep zien, maar niet alle klanten hebben toegang tot het beheer van die root management-groep.
  - Iedereen die toegang tot een abonnement heeft kunt de context van het waar dat aan het abonnement in de hiërarchie is zien.  
  - Er wordt standaardtoegang krijgen tot de root management-groep. Hoofdbeheerders van Active Directory zijn de enige gebruikers die u kunnen zelf om toegang te krijgen met verhoogde bevoegdheden.  Nadat ze toegang hebben, kunnen de directory-beheerders RBAC-rol toewijzen aan andere gebruikers te beheren.  

>[!NOTE]
>Als uw directory wordt gestart met behulp van de beheerservice voor groepen voordat 25-6-2018, kan uw directory niet worden ingesteld met alle abonnementen in de hiërarchie. Het managementteam van de groep wordt met terugwerkende kracht bijgewerkt voor elke map die aan de slag met beheergroepen in de openbare preview-versie voor deze datum in juli/augustus 2018. Alle abonnementen in de mappen gemaakt op onderliggende items onder de root management-groep voorafgaande.  
>
>Als u vragen over dit proces met terugwerkende kracht hebt, contact op met: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Eerste installatie van beheergroepen

Wanneer een gebruiker wordt gestart met behulp van van beheergroepen, is er een eerste configuratie-proces dat plaatsvindt. De eerste stap is dat de root management-groep is gemaakt in de map. Zodra deze groep is gemaakt, worden alle bestaande abonnementen die zijn opgenomen in de map onderliggende items van de root management-groep gemaakt.  De reden voor dit proces is om te controleren of er is slechts één management-groep is een hiërarchie in een map.  De één hiërarchie in de map kan administratieve klanten wereldwijde toegang en beleidsregels die van andere klanten in de map niet overslaan toe te passen. In de hoofdmap van de toegewezen geldt in alle beheergroepen, abonnementen, resourcegroepen en resources in de map door een hiërarchie in de map.  

> [!IMPORTANT]
> Een toewijzing van toegang of beleid toewijzen van gebruikers op de root management-groep **is van toepassing op alle resources in de map**. Alle klanten dienen als gevolg hiervan hoeft te zijn van de items die zijn gedefinieerd in dit bereik te evalueren.  Toegang en beleid-toewijzingen van gebruikers moet 'Moet' alleen op dit bereik.  
  
## <a name="management-group-access"></a>Toegang tot de beheergroep

Azure-ondersteuning voor beheergroepen [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md) voor alle toegang tot een resource en roldefinities. Deze machtigingen worden overgenomen op onderliggende resources die zijn opgenomen in de hiërarchie. Een ingebouwde RBAC-rol kan worden toegewezen aan een beheergroep die worden overgenomen door omlaag in de hiërarchie tot de resources.  De VM-Inzender van RBAC-rol kan bijvoorbeeld worden toegewezen aan een beheergroep. Deze rol heeft geen actie van de beheergroep, maar geldt voor alle VM's in de beheergroep.  

Het volgende diagram ziet u de lijst met functies en de ondersteunde bewerkingen op beheergroepen.

| De naam van de RBAC-rol             | Maken | Naam wijzigen | Verplaatsen | Verwijderen | Toegang toewijzen | Beleid toewijzen | Lezen  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Eigenaar                       | X      | X      | X    | X      | X             | X             | X     |
|Inzender                 | X      | X      | X    | X      |               |               | X     |
|MG Inzender *             | X      | X      | X    | X      |               |               | X     |
|Lezer                      |        |        |      |        |               |               | X     |
|MG-lezer *                  |        |        |      |        |               |               | X     |
|Inzender voor resourcebeleid |        |        |      |        |               | X             |       |
|Beheerder van gebruikerstoegang   |        |        |      |        | X             |               |       |

*: MG Inzender en lezer MG alleen toestaan dat gebruikers deze acties uitvoeren op het beheerbereik van de groep.  

### <a name="custom-rbac-role-definition-and-assignment"></a>De definitie van de aangepaste RBAC-rol en de toewijzing

Aangepaste RBAC-rollen worden niet ondersteund op beheergroepen op dit moment.  Zie de [forum met feedback van management groep](https://aka.ms/mgfeedback) om de status van dit item weer te geven.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de beheergroepen:

- [Beheergroepen voor het ordenen van Azure-resources maken](management-groups-create.md)
- [Wijzigen, verwijderen of uw beheergroepen beheren](management-groups-manage.md)
- [De Azure PowerShell-module installeren](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Controleer de REST API-specificatie](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [De Azure CLI-extensie installeren](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)
