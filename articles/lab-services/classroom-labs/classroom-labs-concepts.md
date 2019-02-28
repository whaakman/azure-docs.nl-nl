---
title: Leslokaal Labs-concepten - Azure Lab-Services | Microsoft Docs
description: Leer de basisconcepten van Lab-Services en hoe deze kan gemakkelijk te maken en beheren van labs.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2019
ms.author: spelluru
ms.openlocfilehash: f0f45e50fba4587e604add937ed45fc78228d44f
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960238"
---
# <a name="classroom-labs-concepts"></a>Concepten van leslokaallabs
De volgende lijst bevat belangrijke concepten voor Lab-Services en definities:

## <a name="quota"></a>Quota
Quotum is de tijdslimiet (in uren) die een docent kunt instellen voor een student een lab-virtuele machine gebruiken. Het kan worden ingesteld op 0, onbeperkt of een bepaald aantal uren. Als het quotum is ingesteld op 0, kunt een student de virtuele machine alleen gebruiken als een planning wordt uitgevoerd of als een docent handmatig op de virtuele machine voor de studenten wordt.
 
##<a name="schedules"></a>Planningen
Schema's zijn de time-sleuven (één keer of terugkerende) die een docent kunt maken voor de klasse. Alle virtuele machines in het lab worden automatisch gestart aan het begin aan de planning en ze gestopt zijn aan het einde van de planning. Quotum uur worden niet gebruikt wanneer een schema wordt uitgevoerd.

## <a name="template-virtual-machine"></a>Sjabloon voor virtuele machine
Een sjabloon voor virtuele machine in een testomgeving is een basis van base VM-installatiekopie waarin alle gebruikers virtuele machines zijn gemaakt. Makers van trainers/lab instellen van de sjabloon voor virtuele machine en configureer deze met de software die ze willen om deelnemers willen labs training te geven. Wanneer u een VM-sjabloon publiceert, wordt Azure Lab-Services wordt gemaakt of bijgewerkt lab-virtuele machines op basis van de VM-sjabloon. 


## <a name="user-profiles"></a>Gebruikersprofielen
In dit artikel worden de verschillende gebruikersprofielen in Azure Lab Services beschreven. 

### <a name="lab-account-owner"></a>Labeigenaar
Meestal fungeert een IT-beheerder van de cloudresources van de organisatie, die ook eigenaar is van het Azure-abonnement, als de eigenaar van een lab-account. Deze persoon is verantwoordelijk voor de volgende taken:   

- Een lab-account instellen voor uw organisatie.
- Beleidsregels beheren en configureren voor alle labs.
- Machtigingen verlenen aan personen binnen de organisatie voor het maken van een lab onder het lab-account.

### <a name="professor"></a>Professor
Normaal gesproken maken gebruikers, bijvoorbeeld een leraar of onlinedocent leslokaallabs in een labaccount. Een docent voert de volgende taken uit: 

- Leslokaallab maken.
- Virtuele machines maken in het lab. 
- De juiste software installeren op virtuele machines.
- Instellen wie toegang heeft tot het lab.
- Registratiekoppeling naar het lab doorgeven aan studenten.

### <a name="student"></a>Student
Een student voert de volgende taken uit:

- De registratiekoppeling volgen die is verstuurd door een labmaker om zich te registreren bij het lab. 
- Verbinding maken met een virtuele machine in het lab en deze gebruiken voor het maken van klassikale oefeningen, huiswerk en projecten. 

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het instellen van een labaccount dat is vereist voor het maken van een leslokaallab met Azure Lab Services:

- [Een lab-account maken](tutorial-setup-lab-account.md)
