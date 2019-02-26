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
ms.date: 02/22/2019
ms.author: spelluru
ms.openlocfilehash: 255cbff94e954dd05acfe77b97929c313b1dd3cf
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56808460"
---
# <a name="classroom-labs-concepts"></a>Leslokaal Labs-concepten
De volgende lijst bevat belangrijke concepten voor Lab-Services en definities:

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
