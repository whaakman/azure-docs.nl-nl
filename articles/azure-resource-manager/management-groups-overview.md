---
title: Uw resources te organiseren met Azure-beheergroepen | Microsoft Docs
description: Meer informatie over de beheergroepen en hoe ze te gebruiken.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/28/2018
ms.author: rithorn
ms.openlocfilehash: 611faef7e4b94b1734896fb64ca29540b12bc057
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102343"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Uw resources te organiseren met Azure-beheergroepen

Als uw organisatie veel abonnementen heeft, moet u mogelijk een manier voor het efficiënt beheren van toegang, beleid en naleving voor deze abonnementen. Azure-beheergroepen bieden een scope hierboven abonnementen. U abonnementen in containers 'beheergroepen' aangeroepen organiseren en uw voorwaarden governance toepassen op de beheergroepen. De voorwaarden die aan de beheergroep wordt automatisch overgenomen door alle abonnementen binnen een beheergroep. Beheergroepen krijgt u bedrijfsniveau management op grote schaal ongeacht welk soort abonnementen die u mogelijk hebt.

De functie management group is beschikbaar in een openbare preview. Om te beheergroepen gebruiken, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com) en zoek naar **beheergroepen** in de **alle Services** sectie.

U kunt bijvoorbeeld beleidsregels toepassen op een beheergroep met beperkingen voor de regio's beschikbaar zijn voor het maken van de virtuele machine (VM). Dit beleid zou worden toegepast op alle beheergroepen, abonnementen en resources onder beheergroep door alleen virtuele machines in deze regio worden gemaakt.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hiërarchie van beheergroepen en abonnementen

U kunt een flexibele structuur van beheergroepen en abonnementen uw om resources te organiseren in een hiërarchie van unified beleid en beheer van toegang op te bouwen.
Het volgende diagram toont een voorbeeld van de hiërarchie die uit de beheergroepen en -abonnementen die zijn geordend op afdelingen bestaat.

![Structuur](media/management-groups/MG_overview.png)

Als u een hiërarchie die worden gegroepeerd per afdelingen maakt, kunt u toewijzen [gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md) rollen die *overnemen* aan de diensten onder de beheergroep. Met behulp van beheergroepen inkorten uw werkbelasting en vermindert het risico van fouten door slechts één keer de rol toewijzen.

### <a name="important-facts-about-management-groups"></a>Belangrijke informatie over beheergroepen

- 10.000 beheergroepen kunnen worden ondersteund in een enkele map.
- Een boomstructuur van de groep management kan maximaal zes niveaus van diepte ondersteunen.
  - Deze limiet bevatten niet het hoogste niveau of het abonnementsniveau.
- Elke beheergroep en het abonnement kan alleen ondersteuning voor één bovenliggend.
- Elke beheergroep kan meerdere onderliggende elementen hebben.
- Alle abonnementen en beheergroepen bevinden zich in een enkele hiërarchie in elke map. Zie [belangrijke feiten over de Root management-groep](#important-facts-about-the-root-management-group) voor uitzonderingen tijdens de Preview.

### <a name="preview-subscription-visibility-limitation"></a>Preview abonnement zichtbaarheid beperking

Er is een beperking in de Preview-versie waarin u kunt abonnementen die u hebt toegang tot overgenomen weergeven niet. De toegang tot het abonnement wordt overgenomen, maar de Azure Resource Manager niet kan voldoen nog aan de overname-toegang.  

Retourneert de gegevens met de REST API voor informatie over het abonnement als u toegang hebt, maar binnen de Azure-portal en Azure Powershell de abonnementen niet weergeven.

Dit item wordt gewerkt en worden opgelost voordat er management groepen worden vermeld als 'Algemene beschikbaarheid'.  

### <a name="cloud-solution-provider-csp-limitation-during-preview"></a>Cloud Solution Provider (CSP) beperking tijdens de Preview

Er is een beperking voor Cloud Solution Provider (CSP)-Partners waar ze kunnen maken of beheren van de beheergroepen zijn klant in de directory van de klant niet.  
Dit item wordt gewerkt en worden opgelost voordat er management groepen worden vermeld als 'Algemene beschikbaarheid'.

## <a name="root-management-group-for-each-directory"></a>De beheergroep hoofdmap voor elke map

Elke directory is een enkel op het hoogste niveau beheergroep met de naam van de beheergroep 'Root' opgegeven. Deze hoofdmap beheergroep is ingebouwd in de hiërarchie hebben alle beheergroepen en abonnementen vouw aan. Deze hoofdmap beheergroep kan globaal beleid en RBAC-toewijzingen moeten worden toegepast op het niveau van de directory. De [Directory-beheerder moet zelf de bevoegdheden](../role-based-access-control/elevate-access-global-admin.md) in eerste instantie moet de eigenaar van deze groep hoofdmap. Nadat de beheerder de eigenaar van de groep is, kunnen ze RBAC-rol toewijzen aan andere directory-gebruikers of groepen voor het beheren van de hiërarchie.  

### <a name="important-facts-about-the-root-management-group"></a>Belangrijke informatie over de Root management-groep

- De naam en de ID van de root management-groep zijn standaard toegekend. De weergavenaam op die kan worden bijgewerkt op elk gewenst moment inschakelen andere binnen de Azure-portal.
  - De naam is 'Hoofdgroep Tenant'.
  - De ID is de Azure Active Directory-ID.
- De root management-groep kan niet worden verplaatst of verwijderd, in tegenstelling tot andere beheergroepen.  
- Alle abonnementen en beheergroepen Vouw met de beheergroep met één hoofdmap binnen de map.
  - Alle resources in de map worden gesloten met de beheergroep hoofdmap voor algemeen beheer.
  - Nieuwe abonnementen worden standaard automatisch behandeld als de beheergroep hoofdmap wanneer gemaakt.
- Alle Azure-klanten de root management-groep kunnen zien, maar niet alle klanten hebben toegang tot het beheer van basis-beheergroep.
  - Iedereen die toegang tot een abonnement heeft ziet de context van het waar dat abonnement bevindt zich in de hiërarchie.  
  - Niemand standaardtoegang krijgt tot de root management-groep. Globale directorybeheerders zijn de alleen gebruikers die kunnen worden de bevoegdheden zelf om toegang te krijgen.  Nadat ze toegang hebben, kunnen de directory-beheerders RBAC-rol toewijzen aan andere gebruikers om te beheren.  

>[!NOTE]
>Als uw directory met de beheerservice groepen voordat 25/6/2018 gestart, kan uw directory niet worden ingesteld met alle abonnementen in de hiërarchie. Het managementteam van de groep wordt elke map die de slag met beheergroepen in de openbare Preview voordat deze datum in juli 2018 met terugwerkende kracht bijgewerkt. Alle abonnementen in de mappen komen onderliggende items onder de hoofdmap beheergroep voorafgaande.  
>
>Als u vragen over dit proces met terugwerkende kracht hebt, contact op met: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Eerste installatie van beheergroepen

Wanneer een gebruiker wordt gestart met behulp van de beheergroepen, wordt er een eerste installatieproces dat plaatsvindt. De eerste stap is dat de root management-groep is gemaakt in de map. Zodra deze groep is gemaakt, worden alle bestaande abonnementen die zijn opgenomen in de map onderliggende elementen van de root management-groep gemaakt.  De reden voor dit proces is om te controleren of er is slechts één management-groep is een hiërarchie in een map.  De enkele hiërarchie in de directory kan administratieve klanten globale toegang en beleidsregels die van andere klanten in de map niet overslaan toe te passen. Alles toegewezen in de hoofdmap toegepast op alle beheergroepen, abonnementen resourcegroepen en resources in de map wanneer er een hiërarchie in de map.  

> [!IMPORTANT]
> Een toewijzing van de gebruiker toegang of beleid toewijzing van de root management-groep **geldt voor alle resources in de map**. Als gevolg hiervan moeten alle klanten nadenken over hoeft te zijn van de items die zijn gedefinieerd in dit bereik.  Toegang en beleid gebruikerstoewijzingen moet 'Moet' alleen aan dit bereik.  
  
## <a name="management-group-access"></a>Groepstoegang Management

Azure-ondersteuning beheergroepen [gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md) voor alle toegang tot een resource en roldefinities. Deze machtigingen worden overgenomen voor de onderliggende resources die zijn opgenomen in de hiërarchie. Een ingebouwde RBAC-rol kan worden toegewezen aan een beheergroep die in de hiërarchie tot de bronnen worden overgenomen.  De RBAC-rol VM medewerker kan bijvoorbeeld worden toegewezen aan een beheergroep. Deze rol heeft geen actie op de beheergroep, maar worden overgenomen door alle VM's onder de beheergroep.  

Het volgende diagram ziet u de lijst met functies en de ondersteunde bewerkingen op beheergroepen.

| De rolnaam RBAC             | Maken | Naam wijzigen | Verplaatsen | Verwijderen | Toegang toewijzen | Beleid toewijzen | Lezen  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Eigenaar                       | X      | X      | X    | X      | X             |               | X     |
|Inzender                 | X      | X      | X    | X      |               |               | X     |
|Lezer                      |        |        |      |        |               |               | X     |
|Inzender voor resourcebeleid |        |        |      |        |               | X             |       |
|Beheerder van gebruikerstoegang   |        |        |      |        | X             |               |       |

### <a name="custom-rbac-role-definition-and-assignment"></a>Aangepaste RBAC roldefinitie en toewijzing

Aangepaste RBAC-rollen worden niet ondersteund op beheergroepen op dit moment.  Zie de [management groep Feedbackforum](https://aka.ms/mgfeedback) om de status van dit object weer te geven.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over beheergroepen:

- [Beheergroepen te organiseren Azure-resources maken](management-groups-create.md)
- [Wijzigen, verwijderen of beheren van uw beheergroepen](management-groups-manage.md)
- [Installeer de Azure PowerShell-module](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Bekijk de REST-API-specificaties](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [De uitbreiding voor de Azure CLI installeren](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
