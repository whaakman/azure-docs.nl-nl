---
title: Azure Stack en de globale Azure vergelijken | Microsoft Docs
description: Informatie over hoe Microsoft biedt Azure en de Azure Stack-familie van services in één Azure-ecosysteem
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/29/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: ''
ms.lastreviewed: 03/29/2019
ms.openlocfilehash: 5e871d467fec476f1dac77fb879d180ea2322c80
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650081"
---
# <a name="differences-between-global-azure-azure-stack-and-azure-stack-hci"></a>Verschillen tussen de globale Azure, Azure Stack en Azure Stack HC1

Microsoft biedt Azure en de Azure Stack-familie van services in één Azure-ecosysteem. Gebruik het toepassingsmodel, selfserviceportals en API's met Azure Resource Manager bieden mogelijkheden voor cloud-gebaseerde of uw bedrijf maakt gebruik van global Azure of on-premises resources.

Dit artikel worden algemene mogelijkheden van Azure, Azure Stack en Azure Stack HC1 en bevat algemene scenario aanbevelingen voor het maken van de beste keuze voor het leveren van Microsoft cloud-gebaseerde services voor uw organisatie.

![Overzicht van Azure-ecosysteem](./media/compare-azure-azure-stack/azure-family.png)

## <a name="global-azure"></a>Global Azure

Microsoft Azure is een set cloudservices die constant blijft groeien en uw organisatie helpt te voorzien in allerlei zakelijke uitdagingen. Azure geeft u de vrijheid om met uw favoriete hulpprogramma's en frameworks, toepassingen te ontwikkelen, beheren en implementeren op een omvangrijk, wereldwijd netwerk.

Globale Azure biedt meer dan 100 services beschikbaar in 54 regio's over de hele wereld. Zie voor de meest recente lijst met algemene Azure-services, de [ *producten beschikbaar per regio*](https://azure.microsoft.com/regions/services). De services die beschikbaar zijn in Azure worden weergegeven op categorie, evenals of die algemeen beschikbaar zijn of zijn beschikbaar via de Preview-versie.

Zie voor meer informatie over algemene Azure-services, [aan de slag met Azure](https://docs.microsoft.com/azure/#pivot=get-started&panel=get-started1).

## <a name="azure-stack"></a>Azure Stack

Azure Stack is een uitbreiding van Azure die zorgt voor de flexibiliteit en innovatie van cloudcomputing tot uw on-premises omgeving. On-premises geïmplementeerd, Azure Stack kunnen worden gebruikt voor consistente Azure-services die een met internet (en Azure) of in niet-verbonden omgevingen zonder internetverbinding verbonden. Azure Stack maakt gebruik van dezelfde onderliggende technologieën als globale Azure, met inbegrip van de belangrijkste onderdelen van infrastructuur-as-a-Service (IaaS), Software-as-a-Service (SaaS), en optionele Platform-as-a-Service (PaaS)-mogelijkheden, waaronder:

- Azure VM's voor Windows en Linux
- Azure-Web-Apps en Functions
- Azure Key Vault
- Azure Resource Manager
- Azure Marketplace
- Containers
- Azure IoT Hub en Eventhubs
- Beheerhulpprogramma's (plannen, aanbiedingen, RBAC, enz.)

De PaaS-mogelijkheden van Azure Stack zijn optioneel, omdat Azure Stack wordt niet beheerd door Microsoft: het wordt beheerd door onze klanten. Dit betekent dat u welke PaaS-service die u eindgebruikers wilt als u bent voorbereid op de onderliggende infrastructuur en processen van de eindgebruiker abstracte kunt aanbieden. Azure Stack echter verschillende optionele PaaS-serviceproviders zoals App Service, SQL-databases en MySQL-databases bevatten. Dit zijn geleverd als resourceproviders, zodat deze klaar voor meerdere tenants, bijgewerkt na verloop van tijd met-standaard die Azure Stack-updates, zichtbaar zijn in de Azure Stack-portal zijn en goed worden geïntegreerd met Azure Stack.

Naast de resourceproviders die hierboven worden beschreven, zijn er extra PaaS-services beschikbaar en zijn getest als [Azure Resource Manager-sjabloon gebaseerde oplossingen](https://github.com/Azure/AzureStack-QuickStart-Templates) die worden uitgevoerd in IaaS, maar u als een Azure Stack-operator kan bieden als PaaS-services aan uw gebruikers met inbegrip van:

- Service Fabric
- Kubernetes-Containerservice
- IoT-Hub en Event Hub
- Etherium Blockchain
- Cloud Foundry

### <a name="example-use-cases-for-azure-stack"></a>Voorbeelden van use cases voor Azure Stack:

- Modellering van financiële
- Gegevens van de klinische en claims
- Analytics voor IoT-apparaat
- Detailhandel assortiment optimalisatie
- Optimalisatie van de toeleveringsketen
- Industriële IoT
- Voorspellend onderhoud
- Slimme stad
- De betrokkenheid van burgers

Meer informatie over Azure Stack op [wat is Azure Stack](azure-stack-overview.md).

## <a name="azure-stack-hci"></a>Azure Stack HCI 

Azure Stack HC1-oplossingen kunnen u virtuele machines op locaties uitvoeren en eenvoudig verbinding maken met Azure met een hypergeconvergeerd-infrastructuuroplossing (HC1). Bouw cloudtoepassingen en voer ze on-premises uit met behulp van consistente Azure-services, zodat u voldoet aan wettelijke of technische vereisten. Naast de actieve gevirtualiseerde toepassingen on-premises kunt Azure Stack HC1 u vervangen en veroudering serverinfrastructuur te consolideren en verbinding maken met Azure cloud Services met behulp van de Windows Admin Center.

Azure Stack HC1 biedt gevalideerde HC1 oplossingen mogelijk gemaakt door Hyper-V- en Storage Spaces Direct met Windows Server 2019 software-defined Datacenter (SDDC). De Windows Admin Center wordt gebruikt voor beheer en toegang tot Azure-services zoals geïntegreerd:

- Azure Backup
- Azure Site Recovery
- Azure controleren en bijwerken

Voor een bijgewerkte lijst van Azure-services die u kunt verbinding maken Azure Stack HC1 Zie [Windows-Server verbinding te maken met Azure hybrid services](https://docs.microsoft.com/windows-server/azure-hybrid-services/index).

### <a name="example-use-cases-for-azure-stack-hci"></a>Voorbeelden van use cases voor Azure Stack HC1
- Externe locatie of branch office-systemen
- Consolidatie van het datacenter
- Virtual desktop Infrastructure
- Business-kritische infrastructuur
- Lagere kosten opslag
- Hoge beschikbaarheid en herstel na noodgevallen in de cloud
- Zakelijke apps, zoals SQL Server

Ga naar de [Azure Stack HC1 website](https://azure.microsoft.com/overview/azure-stack/hci/) om 70 Azure Stack HC1 solutions momenteel beschikbaar is via Microsoft-partners weer te geven.

## <a name="next-steps"></a>Volgende stappen

[Basisbeginselen voor het beheer van Azure Stack](azure-stack-manage-basics.md)

[Snelstartgids: Gebruik de Azure Stack-beheerportal](azure-stack-manage-portals.md)
