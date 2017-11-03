---
title: Uw Azure-beheergroepen - Azure-resources te organiseren | Microsoft Docs
description: Meer informatie over de beheergroepen en hoe ze te gebruiken.
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: 18541c68b02ae1b59ae4a6a85122dff614c9978c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Breng uw resources met Azure-beheergroepen 

Als u meerdere abonnementen hebt, kunt u deze indelen in containers, genaamd 'beheergroepen' voor het beheren van toegang, beleid, kosten en naleving voor uw abonnementen. Een voorbeeld: u kunt beleid toepassen op een beheergroep die grens brontypen kunnen worden gemaakt.

> [!Note]
> Deze functie is momenteel in een private preview. [Hier aanmelden](https://aka.ms/MGPreviewSignup) uw inschrijving voor het koppelen van de Preview-versie hebben.   
 


Beheergroepen kunnen worden georganiseerd in een hiërarchie. De structuur die wordt weergegeven, is een voorbeeld-weergave van de hiërarchie van een management-groep die kan optreden:


![Hiërarchische structuur](media/billing-enterprise-mgmt-groups/tree.png)



## <a name="how-management-groups-are-related-to-your-azure-enterprise-enrollment"></a>Hoe beheergroepen zijn gerelateerd aan uw Azure Enterprise-inschrijving

De introductie van beheergroepen is een stap in de grotere reis van overgang [Enterprise portal](https://ea.azure.com) onderdelen moeten worden de [Azure-portal](https://portal.azure.com).

De structuur van de groep management wordt gemaakt die is gedefinieerd in de Enterprise portal. De gehele hiërarchie, die bestaan uit inschrijving, afdelingen en -accounts zijn toegewezen aan de bijbehorende beheergroepen. 

Hier ziet u hoe de structuur van de huidige EA wordt toegewezen aan de groep beheerhiërarchie. 

![Hiërarchische structuur](media/billing-enterprise-mgmt-groups/tree2.png)

De onderstaande tabel ziet hoe de gebruikers van de Enterprise portal zijn toegewezen aan de groep beheerhiërarchie.

|    EA rol                                       |    De rol op het knooppunt van de groep toegewezen beheer    |    Machtigingen voor beheer groepsknooppunt                                                          |
|--------------------------------------------------|--------------------------------------------------|----------------------------------------------------------------------------------------------------|
|    EA beheerder                              |    Resource beleid Inzender                   |    Kan kosten weergeven, resource beleids- en hiërarchie bij en onder het knooppunt inschrijving beheren    |
|    EA beheerder in de modus alleen-lezen            |    Lezer facturering                                |    Kan lezen kosten en hiërarchie bij en onder het knooppunt inschrijving weergeven                              |
|    Afdeling beheerder                      |    Lezer facturering                                |    Kan lezen kosten en hiërarchie weergeven en onder het knooppunt afdeling                                 |
|    Afdeling beheerder in de modus alleen-lezen    |    Lezer facturering                                |    Kan lezen kosten en hiërarchie weergeven en onder het knooppunt afdeling                                 |
|    De eigenaar van account                                 |    Resource beleid Inzender                   |    Kan kosten weergeven, resource beleids- en hiërarchie bij en onder het accountknooppunt beheren       |




## <a name="view-management-groups-in-the-azure-portal"></a>Beheergroepen weergeven in de Azure portal

Als u wilt weergeven in een inschrijving, afdeling of een account in de preview, aanmelden bij de Azure-portal met de koppeling in het welkomstbericht.   

![hiërarchie](media/billing-enterprise-mgmt-groups/hierarchy.png)

### <a name="viewing-costs"></a>Kosten weergeven 
In de schermen detail van beheergroepen ziet u de huidige maand tot datum kosten. Deze kosten zijn gebaseerd op informatie over het gebruik en geen rekening gehouden met vooruitbetaalde bedragen, gebruikskosten opgenomen hoeveelheden, aanpassingen en belastingen. Voor de beheergroep die overeenkomt met uw registratie, de kosten sectie ziet u de resterende vastlegging.  

![Details van de inschrijving](media/billing-enterprise-mgmt-groups/enrollment.png)

 'Kosten afzonderlijk in rekening gebracht' zijn de maandelijkse opeenstapeling van afzonderlijke wijzigingen zoals marketplace, gebruikskosten en andere kosten die niet op basis van uw registratie streven gaan.  Zie voor meer informatie over de verdeling van de kosten, de [Enterprise portal](https://ea.azure.com). 

### <a name="enabling-access-to-costs"></a>Toegang tot kosten inschakelen
Als u kosten niet ziet, raadpleegt u onze [oplossen enterprise kosten weergaven](https://aka.ms/enableazurecosts) document voor hulp.  

### <a name="delays-between-the-enterprise-portal-and-azure-portal"></a>Vertraging tussen de Enterprise portal en Azure-portal 
* Tijdens de preview kunnen bedragen weergegeven in de Azure-portal worden vertraagd in vergelijking met waarden in de Enterprise portal. Dit probleem is tijdelijk en wordt gewerkt.
* Bijgewerkte instellingen in de Enterprise portal hebben een vertraging van enkele minuten voordat de updates worden weergegeven in de Azure-portal. 

## <a name="management-groups-have-a-relationship-with-your-directory"></a>Beheergroepen hebben een relatie heeft met uw directory   
Zoals abonnementen, ook hebben een vertrouwensrelatie met Azure AD beheergroepen. Een hiërarchie van de groep management vertrouwt één map om gebruikers te verifiëren. Alle beheerders die zijn gekoppeld aan een hiërarchie van de groep management moet behoren tot dezelfde directory. 

Als uw hiërarchie inschrijving is toegewezen aan beheergroepen, een vertrouwensrelatie tot stand is gebracht met een enkele map. Waar mogelijk, is een bestaande map die is gekoppeld aan de gebruikersaccounts van de inschrijving geselecteerd. In sommige gevallen kan een nieuwe map wordt gemaakt en alle bestaande inschrijving gebruikers worden uitgenodigd in die map. Mappen die zijn gekoppeld aan de inschrijving abonnementen worden niet getroffen. De hiërarchie mogelijk daarom in een map die verschilt van de abonnementen gemaakt. [Meer informatie](billing-enterprise-mgmt-grp-find.md) over hoe dit proces heeft impact op de ervaring van navigeren tussen de hiërarchie en de abonnementen.

## <a name="administering-your-management-groups"></a>Uw beheergroepen beheren
Beheergroepen binnen de Azure-portal zijn Preview-versie en zijn alleen-lezen in deze eerste release. Als u eventuele updates, gaat u naar de Enterprise portal. De updates worden automatisch doorgevoerd in de Azure-portal. Zie de documentatie in de enterprise portal voor hulp bij het maken van wijzigingen en toevoegingen.   

## <a name="policy-management"></a>Beleidsbeheer
Met Resource Manager kunt u aangepaste beleidsregels maken voor het beheer van resources. Met beheergroepen, het beleid kan worden toegewezen op elk niveau in de hiërarchie en de resources nemen over deze beleidsregels.  [Meer informatie](https://go.microsoft.com/fwlink/?linkid=858942)

> [!Note]
> Beleid wordt niet afgedwongen op mappen. 


