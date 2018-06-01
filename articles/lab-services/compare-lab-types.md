---
title: De verschillende soorten labs in Azure Lab Services vergelijken | Microsoft Docs
description: Legt uit en vergelijkt verschillende soorten labs die u met behulp van Azure Lab Services (voorheen DevTest Labs) kunt maken.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 22a1c90dd1a1ca305431d91a801e5293a6d08703
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361179"
---
# <a name="compare-managed-and-devtest-labs-in-azure-lab-services"></a>Beheerde en DevTest-labs in Azure Lab Services vergelijken
U kunt twee soorten labs maken: **beheerde labs** met Azure Lab Services en **aangepaste labs** met Azure DevTest Labs. Als u alleen wilt invoeren wat u nodig hebt in een lab en de service de voor het lab vereiste infrastructuur wilt laten instellen, dient u een van de **beheerde labs** te kiezen. Op dit moment is het **leslokaallab** het enige type beheerde Lab dat u met Azure Lab-Services kunt maken. Als u uw eigen infrastructuur wilt beheren, maakt u een **aangepaste lab** met behulp van Azure DevTest Labs.

De volgende secties bevatten meer informatie over deze labs. 

## <a name="managed-labs"></a>Beheerde labs
Beheerde labs bieden verschillende soorten labs die geschikt zijn voor uw specifieke behoeften. Azure Lab Services ondersteunt momenteel alleen het **leslokaallab** als beheerd lab. Met beheerde labs kunt u meteen aan de slag met een minimale installatie. De service zelf is verantwoordelijk voor het volledige beheer van de infrastructuur voor het lab, van het inrichten van de VM's tot de afhandeling van fouten, en het schalen van de infrastructuur. Als u wilt een beheerd lab maken, moet u eerst een lab-account voor uw organisatie maken. Het lab-account fungeert als het centrale account waarin alle labs in de organisatie worden beheerd. 

Wanneer u Azure-resources in deze beheerde labs maakt en gebruikt, worden deze door de service in interne abonnementen van Microsoft gemaakt en beheerd. Ze worden niet in uw eigen Azure-abonnement gemaakt. De service houdt het gebruik van deze resources bij in de interne Microsoft-abonnementen. Dit gebruik wordt weer in rekening gebracht op uw Azure-abonnement dat het lab-account bevat.   

Hier volgen enkele van de **gebruikscases voor beheerde labs**: 

- Bied leerlingen en studenten een lab met virtuele machines die zijn geconfigureerd met exact wat nodig is voor een les of college. Geef elke leerling of student een beperkt aantal uren voor het gebruik van de VM's voor hun huiswerk of persoonlijke projecten.
- Stel een pool in van hoogwaardige reken-VM’s om rekenintensief of grafisch-intensief onderzoek uit te voeren. Voer de virtuele machines uit, indien nodig, en schoon de machines op wanneer u klaar bent. 
- Breng het fysieke computerlab van uw school over naar de cloud. U kunt het aantal VM's automatisch schalen tot de drempelwaarde voor gebruik en kosten die u instelt voor het lab.  
- Richt snel een lab met virtuele machines in voor het hosten van een hackathon. Verwijder het lab met één muisklik als u klaar bent. 


## <a name="devtest-labs"></a>DevTest-labs
Mogelijk hebt u scenario's waarin u alle infrastructuur en configuratie zelf wilt beheren binnen uw eigen abonnement. Hiervoor kunt u een aangepast lab maken met Azure DevTest Labs in de Azure-portal. Voor deze labs hoeft u geen lab-account te maken. Deze labs worden niet weergegeven in het lab-account (wat voor de beheerde labs bestaat).  

Hier volgen enkele **gebruikscases van DevTest-labs**: 

- Richt snel een lab met virtuele machines in voor het hosten van een hackathon of een praktische sessie op een conferentie. Verwijder het lab met één muisklik als u klaar bent. 
- Maak een pool met virtuele machines die zijn geconfigureerd met uw toepassing en laat uw team eenvoudig gebruikmaken van een virtuele machine voor bug-bashes.  
- Geef ontwikkelaars virtuele machines die zijn geconfigureerd met alle hulpprogramma's die ze nodig hebben. Maak een schema voor automatisch starten en afsluiten om kosten te minimaliseren. 
- Maak herhaaldelijk een lab met testmachines als onderdeel van uw implementatie. Test uw meest recente bits en ruim de testmachines op wanneer u klaar bent. 
- Stel tal van anders geconfigureerde virtuele machines en meerdere testagenten in voor het testen van de schaal en prestaties. 
- Bied uw klanten trainingssessies aan met een lab dat is geconfigureerd met de nieuwste versie van uw product. Geef elke klant een beperkt aantal uren voor het gebruik van het lab. 


## <a name="managed-labs-vs-devtest-labs"></a>Beheerde labs versus DevTest-labs
In de volgende tabel worden twee soorten testlaboratoria vergeleken die worden ondersteund door Azure Lab Services: 

| Functies | Beheerde labs | DevTest-labs |
| -------- | ----------------  | ---------- |
| Beheer van Azure-infrastructuur in het lab. |  Automatisch beheerd door de service | U beheert zelf  |
| Ingebouwde tolerantie voor problemen in de infrastructuur | Automatisch afgehandeld door de service | U beheert zelf  |
| Abonnementsbeheer | Service verwerkt de toewijzing van resources binnen Microsoft-abonnementen die de service ondersteunen. Schalen wordt automatisch afgehandeld door de service. | U beheert zelf in uw eigen Azure-abonnement. Er is geen automatische schaling van abonnementen. |
| Implementatie van Azure Resource Manager in het lab | Niet beschikbaar | Beschikbaar |

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het installeren van een lab met Azure Lab Services:

- [Een leslokaallab instellen](tutorial-setup-classroom-lab.md)
- [Een aangepast lab instellen](tutorial-create-custom-lab.md)
