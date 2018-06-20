---
title: Informatie over Azure Lab Services | Microsoft Docs
description: Lees hoe u met Lab Services gemakkelijk labs kunt maken, beheren en beveiligen met virtuele machines die kunnen worden gebruikt door ontwikkelaars, testers, docenten, studenten en anderen.
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
ms.date: 05/21/2018
ms.author: spelluru
ms.openlocfilehash: e9c3cae7c7129cc489ddd38b5b2de18dd6f52e58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660496"
---
# <a name="introduction-to-classroom-labs"></a>Inleiding tot leslokaallabs
Met Azure Lab Services kunt u snel een omgeving voor een leslokaallab in de cloud instellen. Een docent maakt een leslokaallab, richt virtuele machines met Windows of Linux in, installeert de benodigde software en labhulpprogramma's in het leslokaal en maakt deze beschikbaar voor studenten. De studenten in het leslokaal maken verbinding met virtuele machines (VM's) in het lab en gebruiken deze voor hun projecten, opdrachten en klassikale oefeningen. 

De leslokaallabs zijn beheerde labs die worden beheerd door Azure. De service zelf is verantwoordelijk voor het volledige beheer van de infrastructuur voor een beheerd lab, van het inrichten van VM's (virtuele machines) tot de afhandeling van fouten, en het schalen van de infrastructuur. U geeft op welke infrastructuur u nodig hebt en installeert alle hulpprogramma's of software die in het leslokaal nodig zijn. De beheerde labs zijn momenteel beschikbaar als preview.  

## <a name="scenarios"></a>Scenario's
Hier ziet u het hoofdscenario dat door leslokaallabs van Azure Lab Services wordt ondersteund: 

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Inrichten van een schaalbaar computerlab in de cloud voor uw leslokaal  

- Maak een beheerd lab voor het leslokaal. U geeft precies aan wat u nodig hebt en de service zorgt er vervolgens voor dat de infrastructuur van het lab wordt gemaakt en beheerd. Hierdoor kunt u zich volledig richten op uw leerlingen of studenten en hoeft u zich niet bezig te houden met de technische details van een lab. 
- Bied leerlingen en studenten een lab met virtuele machines die zijn geconfigureerd met exact wat nodig is voor een les of college. Geef elke leerling of student een beperkt aantal uren voor het gebruik van de VM's voor het uitvoeren van hun taken.  
- Breng het fysieke computerlab van uw school over naar de cloud. U kunt het aantal VM's automatisch schalen tot de drempelwaarde voor gebruik en kosten die u instelt voor het lab. 
- Verwijder het lab met één muisklik als u klaar bent. 

## <a name="user-profiles"></a>Gebruikersprofielen
In dit artikel worden de verschillende gebruikersprofielen in Azure Lab Services beschreven. 

### <a name="lab-account-owner"></a>Labeigenaar
Meestal fungeert een IT-beheerder van de cloudresources van de organisatie, die ook eigenaar is van het Azure-abonnement, als de eigenaar van een lab-account. Deze persoon is verantwoordelijk voor de volgende taken:   

- Een lab-account instellen voor uw organisatie.
- Beleidsregels beheren en configureren voor alle labs.
- Machtigingen verlenen aan personen binnen de organisatie voor het maken van een lab onder het lab-account.

### <a name="educator"></a>Docent
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
