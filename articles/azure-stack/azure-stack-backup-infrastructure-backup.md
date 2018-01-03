---
title: Back-up en gegevensherstel voor Azure-Stack met de infrastructuur Backup-Service | Microsoft Docs
description: U kunt back-up en herstellen van de configuratie en servicegegevens met behulp van de infrastructuur Backup-Service.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9B51A3FB-EEFC-4CD8-84A8-38C52CFAD2E4
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 84ce0d72ff826ecb3f5deff165db00a1e50ae89d
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="backup-and-data-recovery-for-azure-stack-with-the-infrastructure-backup-service"></a>Back-up en gegevensherstel voor Azure-Stack met de infrastructuur Backup-Service

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt back-up en herstellen van de configuratie en servicegegevens met behulp van de infrastructuur Backup-Service. Elke installatie van een Azure-Stack bevat een exemplaar van de service. U kunt back-ups gemaakt door de service voor het opnieuw distribueren van de Azure-Cloud-Stack identiteit, beveiliging en Azure Resource Manager-gegevens herstellen.

U kunt back-up inschakelen wanneer u klaar bent voor uw cloud in productie te plaatsen. Schakel geen back-up als u van plan bent om te testen en valideren voor een lange periode.

Voordat u uw back-service inschakelt, zorg ervoor dat u hebt [vereisten voldaan](#verify-requirements-for-the-infrastructure-backup-service).

> [!Note]  
> De infrastructuur Backup-Service bevat geen gebruikersgegevens en -toepassingen. Zie de volgende artikelen voor meer informatie over back-ups en terugzetten [App Services](https://aka.ms/azure-stack-app-service), [SQL](https://aka.ms/azure-stack-ms-sql), en [MySQL](https://aka.ms/azure-stack-mysql) resourceproviders en de bijbehorende gebruikersgegevens...

## <a name="the-infrastructure-backup-service"></a>De Backup-Service van de infrastructuur

De services bevat de volgende functies.

| Functie                                            | Beschrijving                                                                                                                                                |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Back-upinfrastructuur Services                     | Coördinaat back-up in een subset van infrastructuurservices in Azure-Stack. Als er een ramp optreedt, kunnen de gegevens worden hersteld als onderdeel van opnieuw distribueren. |
| Compressie en codering van de geëxporteerde gegevens van de back-up | Back-upgegevens wordt gecomprimeerd en versleuteld door het systeem voordat deze wordt geëxporteerd naar de locatie van de externe opslag verstrekt door de beheerder.                |
| Back-uptaak bewaking                              | Systeem waarschuwt als back-up mislukken en het doorvoeren stappen taken.                                                                                                |
| Back-beheerervaring                       | Back-up RP ondersteunt inschakelen back-up.                                                                                                                         |
| Herstel van de cloud                                     | Als er een onherstelbaar gegevensverlies, kunnen back-ups herstellen van de belangrijkste Azure Stack informatie als onderdeel van de implementatie worden gebruikt.                                 |

## <a name="verify-requirements-for-the-infrastructure-backup-service"></a>Vereisten voor de infrastructuur Backup-Service controleren

- **Locatie voor de opslag**  
  U moet een bestandsshare toegankelijk is vanaf de Azure-Stack die zeven back-ups kunnen bevatten. Elke back-up is ongeveer 10 GB. De share moet kunnen worden opgeslagen 140 GB van back-ups. Zie voor meer informatie over het selecteren van een opslaglocatie voor de back-up-Service van Azure Stack infrastructuur [vereisten voor de back-up domeincontroller](azure-stack-backup-reference.md#backup-controller-requirements).
- **Referenties**  
  U moet een domeingebruikersaccount en referenties, bijvoorbeeld: u kunt de Azure-Stack-beheerdersreferenties.
- **Coderingssleutel**  
  Back-upbestanden zijn versleuteld met behulp van deze sleutel. Zorg ervoor dat deze sleutel opslaan op een veilige locatie. Zodra u deze sleutel voor het eerst definieert of de sleutel in de toekomst draaien, kunt u deze sleutel van deze interface niet weergeven. Voor meer instructies voor het genereren van een vooraf gedeelde sleutel, volgt u de scripts op [back-up inschakelen voor Azure-Stack met PowerShell](http://azure-stack-backup-enable-backup-powershell.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [back-up inschakelen voor Azure-Stack van de beheerportal](azure-stack-backup-enable-backup-console.md).
- Meer informatie over hoe [back-up inschakelen voor Azure Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).
- Meer informatie over hoe [Back-up van Azure-Stack](azure-stack-backup-back-up-azure-stack.md )
- Meer informatie over hoe [onherstelbaar gegevensverlies herstellen](azure-stack-backup-recover-data.md)
