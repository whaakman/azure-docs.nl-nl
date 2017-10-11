---
title: Azure Traffic Manager-profielen beheren | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een Azure Traffic Manager-profiel maakt, uitschakelt, inschakelt en verwijdert.
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: kumud
ms.openlocfilehash: a5164282264124835692bc72a4ab61891aa7af9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-an-azure-traffic-manager-profile"></a>Een Azure Traffic Manager-profiel beheren

Traffic Manager-profielen gebruiken verkeersrouteringsmethoden voor het beheren van de distributie van verkeer naar uw cloudservices of de website-eindpunten. In dit artikel wordt uitgelegd hoe u deze profielen kunt maken en beheren.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

U kunt via Azure Portal een Traffic Manager-profiel maken. Zodra u het profiel hebt gemaakt, kunt u eindpunten configureren, bewaken en instellingen wijzigen in Azure Portal. Traffic Manager biedt ondersteuning voor maximaal 200 eindpunten per profiel. Voor de meeste gebruiksscenario's zijn echter slechts enkele eindpunten vereist.

### <a name="to-create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

1. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/). 
2. In het menu **Hub** klikt u op **Nieuw** > **Netwerken** > **Alles weergeven** en klikt u op het **Traffic Manager**-profiel om de blade **Traffic Manager-profiel** te openen. Klik vervolgens op **Maken**.
3. Op de blade **Traffic Manager-profiel maken** vult u het volgende in:
    1. In **Naam** geeft u een naam op voor het profiel. Deze naam moet uniek zijn binnen de zone trafficmanager.net. De naam wordt gebruikt voor de DNS-naam <name>, trafficmanager.net, die wordt gebruikt voor het openen van uw Traffic Manager-profiel.
    2. In **Routeringsmethode** selecteert u de routeringsmethode **Prioriteit**.
    3. In **Abonnement** selecteert u het abonnement waarvoor u dit profiel wilt maken
    4. In **Resourcegroep** maakt u een nieuwe resourcegroep om dit profiel voor te maken.
    5. In **Locatie van de resourcegroep** selecteert u de locatie van de resourcegroep. Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op het Traffic Manager-profiel dat wereldwijd wordt geïmplementeerd.
    6. Klik op **Create**.
    7. Wanneer de wereldwijde implementatie van uw Traffic Manager-profiel is voltooid, wordt het in de bijbehorende resourcegroep vermeld als één van de resources.

## <a name="disable-enable-or-delete-a-profile"></a>Een profiel uitschakelen, inschakelen of verwijderen

U kunt een bestaand profiel uitschakelen zodat Traffic Manager geen gebruikersaanvragen naar de geconfigureerde eindpunten verwijst. Wanneer u een Traffic Manager-profiel uitschakelt, blijven het profiel en de informatie in het profiel intact en kunt u het profiel en de inhoud bewerken in de Traffic Manager-interface.  Verwijzingen worden hervat wanneer u het profiel opnieuw inschakelt. Wanneer u in de Azure-portal een Traffic Manager-profiel maakt, wordt dit automatisch ingeschakeld. Als u een profiel niet meer nodig hebt, kunt u het verwijderen.

### <a name="to-disable-a-profile"></a>Een profiel uitschakelen

1. Als u een aangepaste domeinnaam gebruikt, wijzigt u de CNAME-record op de Internet-DNS-server zodanig dat deze niet langer naar uw Traffic Manager-profiel wijst.
2. Het verkeer wordt niet meer via de Traffic Manager-profielinstellingen omgeleid naar de eindpunten.
3. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com).
2. Zoek in de zoekbalk van de portal naar de naam van het **Traffic Manager-profiel** dat u wilt wijzigen, en klik vervolgens in de weergegeven resultaten op het Traffic Manager-profiel.
3. Op de blade **Traffic Manager-profiel** klikt u op **Overzicht**. Op de blade Overzicht klikt u op **Uitschakelen** en daarna bevestigt u om het Traffic Manager-profiel uit te schakelen.

### <a name="to-enable-a-profile"></a>Een profiel inschakelen

1. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com).
2. Zoek in de zoekbalk van de portal naar de naam van het **Traffic Manager-profiel** dat u wilt wijzigen, en klik vervolgens in de weergegeven resultaten op het Traffic Manager-profiel.
3. Op de blade **Traffic Manager-profiel** klikt u op **Overzicht**. Op de blade Overzicht klikt u vervolgens op **Inschakelen**.
5. Als u een aangepaste domeinnaam gebruikt, maakt u een CNAME-resourcerecord op de Internet-DNS-server zodat deze naar de domeinnaam van uw Traffic Manager-profiel wijst.
6. Het verkeer wordt weer omgeleid naar de eindpunten.

### <a name="to-delete-a-profile"></a>Een profiel verwijderen

1. Zorg ervoor dat de DNS-resourcerecord op uw Internet DNS-server geen CNAME-resourcerecord meer gebruikt die naar de domeinnaam van uw Traffic Manager-profiel wijst.
2. Zoek in de zoekbalk van de portal naar de naam van het **Traffic Manager-profiel** dat u wilt wijzigen, en klik vervolgens in de weergegeven resultaten op het Traffic Manager-profiel.
3. Op de blade **Traffic Manager-profiel** klikt u op **Overzicht**. Op de blade Overzicht klikt u op **Verwijderen** en daarna bevestigt u om het Traffic Manager-profiel te verwijderen.

## <a name="next-steps"></a>Volgende stappen

* [Een eindpunt toevoegen](traffic-manager-endpoints.md)
* [Routeringsmethode op basis van prioriteit configureren](traffic-manager-configure-priority-routing-method.md)
* [Routeringsmethode op basis van geografische locatie configureren](traffic-manager-configure-geographic-routing-method.md) 
* [Routeringsmethode op basis van gewogen distributie configureren](traffic-manager-configure-weighted-routing-method.md)
* [Routeringsmethode op basis van prestaties configureren](traffic-manager-configure-performance-routing-method.md)
