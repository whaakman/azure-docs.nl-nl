---
title: Fysiek geheugencapaciteit voor Azure-Stack beheren | Microsoft Docs
description: Bewaken en beheren van de beschikbare opslagruimte voor Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84518E90-75E1-4037-8D4E-497EAC72AAA1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/13/2018
ms.author: mabrigg
ms.reviewer: Thomas.Roettinger
ms.openlocfilehash: b922790d51c7028c37bb5863d43e99e19790488c
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="manage-physical-memory-capacity-for-azure-stack"></a>Fysiek geheugencapaciteit voor Azure-Stack beheren

*Van toepassing op: Azure Stack geïntegreerd systemen*

U kunt extra geheugen toevoegen voor een verhoging van de capaciteit van de totale beschikbare geheugen voor Azure-Stack. In Azure-Stack uw fysieke server ook aangeduid als een *scale unit knooppunt*. Alle knooppunten van de scale-eenheid die lid van een enkele schaaleenheid zijn moeten dezelfde hoeveelheid geheugen hebben.

> [!note]  
> Voordat u doorgaat, Raadpleeg de documentatie van uw hardware om te zien als een geheugen upgrades dat de fabrikant van uw biedt ondersteuning voor een fysieke geheugen. Uw OEM hardware leverancier support-contract kan vereisen dat de leverancier van de fysieke server rack plaatsing en de apparaat-firmware-update uitvoeren.

Het volgende stroomdiagram ziet u het algemene proces geheugen toevoegen aan elk knooppunt van de eenheid schaal.

![Geheugen in elk knooppunt van scale-eenheid toevoegen](media\azure-stack-manage-storage-physical-capacity\process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>Geheugen aan een bestaand knooppunt toevoegen
De volgende stappen bevatten een overzicht van het toevoegproces-geheugen. 

> [!Warning]  
Volg deze stappen niet zonder te verwijzen naar de OEM geleverde documentatie.

> [!Warning]  
De volledige schaaleenheid moet worden afgesloten als een rolling geheugenupgrade wordt niet ondersteund.

1. Stoppen van de Azure-Stack met behulp van de stappen beschreven in de [starten en stoppen Azure Stack](azure-stack-start-and-stop.md) artikel.
2. Upgrade van het geheugen op elke fysieke computer met behulp van de documentatie van de hardwarefabrikant van uw.
3. Starten van de Azure-Stack met behulp van de stappen in de [starten en stoppen Azure Stack](azure-stack-start-and-stop.md) artikel.

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over het beheren van storage-accounts in Azure-Stack om te zoeken, herstellen en vrijmaken opslagcapaciteit op basis van bedrijfsbehoeften, [storage-accounts in Azure-Stack beheren](azure-stack-manage-storage-accounts.md).
 - Zie voor meer informatie over de Azure-Stack cloud operator bewaakt en beheert de opslagcapaciteit van de Azure-Stack-implementatie, [beheren opslagcapaciteit voor Azure-Stack](azure-stack-manage-storage-shares.md). 