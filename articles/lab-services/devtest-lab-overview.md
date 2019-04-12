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
ms.openlocfilehash: e07149865d2dda52e33003964c2852a8aaccf76f
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59493192"
---
# <a name="about-azure-devtest-labs"></a>Over Azure DevTest Labs
Azure DevTest Labs kunnen ontwikkelaars van teams voor het efficiënt zelf beheren van virtuele machines (VM's) en PaaS-resources zonder te wachten op goedkeuring.

DevTest Labs wordt gemaakt die bestaat uit een vooraf geconfigureerde bases of Azure Resource Manager-sjablonen labs. Deze moeten alle benodigde hulpprogramma's en software die u gebruiken kunt om omgevingen te maken. U kunt omgevingen maken in een paar minuten, in plaats van uren of dagen kwijt bent.

Met behulp van DevTest Labs, kunt u de nieuwste versies van uw toepassingen testen door de volgende taken uitvoeren:

- Richt snel Windows en Linux-omgevingen met behulp van herbruikbare sjablonen en artefacten.
- Integreer uw implementatiepijplijn eenvoudig met DevTest Labs om in te richten op aanvraag omgevingen.
- Schaal uw belastingtests door in te richten van meerdere testagents en maak vooraf ingerichte omgevingen voor training en demo's.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

## <a name="capabilities"></a>Functionaliteit
DevTest Labs biedt de volgende mogelijkheden voor ontwikkelaars die werken met virtuele machines:

- Snel virtuele machines maken met minder dan vijf eenvoudige stappen te volgen.
- Kies uit een gecureerde lijst met VM-basissen die zijn geconfigureerd, goedgekeurd en geautoriseerd is door de teamleider of de centrale IT.
- Virtuele machines maken van vooraf gemaakte aangepaste installatiekopieën die over de software en hulpprogramma's zijn al geïnstalleerd. 
- Virtuele machines maken van aangepaste installatiekopieën die in combinatie met de meest recente builds van de software geïnstalleerd wanneer de virtuele machines worden gemaakt.
- Artefacten die geïmplementeerd op virtuele machines zijn nadat deze zijn ingericht extensies installeren.
- Automatisch afsluiten instelt en schema's op virtuele machines automatisch starten.
- Claim een vooraf gemaakte virtuele machine zonder tussenkomst van het proces voor het maken.

DevTest Labs biedt de volgende mogelijkheden voor ontwikkelaars die werken met PaaS-omgevingen:

- Resourcemanager gebruiken voor het snel PaaS-omgevingen maken door minder dan drie eenvoudige stappen te volgen.
- Kies uit een gecureerde lijst met Resource Manager-sjablonen die zijn geconfigureerd en geautoriseerd is door de teamleider of de centrale IT.
- Stel een lege resourcegroep (sandbox) met behulp van Resource Manager-sjabloon om Azure te verkennen in de context van een lab.

Met DevTest Labs kunt centrale IT te beheren afval, kosten van resources te optimaliseren en binnen een budgetten blijven door de volgende taken uitvoeren: 

- Instellen van schema's voor automatisch afsluiten en automatisch wordt gestart op virtuele machines.
- Instellen van beleidsregels voor het aantal virtuele machines die gebruikers kunnen maken.
- Instellen van beleidsregels op een VM-grootten en galerie met installatiekopieën die gebruikers kiezen.
- Bijhouden van kosten en doelen instellen voor labs.
- Een melding te ontvangen op hoge geschatte kosten voor labs zodat u de nodige maatregelen kunt nemen.

DevTest Labs biedt de volgende voordelen bij het maken, configureren en beheren van omgevingen in de cloud.

## <a name="control-costs-and-governance"></a>Beheersen van kosten en beheer
DevTest Labs wordt het beheersen van kosten gemakkelijker doordat u de volgende taken uitvoeren:

- Beleid instellen voor uw labs, zoals het aantal virtuele machines per gebruiker of per lab. 
- Beleid maken om automatisch afsluiten en start virtuele machines.
- De kosten voor virtuele machines en PaaS-resources bijhouden vlechtstoffen omhoog in labs om binnen uw budget te blijven.
- Binnen de context van uw labs blijven, zodat u resources buiten deze niet instellen.

## <a name="quickly-get-to-ready-to-test"></a>Snel toegang verkrijgen tot gereed om te testen
DevTest Labs kunt u vooraf ingerichte omgevingen die zijn uitgerust met alles wat die uw team nodig heeft om te ontwikkelen en testen van toepassingen maken. Alleen de claim van de omgevingen waarop de laatste goede versie van uw toepassing is geïnstalleerd en aan de slag gaat. Of gebruik containers voor het maken van de omgeving nog sneller en slanker zijn.

## <a name="create-once-use-everywhere"></a>Eenmaal maken, overal gebruiken
Vastleggen en PaaS-omgevingssjablonen en artefacten binnen uw team of organisatie delen, alles in bronbeheer, gemakkelijk maken developer-en testomgevingen.

## <a name="save-time-on-setup"></a>Bespaar tijd op installatie  
U kunt eenvoudig virtuele machines van IaaS en PaaS-resources maken met behulp van een set met vooraf geconfigureerde resources.

## <a name="use-iaas-and-paas-resources"></a>Gebruik van IaaS en PaaS-resources 
Ontwikkelaars kunnen ook Stel PaaS-resources, zoals Azure Service Fabric-clusters, de functie Web Apps van Azure App Service en SharePoint-farms, met behulp van Resource Manager-sjablonen. Om te beginnen op PaaS in labs, gebruikt u de sjablonen uit de opslagplaats voor openbare-omgeving of in het lab verbinden met uw eigen Git-opslagplaats. U kunt ook kosten bijhouden op deze resources om binnen uw budget te blijven.

## <a name="integrate-with-your-existing-toolchain"></a>Integratie met uw bestaande hulpprogrammaketen
Gebruik vooraf gemaakte invoegtoepassingen of de API voor het inrichten ontwikkeling/testen testomgevingen rechtstreeks vanaf uw gewenste tool continue integratie (CI), integrated development environment (IDE) of pijplijn voor geautomatiseerde release. U kunt ook de uitgebreide opdrachtregelprogramma gebruiken.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- Zie voor meer informatie over DevTest Labs, [DevTest Labs-concepten](devtest-lab-concepts.md).
- Zie voor een overzicht met stapsgewijze instructies [zelfstudie: Een lab instellen met behulp van Azure DevTest Labs](tutorial-create-custom-lab.md).


