---
title: Upgrade back-upkluis naar Recovery Services-kluis van Azure Backup'
description: Upgrade back-upkluis naar Recovery Services-kluis om de nieuwe functies, zoals back-up van resourcemanager VM's, verbeterde beveiliging, VMware-VM back-up en de systeemstatusback-up voor Windows-Servers
services: backup
author: raynew
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: raynew
ms.openlocfilehash: b7671271e569802311884861265a7825404c9c75
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490342"
---
# <a name="backup-vault-upgraded-to-recovery-services-vault"></a>Back-upkluis is bijgewerkt naar Recovery Services-kluis
In dit artikel biedt een overzicht van de Recovery Services-kluis biedt, veelgestelde vragen over het upgraden van bestaande back-up-kluis naar Recovery Services-kluis en stappen na de upgrade. Een Recovery Services-kluis is de Azure Resource Manager-equivalent van een back-upkluis met uw back-upgegevens. De gegevens zijn of doorgaans kopieën van gegevens of configuratie-informatie voor virtuele machines (VM's), werkbelastingen, servers of werkstations, on-premises of in Azure.

## <a name="what-is-a-recovery-services-vault"></a>Wat is een Recovery Services-kluis?
Een Recovery Services-kluis is een online-opslagentiteit in Azure die wordt gebruikt om gegevens zoals back-ups, herstelpunten en back-upbeleid op te slaan. U kunt de Recovery Services-kluizen gebruiken voor het opslaan van back-upgegevens voor verschillende Azure-services, zoals virtuele machines van IaaS (Linux of Windows) en Azure SQL-databases. Recovery Services-kluizen ondersteuning van System Center DPM, Windows Server, Azure Backup Server en meer. Recovery Services-kluizen maken het eenvoudig om uw back-upgegevens te ordenen, terwijl de beheertaken minimaal zijn.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Het vergelijken van Recovery Services-kluizen en back-upkluizen
Recovery Services-kluizen zijn gebaseerd op het Azure Resource Manager-model van Azure, terwijl de Backup-kluizen zijn gebaseerd op de Azure Service Manager-model. Wanneer u een back-upkluis naar een Recovery Services-kluis upgraden, blijft de back-upgegevens behouden tijdens en na het upgradeproces. Recovery Services-kluizen bieden functies niet beschikbaar voor back-upkluizen, zoals:

- **Verbeterde mogelijkheden om gegevens te beveiligen back-up**: Met Recovery Services-kluizen biedt Azure Backup beveiligingsmogelijkheden cloudback-ups te beveiligen. Deze beveiligingsfuncties ervoor te zorgen dat u kunt uw back-ups beveiligen en veilig gegevens vanaf back-ups van de cloud herstellen, zelfs als de productie- en back-up-servers zijn aangetast. [Meer informatie](backup-azure-security-feature.md)

- **Centrale bewaking voor uw hybride IT-omgeving**: U kunt controleren met Recovery Services-kluizen niet alleen uw [Azure IaaS VM's](backup-azure-manage-vms.md) , maar ook uw [on-premises activa](backup-azure-manage-windows-server.md#manage-backup-items) vanuit een centrale portal. [Meer informatie](https://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rollen gebaseerd toegangsbeheer (RBAC)**: RBAC biedt uiterst gedetailleerd toegangsbeheer management in Azure. [Azure biedt verschillende ingebouwde rollen](../role-based-access-control/built-in-roles.md), en Azure Backup heeft drie [ingebouwde rollen voor het beheren van herstelpunten](backup-rbac-rs-vault.md). Recovery Services-kluizen zijn compatibel met RBAC, dat voorkomt back-up dat en herstellen van toegang tot een gedefinieerde set gebruikersrollen. [Meer informatie](backup-rbac-rs-vault.md)

- **Alle configuraties van virtuele Azure-Machines beveiligen**: Recovery Services-kluizen beveiligen op basis van Resource Manager VM's met Premium-schijven, Managed Disks en versleutelde virtuele machines. Een Backup-kluis upgraden naar een Recovery Services-kluis biedt u de mogelijkheid om te upgraden van uw Service Manager-VM's naar Resource Manager gebaseerde virtuele machines. Tijdens het upgraden van de kluis, kunt u uw herstelpunten op basis van een Service Manager-virtuele machine en configureer beveiliging voor de upgrade is uitgevoerd (Resource Manager-functionaliteit) virtuele machines. [Meer informatie](https://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Instant herstel voor IaaS-VM's**: Met behulp van Recovery Services-kluizen, u kunt bestanden en mappen terugzetten vanuit een IaaS-VM zonder het herstellen van de volledige virtuele machine, waardoor sneller worden hersteld. Instant herstel voor IaaS-VM's is beschikbaar voor zowel Windows als Linux-VM's. [Meer informatie](https://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

> [!NOTE]
> Als u items naar een Backup-kluis geregistreerd bij de MARS-agent ouder is dan 2.0.9083.0, [download de nieuwste MARS-agent]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe) versie te laten de voordelen van alle functies van Recovery Services-kluis. 
> 

## <a name="managing-your-recovery-services-vaults"></a>Beheren van uw Recovery Services-kluizen
De volgende schermen weer een nieuwe Recovery Services-kluis, een upgrade uitgevoerd van de Backup-kluis in Azure portal. De bijgewerkte kluis zijn in een standaard-resourcegroep met de naam "Standaard-RecoveryServices-ResourceGroup-geo" aanwezig. Voorbeeld: Als uw back-upkluis is in VS-West, wordt deze in een standaard RG met de naam standaard-RecoveryServices-ResourceGroup-westus omhoog worden geplaatst.
> [!NOTE]
> Voor klanten van de CPS-standaard, resourcegroep niet is gewijzigd na de upgrade van de kluis en blijft hetzelfde zoals deze voordat de upgrade was.

Het eerste scherm ziet u het dashboard van de kluis waarin belangrijke entiteiten voor de kluis worden weergegeven.
![Voorbeeld van een upgrade uitgevoerd van een Backup-kluis van Recovery Services-kluis](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

De tweede schermafbeelding geeft de help van koppelingen om u te helpen aan de slag met behulp van de Recovery Services-kluis.

![Help-koppelingen in de blade snel starten](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Stappen na de upgrade
Recovery Services-kluis biedt ondersteuning voor informatie over de tijdzone van het opgeven in de back-upbeleid. Nadat de kluis upgrade is voltooid, gaat u naar de back-upbeleid in kluismenu-instellingen en bijwerken van de informatie over de tijdzone voor elk van de beleidsregels die zijn geconfigureerd in de kluis. Dit scherm bevat al de back-upschema-tijd opgegeven als per lokale tijdzone die wordt gebruikt wanneer u beleid hebt gemaakt. 

## <a name="enhanced-security"></a>Verbeterde beveiliging
Wanneer een back-upkluis is bijgewerkt naar een Recovery Services-kluis, worden de beveiligingsinstellingen voor deze kluis automatisch ingeschakeld. Als de beveiligingsinstellingen zijn op bepaalde bewerkingen zoals het verwijderen van back-ups of het wijzigen van een wachtwoordzin moet een [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) PINCODE. Zie het artikel voor meer informatie over de verbeterde beveiliging [beveiligingsfuncties die beschermen hybride back-ups](backup-azure-security-feature.md). Wanneer de verbeterde beveiliging is ingeschakeld, worden gegevens van bewaard gedurende 14 dagen na de punt herstelgegevens is verwijderd uit de kluis. Klanten worden in rekening gebracht voor opslag van deze beveiligingsgegevens. Beveiliging het bewaren van gegevens is van toepassing op de herstelpunten die voor de Azure backup-agent, de Azure Backup Server en de System Center Data Protection Manager worden uitgevoerd. 

## <a name="gather-data-on-your-vault"></a>Verzamelen van gegevens voor uw kluis
Nadat u een upgrade uitvoert naar een Recovery Services-kluis, rapporten configureren voor Azure Backup (voor IaaS-VM's en Microsoft Azure Recovery Services agent) en Power BI gebruiken voor toegang tot de rapporten. Zie het artikel voor meer informatie over het verzamelen van gegevens, [configureren van Azure Backup-rapporten](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**De upgrade-abonnement invloed heeft op mijn back-ups?**</br>
Nee. Uw back-ups worden onderbroken tijdens en na de upgrade.

**Wat doet deze upgrade gemiddelde voor mijn bestaande tooling?**</br>
U moet uw bestaande automatisering of hulpprogramma's bijwerken naar het Resource Manager-implementatiemodel om ervoor te zorgen dat het blijft werken na de upgrade. Raadpleeg de verwijzingen van de PowerShell-cmdlets voor de [Resource Manager-implementatiemodel](backup-client-automation.md).

**Kan ik terugdraaien na de upgrade?**</br>
Nee. Terugdraaien wordt niet ondersteund wanneer de resources zijn bijgewerkt.

**Kan ik mijn klassieke kluis na het bijwerken bekijken?**</br>
Nee. U kunt geen weergeven of beheren van uw klassieke kluis na het bijwerken. Alleen mogelijk met de nieuwe Azure-portal voor alle acties op de kluis.

**Waarom zie ik niet servers die worden beveiligd door de MARS-agent in mijn bijgewerkte kluis?**</br>
U moet voor het installeren van de meest recente MARS-agent om te zien van alle servers die worden beveiligd door de MARS-agent in uw kluis. U kunt de meest recente versie van de agent uit downloaden [hier]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).

**Zie ik kan geen back-upbeleid voor de servers die worden beveiligd door de MARS-agent na de upgrade**</br>
De kluis back-upbeleid mogelijk verouderd en daarom niet kan worden gesynchroniseerd met de bijgewerkte kluis. Werk het beleid ervoor te zorgen dat u om te zien van uw beleid in de bijgewerkte kluis.
Voor het bijwerken van het beleid, gaat u naar de MARS-agent en de geconfigureerde back-upbeleid bijwerken.

**Waarom kan ik mijn back-upbeleid niet bijwerken na de upgrade?**</br>
Dit gebeurt wanneer u zich op een oude back-agent en selecteert u de minimale bewaarperiode moet kleiner zijn dan de toegestane minimumwaarde. Wanneer een back-upkluis is bijgewerkt naar een Recovery Services-kluis, worden de beveiligingsinstellingen voor deze kluis automatisch ingeschakeld. Om ervoor te zorgen dat er altijd een geldig aantal herstelpunten beschikbaar zijn, is er een bepaalde minimale bewaartermijn die moet worden onderhouden aan de hand van de functie voor beveiliging. Raadpleeg voor meer informatie, [hier](backup-azure-security-feature.md).
Bovendien moet u uw Azure Backup-agents bijwerken naar nieuwste versie te nemen van de voordelen van de nieuwste functies van Azure Backup.

**Kan ik mijn agent hebt bijgewerkt, maar ik kan nog steeds niet weergegeven op alle objecten wordt gesynchroniseerd, zelfs dagen na de upgrade**</br>
Controleer of als u dezelfde computer voor meerdere kluizen hebt geregistreerd. Zorg ervoor dat u zoekt op dezelfde kluis waarbij de MARS-Agent is geregistreerd. Als u wilt weten welke kluis de MARS-Agent is geregistreerd, opent u het Windows-register en controleer de waarde voor de sleutel ServiceResourceName onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config de kluis geregistreerd bij dat MARS-agent wordt weergegeven Er zijn. Als de sleutel ServiceResourceName niet weergegeven in uw systeem wordt, contact met ons opnemen met de waarde van de sleutels ResourceId en MachineId onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config en wij helpen u bij het probleem is opgelost.

**Waarom zie ik niet de informatie van de taken voor mijn resources na de upgrade?**</br>
Controle van het back-ups (MARS-agent en IaaS) is een nieuwe functie die u ontvangt wanneer u uw back-upkluis naar Recovery Services-kluis upgraden. De informatie voor prestatiebewaking duurt tot 12 uur te synchroniseren met de service.

**Hoe meld ik een probleem?**</br>
Als een gedeelte van de upgrade van de kluis is mislukt, houd er rekening mee dat de bewerkings-ID in de volgende fout weergegeven. Microsoft Support werkt proactief als het probleem wilt oplossen. U kunt contact opnemen met ondersteuning of een e-mail verzenden naar rsvaultupgrade@service.microsoft.com met uw abonnements-ID, de naam van de kluis en de bewerkings-id. We zullen proberen om het probleem zo snel mogelijk opgelost. Probeer de bewerking niet tenzij expliciet om dit te doen door Microsoft.

## <a name="next-steps"></a>Volgende stappen
Gebruik de volgende artikelen voor:</br>
[Back-up van een IaaS-VM](backup-azure-arm-vms-prepare.md)</br>
[Back-up van een Azure Backup Server](backup-azure-microsoft-azure-backup.md)</br>
[Back-up van een Windows-Server](backup-configure-vault.md)
