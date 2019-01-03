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
ms.date: 11/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: f5610f03cb613c7da2f89da1a38b6bf058a4e0cc
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53714403"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Back-up inschakelen voor Azure Stack vanuit de beheerportal
Schakel de infrastructuur voor Backup-Service via de beheerportal zodat Azure Stack-back-ups kunt genereren. U kunt deze back-ups gebruiken om te herstellen van uw omgeving met behulp van de cloudherstel in geval van [een onherstelbare fout](./azure-stack-backup-recover-data.md). Het doel van cloudherstel is om ervoor te zorgen dat uw operators en gebruikers zich weer in de portal aanmelden kunnen nadat het herstel is voltooid. Gebruikers hebben hun abonnementen hersteld met inbegrip van machtigingen voor toegang op basis van rollen en functies, oorspronkelijke plannen, aanbiedingen, en eerder gedefinieerde compute, opslag en netwerkquota.

Echter, de infrastructuur voor Backup-Service heeft geen back-up IaaS-VM's, netwerkconfiguraties en opslagresources, zoals storage-accounts, -blobs, tabellen en enzovoort, zodat gebruikers die zich aanmelden na het cloudherstel is voltooid niet een van de eerder bestaande weergegeven resources. Platform as a Service (PaaS)-resources en de gegevens worden ook niet ondersteund door de service. 

Beheerders en gebruikers zijn die verantwoordelijk is voor back-up en herstellen van IaaS en PaaS-resources afzonderlijk van de infrastructuur voor back-processen. Zie de volgende koppelingen voor meer informatie over back-ups van IaaS en PaaS-resources:

- [Virtuele machines](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/manage-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


## <a name="enable-or-reconfigure-backup"></a>In- of back-up configureren

1. Open de [Azure Stack-beheerportal](azure-stack-manage-portals.md).
2. Selecteer **alle services**, en klik vervolgens onder de **beheer** categorie selecteren **infrastructuur back-up**. Kies **configuratie** in de **infrastructuur back-up** blade.
3. Typ het pad naar de **back-up van opslaglocatie**. Een Universal Naming Convention (UNC)-tekenreeks voor het pad naar een bestandsshare die wordt gehost op een afzonderlijk apparaat gebruiken. Een UNC-tekenreeks geeft de locatie van bronnen zoals gedeelde bestanden of apparaten. Voor de service, kunt u een IP-adres. Voor beschikbaarheid van de back-upgegevens na een noodgeval, moet het apparaat zich in een andere locatie.

    > [!Note]  
    > Als uw omgeving biedt ondersteuning voor naamomzetting van het netwerk voor Azure Stack-infrastructuur naar de omgeving van uw bedrijf, kunt u een FQDN-naam in plaats van het IP-adres gebruiken.
    
4. Type de **gebruikersnaam** met behulp van het domein en gebruikersnaam met voldoende toegangsrechten voor bestanden lezen en schrijven. Bijvoorbeeld `Contoso\backupshareuser`.
5. Type de **wachtwoord** voor de gebruiker.
6. Typ het wachtwoord opnieuw **wachtwoord bevestigen**.
7. De **frequentie in uren** wordt bepaald hoe vaak back-ups worden gemaakt. De standaardwaarde is 12. Scheduler biedt ondersteuning voor maximaal 12 en een minimum van 4. 
8. De **bewaarperiode in dagen** bepaalt het aantal dagen van de back-ups, blijven behouden in de externe locatie. De standaardwaarde is 7. Scheduler biedt ondersteuning voor maximaal 14 en een minimum van 2. Back-ups ouder is dan de bewaarperiode worden automatisch verwijderd van de externe locatie.

    > [!Note]  
    > Als u wilt archiveren van back-ups ouder is dan de bewaarperiode liggen, zorg ervoor dat u de back-up van de bestanden voordat de scheduler wordt verwijderd als de back-ups. Als u de back-up bewaarperiode verkorten (bijvoorbeeld van 7 dagen tot 5 dagen), wordt de scheduler verwijderd ouder is dan de nieuwe bewaarperiode alle back-ups. Zorg ervoor dat u ok bent met de back-ups verwijderd voordat u deze waarde aanpassen. 

9. Geef een vooraf gedeelde sleutel in de **versleutelingssleutel** vak. Back-upbestanden zijn versleuteld met behulp van deze sleutel. Zorg ervoor dat u deze sleutel opslaan op een veilige locatie. Nadat u deze sleutel voor de eerste keer instellen of de sleutel in de toekomst draaien, kunt u de sleutel van deze interface niet weergeven. Voer de volgende PowerShell voor Azure Stack-opdrachten voor het maken van de sleutel:
    ```powershell
    New-AzsEncryptionKeyBase64
    ```
10. Selecteer **OK** uw back-controller-instellingen op te slaan.

    ![Azure Stack - instellingen voor back-up-controller](media/azure-stack-backup/backup-controller-settings.png)

## <a name="start-backup"></a>Back-up starten
Voor het starten van een back-up, klikt u op **back-up nu** starten van een on-demand back-up. Een on-demand back-up wordt de tijd voor de volgende geplande back-up niet worden gewijzigd. Nadat de taak is voltooid, kunt u Bevestig de instellingen in **Essentials**:

![Azure Stack - back-up op aanvraag](media/azure-stack-backup/scheduled-backup.png)

U kunt ook de PowerShell-cmdlet uitvoeren **Start AzsBackup** op uw computer Azure Stack-beheer. Zie voor meer informatie, [maakt u een Back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md).

## <a name="enable-or-disable-automatic-backups"></a>In- of uitschakelen van automatische back-ups
Back-ups worden automatisch gepland wanneer u back-up inschakelen. U kunt de volgende back-uptijd van planning controleren in **Essentials**. 

![Azure Stack - back-up op aanvraag](media/azure-stack-backup/on-demand-backup.png)

Als u uitschakelen toekomstige geplande back-ups wilt, klikt u op **uitschakelen automatische back-ups**. Uitschakelen automatische back-ups blijven back-instellingen die zijn geconfigureerd en de back-upschema, behouden. Deze actie wordt gewoon de scheduler om over te slaan toekomstige back-ups. 

![Azure Stack - uitschakelen geplande back-ups](media/azure-stack-backup/disable-auto-backup.png)

Bevestig dat toekomstige geplande back-ups zijn uitgeschakeld **Essentials**:

![Azure Stack - back-ups zijn uitgeschakeld bevestigen](media/azure-stack-backup/confirm-disable.png)

Klik op **inschakelen automatische back-ups** om te informeren over de planner voor het starten van toekomstige back-ups op het geplande tijdstip. 

![Azure Stack - inschakelen geplande back-ups](media/azure-stack-backup/enable-auto-backup.png)


> [!Note]  
> Als u infrastructuur back-up voordat u bijwerkt naar 1807 hebt geconfigureerd, kunt u automatische back-ups wordt uitgeschakeld. Op deze manier de back-ups die aan de slag met Azure Stack niet conflicteren met het back-ups aan de slag door een externe taak plannings-engine. Als u een externe Taakplanner uitschakelen, klikt u op **inschakelen automatische back-ups**.


## <a name="next-steps"></a>Volgende stappen

- Leer hoe u een back-up uitvoeren. Zie [maakt u een Back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Meer informatie om te controleren of uw back-up wordt uitgevoerd. Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md).
