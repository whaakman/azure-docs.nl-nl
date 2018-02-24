---
title: Uw Azure-beheergroepen-resources te organiseren | Microsoft Docs
description: Meer informatie over de beheergroepen en hoe ze te gebruiken.
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/22/2018
ms.author: rithorn
ms.openlocfilehash: bc7d98851e8d84d7db8586138608220c838ca776
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Breng uw resources met Azure-beheergroepen 

Als u meerdere abonnementen hebt, kunt u deze indelen in containers, genaamd 'beheergroepen' voor het beheren van toegang, beleid en naleving voor uw abonnementen. Deze containers, krijgt u bedrijfsniveau management op grote schaal ongeacht welk soort abonnementen die u mogelijk hebt.  

De functie management group is beschikbaar in een openbare preview. Aan de slag met beheer van groepen, meld u aan bij de [Azure-portal](https://portal.azure.com) en zoek naar **beheergroepen** in de **alle Services** sectie. 

Beleid voor de ondersteuning van Azure voor beheergroepen is niet beschikbaar is nog in de openbare Preview en afkomstig is in de volgende weken.  

Als u bijvoorbeeld kunt u beleidsregels toepassen op een beheergroep met beperkingen voor de regio's beschikbaar zijn voor het maken van de virtuele machine (VM). Dit beleid zou worden toegepast op alle beheergroepen, abonnementen en resources onder beheergroep door alleen virtuele machines in deze regio worden gemaakt.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hiërarchie van beheergroepen en abonnementen 

U kunt een flexibele structuur van beheergroepen en abonnementen uw om resources te organiseren in een hiërarchie van unified beleid en beheer van toegang op te bouwen. Het volgende diagram toont een voorbeeld van de hiërarchie die uit de beheergroepen en -abonnementen die zijn geordend op afdelingen bestaat.    

![hiërarchie](media/management-groups/MG_overview.png)

Als u een hiërarchie die worden gegroepeerd per afdelingen maakt, u zich kunt toewijzen [gebaseerd toegangsbeheer (RBAC)](../active-directory/role-based-access-control-what-is.md) rollen die *overnemen* aan de diensten onder de beheergroep. Met behulp van beheergroepen inkorten uw werkbelasting en vermindert het risico van fouten door slechts één keer de rol toewijzen. 

### <a name="important-facts-about-management-groups"></a>Belangrijke informatie over beheergroepen
- 10.000 beheergroepen kunnen worden ondersteund in een enkele map. 
- Een boomstructuur van de groep management kan maximaal zes niveaus van diepte ondersteunen.
    - Deze limiet bevat niet het hoogste niveau of het abonnementsniveau.
- Elke beheergroep kan slechts één bovenliggend ondersteunen.
- Elke beheergroep kan meerdere onderliggende elementen hebben. 

## <a name="root-management-group-for-each-directory"></a>De beheergroep hoofdmap voor elke map

Elke directory is een enkel op het hoogste niveau beheergroep met de naam van de beheergroep 'Root' opgegeven. Deze hoofdmap beheergroep is ingebouwd in de hiërarchie hebben alle beheergroepen en abonnementen vouw aan. Deze hoofdmap beheergroep kan globaal beleid en RBAC-toewijzingen moeten worden toegepast op het niveau van de directory. De [Directory-beheerder moet zelf de bevoegdheden](../active-directory/role-based-access-control-tenant-admin-access.md) in eerste instantie moet de eigenaar van deze groep hoofdmap. Nadat de beheerder de eigenaar van de groep is, kunnen ze RBAC-rol toewijzen aan andere directory-gebruikers of groepen voor het beheren van de hiërarchie.  

### <a name="important-facts-about-the-root-management-group"></a>Belangrijke informatie over de Root management-groep
- De naam en de ID van de root management-groep, krijgen de ID van Azure Active Directory standaard. De weergavenaam op die kan worden bijgewerkt op elk gewenst moment inschakelen andere binnen de Azure-portal. 
- Alle abonnementen en beheergroepen kunnen Vouw met de beheergroep met één hoofdmap binnen de map.  
    - Het verdient aanbeveling om alle items in de directory Vouw tot aan de beheergroep hoofdmap voor algemeen beheer.  
    - Tijdens de openbare Preview worden alle abonnementen in de directory automatisch niets onderliggende elementen van de hoofdmap.   
    - Tijdens de openbare Preview, worden nieuwe abonnementen niet automatisch gebruikt met de beheergroep hoofdmap. 
- De root management-groep kan niet worden verplaatst of verwijderd, in tegenstelling tot andere beheergroepen. 
  
## <a name="management-group-access"></a>Toegang van de beheergroep

Biedt ondersteuning voor Azure beheergroepen [gebaseerd toegangsbeheer (RBAC)](../active-directory/role-based-access-control-what-is.md) voor alle toegang tot een resource en roldefinities. Deze machtigingen worden overgenomen voor de onderliggende resources die zijn opgenomen in de hiërarchie.   

Tijdens een [ingebouwde RBAC-rol](../active-directory/role-based-access-control-what-is.md#built-in-roles) kunnen worden toegewezen aan een beheergroep zijn vier rollen die vaak worden gebruikt: 
- **Eigenaar** heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren. 
- **Inzender** kunt maken en beheren van alle soorten Azure-resources, maar kan geen toegang tot de overige verlenen.
- **Resource beleid Inzender** kunt maken en beheren van beleid in de map op de bronnen.     
- **Lezer** bestaande Azure-resources kunt weergeven. 


## <a name="next-steps"></a>Volgende stappen 
Zie voor meer informatie over beheergroepen: 
- [Beheergroepen te organiseren Azure-resources maken](management-groups-create.md)
- [Wijzigen, verwijderen of beheren van uw beheergroepen](management-groups-manage.md)
- [Installeer de Azure Powershell-module](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Bekijk de REST-API-specificaties](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [De uitbreiding voor de Azure CLI installeren](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)

