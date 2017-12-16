---
title: Voorbeeld van de Azure-infrastructuur overzicht | Microsoft Docs
description: Meer informatie over de sleutel ontwerpen en implementeren van de richtlijnen voor het implementeren van een voorbeeld van de infrastructuur in Azure.
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 281fc2c0-b533-45fa-81a3-728c0049c73d
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ae7df08e7502fbfd500944f89a3fa6ee4806522a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-linux-vms"></a>Voorbeeld van de Azure-infrastructuur overzicht voor virtuele Linux-machines
In dit artikel wordt uitgelegd opzetten van een voorbeeld van de toepassing-infrastructuur. We beschreven ontwerpen van een infrastructuur voor een eenvoudige online winkel die de richtlijnen en beslissingen rond naamconventies, beschikbaarheidssets, virtuele netwerken en taakverdelers verenigt en distribueren van uw virtuele machines (VM's).

## <a name="example-workload"></a>Voorbeeld van de werkbelasting
Adventure Works Cycles wil maken van een online store-toepassing in Azure die bestaat uit:

* Twee nginx-servers uitvoeren van de client in de weblaag van een-front-
* Twee nginx-servers verwerken van gegevens en bestellingen in de toepassingslaag van een
* Twee servers van MongoDB-onderdeel van een shard cluster voor het opslaan van productgegevens- en bestellingen in een databaselaag
* Twee Active Directory-domeincontrollers voor klantaccounts en leveranciers in een verificatie-laag
* Alle servers bevinden zich in twee subnetten:
  * een front-end-subnet voor de webservers 
  * een back-end-subnet voor de toepassingsservers, de MongoDB-cluster en de domeincontrollers

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
Adventure Works Cycles bepaald dat ze beheerd Azure-schijven moeten gebruiken. Bij het maken van virtuele machines worden beide beschikbare opslag opslaglagen gebruikt:

* **Standard-opslag** voor de webservers, toepassingsservers, en domeincontrollers en hun gegevensschijven.
* **Premium-opslag** voor de shard-clusterservers van MongoDB en hun gegevensschijven.

## <a name="virtual-network-and-subnets"></a>Virtueel netwerk en subnetten
Omdat het virtuele netwerk heeft geen actieve verbinding met de Adventure werk cycli on-premises netwerk nodig, worden ze besloten op een virtueel netwerk alleen in de cloud.

Ze een virtueel netwerk alleen in de cloud gemaakt met de volgende instellingen met behulp van de Azure-portal:

* Naam: AZOS gebruik VN01
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
* **azos gebruik als db** voor de servers in de shard MongoDB-cluster
* **azos gebruik als dc** voor de domeincontrollers

## <a name="virtual-machines"></a>Virtuele machines
Adventure Works Cycles besloten op de volgende namen voor hun virtuele Azure-machines:

* **azos, gebruik, vm, web01** voor de eerste webserver
* **azos, gebruik, vm, web02** voor de tweede webserver
* **azos, gebruik, vm, app01** voor de eerste toepassingsserver
* **azos, gebruik, vm, app02** voor de tweede toepassingsserver
* **azos, gebruik, vm, db01** voor de eerste MongoDB-server in het cluster
* **azos, gebruik, vm, db02** voor de tweede MongoDB-server in het cluster
* **azos, gebruik, vm, dc01** voor de eerste domeincontroller
* **azos, gebruik, vm, dc02** voor de tweede domeincontroller

Hier is de resulterende configuratie.

![Laatste infrastructuur geïmplementeerd in Azure](./media/infrastructure-example/example-config.png)

Deze configuratie omvat:

* Een virtueel netwerk met twee subnetten (FrontEnd en BackEnd) alleen in de cloud
* Azure-beheerde schijven die met behulp van standaard- en Premium-schijven
* Vier beschikbaarheidssets, één voor elke laag van de online winkel
* De virtuele machines voor de vier lagen
* Een externe set met gelijke taakverdeling voor HTTPS-gebaseerde internetverkeer van Internet naar de webservers
* Een interne set voor niet-versleuteld webverkeer van de webservers naar de toepassingsservers met gelijke taakverdeling
* Één resourcegroep
