---
title: Configuratie en het gebruik van openbare omgevingen in Azure DevTest Labs | Microsoft Docs
description: Informatie over het configureren en gebruiken van openbare omgevingen in Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2018
ms.author: spelluru
ms.openlocfilehash: d93818cd875c4050b1b35f21ce580933776c5bc5
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234911"
---
# <a name="configure-and-use-public-environments-in-azure-devtest-labs"></a>Configuratie en het gebruik van openbare omgevingen in Azure DevTest Labs
Azure DevTest Labs is een [openbare opslagplaats van Azure Resource Manager-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/Environments) die u kunt gebruiken om omgevingen te maken zonder verbinding maken met een externe bron van GitHub zelf. Deze opslagplaats bevat veelgebruikte sjablonen, zoals Azure Web Apps, Service Fabric-Cluster en development environment van de SharePoint-Farm. Deze functie is vergelijkbaar met de openbare opslagplaats voor artefacten die is opgenomen voor elke lab die u maakt. De opslagplaats van de omgeving kunt u snel aan de slag met vooraf opgestelde omgevingssjablonen met minimale invoer parameters, zodat u met een goede aan de slag te gaan ervaring te bieden voor PaaS-resources binnen labs. 

## <a name="configuring-public-environments"></a>Openbare omgevingen configureren
Als de eigenaar van een lab, kunt u de opslagplaats openbare omgeving voor uw lab inschakelen tijdens het maken van het lab. Als u openbare omgevingen voor uw lab, schakelt **op** voor de **openbare omgevingen** veld tijdens het maken van een lab. 

![Openbare omgeving voor een nieuw lab inschakelen](media/devtest-lab-configure-use-public-environments/enable-public-environment-new-lab.png)


De opslagplaats van de openbare-omgeving is niet ingeschakeld voor bestaande labs. Deze functie handmatig inschakelen met sjablonen in de opslagplaats. De opslagplaats is voor labs gemaakt met behulp van Resource Manager-sjablonen, evenals standaard uitgeschakeld.

U kunt in-of uitschakelen openbare omgevingen voor uw lab, en ook alleen specifieke omgevingen beschikbaar maken voor labgebruikers met behulp van de volgende stappen uit: 

1. Selecteer **configuratie en het beleid** voor uw testomgeving. 
2. In de **VM-BASISSEN** sectie, selecteer **openbare omgevingen**.
3. Als u openbare omgevingen voor de testomgeving, schakelt **Ja**. Selecteer anders **Nee**. 
4. Als u openbare omgevingen hebt ingeschakeld, worden alle omgevingen in de opslagplaats worden ingeschakeld door de standaardinstellingen. Ongedaan maken kunt u een omgeving zodat deze niet beschikbaar voor uw labgebruikers. 

![Openbare omgevingen pagina](media/devtest-lab-configure-use-public-environments/public-environments-page.png)

## <a name="use-environment-templates-as-a-lab-user"></a>Omgevingssjablonen als een lab-gebruiker gebruiken
Als een gebruiker lab, kunt u een nieuwe omgeving maken in de ingeschakelde lijst omgevingssjablonen door eenvoudig te selecteren **+ toevoegen** vanaf de werkbalk op de pagina lab. De lijst met databases bevat de openbare omgevingen sjablonen ingeschakeld door de beheerder van de testomgeving aan de bovenkant van de lijst.

![Openbare omgevingssjablonen](media/devtest-lab-configure-use-public-environments/public-environment-templates.png)

## <a name="next-steps"></a>Volgende stappen
Deze opslagplaats is een open-source-opslagplaats die u bijdragen kunt om toe te voegen vaak gebruikte en nuttige Resource Manager-sjablonen van uw eigen. Als u wilt bijdragen, moet u gewoon een pull-aanvraag op basis van de opslagplaats indient.  
