---
title: Voorbeeld van de Azure-infrastructuur overzicht | Microsoft Docs
description: Meer informatie over de sleutel ontwerpen en implementeren van de richtlijnen voor het implementeren van een voorbeeld van de infrastructuur in Azure.
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c532657951d6d0241a5d8d25a56bb237ad481567
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a>Voorbeeld van de Azure-infrastructuur overzicht voor het Windows-VM 's
In dit artikel wordt uitgelegd opzetten van een voorbeeld van de toepassing-infrastructuur. Een infrastructuur voor een eenvoudige onlinewinkel die de richtlijnen en beslissingen rond naamconventies, beschikbaarheidssets, virtuele netwerken en taakverdelers samenbrengt ontwerpen en distribueren van uw virtuele machines (VM's) detail te beschrijven.

## <a name="example-workload"></a>Voorbeeld van de werkbelasting
Adventure Works Cycles wil maken van een online store-toepassing in Azure die bestaat uit:

* Twee IIS-servers met de in de weblaag van een-front-client
* Twee IIS-servers verwerken van gegevens en bestellingen in de toepassingslaag van een
* Twee exemplaren van Microsoft SQL Server met AlwaysOn availability groups (twee SQL-Servers en een meerderheid knooppunt witness) voor het opslaan van productgegevens- en bestellingen in een databaselaag
* Twee Active Directory-domeincontrollers voor klantaccounts en leveranciers in een verificatie-laag
* Alle servers bevinden zich in twee subnetten:
  * een front-end-subnet voor de webservers 
  * een back-end-subnet voor de toepassingsservers, de SQL-cluster en de domeincontrollers

![Diagram van verschillende lagen voor de infrastructuur van de toepassing](./media/infrastructure-example/example-tiers.png)

Binnenkomende beveiligde webverkeer moet worden verdeeld over de webservers zoals klanten de online winkel bladeren. Volgorde voor het verwerken van verkeer in de vorm van HTTP-aanvragen via het web servers moeten worden verdeeld over de toepassingsservers. Bovendien moet de infrastructuur zijn ontworpen voor hoge beschikbaarheid.

Het ontwerp van de resulterende moet gebruikmaken van:

* Een Azure-abonnement en -account
* Één resourcegroep
* Azure Managed Disks
* Een virtueel netwerk met twee subnetten
* Beschikbaarheidssets voor de virtuele machines met een vergelijkbare rol
* Virtuele machines

Alle bovenstaande voldoen aan deze naamgeving:

* Adventure Works Cycles gebruikt **[IT werkbelasting]-[locatie]-[Azure resource]** als voorvoegsel
  * Bijvoorbeeld, "**azos**' (Azure Online Store) is de naam van de werkbelasting IT en '**gebruiken**' (VS-Oost 2) is de locatie
* Virtuele netwerken maken gebruik van AZOS-gebruik-VN**[aantal]**
* Beschikbaarheidssets gebruiken azos-gebruiken-als-**[rol]**
* Namen van de virtuele machine gebruiken azos-gebruiken-vm -**[vmname]**

## <a name="azure-subscriptions-and-accounts"></a>Azure-abonnementen en accounts
Adventure Works Cycles gebruikt om de facturering van deze workload IT hun Enterprise-abonnement, met de naam Adventure Works Enterprise-abonnement.

## <a name="storage"></a>Storage
Adventure Works Cycles bepaald dat ze beheerd Azure-schijven moeten gebruiken. Bij het maken van virtuele machines worden beide lagen beschikbare opslag worden gebruikt:

* **Standard-opslag** voor de webservers, toepassingsservers, en domeincontrollers en hun gegevensschijven.
* **Premium-opslag** voor de VM's van SQL Server en hun gegevensschijven.

## <a name="virtual-network-and-subnets"></a>Virtueel netwerk en subnetten
Omdat het virtuele netwerk heeft geen actieve verbinding met de Adventure werk cycli on-premises netwerk nodig, worden ze besloten op een virtueel netwerk alleen in de cloud.

Ze een virtueel netwerk alleen in de cloud gemaakt met de volgende instellingen met behulp van de Azure-portal:

* Name: AZOS-USE-VN01
* Locatie: VS-Oost 2
* Adresruimte virtuele netwerk: 10.0.0.0/8
* Eerste subnet:
  * Naam: FrontEnd
  * Adresruimte: 10.0.1.0/24
* Tweede subnet:
  * Naam: back-end
  * Adresruimte: 10.0.2.0/24

## <a name="availability-sets"></a>Beschikbaarheidssets
Om te blijven van hoge beschikbaarheid van alle vier lagen van de online winkel, besloten Adventure Works Cycles op vier beschikbaarheidssets:

* **azos gebruik als web** voor de webservers
* **azos gebruik als app** voor de toepassingsservers
* **azos gebruiken als sql** voor de SQL-Servers
* **azos gebruik als dc** voor de domeincontrollers

## <a name="virtual-machines"></a>Virtuele machines
Adventure Works Cycles besloten op de volgende namen voor hun virtuele Azure-machines:

* **azos, gebruik, vm, web01** voor de eerste webserver
* **azos, gebruik, vm, web02** voor de tweede webserver
* **azos, gebruik, vm, app01** voor de eerste toepassingsserver
* **azos, gebruik, vm, app02** voor de tweede toepassingsserver
* **azos, gebruik, vm, sql01** voor de eerste SQL Server-server in het cluster
* **azos, gebruik, vm, sql02** voor de tweede SQL Server-server in het cluster
* **azos, gebruik, vm, dc01** voor de eerste domeincontroller
* **azos, gebruik, vm, dc02** voor de tweede domeincontroller

Hier is de resulterende configuratie.

![Laatste infrastructuur geïmplementeerd in Azure](./media/infrastructure-example/example-config.png)

Deze configuratie omvat:

* Een virtueel netwerk met twee subnetten (FrontEnd en BackEnd) alleen in de cloud
* Azure-beheerde schijven met standaard- en Premium-schijven
* Vier beschikbaarheidssets, één voor elke laag van de online winkel
* De virtuele machines voor de vier lagen
* Een externe set met gelijke taakverdeling voor HTTPS-gebaseerde internetverkeer van Internet naar de webservers
* Een interne set voor niet-versleuteld webverkeer van de webservers naar de toepassingsservers met gelijke taakverdeling
* Één resourcegroep
