---
title: Back-up voor Azure-Stack inschakelen via de beheerportal | Microsoft Docs
description: De infrastructuur Backup-Service via de beheerportal ingeschakeld zodat Azure-Stack kan worden hersteld als er een storing optreedt.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: jeffgilb
ms.openlocfilehash: 0ef8247eba4605d3c8e5ef0992ce97bce989002e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Back-up voor Azure-Stack inschakelen via de beheerportal
De infrastructuur Backup-Service via de beheerportal ingeschakeld zodat Azure Stack back-ups kunnen genereren. U kunt deze back-ups herstellen van uw omgeving met behulp van cloud-herstel in geval van [een onherstelbare fout](.\azure-stack-backup-recover-data.md). Het doel van herstel van de cloud is om ervoor te zorgen dat uw operators en gebruikers zich in de portal aanmelden kunnen nadat het herstel is voltooid. Gebruikers hebben hun abonnementen met inbegrip van machtigingen voor toegang op basis van rollen en functies, oorspronkelijke plannen, aanbiedingen, en vooraf gedefinieerde compute, storage en netwerkquota worden hersteld.

Echter, de Backup-Service-infrastructuur heeft geen back-up IaaS VM's, netwerkconfiguraties en storage-resources zoals storage-accounts, blobs, tabellen en enzovoort, zodat gebruikers aanmelden na het herstel van de cloud is voltooid, zien geen van hun eerder bestaande bronnen. Platform als een Service (PaaS) resources en gegevens worden ook niet ondersteund door de service. 

Beheerders en gebruikers zijn verantwoordelijk voor een back-up en herstellen van IaaS en PaaS-resources afzonderlijk van de infrastructuur back-processen. Zie de volgende koppelingen voor meer informatie over back-ups van IaaS en PaaS-resources:

- [Virtuele machines](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


> [!Note]  
> Voordat u de back-up via de console inschakelt, moet u voor het configureren van de Backup-service. U kunt de Backup-service met behulp van PowerShell configureren. Zie voor meer informatie [back-up inschakelen voor Azure-Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Back-up inschakelen

1. Open de Stack van Azure-beheerportal op [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
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
- Informatie over het controleren of de back-up is uitgevoerd. Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md).
