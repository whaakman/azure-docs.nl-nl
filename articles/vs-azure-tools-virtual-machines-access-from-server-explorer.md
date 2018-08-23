---
title: Toegang tot virtuele Machines van Azure vanuit Server Explorer | Microsoft Docs
description: Krijg een overzicht van hoe u kunt weergeven, maken en beheren van virtuele Azure-machines (VM's) in Server Explorer in Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 8/31/2017
ms.author: ghogen
ms.openlocfilehash: 81a0e2923ddbb6960066f01d6365e8c9278defac
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056682"
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Toegang tot virtuele Machines van Azure vanuit Server Explorer

Als u virtuele machines die worden gehost op Azure hebt, kunt u ze in Server Explorer openen. U moet eerst aanmelden bij uw Azure-abonnement om uw mobiele services weer te geven. Als u wilt aanmelden, opent u het snelmenu voor het Azure-knooppunt in Server Explorer en kies **verbinding maken met Microsoft Azure**.

1. In Cloud Explorer, kiest u een virtuele machine en kies vervolgens de sleutel F4 om het eigenschappenvenster weer te geven.

    De volgende tabel ziet u welke eigenschappen beschikbaar zijn, maar ze zijn alle alleen-lezen. Om deze te wijzigen, gebruikt u de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

   | Eigenschap | Beschrijving |
   | --- | --- |
   | DNS-naam |De URL met het internetadres van de virtuele machine. |
   | Omgeving |De waarde van deze eigenschap is voor een virtuele machine altijd productie. |
   | Naam |De naam van de virtuele machine. |
   | Grootte |De grootte van de virtuele machine, dat overeenkomt met de hoeveelheid geheugen en schijfruimte die beschikbaar is. Zie voor meer informatie, [grootten van virtuele machines](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs). |
   | Status |Waarden zijn gestart, gestart, moet worden gestopt, gestopt en bij het ophalen van de Status. Als het ophalen van de Status wordt weergegeven, is de huidige status is onbekend. De waarden voor deze eigenschap afwijken van de waarden die worden gebruikt op de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). |
   | abonnements-id |De abonnements-ID voor uw Azure-account. U kunt deze informatie weergeven op de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) door de eigenschappen voor een abonnement weer te geven. |
2. Kies een eindpunt-knooppunt en bekijk vervolgens de **eigenschappen** venster.
3. De volgende tabel beschrijft de beschikbare eigenschappen van eindpunten, maar ze zijn alleen-lezen. Als u wilt toevoegen of bewerken van de eindpunten voor een virtuele machine, gebruikt u de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). 

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Naam |Een id voor het eindpunt. |
   | Particuliere poort |De poort voor netwerktoegang voor intern gebruik binnen uw toepassing. |
   | Protocol |Het protocol dat de transportlaag is opgehaald voor dit eindpunt wordt gebruikt, TCP of UDP. |
   | Openbare poort |De poort die wordt gebruikt voor openbare toegang tot uw toepassing. |
