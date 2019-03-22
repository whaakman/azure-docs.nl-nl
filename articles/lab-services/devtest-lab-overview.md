---
title: Over Azure DevTest Labs | Microsoft Docs
description: Informatie over hoe DevTest Labs kunt gemakkelijk te maken, beheren en controleren van virtuele machines van Azure
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: 599b4325e0eb9aa7f6ccca2626c6d4b14f38149d
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339326"
---
# <a name="about-azure-devtest-labs"></a>Over Azure DevTest Labs
Azure DevTest Labs is een service waarmee ontwikkelaars kunnen op een team dat virtuele machines efficiënt selfservice en/of PaaS-resources die ze nodig hebben voor het ontwikkelen, testen, training en demo's enzovoort zonder te wachten op goedkeuring van de constante op de hulpprogramma's die ze nodig hebben. 

Er bestaat al in het lab van vooraf geconfigureerde bases of Resource Manager-sjablonen met de benodigde hulpprogramma's en software die ontwikkelaars gebruiken kunnen om omgevingen te maken. Ontwikkelaars kunnen hun omgevingen maken in een paar minuten in plaats van uren of dagen kwijt bent. 

Met DevTest Labs, kunt u de meest recente versie van uw toepassing testen door de volgende taken uitvoeren:

- Snel inrichten van Windows en Linux-omgevingen met herbruikbare sjablonen en artefacten
- Integreer uw implementatiepijplijn eenvoudig met DevTest Labs om in te richten op aanvraag omgevingen
- Schaal uw belastingtests door in te richten van meerdere testagents en maak vooraf ingerichte omgevingen voor training en demo's.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

## <a name="capabilities"></a>Functionaliteit
DevTest Labs biedt de volgende mogelijkheden voor ontwikkelaars die werken met virtuele machines:

- Snel een virtuele machine maken met minder dan vijf eenvoudige stappen te volgen.
- Kies uit een gecureerde lijst met VM-basissen die zijn geconfigureerd, goedgekeurd en geautoriseerd is door de teamleider of de centrale IT.
- Maak machines via vooraf gemaakte aangepaste installatiekopieën die over de software en hulpprogramma's zijn geïnstalleerd in de afbeelding. 
- Machines van formules die zijn in feite aangepaste installatiekopieën en een laatste build van de software die is geïnstalleerd op het moment van het maken van de virtuele machine maken.
- Artefacten die geïmplementeerd op de virtuele machine zijn nadat deze ingericht extensies installeren.
- Automatisch afsluiten ingesteld en automatisch start schema's op de machines aan het einde van de dag afsluiten en vervolgens omhoog en volgende ochtend uitgevoerd.
- Claim gewoon een vooraf gemaakte virtuele machine zonder te hoeven gaan door het proces van het maken van machines. 

DevTest Labs biedt de volgende mogelijkheden voor ontwikkelaars die werken met PaaS-omgevingen:

- Snel minder dan drie eenvoudige stappen te volgen PaaS Azure Resource Manager-omgevingen maken.
- Kies uit een gecureerde lijst met Resource Manager-sjablonen die zijn geconfigureerd, goedgekeurd en geautoriseerd is door de teamleider of de centrale IT.
- Stel een lege resourcegroep (Sandbox) bijwerkt nog steeds binnen de context van het testlab gehele van Azure verkennen met behulp van Resource Manager-sjabloon.
- DevTest Labs biedt de volgende voordelen bij het maken, configureren en beheren van de developer- en testomgevingen in de cloud

Afgezien van een model selfservice voor ontwikkelaars in een team met de service kunt centraal te beheren afval, de kosten voor resources optimaliseren en blijven binnen budgetten aan de hand van de volgende taken: 

- Instelling voor automatisch afsluiten en automatisch worden gestart schema's op virtuele machines.
- Instellen van beleidsregels voor het aantal gebruikers van de virtuele machines kunt maken.
- Instellen van beleidsregels op de grootte van virtuele machines en de galerie met installatiekopieën van gebruikers kan kiezen uit.
- Bijhouden van kosten en doelen instellen voor de testomgeving.
- Een melding te ontvangen op hoge geschatte kosten voor de testomgeving zodat u de nodige maatregelen kunt nemen. 

DevTest Labs biedt de volgende voordelen bij het maken, configureren en beheren van omgevingen in de cloud:

## <a name="cost-control-and-governance"></a>Kostenbeheer en beheer
DevTest Labs wordt het beheersen van kosten gemakkelijker doordat u de volgende taken uitvoeren:

- Beleid instellen voor uw lab - zoals het aantal virtuele machines (VM) per gebruiker en aantal VM's per lab. 
- Beleid maken om automatisch afsluiten en start virtuele machines.
- Kunt u de kosten voor virtuele machines en PaaS-resources in uw praktijksessie ingezette zodat u binnen uw vooraf gedefinieerde budget kan blijven bijhouden. 
- Helpt ontwikkelaars binnen de context van het testlab blijven zodat ze niet zetten resources buiten deze in de onderliggende resourcegroep of abonnement terechtkomen.

## <a name="quickly-get-to-ready-to-test"></a>Snel toegang verkrijgen tot gereed om te testen
DevTest Labs kunt u vooraf ingerichte omgevingen maken met alles wat die uw team nodig heeft om te beginnen met het ontwikkelen en testen van toepassingen. Claim gewoon de omgevingen waarop de laatste goede versie van uw toepassing is geïnstalleerd en begin meteen te werken. Of gebruik containers om nog sneller en slanker zijn.

## <a name="create-once-use-everywhere"></a>Eenmaal maken, overal gebruiken
Vastleggen en PaaS-omgevingssjablonen en artefacten binnen uw team of organisatie - alles in Bronbeheer - als u wilt maken van de developer- en testomgevingen eenvoudig delen.

## <a name="worry-free-self-service"></a>Zorgeloze selfservice
DevTest Labs kunnen uw ontwikkelaars en testers snel en eenvoudig virtuele machines (IaaS) en PaaS-resources binnen een paar muisklikken met behulp van een set vooraf zijn geconfigureerd voor deze resources gemaakt.

## <a name="use-iaas-and-paas-resources"></a>Gebruik van IaaS en PaaS-resources 
DevTest Labs kunnen ook ontwikkelaars Stel binnen een PaaS-resources zoals Web-Apps, Service Fabric-clusters, SharePoint-Farms, enzovoort. met behulp van Resource Manager-sjabloon in de testomgeving. De Resource Manager-sjablonen gebruiken vanuit de opslagplaats van onze openbare omgeving of in het lab verbinden met uw eigen Git-opslagplaats aan de slag op PaaS in labs. U kunt ook kosten op deze resources om te blijven in de vooraf gedefinieerde budgetten bijhouden. 

## <a name="integrate-with-your-existing-toolchain"></a>Integratie met uw bestaande hulpprogrammaketen
Gebruik vooraf gemaakte invoegtoepassingen of onze API om in te richten Dev/Test-omgevingen rechtstreeks vanuit uw gewenste tool continue integratie (CI), geïntegreerde ontwikkelingsomgeving (IDE) of pijplijn voor geautomatiseerde release. U kunt ook ons uitgebreide opdrachtregelprogramma gebruiken.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 

- Zie voor meer informatie over DevTest Labs, [DevTest Labs-concepten](devtest-lab-concepts.md)
- Zie voor een overzicht met stapsgewijze instructies [zelfstudie: Een lab instellen met behulp van Azure DevTest Labs](tutorial-create-custom-lab.md)


