---
title: Data-entiteiten | Microsoft Docs
description: Een overzicht van data-entiteiten.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 579856ab87aaf8d051f2e3c161bb2d0e2f693ed5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54446575"
---
# <a name="data-entities"></a>Data-entiteiten

In dit artikel worden gedefinieerd en biedt een overzicht van data-entiteiten. Het bevat informatie over de mogelijkheden van data-entiteiten, de scenario's die worden ondersteund, de categorieën die worden gebruikt voor, en de methoden voor het maken van deze.

## <a name="overview"></a>Overzicht

Een gegevensentiteit is een abstractie van de fysieke implementatie van databasetabellen. Bijvoorbeeld, in genormaliseerde tabellen veel van de gegevens voor elke klant kan worden opgeslagen in een tabel van de klant en vervolgens de rest kan zijn verdeeld over een kleine set van gerelateerde tabellen. In dit geval de gegevensentiteit voor de klant-concept wordt weergegeven als één denormalisatie weergave, waarin elke rij de gegevens uit de tabel van de klant en de gerelateerde tabellen bevat. Een gegevensentiteit ingekapseld het bedrijfsconcept van een naar een indeling die ontwikkelings- en integratie gemakkelijker maakt. De abstracte aard van een gegevensentiteit kunt vereenvoudigen de ontwikkeling van toepassingen en aanpassing. De abstrahering schermt later ook toepassingscode van het onvermijdelijk verloop van de fysieke tabellen tussen versies.

Samenvatting: Gegevensentiteit biedt conceptuele abstractie en inkapseling (de gedenormaliseerde weergeven) van de onderliggende tabel schema's om weer te geven van belangrijke gegevens concepten en functies.

## <a name="capabilities"></a>Functionaliteit

Een gegevensentiteit heeft de volgende mogelijkheden:

- Azure Backup vervangt u uiteenlopende en gefragmenteerde concepten Axd, gegevens importeren/exporteren Framework (DIXF)-entiteiten en statistische query's met één concept.
- Het biedt een single-stack om vast te leggen van de bedrijfslogica en voor het inschakelen van scenario's zoals importeren/exporteren, integratie en programmeren.
- Wordt het primaire mechanisme voor exporteren en importeren van gegevenspakketten voor Application Lifecycle Management (ALM) en demo's van gegevens.
- Worden kan weergegeven als OData-services, en vervolgens worden gebruikt in tabelvorm-stijl synchrone integratiescenario's en Microsoft Office-integraties.

Zie [gegevensentiteiten](https://docs.microsoft.com/dynamics365/operations/dev-itpro/data-entities/data-entities) voor meer informatie.
