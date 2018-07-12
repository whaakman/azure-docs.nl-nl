---
title: Back-up inschakelen voor Azure Stack vanuit de beheerportal | Microsoft Docs
description: Schakel de infrastructuur voor Backup-Service via de beheerportal zodat Azure Stack kan worden hersteld als er een storing optreedt.
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
ms.date: 07/11/2018
ms.author: jeffgilb
ms.openlocfilehash: fba04490aca4c7123ca478ae07a5f0c865d9a826
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968694"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Back-up inschakelen voor Azure Stack vanuit de beheerportal
Schakel de infrastructuur voor Backup-Service via de beheerportal zodat Azure Stack-back-ups kunt genereren. U kunt deze back-ups gebruiken om te herstellen van uw omgeving met behulp van de cloudherstel in geval van [een onherstelbare fout](.\azure-stack-backup-recover-data.md). Het doel van cloudherstel is om ervoor te zorgen dat uw operators en gebruikers zich weer in de portal aanmelden kunnen nadat het herstel is voltooid. Gebruikers hebben hun abonnementen hersteld met inbegrip van machtigingen voor toegang op basis van rollen en functies, oorspronkelijke plannen, aanbiedingen, en eerder gedefinieerde compute, opslag en netwerkquota.

Echter, de infrastructuur voor Backup-Service heeft geen back-up IaaS-VM's, netwerkconfiguraties en opslagresources, zoals storage-accounts, -blobs, tabellen en enzovoort, zodat gebruikers die zich aanmelden na het cloudherstel is voltooid niet een van de eerder bestaande weergegeven resources. Platform as a Service (PaaS)-resources en de gegevens worden ook niet ondersteund door de service. 

Beheerders en gebruikers zijn die verantwoordelijk is voor back-up en herstellen van IaaS en PaaS-resources afzonderlijk van de infrastructuur voor back-processen. Zie de volgende koppelingen voor meer informatie over back-ups van IaaS en PaaS-resources:

- [Virtuele machines](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


> [!Note]  
> Voordat u de back-up via de console inschakelt, moet u de Backup-service configureren. U kunt de Backup-service met behulp van PowerShell configureren. Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Back-up inschakelen

1. Open de Azure Stack-beheerportal bij [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Selecteer **meer services** > **infrastructuur back-up**. Kies **configuratie** in de **infrastructuur back-up** blade.

    ![Azure Stack - instellingen voor back-up-controller](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Typ het pad naar de **back-up van opslaglocatie**. Een Universal Naming Convention (UNC)-tekenreeks voor het pad naar een bestandsshare die wordt gehost op een afzonderlijk apparaat gebruiken. Een UNC-tekenreeks geeft de locatie van bronnen zoals gedeelde bestanden of apparaten. Voor de service, kunt u een IP-adres. Voor beschikbaarheid van de back-upgegevens na een noodgeval, moet het apparaat zich in een andere locatie.
    > [!Note]  
    > Als uw omgeving biedt ondersteuning voor naamomzetting van het netwerk voor Azure Stack-infrastructuur naar de omgeving van uw bedrijf, kunt u een FQDN-naam in plaats van het IP-adres gebruiken.
4. Type de **gebruikersnaam** met behulp van het domein en gebruikersnaam met voldoende toegangsrechten voor bestanden lezen en schrijven. Bijvoorbeeld `Contoso\backupshareuser`.
5. Type de **wachtwoord** voor de gebruiker.
5. Typ het wachtwoord opnieuw **wachtwoord bevestigen**.
6. Geef een vooraf gedeelde sleutel in de **versleutelingssleutel** vak. Back-upbestanden zijn versleuteld met behulp van deze sleutel. Zorg ervoor dat u deze sleutel opslaan op een veilige locatie. Nadat u deze sleutel voor de eerste keer instellen of de sleutel in de toekomst draaien, kunt u deze sleutel uit deze interface niet weergeven. Voor meer instructies voor het genereren van een vooraf gedeelde sleutel, volgt u de scripts op [back-up inschakelen voor Azure Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).
7. Selecteer **OK** uw back-controller-instellingen op te slaan.

Voor het uitvoeren van een back-up, moet u de Azure Stack-hulpprogramma's downloaden en voer de PowerShell-cmdlet **Start AzSBackup** op het knooppunt voor het beheer van Azure Stack. Zie voor meer informatie, [maakt u een Back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u een back-up uitvoeren. Zie [maakt u een Back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Meer informatie om te controleren of uw back-up wordt uitgevoerd. Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md).
