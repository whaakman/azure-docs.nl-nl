---
title: Leslokaallabs gebruikt voor trainingen - Azure Lab-Services | Microsoft Docs
description: Leer hoe u Azure DevTest Labs gebruiken voor training scenario's.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2019
ms.author: spelluru
ms.openlocfilehash: 4d2ba11181977f1976b5ae933e8b93a92424fa96
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56808534"
---
# <a name="use-classroom-labs-for-trainings"></a>Leslokaallabs gebruikt voor trainingen
U kunt een lab voor u kunt instellen. Leslokaal Labs van Azure Lab Services kunt u een lab maken voor uw training waarbij elke leerling identiek zijn en geïsoleerde omgevingen voor training gebruikt. U kunt beleid om ervoor te zorgen dat training omgevingen beschikbaar voor elke leerling zijn alleen wanneer ze deze nodig hebt en voldoende resources - zoals virtuele machines - vereiste voor de training bevatten toepassen. 

![Met classroom lab](../media/classroom-labs-scenarios/classroom.png)

Leslokaallabs voldoet aan de volgende vereisten die nodig zijn voor het trainen in een virtuele omgeving: 

- Stagiairs kunnen snel inrichten voor hun omgevingen voor training
- Elke machine training moeten identiek zijn
- Stagiairs zien virtuele machines die zijn gemaakt door andere stagiairs niet
- Kosten beheren door ervoor te zorgen dat stagiairs meer virtuele machines bevat dan ze nodig voor de trainings- en ook virtuele machines afsluiten hebt wanneer ze niet gebruiken kunnen niet ophalen
- In het lab training eenvoudig delen met elke leerling
- In het lab training steeds weer opnieuw gebruiken

In dit artikel leert u over verschillende functies van Azure Lab-Services die kunnen worden gebruikt om te voldoen aan de eerder beschreven Trainingsvereisten en gedetailleerde stappen die u volgen kunt om het instellen van een testomgeving voor training.  

## <a name="create-the-lab-account-as-a-lab-account-administrator"></a>Het lab-account als de beheerder van een lab maken
De eerste stap bij het gebruik van Azure Lab Services is het maken van een lab-account in Azure portal. Nadat de beheerder van een lab-account het lab-account wordt gemaakt, voegt de beheerder gebruikers die willen labs te maken de **Labmaker** rol. De trainers maken labs met virtuele machines voor studenten te doen voor de cursus die ze geven. Zie voor meer informatie, [maken en beheren van lab-account](how-to-manage-lab-accounts.md).

## <a name="create-and-manage-classroom-labs"></a>Maken en beheren van leslokaallabs
Een trainer, die een lid van de rol Labmaker in een lab-account, kunt u een of meer labs maken in het lab-account. U maakt en configureert een VM-sjabloon met de vereiste software voor het uitvoeren van oefeningen in de cursus. U kiest een kant-en-installatiekopie uit de beschikbare installatiekopieën voor het maken van een leslokaallab en vervolgens aanpassen door de software die is vereist voor de testomgeving te installeren. Zie voor meer informatie, [maken en beheren van leslokaallabs](how-to-manage-classroom-labs.md).

## <a name="configure-usage-settings-and-policies"></a>Instellingen voor het gebruik en het beleid configureren
De maker van de testomgeving kunt toevoegen of verwijderen van gebruikers aan het lab, registratie-koppeling te verzenden naar gebruikers van de testomgeving, instellen van het beleid, zoals afzonderlijke quota instelling per gebruiker, het aantal virtuele machines die beschikbaar zijn in het lab, en meer bijwerken. Zie voor meer informatie, [configureren van instellingen voor het gebruik en het beleid](how-to-configure-student-usage.md).

## <a name="create-and-manage-schedules"></a>Schema's maken en beheren
Schema's kunt u een leslokaallab configureren zodat virtuele machines in het lab automatisch starten en op een bepaald tijdstip afsluiten. U kunt een eenmalige planning of een terugkerend schema definiëren. Zie voor meer informatie, [maken en beheren van schema's voor leslokaallabs](how-to-create-schedules.md).

## <a name="set-up-and-publish-a-template-vm"></a>Instellen en publiceren van een VM-sjabloon
Een sjabloon in een lab is een basiskopie van de virtuele machine waarvan de virtuele machines van alle gebruikers worden gemaakt. De sjabloon voor virtuele machine zo instellen dat deze is geconfigureerd met precies wat u wilt bieden aan de genodigden training. U kunt een naam en een beschrijving opgeven voor de sjabloon die de gebruikers van het lab kunnen zien. Vervolgens publiceert u de sjabloon voor exemplaren van de VM-sjabloon beschikbaar maken voor uw labgebruikers. Wanneer u een sjabloon publiceert, maakt Azure Lab Services virtuele machines in het lab met behulp van de sjabloon. Het aantal virtuele machines dat in dit proces wordt gemaakt, is hetzelfde als het maximum aantal gebruikers dat in het lab is toegestaan. Dit kunt u instellen in het gebruiksbeleid van het lab. Alle virtuele machines hebben dezelfde configuratie als de sjabloon. Zie voor meer informatie, [ingesteld omhoog en publiceren van de sjabloon voor virtuele machines](how-to-create-manage-template.md). 

## <a name="use-vms-in-the-classroom-lab"></a>Gebruik virtuele machines in de leslokaallab
Een student of training deelnemer registreert aan het lab, en maakt verbinding met de virtuele machine te doen voor de cursus. Zie voor meer informatie, [over toegang tot een leslokaallab](how-to-use-classroom-lab.md).

## <a name="next-steps"></a>Volgende stappen
Beginnen met het maken van een lab-account in Leslokaallabs met de instructies in het artikel: [Zelfstudie: Instellen van een lab-account met Azure Lab Services](tutorial-setup-lab-account.md).