---
title: Back-up en gegevensherstel voor Azure Stack met de infrastructuur voor Backup-Service | Microsoft Docs
description: U kunt back-up en herstellen van configuratie- en service-gegevens met behulp van de infrastructuur voor Backup-Service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
mss.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: mabrigg
ms.reviewer: hectorl
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: 607356bb59550626c9fe5f71d4609b9c3e2b2fe0
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251296"
---
# <a name="backup-and-data-recovery-for-azure-stack-with-the-infrastructure-backup-service"></a>Back-up en gegevensherstel voor Azure Stack met de infrastructuur voor Backup-Service

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt back-up en herstellen van configuratie- en service-gegevens met behulp van de infrastructuur voor Backup-Service. Elke Azure Stack-installatie bevat een exemplaar van de service. U kunt back-ups die zijn gemaakt door de service voor het opnieuw distribueren van de Azure Stack-Cloud gebruiken om terug te zetten van identiteit, beveiliging en Azure Resource Manager-gegevens.

U kunt back-up inschakelen wanneer u klaar bent voor uw cloud in productie te plaatsen. Schakel geen back-up als u van plan bent om toepassingen te testen en valideren voor een lange periode.

Voordat u uw back-upservice inschakelt, zorg ervoor dat u hebt [vereisten voldaan](#verify-requirements-for-the-infrastructure-backup-service).

> [!Note]  
> De infrastructuur voor Backup-Service bevat geen gebruikersgegevens en -toepassingen. <!-- See the following articles for instructions on backing up and restore [App Services](https://aka.ms/azure-stack-app-service), [SQL](https://aka.ms/azure-stack-ms-sql), and [MySQL](https://aka.ms/azure-stack-mysql) resource providers and associated user data. -->

## <a name="the-infrastructure-backup-service"></a>De back-upservice voor infrastructuur

De services bevatten de volgende functies.

| Functie                                            | Beschrijving                                                                                                                                                |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Een back-upinfrastructuur Services                     | Coördinatie back-up van een subset van infrastructuurservices in Azure Stack. Als er zich een noodgeval voordoet, kunnen de gegevens worden hersteld als onderdeel van opnieuw implementeren. |
| Compressie en codering van de geëxporteerde gegevens van de back-up | Back-gegevens worden gecomprimeerd en versleuteld door het systeem voordat deze wordt geëxporteerd naar de locatie van de externe opslag die door de beheerder.                |
| Back-uptaak controleren                              | Systeem waarschuwt als back-up mislukken en het doorvoeren stappen taken.                                                                                                |
| Back-upbeheer ervaring                       | Back-RP biedt ondersteuning voor back-up van inschakelen.                                                                                                                         |
| Cloud Recovery                                     | Als er een onherstelbaar gegevensverlies, kunnen de back-ups worden gebruikt om te herstellen core Azure Stack informatie als onderdeel van de implementatie.                                 |

## <a name="verify-requirements-for-the-infrastructure-backup-service"></a>Vereisten voor de infrastructuur voor Backup-Service controleren

- **Opslaglocatie**  
  U moet een bestandsshare toegankelijk is vanaf de Azure-Stack die zeven back-ups kan bevatten. Elke back-up is ongeveer 10 GB. Uw bestandsshare zou het mogelijk om op te slaan 140 GB aan back-ups. Zie voor meer informatie over het selecteren van een opslaglocatie voor de Backup-Service van Azure Stack-infrastructuur [vereisten voor de back-up domeincontroller](azure-stack-backup-reference.md#backup-controller-requirements).
- **Referenties**  
  U moet een domeingebruikersaccount en referenties, bijvoorbeeld, u kunt de Azure Stack-beheerdersreferenties.
- **Coderingssleutel**  
  Back-upbestanden zijn versleuteld met behulp van deze sleutel. Zorg ervoor dat u deze sleutel opslaan op een veilige locatie. Nadat u deze sleutel voor de eerste keer instellen of de sleutel in de toekomst draaien, kunt u deze sleutel uit deze interface niet weergeven. Voor meer instructies voor het genereren van een vooraf gedeelde sleutel, volgt u de scripts op [back-up inschakelen voor Azure Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [back-up inschakelen voor Azure Stack vanuit de beheerportal](azure-stack-backup-enable-backup-console.md).
- Meer informatie over het [back-up inschakelen voor Azure Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).
- Leer hoe u [maakt u een Back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md )
- Meer informatie over het [herstel na onherstelbare gegevensverlies](azure-stack-backup-recover-data.md)
