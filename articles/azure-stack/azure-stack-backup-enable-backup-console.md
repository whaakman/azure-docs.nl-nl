---
title: Back-up voor Azure-Stack inschakelen via de beheerportal | Microsoft Docs
description: De infrastructuur terug Service inschakelen via de beheerportal zodat Azure-Stack kan worden hersteld als er een storing optreedt.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: a5a9757d871c343ba663862de7b6d75b9dd21c31
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Back-up voor Azure-Stack inschakelen via de beheerportal

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

De infrastructuur terug Service inschakelen via de beheerportal zodat Azure Stack back-ups kunnen genereren. U kunt deze gebruiken maakt een back-ups voor het herstellen van uw omgeving, als er een storing optreedt.

## <a name="enable-backup"></a>Back-up inschakelen

1. Open de Stack van Azure-beheerportal op [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Selecteer **meer services** > **infrastructuur back-up**. Kies **configuratie** in de **infrastructuur back-up** blade.

    ![Azure Stack - instellingen voor back-up-domeincontrollers](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Typ het pad naar de **opslaglocatie back-up**. U moet een Universal Naming Convention (UNC)-tekenreeks voor het pad naar een bestandsshare die worden gehost op een afzonderlijk apparaat gebruiken. Een UNC-tekenreeks bevat de locatie van resources, zoals gedeelde bestanden of apparaten. Voor de service, kunt u een IP-adres. Beschikbaarheid van de back-upgegevens in het geval van een noodgeval, zodat moet het apparaat op een andere locatie.
    > [!Note]  
    > U kunt een FQDN-naam in plaats van het IP-adres gebruiken als uw omgeving ondersteuning biedt voor naamomzetting van het infrastructuurnetwerk Azure Stack uw bedrijfsomgeving.
4. Typ de **gebruikersnaam** met behulp van het domein en gebruikersnaam. Bijvoorbeeld `Contoso\administrator`.
5. Typ de **wachtwoord** voor de gebruiker.
5. Typ het wachtwoord opnieuw **wachtwoord bevestigen**.
6. Een vooraf gedeelde sleutel opgeven in de **versleutelingssleutel** vak. Back-upbestanden zijn versleuteld met behulp van deze sleutel. Zorg ervoor dat deze sleutel opslaan op een veilige locatie. Zodra u deze sleutel voor het eerst definieert of de sleutel in de toekomst draaien, kunt u deze sleutel van deze interface niet weergeven. Voor meer instructies voor het genereren van een vooraf gedeelde sleutel, volgt u de scripts op [back-up inschakelen voor Azure-Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Selecteer **OK** uw back-controller-instellingen op te slaan.

Voor het uitvoeren van een back-up, moet u de hulpprogramma's van Azure Stack downloaden en voer de PowerShell-cmdlet **Start AzSBackup** in uw Azure-Stack administration-knooppunt. Zie voor meer informatie [Back-up van Azure-Stack](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Volgende stappen

 - Informatie over het uitvoeren van een back-up, Zie [Back-up van Azure-Stack](azure-stack-backup-back-up-azure-stack.md ).
- Informatie over het controleren of de back-up is uitgevoerd, Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md ).
