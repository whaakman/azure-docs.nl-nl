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
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 03/14/2019
ms.openlocfilehash: 98f793b7d94cd554d426a0eec30d8bb4553d3d81
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58105400"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Back-up inschakelen voor Azure Stack vanuit de beheerportal
Schakel de infrastructuur voor Backup-Service via de beheerportal zodat Azure Stack infrastructuur back-ups kunt genereren. De hardware-partner deze back-ups kunt gebruiken om te herstellen van uw omgeving met behulp van de cloudherstel in geval van [een onherstelbare fout](./azure-stack-backup-recover-data.md). Het doel van cloudherstel is om ervoor te zorgen dat uw operators en gebruikers zich weer in de portal aanmelden kunnen nadat het herstel is voltooid. Gebruikers hebben hun abonnementen hersteld met inbegrip van machtigingen voor toegang op basis van rollen en functies, oorspronkelijke plannen, aanbiedingen, en eerder gedefinieerde compute, opslag, netwerk quota's en Key Vault-geheimen.

Echter, de infrastructuur voor Backup-Service niet back-up van IaaS-VM's, configuratie en opslagresources, zoals storage-accounts,-blobs, tabellen, enzovoort, zodat u zich aanmeldt nadat het cloudherstel is voltooid niet zichtbaar voor gebruikers van hun eerder bestaande resources. Platform as a Service (PaaS)-resources en de gegevens worden ook niet ondersteund door de service. 

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

9. Geef een certificaat in cer-bestand in het certificaat in instellingen voor codering. Back-upbestanden zijn versleuteld met behulp van deze openbare sleutel in het certificaat. U moet een certificaat dat alleen de openbare sleutel gedeelte bevat bij het configureren van back-upinstellingen opgeven. Als u dit certificaat voor de eerste keer instellen of het certificaat in de toekomst draaien, kunt u alleen de vingerafdruk van het certificaat bekijken. U niet downloaden of weergeven van het geüploade certificaatbestand. Voer de volgende PowerShell-opdracht om een zelfondertekend certificaat maken met de openbare en persoonlijke sleutels en certificaten exporteren met alleen de openbare sleutel gedeelte voor het maken van het certificaatbestand.

    ```powershell

        $cert = New-SelfSignedCertificate `
            -DnsName "www.contoso.com" `
            -CertStoreLocation "cert:\LocalMachine\My"

        New-Item -Path "C:\" -Name "Certs" -ItemType "Directory" 
        Export-Certificate `
            -Cert $cert `
            -FilePath c:\certs\AzSIBCCert.cer 
    ```

   > [!Note]
   > **1901 en hoger**: Azure Stack accepteert een certificaat voor het versleutelen van back-upgegevens van infrastructuur. Zorg ervoor dat u het certificaat met de openbare en persoonlijke sleutel op een veilige locatie wordt opgeslagen. Uit veiligheidsoverwegingen wordt niet aanbevolen dat u het certificaat met de openbare en persoonlijke sleutels wilt gebruiken om back-instellingen te configureren. Zie voor meer informatie over het beheren van de levenscyclus van dit certificaat [aanbevolen procedures voor back-up-Mailinfrastructuurdienst](azure-stack-backup-best-practices.md).
   > 
   > **1811 of eerder**: Azure Stack accepteert een symmetrische sleutel voor het versleutelen van back-upgegevens van infrastructuur. Gebruik de [New-AzsEncryptionKey64 cmdlet voor het maken van een sleutel](https://docs.microsoft.com/en-us/powershell/module/azs.backup.admin/new-azsencryptionkeybase64). Na de upgrade van 1811 naar 1901, back-upinstellingen de versleutelingssleutel te behouden. De aanbeveling is het bijwerken van de back-upinstellingen voor het gebruik van een certificaat. Ondersteuning voor de sleutel van de versleuteling is nu verouderd. Hebt u ten minste 3 releases om instellingen voor het gebruik van een certificaat te werken. 

10. Selecteer **OK** uw back-controller-instellingen op te slaan.

![Azure Stack - instellingen voor back-up-controller](media/azure-stack-backup/backup-controller-settings-certificate.png)


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

## <a name="update-backup-settings"></a>Back-instellingen bijwerken
Biedt ondersteuning voor van 1901 is versleutelingssleutel afgeschaft. Als u back-up voor de eerste keer in 1901 configureert, moet u een certificaat gebruiken. Azure Stack biedt ondersteuning voor versleutelingssleutel als de sleutel voordat u bijwerkt naar 1901 is geconfigureerd. Compatibiliteit met eerdere versies modus wordt voortgezet voor drie versies. Daarna zijn de versleutelingssleutels worden niet meer ondersteund. 

### <a name="default-mode"></a>Standaardmodus
In de instellingen voor codering, als u back-up van de infrastructuur voor het eerst configureert na het installeren of bijwerken naar 1901, u moet back-up configureren met een certificaat. Met een versleutelingssleutel is niet meer ondersteund. 

U kunt een nieuwe uploaden voor het bijwerken van het certificaat dat wordt gebruikt voor het versleutelen van back-upgegevens. CER-bestand met de openbare sleutel gedeelte en klik op OK instellingen op te slaan. 

Nieuwe back-ups begint met het gebruik van de openbare sleutel in het nieuwe certificaat. Er zijn geen gevolgen voor alle bestaande back-ups die zijn gemaakt met het vorige certificaat. Zorg ervoor dat u het oudere certificaat rond bewaren op een veilige locatie in het geval u ze nodig voor naar de cloud hebt.

![Azure Stack - weergave de vingerafdruk van certificaat](media/azure-stack-backup/encryption-settings-thumbprint.png)

### <a name="backwards-compatibility-mode"></a>Achterwaartse compatibiliteitsmodus
Als u back-up voordat u bijwerkt naar 1901 hebt geconfigureerd, worden de instellingen zonder wijzigingen in gedrag overgedragen. In dit geval versleutelingssleutel wordt ondersteund voor achterwaartse compatibiliteit. U hebt de mogelijkheid om een certificaat te gebruiken voor het bijwerken van de versleutelingssleutel of het overschakelen. Hebt u ten minste drie versies om door te gaan met het bijwerken van de versleutelingssleutel. Gebruik dit moment overstappen naar een certificaat. Voor het maken een nieuwe sleutel gebruiken voor versleuteling de [cmdlet New-AzsEncryptionKeyBase64](https://docs.microsoft.com/en-us/powershell/module/azs.backup.admin/new-azsencryptionkeybase64).

![Azure Stack - met behulp van de versleutelingssleutel in de modus voor achterwaartse compatibiliteit](media/azure-stack-backup/encryption-settings-backcompat-encryption-key.png)

> [!Note]  
> Bijwerken van de versleutelingssleutel naar certificaat is bewerking definitief. Nadat u deze wijziging, kunt niet u overstappen naar versleutelingssleutel. Alle bestaande back-ups blijven versleuteld met de vorige versleutelingssleutel. 

![Azure Stack - versleutelingscertificaat voor gebruik in de modus voor achterwaartse compatibiliteit](media/azure-stack-backup/encryption-settings-backcompat-certificate.png)

## <a name="next-steps"></a>Volgende stappen

Leer hoe u een back-up uitvoeren. Zie [maakt u een Back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md)

Meer informatie om te controleren of uw back-up wordt uitgevoerd. Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md)
