---
title: Voorbeeld Azure-infrastructuur doorlopen | Microsoft Docs
description: Meer informatie over de belangrijke ontwerp- en richtlijnen voor het implementeren van een voorbeeld van de infrastructuur in Azure.
documentationcenter: ''
services: virtual-machines-windows
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ddbaed6704fd32f7fd4fe5a790424cbf829d2f1c
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932854"
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a>Voorbeeld van de Azure-infrastructuur-overzicht voor het Windows-VM 's
Dit artikel helpt bij het bouwen van de infrastructuur van een voorbeeld van de toepassing. We informatie over het ontwerpen van een infrastructuur voor een eenvoudige online winkel die de richtlijnen en besluiten over naamconventies, beschikbaarheidssets, virtuele netwerken en taakverdelers combineert en distribueren van uw virtuele machines (VM's).

## <a name="example-workload"></a>Voorbeeld van de werkbelasting
Adventure Works Cycles wil maken van een online store-toepassing in Azure die bestaat uit:

* Twee IIS-servers die de front-end in een weblaag client uit te voeren
* Twee IIS-servers voor het verwerken van gegevens en orders in de toepassingslaag van een
* Twee Microsoft SQL Server-exemplaren met AlwaysOn-beschikbaarheidsgroepen (twee SQL-Servers en een meerderheid knooppunt witness) voor het opslaan van productgegevens- en orders in een database-laag
* Twee Active Directory-domeincontrollers voor klanten en leveranciers in een verificatie-laag
* Alle servers bevinden zich in twee subnetten:
  * een front-end-subnet voor de webservers 
  * een back-end-subnet voor de toepassingsservers, de SQL-cluster en de domeincontrollers

![Diagram van verschillende lagen voor toepassingsinfrastructuur](./media/infrastructure-example/example-tiers.png)

Veilig binnenkomend webverkeer moet worden taakverdeling tussen de webservers zoals klanten de online winkel bladeren. Volgorde van verwerking van verkeer in de vorm van HTTP-aanvragen van de web-servers moeten worden verdeeld tussen de toepassingsservers. Bovendien moet de infrastructuur zijn ontworpen voor hoge beschikbaarheid.

Het ontwerp van de resulterende moet gebruikmaken van:

* Een Azure-abonnement en -account
* Een enkele resourcegroep
* Azure Managed Disks
* Een virtueel netwerk met twee subnetten
* Beschikbaarheidssets voor de virtuele machines met een vergelijkbare rol
* Virtuele machines

Alle bovenstaande voldoen aan deze naamgeving:

* Adventure Works Cycles gebruikt **[IT werkbelasting]-[location]-[Azure-resource]** als voorvoegsel
  * In dit voorbeeld '**azos**"(Azure Online Store) is de naam van de workload IT en"**gebruiken**' (VS-Oost 2) is de locatie
* Virtuele netwerken gebruiken AZOS-USE-VN **[aantal]**
* Beschikbaarheidssets gebruiken azos-gebruiken-als-**[rol]**
* Namen van virtuele machines gebruiken azos-gebruiken-vm -**[vmname]**

## <a name="azure-subscriptions-and-accounts"></a>Azure-abonnementen en accounts
Adventure Works Cycles maakt gebruik van de Enterprise-abonnement, met de naam van Adventure Works Enterprise-abonnement voor facturering voor deze workload IT.

## <a name="storage"></a>Storage
Adventure Works Cycles bepaald dat ze Azure Managed Disks te gebruiken. Bij het maken van virtuele machines, worden beide lagen beschikbare opslag worden gebruikt:

* **Standard-opslag** voor de webservers, toepassingsservers, en domeincontrollers en hun gegevensschijven.
* **Premium-opslag** voor de SQL Server-VM's en de gegevensschijven.

## <a name="virtual-network-and-subnets"></a>Virtueel netwerk en subnetten
Omdat het virtuele netwerk niet nodig heeft voor actieve verbinding met de Adventure Work Cycles on-premises netwerk, besloot ze in een virtueel netwerk alleen in de cloud.

Een virtueel netwerk alleen in de cloud worden ze gemaakt met de volgende instellingen met behulp van de Azure-portal:

* Name: AZOS-USE-VN01
* Locatie: VS-Oost 2
* Virtuele netwerkadresruimte: 10.0.0.0/8
* Eerste subnet:
  * Naam: FrontEnd
  * Adresruimte: 10.0.1.0/24
* Tweede subnet:
  * Naam: back-end
  * Adresruimte: 10.0.2.0/24

## <a name="availability-sets"></a>Beschikbaarheidssets
Als u wilt behouden hoge beschikbaarheid van alle vier lagen van hun online winkel, besloten Adventure Works Cycles op vier beschikbaarheidssets:

* **azos gebruiken als web** voor de webservers
* **azos gebruiken als app** voor de toepassingsservers
* **azos gebruik als sql** voor de SQL-Servers
* **azos gebruiken als dc** voor de domeincontrollers

## <a name="virtual-machines"></a>Virtuele machines
Adventure Works Cycles besloten op de volgende namen voor hun Azure-VM's:

* **azos-use-vm-web01** voor de eerste webserver
* **azos-use-vm-web02** voor de tweede webserver
* **azos-use-vm-app01** voor de eerste toepassingsserver
* **azos-use-vm-app02** voor de tweede toepassingsserver
* **azos-use-vm-sql01** voor de eerste server in de SQL Server in het cluster
* **azos-use-vm-sql02** voor het tweede SQL Server-server in het cluster
* **azos-use-vm-dc01** voor de eerste domeincontroller
* **azos-use-vm-dc02** voor de tweede domeincontroller

Hier is de resulterende configuratie.

![Laatste toepassingsinfrastructuur geïmplementeerd in Azure](./media/infrastructure-example/example-config.png)

Deze configuratie omvat:

* Een virtueel netwerk met twee subnetten (front-end en back-end) alleen in de cloud
* Azure Managed Disks voor Standard en Premium-schijven
* Vier beschikbaarheidssets, één voor elke laag van de online winkel
* De virtuele machines voor de vier lagen
* Een externe set met gelijke voor HTTPS-gebaseerde webverkeer van Internet naar de webservers
* Een interne set met gelijke voor niet-versleuteld webverkeer van de webservers naar de toepassingsservers
* Een enkele resourcegroep
