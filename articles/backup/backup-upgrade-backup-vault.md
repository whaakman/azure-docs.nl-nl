---
title: Upgrade Backup-kluis in Recovery Services-kluis van Azure Backup | Microsoft Docs
description: Upgrade Backup-kluis in Recovery Services-kluis nieuwe functies zoals back-up van resourcemanager virtuele machines, verbeterde beveiliging, VMware VM-back- en back-up van systeemstatus voor Windows-Servers ophalen
services: backup
documentationcenter: 
author: trinadhk
manager: vijayts
editor: 
keyword: backup vault; upgrade vault; recovery services vault
ms.assetid: d037a8bf-49f2-4578-974a-3471d87ca278
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: trinadhk, sogup
ms.openlocfilehash: 708983fc2c5264d1213bdb32b665dcccc5ca9df9
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="backup-vault-upgraded-to-recovery-services-vault"></a>Back-upkluis bijgewerkt naar een Recovery Services-kluis
In dit artikel biedt een overzicht van welke Recovery Services-kluis biedt, veelgestelde vragen over het upgraden van bestaande back-up vault Recovery Services-kluis en stappen na de upgrade. Een Recovery Services-kluis is de Azure Resource Manager-equivalent van een back-upkluis met uw back-upgegevens. De gegevens zijn of doorgaans kopieën van gegevens of configuratie-informatie voor virtuele machines (VM's), werkbelastingen, servers of werkstations, lokaal of in Azure.

## <a name="what-is-a-recovery-services-vault"></a>Wat is een Recovery Services-kluis?
Een Recovery Services-kluis is een online-opslagentiteit in Azure die wordt gebruikt om gegevens zoals back-ups, herstelpunten en back-upbeleid op te slaan. U kunt de Recovery Services-kluizen gebruiken voor het opslaan van back-upgegevens voor verschillende Azure-services zoals IaaS VM's (Linux of Windows) en Azure SQL-databases. Recovery Services-kluizen ondersteuning van System Center DPM, Windows Server back-upserver van Azure, en meer. Recovery Services-kluizen maken het eenvoudig om uw back-upgegevens te ordenen, terwijl de beheertaken minimaal zijn.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Vergelijken Recovery Services-kluizen en back-upkluizen
Recovery Services-kluizen zijn gebaseerd op het Azure Resource Manager-model van Azure, terwijl de Backup-kluizen zijn gebaseerd op de Azure Service Manager-model. Wanneer u een back-upkluis naar een Recovery Services-kluis bijwerkt, blijft de back-upgegevens intact tijdens en na het upgradeproces. Recovery Services-kluizen bieden, zoals functies die niet beschikbaar voor de Backup-kluizen:

- **Uitgebreide mogelijkheden om te helpen beveiligen back-upgegevens**: met Recovery Services-kluizen, Azure Backup biedt beveiligingsmogelijkheden voor het beveiligen van back-ups van de cloud. Deze beveiligingsfuncties Zorg ervoor dat u kunt uw back-ups te beveiligen en veilig gegevens herstellen vanaf back-ups van de cloud, zelfs als de productie- en back-up-servers worden getroffen. [Meer informatie](backup-azure-security-feature.md)

- **Centrale bewaking voor uw hybride IT-omgeving**: met Recovery Services-kluizen, kunt u controleren niet alleen uw [Azure IaaS VM's](backup-azure-manage-vms.md) , maar ook uw [lokale activa](backup-azure-manage-windows-server.md#manage-backup-items) vanuit een centrale portal. [Meer informatie](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Op rollen gebaseerde toegangsbeheer (RBAC)**: RBAC verfijnd toegangsbeheer management in Azure biedt. [Azure biedt diverse ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md), en de Azure Backup bevat drie [ingebouwde functies voor het beheren van herstelpunten](backup-rbac-rs-vault.md). Recovery Services-kluizen compatibel zijn met RBAC waarmee de beperkt back-up en herstel van toegang aan de gedefinieerde set van gebruikersrollen. [Meer informatie](backup-rbac-rs-vault.md)

- **Alle configuraties van Azure Virtual Machines beveiligen**: Recovery Services-kluizen beveiligen Resource Manager gebaseerde virtuele machines met inbegrip van Premium-schijven, schijven beheerd en virtuele machines versleuteld. Een back-upkluis upgraden naar een Recovery Services-kluis biedt u de mogelijkheid uw Service Manager-VM's bijwerken naar Resource Manager gebaseerde virtuele machines. Tijdens het upgraden van de kluis, kunt u uw herstelpunten op basis van Service Manager VM bewaren en configureer beveiliging voor de bijgewerkte (Resource Manager is ingeschakeld) virtuele machines. [Meer informatie](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Chatberichten terugzetten voor IaaS VM's**: met behulp van Recovery Services-kluizen, u kunt bestanden en mappen van een IaaS-VM herstellen zonder te herstellen van de hele virtuele machine, waardoor sneller worden hersteld. Chatberichten terugzetten voor IaaS VM's is beschikbaar voor zowel Windows als een virtuele Linux-machines. [Meer informatie](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

> [!NOTE]
> Als u items naar een Backup-kluis geregistreerd bij MARS-agent ouder is dan 2.0.9083.0, [de meest recente MARS-agent downloaden]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe) versie te laten de voordelen van alle functies van de Recovery Services-kluis. 
> 

## <a name="managing-your-recovery-services-vaults"></a>Het beheren van de Recovery Services-kluizen
De volgende schermen ziet een nieuwe Recovery Services-kluis bijgewerkt van de Backup-kluis, in de Azure portal. De bijgewerkte kluis zijn in een standaardresourcegroep met de naam 'Standaard-RecoveryServices-ResourceGroup-geografisch' aanwezig. Voorbeeld: Als uw Backup-kluis is opgeslagen in VS-West, deze zal worden aangeboden in een standaard RG standaard RecoveryServices-ResourceGroup westus met de naam.
> [!NOTE]
> Voor klanten CPS Standard, resourcegroep niet is gewijzigd na de upgrade van de kluis en hetzelfde is gebleven zoals deze voordat de upgrade was.

Het eerste scherm toont het kluisdashboard die belangrijke entiteiten voor de kluis wordt weergegeven.
![Voorbeeld van een upgrade uitgevoerd van een back-upkluis Recovery Services-kluis](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

Het tweede scherm ziet u de Help-informatie beschikbaar om u te helpen koppelingen aan de slag met de Recovery Services-kluis.

![Help-koppelingen in de blade snel starten](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Stappen na de upgrade
Recovery Services-kluis ondersteunt geven informatie over de tijdzone in de back-upbeleid. Nadat de kluis upgrade is voltooid, gaat u naar de back-upbeleid in kluis-instellingen en bijwerken van de tijdzone-informatie voor elk van de beleidsregels die zijn geconfigureerd in de kluis. Dit scherm bevat al de back-upschema tijd opgegeven als per lokale tijdzone die wordt gebruikt wanneer u beleid hebt gemaakt. 

## <a name="enhanced-security"></a>Verbeterde beveiliging
Wanneer een back-upkluis is bijgewerkt naar een Recovery Services-kluis, worden de beveiligingsinstellingen voor de kluis die automatisch ingeschakeld. Wanneer de beveiligingsinstellingen op bepaalde bewerkingen zoals het verwijderen van de back-ups, of het wijzigen van een wachtwoordzin vereist een [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) PINCODE. Zie het artikel voor meer informatie over de verbeterde beveiliging [beveiligingsfuncties die beschermen hybride back-ups](backup-azure-security-feature.md). Als de verbeterde beveiliging is ingeschakeld, wordt gegevens behouden gedurende 14 dagen na de punt herstelgegevens is verwijderd uit de kluis. Klanten wordt gefactureerd voor opslag van deze beveiligingsgegevens. Beveiliging Gegevensretentie geldt voor de herstelpunten die voor de Azure backup-agent, de Azure Backup-Server en de System Center Data Protection Manager worden uitgevoerd. 

## <a name="gather-data-on-your-vault"></a>Gegevens verzamelen over uw kluis
Eenmaal u upgraden naar een Recovery Services-kluis, rapporten configureren voor Azure Backup (voor IaaS VM's en Microsoft Azure Recovery Services agent) en Power BI gebruiken voor toegang tot de rapporten. Zie het artikel voor meer informatie over het verzamelen van gegevens [Azure Backup configureren rapporten](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**De upgrade-abonnement invloed heeft op Mijn lopende back-ups?**</br>
Nee. Uw actieve back-ups niet wordt onderbroken tijdens en na de upgrade.

**Wat houdt de upgrade voor mijn bestaande tooling?**</br>
U moet uw bestaande automatisering of tooling bijwerken naar het Resource Manager-implementatiemodel om ervoor te zorgen dat het blijft werken na de upgrade. Raadpleeg de verwijzingen van de PowerShell-cmdlets voor de [Resource Manager-implementatiemodel](backup-client-automation.md).

**Kan ik terugdraaien na de upgrade?**</br>
Nee. Terugdraaien wordt niet ondersteund nadat de resources hebt geüpgraded.

**Kan ik mijn klassieke kluis na het bijwerken weergeven?**</br>
Nee. U kunt weergeven of beheren van uw klassieke kluis na het bijwerken. Alleen mogelijk met de nieuwe Azure portal voor alle acties op de kluis.

**Waarom zie ik niet servers die zijn beveiligd door MARS-agent in mijn bijgewerkte kluis?**</br>
U moet de meest recente MARS-agent om te zien van de servers die zijn beveiligd door MARS-agent in uw kluis te installeren. U kunt de nieuwste versie van de agent uit downloaden [hier]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).

**Ik kan beleid van de back-up voor de servers die zijn beveiligd door MARS-agent na de upgrade niet zien.**</br>
Back-upbeleid voor de kluis is mogelijk verouderd en daarom kan niet worden gesynchroniseerd naar het bijgewerkte kluis. Werk het beleid om te controleren of dat uw beleid in de bijgewerkte kluis aanhoudt.
Voor het bijwerken van het beleid, gaat u naar MARS-agent en het geconfigureerde back-upbeleid bijwerken.

**Waarom kan ik mijn back-up beleid na de upgrade niet bijwerken?**</br>
Dit gebeurt wanneer u op een oude back-agent en selecteert u de minimale bewaarperiode moet kleiner zijn dan de toegestane minimumwaarde. Wanneer een back-upkluis is bijgewerkt naar een Recovery Services-kluis, worden de beveiligingsinstellingen voor de kluis die automatisch ingeschakeld. Om ervoor te zorgen dat er altijd een geldig aantal herstelpunten beschikbaar zijn, is er een minimale bewaarperiode die moet worden onderhouden aan de hand van de functie voor beveiliging. Raadpleeg voor meer informatie [hier](backup-azure-security-feature.md).
Bovendien moet u de Azure Backup-agents bijwerken naar de meest recente versie te laten worden de voordelen van de nieuwste functies van Azure Backup.

**Ik heb mijn agent bijgewerkt, maar ik nog steeds niet alle objecten wordt gesynchroniseerd, zelfs dagen na de upgrade**</br>
Controleer als u dezelfde machine aan meerdere kluizen hebt geregistreerd. Zorg ervoor dat u wilt bekijken bij dezelfde kluis waarbij de MARS-Agent is geregistreerd. Als u wilt weten welke kluis MARS-Agent is geregistreerd, opent u het Windows-register en controleer de waarde voor de sleutel ServiceResourceName onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config de kluis geregistreerd bij dat MARS-agent wordt weergegeven Er zijn. Als de sleutel ServiceResourceName niet zichtbaar in uw systeem is, aan ons door de waarde van de sleutels ResourceId en MachineId onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config bereiken en wij helpen u bij het probleem is opgelost.

**Waarom zie ik niet de informatie van de taken voor mijn resources na de upgrade?**</br>
Bewaking voor back-ups (MARS-agent en IaaS) is een nieuwe functie die u krijgt wanneer u uw Backup-kluis naar de Recovery Services-kluis bijwerkt. De controlegegevens duurt maximaal 12 uur te synchroniseren met de service.

**Hoe kan ik een probleem melden?**</br>
Als een gedeelte van de kluis upgrade mislukt, ziet u dat de OperationId weergegeven in de fout. Microsoft Support werkt proactief als het probleem wilt oplossen. U kunt ondersteuning bereiken of een e-mail sturen naar rsvaultupgrade@service.microsoft.com met uw abonnements-ID, de kluisnaam van de en OperationId. Er wordt geprobeerd het probleem zo snel mogelijk oplossen. Probeer de bewerking niet tenzij expliciet om dit te doen door Microsoft.

## <a name="next-steps"></a>Volgende stappen
Gebruik de volgende artikelen voor:</br>
[Back-up van een IaaS VM](backup-azure-arm-vms-prepare.md)</br>
[Back-up van een Azure Backup-Server](backup-azure-microsoft-azure-backup.md)</br>
[Back-up van een Windows-Server](backup-configure-vault.md)
