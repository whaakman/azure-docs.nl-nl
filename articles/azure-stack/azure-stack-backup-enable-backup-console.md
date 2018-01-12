---
title: Back-up voor Azure-Stack inschakelen via de beheerportal | Microsoft Docs
description: De infrastructuur Backup-Service via de beheerportal ingeschakeld zodat Azure-Stack kan worden hersteld als er een storing optreedt.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 456a0db9771f5963c8d4375d54a22257f6ca1c56
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Back-up voor Azure-Stack inschakelen via de beheerportal

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

De infrastructuur Backup-Service via de beheerportal ingeschakeld zodat Azure Stack back-ups kunnen genereren. Deze back-ups kunt u uw omgeving bij een storing te herstellen.

> [!Note]  
> Voordat u de back-up via de console inschakelt, moet u voor het configureren van de Backup-service. U kunt de Backup-service met behulp van PowerShell configureren. Zie voor meer informatie [back-up inschakelen voor Azure-Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Back-up inschakelen

1. Open de Stack van Azure-beheerportal op [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Selecteer **meer services** > **infrastructuur back-up**. Kies **configuratie** in de **infrastructuur back-up** blade.

    ![Azure Stack - instellingen voor back-up-domeincontrollers](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Typ het pad naar de **opslaglocatie back-up**. Gebruik een Universal Naming Convention (UNC)-tekenreeks voor het pad naar een bestandsshare die wordt gehost op een afzonderlijk apparaat. Een UNC-tekenreeks bevat de locatie van resources, zoals gedeelde bestanden of apparaten. Voor de service, kunt u een IP-adres. Beschikbaarheid van de back-upgegevens na een noodgeval, zodat moet het apparaat op een andere locatie.
    > [!Note]  
    > Als uw omgeving naamomzetting vanaf het netwerk van Azure-Stack-infrastructuur voor uw omgeving enterprise ondersteunt, kunt u een FQDN-naam in plaats van het IP-adres.
4. Typ de **gebruikersnaam** met behulp van het domein en gebruikersnaam. Bijvoorbeeld `Contoso\administrator`.
5. Typ de **wachtwoord** voor de gebruiker.
5. Typ het wachtwoord opnieuw **wachtwoord bevestigen**.
6. Een vooraf gedeelde sleutel opgeven in de **versleutelingssleutel** vak. Back-upbestanden zijn versleuteld met behulp van deze sleutel. Zorg ervoor dat deze sleutel opslaan op een veilige locatie. Zodra u deze sleutel voor het eerst definieert of de sleutel in de toekomst draaien, kunt u deze sleutel van deze interface niet weergeven. Voor meer instructies voor het genereren van een vooraf gedeelde sleutel, volgt u de scripts op [back-up inschakelen voor Azure-Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Selecteer **OK** uw back-controller-instellingen op te slaan.

Voor het uitvoeren van een back-up, moet u de hulpprogramma's van Azure Stack downloaden en voer de PowerShell-cmdlet **Start AzSBackup** in uw Azure-Stack administration-knooppunt. Zie voor meer informatie [Back-up van Azure-Stack](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Volgende stappen

 - Informatie over het uitvoeren van een back-up. Zie [Back-up van Azure-Stack](azure-stack-backup-back-up-azure-stack.md ).
- Informatie over het controleren of de back-up is uitgevoerd. Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md ).
