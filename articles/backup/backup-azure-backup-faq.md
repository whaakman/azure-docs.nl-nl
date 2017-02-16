
---
title: Veelgestelde vragen over Azure Backup | Microsoft Docs
description: Antwoorden op veelgestelde vragen over de Backup-service, Backup-agent, het maken van back-ups en de retentie, het herstel, de beveiliging en andere veelgestelde vragen over het terugzetten van een back-up en herstel na noodgeval.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-up en herstel na noodgeval; Backup-service
ms.assetid: 1011bdd6-7a64-434f-abd7-2783436668d7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/1/2017
ms.author: trinadhk;giridham;arunak;markgal;jimpark;
translationtype: Human Translation
ms.sourcegitcommit: b50b62b9b9a800c4f42e763a7ff6eecd84de9e69
ms.openlocfilehash: 7033b660608897819b629d398450cd32ea7b48fa


---
# <a name="azure-backup-service--faq"></a>Veelgestelde vragen over de Azure Backup-service
Dit artikel bevat een overzicht met veelgestelde vragen (en de bijbehorende antwoorden) over de Azure Backup-service. De vragen worden doorgaans vrij snel door de community beantwoordt, en als een bepaalde vraag veelvuldig wordt gesteld, wordt deze toegevoegd aan dit artikel. De antwoorden op vragen bevatten doorgaans naslag- en ondersteuningsinformatie. U kunt via **Opmerkingen** (aan de rechterkant) vragen stellen over Azure Backup. De opmerkingen worden onder aan dit artikel weergegeven. Er is een Livefyre-account vereist om opmerkingen te kunnen plaatsen. U kunt ook in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) vragen over de Azure Backup-service plaatsen.

## <a name="what-is-the-list-of-supported-operating-systems-from-which-i-can-back-up-to-azure-using-azure-backup-br"></a>Wat is de lijst met ondersteunde besturingssystemen waarmee ik een back-up naar Azure met Azure Backup kan maken? <br/>
Azure Backup ondersteunt de volgende besturingssystemen voor het maken van back-ups van bestanden, mappen en workloadtoepassingen die worden beschermd met behulp van Azure Backup Server en SCDPM.

| Besturingssysteem | Platform | SKU |
|:--- | --- |:--- |
| Windows 8 en de meest recente SP's |64 bits |Enterprise, Pro |
| Windows 7 en de meest recente SP's |64 bits |Ultimate, Enterprise, Professional, Home Premium Home Basic, Starter |
| Windows 8.1 en de meest recente SP's |64 bits |Enterprise, Pro |
| Windows 10 |64 bits |Enterprise, Pro, Home |
| Windows Server 2016 |64 bits |Standard, Datacenter, Essentials |
| Windows Server 2012 R2 en de meest recente SP's |64 bits |Standard, Datacenter, Foundation |
| Windows Server 2012 en de meest recente SP's |64 bits |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 en de meest recente SP's |64 bits |Standard, Workgroup |
| Windows Storage Server 2012 en de meest recente SP's |64 bits |Standard, Workgroup |
| Windows Server 2012 R2 en de meest recente SP's |64 bits |Essential |
| Windows Server 2008 R2 SP1 |64 bits |Standard, Enterprise, Datacenter, Foundation |
| Windows Server 2008 SP2 |64 bits |Standard, Enterprise, Datacenter, Foundation |

Voor Azure VM Backup:

* **Linux**: Azure Backup ondersteunt [een lijst met distributies die zijn goedgekeurd door Azure](../virtual-machines/virtual-machines-linux-endorsed-distros.md), met uitzondering van Core OS Linux.  Andere Bring-Your-Own-Linux-distributies werken mogelijk ook, mits de VM-agent beschikbaar is op de virtuele machine en ondersteuning voor Python aanwezig is.
* **Windows Server**: versies ouder dan Windows Server 2008 R2 worden niet ondersteund.

## <a name="where-can-i-download-the-latest-azure-backup-agent-br"></a>Waar kan ik de meest recente Azure Backup-agent downloaden? <br/>
U kunt [hier](http://aka.ms/azurebackup_agent) de meest recente agent downloaden voor back-ups van Windows Server, System Center DPM of Windows-client. Als u een back-up van een virtuele machine wilt maken, gebruikt u de VM-agent (die automatisch de juiste extensie installeert). Virtuele machines die zijn gemaakt via de Azure-galerie, beschikken al over de VM-agent.

## <a name="which-version-of-scdpm-server-is-supported-br"></a>Welke versie van SCDPM-server wordt ondersteund? <br/>
U kunt het beste de [nieuwste](http://aka.ms/azurebackup_agent) Azure Backup-agent op het meest recente updatepakket van SCDPM installeren (UR11 van augustus 2016)

## <a name="when-configuring-the-azure-backup-agent-i-am-prompted-to-enter-the-vault-credentials-do-vault-credentials-expire"></a>Wanneer ik de Azure Backup-agent configureer, moet ik de kluisreferenties opgeven. Verlopen kluisreferenties?
Ja, de kluisreferenties verlopen na 48 uur. Als het bestand is verlopen, meldt u zich aan bij de Azure Portal en downloadt u de kluisreferentiebestanden uit uw kluis.

## <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Is er een limiet voor het aantal kluizen dat voor elk Azure-abonnement kan worden gemaakt? <br/>
Ja. U kunt vanaf september 2016 25 back-upkluizen per abonnement maken. U kunt per abonnement maximaal 25 Recovery Services-kluizen per ondersteunde regio van Azure Backup maken. Als u extra kluizen nodig hebt, maakt u een nieuw abonnement.

## <a name="are-there-any-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Zijn er beperkingen met betrekking tot het aantal servers/machines dat kan worden geregistreerd voor elke kluis? <br/>
Ja, u kunt maximaal vijftig machines per kluis registreren. Voor virtuele machines van Azure IaaS geldt een limiet van 200 virtuele machines per kluis. Als u meer machines wilt registreren, maakt u een nieuwe kluis.

## <a name="how-do-i-register-my-server-to-another-datacenterbr"></a>Hoe kan ik mijn server bij een ander datacenter registreren?<br/>
De back-upgegevens worden verzonden naar het datacenter van de kluis waarbij de server is geregistreerd. De eenvoudigste manier om het datacenter te wijzigen, is de agent te verwijderen en opnieuw te installeren en u vervolgens te registreren bij een nieuwe kluis die behoort bij het gewenste datacenter.

## <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azurebr"></a>Wat gebeurt er als ik de naam van een Windows-server wijzig waarmee back-ups van gegevens naar Azure worden opgeslagen?<br/>
Wanneer u de naam van een server wijzigt, worden alle back-ups die momenteel zijn geconfigureerd, gestopt.
Registreer de nieuwe naam van de server bij de back-upkluis. Als u de nieuwe naam bij de kluis registreert, is de eerste back-upbewerking een *volledige* back-up. Als u gegevens moet herstellen waarvan eerder een back-up naar de kluis is gemaakt met de oude servernaam, kunt u deze gegevens herstellen met de optie [**Another server**](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) (Andere server) in de wizard **Gegevens herstellen**.

## <a name="what-types-of-drives-can-i-backup-files-and-folders-from-br"></a>Van welke typen stations kan ik back-up van bestanden en mappen maken? <br/>
U kunt geen back-up van de volgende stations/volumes maken:

* Verwisselbare media: het station moet als vast station worden gerapporteerd om te kunnen worden gebruikt als bron voor back-upitems.
* Alleen-lezenvolumes: de Volume Shadow Copy Service (VSS) werkt alleen als het volume schrijfbaar is.
* Offlinevolumes: VSS werkt alleen als het volume online is.
* Netwerkshare: er kan alleen een online back-up worden gemaakt wanneer het volume zich lokaal op de server bevindt.
* Met BitLocker beveiligde volumes: het volume moet worden ontgrendeld voordat er een back-up kan worden gemaakt.
* Bestandsysteemidentificatie: NTFS is het enige bestandssysteem dat door deze versie van de onlineback-upservice wordt ondersteund.

## <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Van welke typen bestanden en mappen op mijn server kan ik een back-up maken?<br/>
De volgende typen worden ondersteund:

* Versleuteld
* Gecomprimeerd
* Sparse
* Gecomprimeerd + Sparse
* Vaste koppelingen: niet ondersteund, worden overgeslagen
* Reparsepunt: niet ondersteund, wordt overgeslagen
* Versleuteld + sparse: niet ondersteund, wordt overgeslagen
* Gecomprimeerde stream: niet ondersteund, wordt overgeslagen
* Sparse stream: niet ondersteund, wordt overgeslagen

## <a name="whats-the-minimum-size-requirement-for-the-cache-folder-br"></a>Wat is de vereiste minimumgrootte voor de cachemap? <br/>
De grootte van de cachemap bepaalt de hoeveelheid gegevens waarvan u een back-up maakt. Uw cachemap moet 5% van de benodigde ruimte voor de opslag van gegevens zijn.

## <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Als mijn organisatie één kluis heeft, hoe kan ik bij het herstellen van gegevens de gegevens van de ene server dan isoleren van de gegevens van een andere server?<br/>
Alle servers die zijn geregistreerd bij dezelfde kluis, kunnen de gegevensback-ups herstellen van andere servers *die gebruikmaken van dezelfde wachtwoordzin*. Als u servers hebt waarvan u de back-upgegevens wilt isoleren van andere servers in uw organisatie, gebruikt u een speciale wachtwoordzin voor die servers. U kunt bijvoorbeeld verschillende wachtwoordzinnen voor de human resource-server, de accountingserver en de opslagserver gebruiken.

## <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Kan ik mijn back-upgegevens- of back-upkluis 'migreren' tussen abonnementen? <br/>
Nee. De kluis is gemaakt op abonnementsniveau en kan, zodra deze is gemaakt, niet opnieuw worden toegewezen aan een ander abonnement.

## <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Werkt de Azure Backup-agent op een server die gebruikmaakt van Windows Server 2012-gegevensontdubbeling? <br/>
Ja. De agentservice converteert de ontdubbelde gegevens naar normale gegevens wanneer de back-upbewerking wordt voorbereid. Vervolgens worden de gegevens geoptimaliseerd voor het maken van een back-up en versleuteld en worden de versleutelde gegevens verzonden naar de online back-upservice.

## <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Als ik een back-uptaak annuleer nadat deze is gestart, worden de overgedragen back-upgegevens dan verwijderd? <br/>
Nee. Alle gegevens die vóór het annuleren naar de kluis zijn overgedragen, blijven in de kluis. Azure Backup gebruikt een controlepuntmechanisme waarmee tijdens de back-up van tijd tot tijd controlepunten worden toegevoegd aan de back-upgegevens. Omdat de back-upgegevens controlepunten bevatten, kan tijdens het volgende back-upproces de integriteit van de bestanden worden gecontroleerd. De volgende back-uptaak is incrementeel ten opzichte van de gegevens waarvan eerder een back-up is gemaakt. Incrementele back-ups dragen alleen nieuwe of gewijzigde gegevens over, zodat de bandbreedte beter wordt benut.

Als u een back-uptaak voor een virtuele Azure-machine annuleert, worden eventuele overgedragen gegevens geannuleerd. De volgende back-uptaak draagt incrementele gegevens over van na de vorige succesvolle back-up-taak.

## <a name="why-am-i-seeing-the-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-had-scheduled-regular-backups-previously-br"></a>Waarom wordt er een waarschuwing weergegeven dat er geen Azure back-ups zijn geconfigureerd voor deze server, terwijl ik regelmatige back-ups heb gepland? <br/>
Deze waarschuwing wordt weergegeven wanneer de instellingen voor het back-upschema die zijn opgeslagen op de lokale server niet overeenkomen met de instellingen die zijn opgeslagen in de back-upkluis. Wanneer de server of de instellingen zijn hersteld naar een bekende goede status, worden de back-upschema's mogelijk niet meer gesynchroniseerd. Als u deze waarschuwing ontvangt, [configureert u het back-upbeleid opnieuw](backup-azure-manage-windows-server.md) en **voert u een back-up uit** om de lokale server te synchroniseren met Azure.

## <a name="what-firewall-rules-should-be-configured-for-azure-backup-br"></a>Welke firewallregels moeten worden geconfigureerd voor Azure Backup firewall? <br/>
Voor een probleemloze beveiliging van de on-premises- of workloadgegevens die naar Azure worden verzonden, is het raadzaam om ervoor te zorgen dat uw firewall kan communiceren met de volgende URL's:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension-br"></a>Kan ik de Azure Backup-agent installeren op een virtuele machine van Azure waarvan de Azure Backup-service al een back-up heeft gemaakt met de VM-extensie? <br/>
Absoluut. Azure Backup biedt back-ups op VM-niveau voor de virtuele machines van Azure door gebruik te maken van de VM-extensie. Installeer de Azure Backup-agent op het Windows-gastbesturingssysteem om bestanden en mappen op dat gastbesturingssysteem te beveiligen.

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm-br"></a>Kan ik de Azure Backup-agent op een virtuele machine van Azure installeren om back-ups van bestanden en mappen te maken die zich in de tijdelijke opslag van de virtuele machine van Azure bevinden? <br/>
Ja. Installeer de Azure Backup-agent op het Windows-gastbesturingssysteem en maak back-ups van bestanden en mappen naar de tijdelijke opslag. Houd er rekening mee dat back-ups mislukken als de tijdelijke opslaggegevens zijn gewist. Bovendien kunt u alleen herstelbewerkingen naar een niet-vluchtige opslag uitvoeren als de tijdelijke opslaggegevens zijn verwijderd.

## <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-now-install-scdpm-to-work-with-azure-backup-agent-to-protect-on-premises-applicationvm-workloads-to-azure-br"></a>Ik heb de Azure Backup-agent geïnstalleerd om bestanden en mappen te beveiligen. Kan ik SCDPM nu installeren voor de samenwerking met de Azure Backup-agent om on-premises toepassings-/VM-workloads naar Azure te beveiligen? <br/>
Installeer eerst System Center Data Protection Manager (DPM) en vervolgens de Azure Backup-agent om Azure Backup met DPM te gebruiken. Als u de Azure Backup-onderdelen in deze volgorde installeert, zorgt u ervoor dat de Azure Backup-agent werkt met DPM. Het installeren van de Azure Backup-agent vóór DPM wordt niet geadviseerd of ondersteund.

## <a name="what-is-the-length-of-file-path-that-can-be-specified-as-part-of-azure-backup-policy-using-azure-backup-agent-br"></a>Wat is de lengte van het bestandspad dat met de Azure Backup-agent kan worden opgegeven als onderdeel van het Azure Backup-beleid? <br/>
De Azure Backup-agent is afhankelijk van NTFS. De [lengtespecificatie van het bestandspad wordt beperkt door de Windows-API](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Wanneer er een back-up wordt gemaakt van bestanden met een bestandspad dat langer is dan door de Windows-API is toegestaan, kunt u ervoor kiezen om een back-up van de bovenliggende map of van het schijfstation van de back-upbestanden te maken.  

## <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Welke tekens zijn toegestaan in het bestandspad van het Azure Backup-beleid met Azure Backup-agent? <br>
 De Azure Backup-agent is afhankelijk van NTFS. Er worden [NTFS-ondersteunde tekens](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) ingeschakeld als onderdeel van de bestandsspecificatie.  

## <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Kan ik de Azure Backup-server gebruiken om een BMR-back-up (Bare Metal Recovery) te maken voor een fysieke server? <br/>
Ja.

## <a name="can-i-configure-the-backup-service-to-send-mail-if-a-backup-job-fails-br"></a>Kan ik de Backup-service zodanig configureren dat er een mail wordt verzonden wanneer de back-uptaak mislukt? <br/>
Ja, de Backup-service beschikt over enkele waarschuwingen op basis van gebeurtenissen die kunnen worden gebruikt met een PowerShell-script. Zie [Meldingen configureren](backup-azure-monitor-vms.md#configure-notifications) voor een volledige beschrijving

## <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Geldt er een limiet voor de grootte van elke gegevensbron waarvan een back-up wordt gemaakt? <br/>
Er geldt geen limiet voor de hoeveelheid gegevens waarvan u een back-up kunt maken naar een kluis. Azure Backup beperkt de maximumgrootte voor de gegevensbron. Deze limieten zijn echter extreem hoog. Vanaf augustus 2015 geldt er een maximale gegevensbrongrootte voor de ondersteunde besturingssystemen van:

| S.Nee | Besturingssysteem | Maximale grootte van de gegevensbron |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 of hoger |54.400 GB |
| 2 |Windows 8 of hoger |54.400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

In de volgende tabel wordt uitgelegd hoe de grootte voor elke gegevensbron wordt bepaald.

| Gegevensbron | Details |
|:---:|:--- |
| Volume |De hoeveelheid gegevens van één volume van een server of clientcomputer waarvan een back-up wordt gemaakt. |
| Virtuele Hyper-V-machine  |De som van de gegevens van alle VHD's van de virtuele machine waarvan een back-up wordt gemaakt. |
| Microsoft SQL Server-database |De grootte van één SQL-database waarvan een back-up wordt gemaakt. |
| Microsoft SharePoint |De som van de inhoud en configuratiedatabases in een SharePoint-farm waarvan een back-up wordt gemaakt. |
| Microsoft Exchange |De som van alle Exchange-databases in een Exchange-server waarvan een back-up wordt gemaakt. |
| BMR/systeemstatus |Elke afzonderlijke kopie van de BMR of systeemstatus van de machine waarvan een back-up wordt gemaakt. |

## <a name="are-there-limits-on-the-number-of-times-a-backup-job-can-be-scheduled-per-daybr"></a>Gelden er limieten voor het aantal keer per dag dat een back-uptaak kan worden gepland?<br/>
Ja, u kunt maximaal drie keer per dag back-uptaken op Windows Server of de Windows-client uitvoeren. Op System Center DPM kunt u maximum twee back-uptaken per dag uitvoeren. Voor IaaS VM's kunt u maximaal één keer per dat een back-uptaak uitvoeren.

## <a name="is-there-a-difference-between-the-scheduling-policy-for-dpm-and-windows-server-ie-on-windows-server-without-dpm-br"></a>Is er een verschil tussen het planningsbeleid voor DPM en Windows Server (dat wil zeggen op Windows Server zonder DPM)? <br/>
Ja. Als u DPM gebruikt, kunt u een dagelijkse, wekelijkse, maandelijkse en jaarlijkse planning. Voor Windows Server (zonder DPM) kunt u alleen dagelijkse en wekelijkse planningen opgeven.

## <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-ie-on-windows-server-without-dpmbr"></a>Is er een verschil tussen het bewaarbeleid voor DPM en Windows Server of de Windows-client (dat wil zeggen op Windows Server zonder DPM)?<br/>
Nee, u kunt zowel voor DPM als voor Windows Server of de Windows-client een dagelijks, wekelijks, maandelijks en jaarlijks bewaarbeleid instellen.

## <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Kan ik mijn bewaarbeleidsregels selectief configureren, oftewel wekelijks en dagelijks configureren, maar niet jaarlijks en maandelijks?<br/>
Ja, de bewaarstructuur van Azure Backup biedt u de volledige flexibiliteit om het bewaarbeleid te configureren overeenkomstig uw vereisten.

## <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Kan ik 'een back-up plannen' om 18:00 uur en voor de 'bewaarbeleidsregels' een ander tijdstip opgeven?<br/>
Nee. Het bewaarbeleid kan alleen worden toegepast op back-uppunten. Het bewaarbeleid in de volgende afbeelding is opgegeven voor de back-ups die tussen 12:00 uur en 18:00 uur zijn gemaakt. <br/>

![Back-up en retentie plannen](./media/backup-azure-backup-faq/Schedule.png)
<br/>

## <a name="is-an-incremental-copy-transferred-for-the-retention-policies-scheduled-br"></a>Wordt er een incrementele kopie overgedragen voor de geplande bewaarbeleidsregels? <br/>
Nee, de incrementele kopie wordt verzonden op basis van de tijd die worden vermeld op de pagina met het back-upschema. Er wordt op basis van het bewaarbeleid bepaald welke punten kunnen worden bewaard.

## <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Als een back-up langdurig wordt bewaard, duurt het dan langer om een oud gegevenspunt te herstellen? <br/>
 Nee, de tijd die nodig is om het oudste of nieuwste punt te herstellen, is hetzelfde. Elk herstelpunt gedraagt zich als een volledig punt.

## <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Als elk herstelpunt een volledig punt is, heeft dit dan invloed op de totale factureerbare back-upopslag?<br/>
Producten met een lange bewaartermijn slaan de back-upgegevens doorgaans op als volledige punten. De volledige punten maken *inefficiënt* gebruik van de opslagruimte, maar kunnen wel gemakkelijker en sneller worden hersteld. Incrementele kopieën maken *efficiënt* gebruik van de opslagruimte, maar vereisen dat u een keten van gegevens hersteld, wat van invloed is op de hersteltijd. De opslagarchitectuur van Azure Backup biedt u het beste van beide werelden door de gegevens optimaal op te slaan voor snelle herstelbewerkingen en lage opslagkosten. Deze benadering van gegevensopslag zorgt ervoor dat uw bandbreedte voor inkomend en uitgaand verkeer efficiënt wordt gebruikt. Zowel de hoeveelheid gegevens die moet worden opgeslagen als de benodigde tijd om de gegevens te herstellen, blijft tot een minimum beperkt. Lees waarom het gebruik van [incrementele back-ups](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) efficiënt is.

## <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Geldt er een limiet voor het aantal herstelpunten dat kan worden gemaakt?<br/>
Nee. De limieten voor herstelpunten zijn verwijderd. U kunt zoveel herstelpunten maken als u wilt.

## <a name="why-is-the-amount-of-data-transferred-in-backup-not-equal-to-the-amount-of-data-i-backed-upbr"></a>Waarom is de hoeveelheid gegevens die wordt overgedragen naar de back-up niet gelijk aan de hoeveelheid gegevens waarvan ik een back-up heb gemaakt?<br/>
 Alle gegevens waarvan een back-up is gemaakt via Azure Backup-agent, SCDPM of Azure Backup-server, worden gecomprimeerd en versleuteld voordat ze worden overgedragen. Zodra de gegevens zijn gecomprimeerd en versleuteld, is de back-upkluis 30 tot 40% kleiner.

## <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-servicebr"></a>Is er een manier om de hoeveelheid bandbreedte aan te passen die door de Backup-service wordt gebruikt?<br/>
 Ja, u kunt in de Backup-agent de optie **Eigenschappen wijzigen** gebruiken om de bandbreedte aan te passen. U kunt de hoeveelheid bandbreedte aanpassen, evenals de tijden waarop deze bandbreedte wordt gebruikt. Zie **[Netwerkbeperking inschakelen](backup-configure-vault.md#enable-network-throttling)** in het artikel [Een back-up van een Windows-server of -client maken op Azure met behulp van het Resource Manager-implementatiemodel] voor stapsgewijze instructies.

## <a name="my-internet-bandwidth-is-limited-for-the-amount-of-data-i-need-to-back-up-is-there-a-way-i-can-move-data-to-a-certain-location-with-a-large-network-pipe-and-push-that-data-into-azure-br"></a>Mijn internetbandbreedte wordt beperkt voor de hoeveelheid gegevens waarvan ik een back-up moet maken. Is er een manier om gegevens via een grote netwerkpipe naar een bepaalde locatie te verplaatsen en die gegevens naar Azure te pushen? <br/>
U kunt een back-up van gegevens naar Azure maken via het standaard onlineback-upprocess, maar u kunt ook de Azure Import/Export-service gebruiken om gegevens naar de Blob Storage in Azure over te dragen. Er zijn geen aanvullende manieren om gegevens waarvan een back-up is gemaakt naar Azure Storage te verplaatsen. Zie het artikel [Offlineback-upwerkstroom](backup-azure-backup-import-export.md) voor meer informatie over het gebruik van de Azure Import/Export-service met Azure Backup.

## <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Hoeveel herstelbewerkingen kan ik uitvoeren voor de gegevens waarvan een back-up naar Azure is gemaakt?<br/>
Er is geen limiet voor het aantal herstelbewerkingen vanuit Azure Backup.

## <a name="do-i-have-to-pay-for-the-egress-traffic-from-azure-data-center-during-recoveriesbr"></a>Als ik herstelbewerkingen uitvoer, moet ik dan betalen voor het uitgaande verkeer vanuit het Azure-datacenter?<br/>
 Nee. Uw herstelbewerkingen zijn gratis en er worden geen kosten in rekening gebracht voor het uitgaande verkeer.

## <a name="is-the-data-sent-to-azure-encrypted-br"></a>Worden de gegevens die naar Azure worden verzonden, versleuteld? <br/>
Ja. Ja, de gegevens worden versleuteld op de on-premises server-/client-/SCDPM-machine met AES256 en de gegevens worden verzonden via een beveiligde HTTPS-koppeling.

## <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Worden de back-upgegevens op Azure ook versleuteld?<br/>
 Ja. De gegevens die naar Azure worden verzonden, blijven versleuteld (at rest). De back-upgegevens worden nooit door Microsoft ontsleuteld. Azure Backup is afhankelijk van de versleuteling van de virtuele machine bij het maken van een back-up van een virtuele Azure-machine. Als de virtuele machine bijvoorbeeld is versleuteld met Azure Disk Encryption of een andere versleutelingstechnologie, gebruikt Azure Backup die versleuteling om uw gegevens te beveiligen.

## <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Wat is de minimale lengte van de versleutelingssleutel die wordt gebruikt om de back-upgegevens te versleutelen? <br/>
 De versleutelingssleutel moet minimaal 16 tekens lang zijn.

## <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Wat gebeurt er als ik de versleutelingssleutel kwijtraak? Kan ik of Microsoft de gegevens herstellen? <br/>
Alleen de klant beschikt over de sleutel die wordt gebruikt om de back-upgegevens te versleutelen. Microsoft bewaart geen kopie in Azure en heeft geen toegang tot de sleutel. Als de klant de sleutel kwijtraakt, kan Microsoft back-upgegevens niet herstellen.

## <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agentbr"></a>Hoe kan ik de cachelocatie wijzigen die is opgegeven voor de Azure Backup-agent?<br/>
 Ga in de aangegeven volgorde door de onderstaande lijst met opsommingstekens om de cachelocatie te wijzigen.

* Stop de Backup-engine door de volgende opdracht uit te voeren via een opdrachtprompt met verhoogde bevoegdheid:

  ```PS C:\> Net stop obengine```
* Verplaats de bestanden niet. Kopieer in plaats daarvan de map met de cacheruimte naar een ander station met voldoende ruimte. De oorspronkelijke cacheruimte kan worden verwijderd nadat is bevestigd dat de back-ups werken met de nieuwe cacheruimte.
* Werk de volgende registervermeldingen bij met het pad naar de nieuwe map van de cacheruimte.<br/>

| Registerpad | Registersleutel | Waarde |
| --- | --- | --- |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Nieuwe locatie van de cachemap* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Nieuwe locatie van de cachemap* |

* Start de Backup-engine opnieuw door de volgende opdracht uit te voeren via een opdrachtprompt met verhoogde bevoegdheid:

  ```PS C:\> Net start obengine```

  Zodra de back-up op de nieuwe cachelocatie is gemaakt, kunt u de oorspronkelijke cachemap verwijderen.

## <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expectedbr"></a>Waar moet ik de cachemap voor de Azure Backup-agent plaatsen om ervoor te zorgen dat deze naar verwachting werkt?<br/>
De volgende locaties worden niet aanbevolen voor de cachemap:

* Netwerkshare of verwisselbare media: de cachemap moet zich lokaal op de server bevinden waarvan een back-up via een onlineback-up moet worden gemaakt. Netwerklocaties of verwisselbare media zoals USB-stations worden niet ondersteund.
* Offlinevolumes: de cachemap moet online zijn voor de verwacht back-up met de Azure Backup-agent.

## <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supportedbr"></a>Zijn er kenmerken van de cachemap die niet worden ondersteund?<br/>
 De volgende kenmerken of combinaties van kenmerken worden niet ondersteund voor de cachemap:

* Versleuteld
* Ontdubbeld
* Gecomprimeerd
* Sparse
* Reparsepunt

De cachemap en de metagegevens-VHD hebben beide niet de juiste kenmerken voor de Azure Backup-agent.

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Recovery Services-kluizen zijn op basis van Resource Manager. Worden Backup-kluizen (klassieke modus) nog steeds ondersteund? <br/>
Ja, Backup-kluizen worden nog steeds ondersteund. U maakt Backup-kluizen in de [klassieke portal](https://manage.windowsazure.com). Recovery Services-kluizen worden gemaakt in de [Azure Portal](https://portal.azure.com). We raden u echter aan Recovery Services-kluizen te maken, aangezien alle toekomstige verbeteringen alleen beschikbaar zullen zijn in Recovery Services-kluizen.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Kan ik een Backup-kluis migreren naar een Recovery Services-kluis? <br/>
Op dit moment is het niet mogelijk om de inhoud van een Backup-kluis te migreren naar een Recovery Services-kluis. We zijn bezig met het toevoegen van de functionaliteit, maar deze is momenteel nog niet beschikbaar.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Ondersteunen Recovery Services-kluizen klassieke virtuele machines of virtuele machines op basis van Resource Manager? <br/>
Recovery Services-kluizen ondersteunen beide modellen.  U kunt een back-up naar een Recovery Services-kluis maken van een klassieke virtuele machine die in de klassieke portal is gemaakt of van een virtuele Resource Manager-machine die in Azure Portal is gemaakt.

## <a name="i-have-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Ik heb in een back-upkluis een back-up gemaakt van mijn klassieke virtuele machines. Kan ik mijn virtuele machines migreren van de klassieke modus naar de Resource Manager-modus en ze beschermen in een Recovery Services-kluis?
Herstelpunten van klassieke virtuele machines in een back-upkluis worden niet automatisch naar de Recovery Services-kluis gemigreerd wanneer u de virtuele machine van de klassieke naar de Resource Manager-modus migreert. Volg deze stappen om de back-ups van uw virtuele machine over te dragen:

1. Ga in de back-upkluis naar het tabblad **Beveiligde items** en selecteer de virtuele machine. Klik op [Beveiliging stoppen](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Laat de optie *Gekoppelde back-upgegevens verwijderen* **uitgeschakeld**.
2. Migreer de virtuele machines van de klassieke modus naar de Resource Manager-modus. Zorg ervoor dat de opslagruimte en het netwerk die corresponderen met de virtuele machine ook naar de Resource Manager modus worden gemigreerd.
3. Maak een Recovery Services-kluis en configureer de back-up op de gemigreerde virtuele machine met behulp van de actie **Back-up** bovenaan in het dashboard van de kluis. Zie het artikel [Virtuele Azure-machines beveiligen met een Recovery Services-kluis](backup-azure-vms-first-look-arm.md) voor meer informatie over het maken van back-ups van virtuele machines naar een Recovery Services-kluis.



<!--HONumber=Feb17_HO1-->


