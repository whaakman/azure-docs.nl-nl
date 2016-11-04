---
title: Veelgestelde vragen over Azure Backup | Microsoft Docs
description: Antwoorden op veelgestelde vragen over de Backup-service, Backup-agent, het maken van back-ups en de retentie, het herstel, de beveiliging en andere veelgestelde vragen over het terugzetten van een back-up en herstel na noodgeval.
services: backup
documentationcenter: ''
author: markgalioto
manager: jwhit
editor: ''
keywords: back-up en herstel na noodgeval; Backup-service

ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/29/2016
ms.author: trinadhk; giridham; arunak; markgal; jimpark;

---
# <a name="azure-backup-service--faq"></a>Veelgestelde vragen over de Azure Backup-service
> [!div class="op_single_selector"]
> * [Veelgestelde vragen over Backup voor de klassieke modus](backup-azure-backup-faq.md)
> * [Veelgestelde vragen over Backup voor de Resource Manager-modus](backup-azure-backup-ibiza-faq.md)
> 
> 

Dit artikel bevat een overzicht met veelgestelde vragen (en de bijbehorende antwoorden) over de Azure Backup-service. De vragen worden doorgaans vrij snel door de community beantwoordt, en als een bepaalde vraag veelvuldig wordt gesteld, wordt deze toegevoegd aan dit artikel. De antwoorden op vragen bevatten doorgaans naslag- en ondersteuningsinformatie. Vragen over Azure Backup kunt u stellen in de sectie Disqus van dit of een verwant artikel. U kunt ook in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) vragen over de Azure Backup-service plaatsen.

## Wat is de lijst met ondersteunde besturingssystemen waarmee ik een back-up naar Azure met Azure Backup kan maken? <br/>
Azure Backup ondersteunt de volgende besturingssystemen voor back-ups van bestanden en mappen, en back-ups van toepassingen met behulp van Azure Backup en SCDPM. 

| Besturingssysteem | Platform | SKU |
|:--- | --- |:--- |
| Windows 8 en de meest recente SP's |64 bits |Enterprise, Pro |
| Windows 7 en de meest recente SP's |64 bits |Ultimate, Enterprise, Professional, Home Premium Home Basic, Starter |
| Windows 8.1 en de meest recente SP's |64 bits |Enterprise, Pro |
| Windows 10 |64 bits |Enterprise, Pro, Home |
| Windows Server 2012 R2 en de meest recente SP's |64 bits |Standard, Datacenter, Foundation |
| Windows Server 2012 en de meest recente SP's |64 bits |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 en de meest recente SP's |64 bits |Standard, Workgroup |
| Windows Storage Server 2012 en de meest recente SP's |64 bits |Standard, Workgroup |
| Windows Server 2012 R2 en de meest recente SP's |64 bits |Essential |
| Windows Server 2008 R2 SP1 |64 bits |Standard, Enterprise, Datacenter, Foundation |
| Windows Server 2008 SP2 |64 bits |Standard, Enterprise, Datacenter, Foundation |

Voor Azure VM Backup:

* **Linux**: Azure Backup ondersteunt [een lijst met distributies die zijn goedgekeurd door Azure](../virtual-machines/virtual-machines-linux-endorsed-distros.md), behalve Core OS Linux.  Andere Bring-Your-Own-Linux-distributies werken mogelijk ook, mits de VM-agent beschikbaar is op de virtuele machine en ondersteuning voor Python aanwezig is.
* **Windows Server**: versies ouder dan Windows Server 2008 R2 worden niet ondersteund.

## Waar kan ik de meest recente Azure Backup-agent downloaden? <br/>
U kunt [hier](http://aka.ms/azurebackup_agent) de meest recente agent downloaden voor back-ups van Windows Server, System Center DPM of Windows-client. Als u een back-up van een virtuele machine wilt maken, gebruikt u de VM-agent (die automatisch de juiste extensie installeert). Virtuele machines die zijn gemaakt via de Azure-galerie, beschikken al over de VM-agent.

## Welke versie van SCDPM-server wordt ondersteund? <br/>
U kunt het beste de [nieuwste](http://aka.ms/azurebackup_agent) Azure Backup-agent op het meest recente updatepakket van SCDPM installeren (UR11 van augustus 2016)

## Wanneer ik de Azure Backup-agent configureer, moet ik de kluisreferenties opgeven. Verlopen kluisreferenties?
Ja, de kluisreferenties verlopen na 48 uur. Als het bestand is verlopen, meldt u zich aan bij de Azure Portal en downloadt u de kluisreferentiebestanden uit uw kluis. 

## Is er een limiet voor het aantal kluizen dat voor elk Azure-abonnement kan worden gemaakt? <br/>
Ja. U kunt vanaf september 2016 25 back-upkluizen per abonnement maken. U kunt per abonnement maximaal 25 Recovery Services-kluizen per ondersteunde regio van Azure Backup maken. Als u meer kluizen nodig hebt, maakt u een nieuw abonnement.

## Zijn er beperkingen met betrekking tot het aantal servers/machines dat kan worden geregistreerd voor elke kluis? <br/>
Ja, u kunt maximaal vijftig machines per kluis registreren. Voor virtuele machines van Azure IaaS geldt een limiet van 200 VM's per kluis. Als u meer machines moet registreren, maakt u een nieuwe kluis.

## Hoe kan ik mijn server bij een ander datacenter registreren?<br/>
De back-upgegevens worden verzonden naar het datacenter van de kluis waarbij de server is geregistreerd. De eenvoudigste manier om het datacenter te wijzigen, is de agent te verwijderen en opnieuw te installeren en u vervolgens te registreren bij een nieuwe kluis die behoort bij het gewenste datacenter.

## Wat gebeurt er als ik de naam van een Windows-server wijzig waarmee back-ups van gegevens naar Azure worden opgeslagen?<br/>
Wanneer u de naam van een server wijzigt, worden alle back-ups die momenteel zijn geconfigureerd, gestopt.
U moet de nieuwe naam van de server registreren bij de Backup-kluis. Wanneer u een nieuwe registratie maakt, wordt er met de eerste back-upbewerking een volledige back-up en geen incrementele back-up gemaakt. Als u gegevens moet herstellen waarvan eerder een back-up naar de kluis is gemaakt met de oude servernaam, kunt u deze gegevens herstellen met de optie [**Another server**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) (Andere server) in de wizard **Gegevens herstellen**.

## Van welke typen stations kan ik back-up van bestanden en mappen maken? <br/>
U kunt geen back-up van de volgende stations/volumes maken:

* Verwisselbare media: het station moet als vast station worden gerapporteerd om te kunnen worden gebruikt als bron voor back-upitems.
* Alleen-lezenvolumes: de Volume Shadow Copy Service (VSS) werkt alleen als het volume schrijfbaar is.
* Offlinevolumes: VSS werkt alleen als het volume online is.
* Netwerkshare: er kan alleen een online back-up worden gemaakt wanneer het volume zich lokaal op de server bevindt.
* Met BitLocker beveiligde volumes: het volume moet worden ontgrendeld voordat er een back-up kan worden gemaakt.
* Bestandsysteemidentificatie: NTFS is het enige bestandssysteem dat door deze versie van de onlineback-upservice wordt ondersteund.

## Van welke typen bestanden en mappen op mijn server kan ik een back-up maken?<br/>
De volgende typen worden ondersteund:

* Versleuteld
* Gecomprimeerd
* Sparse
* Gecomprimeerd + Sparse
* Vaste koppelingen: niet ondersteund, worden overgeslagen
* Reparsepunt: niet ondersteund, wordt overgeslagen
* Versleuteld + gecomprimeerd: niet ondersteund, wordt overgeslagen
* Versleuteld + sparse: niet ondersteund, wordt overgeslagen
* Gecomprimeerde stream: niet ondersteund, wordt overgeslagen
* Sparse stream: niet ondersteund, wordt overgeslagen

## Wat is de vereiste minimumgrootte voor de cachemap? <br/>
De grootte van de cachemap bepaalt de hoeveelheid gegevens waarvan u een back-up maakt. Uw cachemap moet 5% van de benodigde ruimte voor de opslag van gegevens zijn.

## Als mijn organisatie één kluis heeft, hoe kan ik bij het herstellen van gegevens de gegevens van de ene server dan isoleren van de gegevens van een andere server?<br/>
Alle servers die zijn geregistreerd bij dezelfde kluis, kunnen de gegevensback-ups herstellen van andere servers *die gebruikmaken van dezelfde wachtwoordzin*. Als u servers hebt waarvan u de back-upgegevens wilt isoleren van andere servers in uw organisatie, gebruikt u een speciale wachtwoordzin voor die servers. U kunt bijvoorbeeld verschillende wachtwoordzinnen voor de human resource-server, de accountingserver en de opslagserver gebruiken.

## Kan ik mijn back-upgegevens- of back-upkluis 'migreren' tussen abonnementen? <br/>
Nee. De kluis is gemaakt op abonnementsniveau en kan, zodra deze is gemaakt, niet opnieuw worden toegewezen aan een ander abonnement.

## Werkt de Azure Backup-agent op een server die gebruikmaakt van Windows Server 2012-gegevensontdubbeling? <br/>
Ja. De agentservice converteert de ontdubbelde gegevens naar normale gegevens wanneer de back-upbewerking wordt voorbereid. Vervolgens worden de gegevens geoptimaliseerd voor het maken van een back-up en versleuteld en worden de versleutelde gegevens verzonden naar de online back-upservice.

## Als ik een back-uptaak annuleer nadat deze is gestart, worden de overgedragen back-upgegevens dan verwijderd? <br/>
Nee. De back-upgegevens die tot het moment van de annulering zijn overgedragen, worden opgeslagen in de back-upkluis. Azure Backup gebruikt een controlepuntmechanisme waarmee tijdens de back-up van tijd tot tijd controlepunten worden toegevoegd aan de back-upgegevens. Omdat de back-upgegevens controlepunten bevatten, kan tijdens het volgende back-upproces de integriteit van de bestanden worden gecontroleerd. De volgende back-up die wordt geactiveerd, is een incrementele back-up van de gegevens waarvan eerder een back-up is gemaakt. Een incrementele back-up maakt efficiënter gebruik van de bandbreedte, zodat u niet telkens dezelfde gegevens hoeft over te dragen.

In geval van een back-up van Azure VM worden, zodra de taak is geannuleerd, de overgedragen gegevens genegeerd en worden met een nieuwe back-up incrementele gegevens van een eerdere back-uptaak overgezet. 

## Waarom wordt er een waarschuwing weergegeven dat er geen Azure back-ups zijn geconfigureerd voor deze server, terwijl ik regelmatige back-ups heb gepland? <br/>
Deze waarschuwing wordt weergegeven wanneer de instellingen voor het back-upschema die zijn opgeslagen op de lokale server niet overeenkomen met de instellingen die zijn opgeslagen in de back-upkluis. Wanneer de server of de instellingen zijn hersteld naar een bekende goede status, worden de back-upschema's mogelijk niet meer gesynchroniseerd. Als u deze waarschuwing ontvangt, [configureert u het back-upbeleid opnieuw](backup-azure-manage-windows-server.md) en **voert u een back-up uit** om de lokale server te synchroniseren met Azure.

## Welke firewallregels moeten worden geconfigureerd voor Azure Backup firewall? <br/>
Voor een probleemloze beveiliging van de on-premises- of workloadgegevens die naar Azure worden verzonden, is het raadzaam om ervoor te zorgen dat uw firewall kan communiceren met de volgende URL's:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

## Kan ik de Azure Backup-agent installeren op een virtuele machine van Azure waarvan de Azure Backup-service al een back-up heeft gemaakt met de VM-extensie? <br/>
Absoluut. Azure Backup biedt back-ups op VM-niveau voor de virtuele machines van Azure door gebruik te maken van de VM-extensie. U kunt de Azure Backup-agent Windows-gastbesturingssysteem installeren om bestanden en mappen dat gastbesturingssysteem te beveiligen.

## Kan ik de Azure Backup-agent op een virtuele machine van Azure installeren om back-ups van bestanden en mappen te maken die zich in de tijdelijke opslag van de virtuele machine van Azure bevinden? <br/>
U kunt de Azure Backup-agent installeren op het gastbesturingssysteem van Windows en back-ups van bestanden en mappen naar de tijdelijke opslag maken. U moet er echter rekening mee houden dat zodra de tijdelijke opslaggegevens zijn mislukt, de back-ups mislukken. Bovendien kunt u alleen herstelbewerkingen naar een niet-vluchtige opslag uitvoeren als de tijdelijke opslaggegevens zijn verwijderd.

## Ik heb de Azure Backup-agent geïnstalleerd om bestanden en mappen te beveiligen. Kan ik SCDPM nu installeren voor de samenwerking met de Azure Backup-agent om on-premises toepassings-/VM-workloads naar Azure te beveiligen? <br/>
Als u Azure Backup wilt gebruiken met SCDPM, is het raadzaam om eerst SCDPM te installeren en pas daarna de Azure Backup-agent te installeren. Dit zorgt ervoor naadloze integratie van de Azure Backup-agent met SCDPM en biedt u de mogelijkheid om de bestanden/mappen, toepassings- en VM-workloads naar Azure rechtstreeks te beveiligen vanuit de beheerconsole van SCDPM. Als u de Azure Backup-agent al hebt geïnstalleerd, wordt de installatie van SCDPM voor de hierboven genoemde doeleinden niet aangeraden of ondersteund.

## Wat is de lengte van het bestandspad dat met de Azure Backup-agent kan worden opgegeven als onderdeel van het Azure Backup-beleid? <br/>
De Azure Backup-agent is afhankelijk van NTFS. De [engtespecificatie van het bestandspad wordt beperkt door de Windows-API](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Wanneer er een back-up wordt gemaakt van bestanden met een bestandspad dat langer is dan door de Windows-API is gespecificeerd, kunnen klanten ervoor kiezen om een back-up van de bovenliggende map of van het schijfstation van de back-upbestanden te maken.  

## Welke tekens zijn toegestaan in het bestandspad van het Azure Backup-beleid met Azure Backup-agent? <br>
 De Azure Backup-agent is afhankelijk van NTFS. Er worden [NTFS-ondersteunde tekens](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) ingeschakeld als onderdeel van de bestandsspecificatie.  

## Kan ik de Azure Backup-server gebruiken om een BMR-back-up (Bare Metal Recovery) te maken voor een fysieke server? <br/>
Ja.

## Kan ik de Backup-service zodanig configureren dat er een mail wordt verzonden wanneer de back-uptaak mislukt? <br/>
Ja, de Backup-service beschikt over enkele waarschuwingen op basis van gebeurtenissen die kunnen worden gebruikt met een PowerShell-script. Zie [Waarschuwingsmeldingen](backup-azure-manage-vms.md#alert-notifications) voor een volledige beschrijving.

## Geldt er een limiet voor de grootte van elke gegevensbron waarvan een back-up wordt gemaakt? <br/>
Hoewel er op kluisniveau geen limiet is voor de hoeveelheid gegevens waarvan u een back-up kunt maken, geldt in Azure Backup wel een beperking (uit praktische overwegingen zijn deze limieten zeer hoog) voor de maximale grootte van de gegevensbron. Vanaf augustus 2015 geldt er een maximale gegevensbrongrootte voor de ondersteunde besturingssystemen van:

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
| Virtuele Hyper-V-machine |De som van de gegevens van alle VHD's van de virtuele machine waarvan een back-up wordt gemaakt. |
| Microsoft SQL Server-database |De grootte van één SQL-database waarvan een back-up wordt gemaakt. |
| Microsoft SharePoint |De som van de inhoud en configuratiedatabases in een SharePoint-farm waarvan een back-up wordt gemaakt. |
| Microsoft Exchange |De som van alle Exchange-databases in een Exchange-server waarvan een back-up wordt gemaakt. |
| BMR/systeemstatus |Elke afzonderlijke kopie van de BMR of systeemstatus van de machine waarvan een back-up wordt gemaakt. |

## Gelden er limieten voor het aantal keer per dag dat een back-uptaak kan worden gepland?<br/>
Ja, u kunt maximaal drie keer per dag back-uptaken op Windows Server of de Windows-client uitvoeren. Op System Center DPM kunt u maximum twee back-uptaken per dag uitvoeren. Voor IaaS VM's kunt u maximaal één keer per dat een back-uptaak uitvoeren.

## Is er een verschil tussen het planningsbeleid voor DPM en Windows Server (dat wil zeggen op Windows Server zonder DPM)? <br/>
Ja. Als u DPM gebruikt, kunt u een dagelijkse, wekelijkse, maandelijkse en jaarlijkse planning. Voor Windows Server (zonder DPM) kunt u alleen dagelijkse en wekelijkse planningen opgeven.

## Is er een verschil tussen het bewaarbeleid voor DPM en Windows Server of de Windows-client (dat wil zeggen op Windows Server zonder DPM)?<br/>
Nee, u kunt zowel voor DPM als voor Windows Server of de Windows-client een dagelijks, wekelijks, maandelijks en jaarlijks bewaarbeleid instellen.

## Kan ik mijn bewaarbeleidsregels selectief configureren, oftewel wekelijks en dagelijks configureren, maar niet jaarlijks en maandelijks?<br/>
Ja, de bewaarstructuur van Azure Backup biedt u de volledige flexibiliteit om het bewaarbeleid te configureren overeenkomstig uw vereisten.

## Kan ik 'een back-up plannen' om 18:00 uur en voor de 'bewaarbeleidsregels' een ander tijdstip opgeven?<br/>
Nee. Het bewaarbeleid kan alleen worden toegepast op back-uppunten. Het bewaarbeleid in de volgende afbeelding is opgegeven voor de back-ups die tussen 12:00 uur en 18:00 uur zijn gemaakt. <br/>

![Back-up en retentie plannen](./media/backup-azure-backup-faq/Schedule.png)
<br/>

## Wordt er een incrementele kopie overgedragen voor de geplande bewaarbeleidsregels? <br/>
Nee, de incrementele kopie wordt verzonden op basis van de tijd die worden vermeld op de pagina met het back-upschema. Er wordt op basis van het bewaarbeleid bepaald welke punten kunnen worden bewaard.

## Als een back-up langdurig wordt bewaard, duurt het dan langer om een oud gegevenspunt te herstellen? <br/>
 Nee, de tijd die nodig is om het oudste of nieuwste punt te herstellen, is hetzelfde. Elk herstelpunt gedraagt zich als een volledig punt.

## Als elk herstelpunt een volledig punt is, heeft dit dan invloed op de totale factureerbare back-upopslag?<br/>
Producten met een lange bewaartermijn slaan de back-upgegevens doorgaans op als volledige punten. De volledige punten maken *inefficiënt* gebruik van de opslagruimte, maar kunnen wel gemakkelijker en sneller worden hersteld. Incrementele kopieën maken *efficiënt* gebruik van de opslagruimte, maar vereisen dat u een keten van gegevens hersteld, wat van invloed is op de hersteltijd. De opslagarchitectuur van Azure Backup biedt u het beste van beide werelden door de gegevens optimaal op te slaan voor snelle herstelbewerkingen en lage opslagkosten. Deze benadering van gegevensopslag zorgt ervoor dat uw bandbreedte voor inkomend en uitgaand verkeer efficiënt wordt gebruikt. Zowel de hoeveelheid gegevens die moet worden opgeslagen als de benodigde tijd om de gegevens te herstellen, blijft tot een minimum beperkt. Lees waarom het gebruik van [incrementele back-ups](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) efficiënt is. 

## Geldt er een limiet voor het aantal herstelpunten dat kan worden gemaakt?<br/>
Nee. De limieten voor herstelpunten zijn verwijderd. U kunt zoveel herstelpunten maken als u wilt.

## Waarom is de hoeveelheid gegevens die wordt overgedragen naar de back-up niet gelijk aan de hoeveelheid gegevens waarvan ik een back-up heb gemaakt?<br/>
 Alle gegevens waarvan een back-up is gemaakt via Azure Backup-agent, SCDPM of Azure Backup-server, worden gecomprimeerd en versleuteld voordat ze worden overgedragen. Zodra de gegevens zijn gecomprimeerd en versleuteld, is de back-upkluis 30 tot 40% kleiner.

## Is er een manier om de hoeveelheid bandbreedte aan te passen die door de Backup-service wordt gebruikt?<br/>
 Ja, u kunt in de Backup-agent de optie **Eigenschappen wijzigen** gebruiken om de bandbreedte aan te passen. U kunt de hoeveelheid bandbreedte aanpassen en de tijden waarop deze bandbreedte wordt gebruikt. Zie [Netwerkbeperking](backup-configure-vault.md#enable-network-throttling) voor meer informatie.

## Mijn internetbandbreedte wordt beperkt voor de hoeveelheid gegevens waarvan ik een back-up moet maken. Is er een manier om gegevens via een grote netwerkpipe naar een bepaalde locatie te verplaatsen en die gegevens naar Azure te pushen? <br/>
U kunt een back-up van gegevens naar Azure maken via het standaard onlineback-upprocess, maar u kunt ook de Azure Import/Export-service gebruiken om gegevens naar de Blob Storage in Azure over te dragen. Er zijn geen aanvullende manieren om gegevens waarvan een back-up is gemaakt naar Azure Storage te verplaatsen. Zie het artikel [Offlineback-upwerkstroom](backup-azure-backup-import-export.md) voor meer informatie over het gebruik van de Azure Import/Export-service met Azure Backup.

## Hoeveel herstelbewerkingen kan ik uitvoeren voor de gegevens waarvan een back-up naar Azure is gemaakt?<br/>
Er is geen limiet voor het aantal herstelbewerkingen vanuit Azure Backup.

## Als ik herstelbewerkingen uitvoer, moet ik dan betalen voor het uitgaande verkeer vanuit het Azure-datacenter?<br/>
 Nee. Uw herstelbewerkingen zijn gratis en er worden geen kosten in rekening gebracht voor het uitgaande verkeer.

## Worden de gegevens die naar Azure worden verzonden, versleuteld? <br/>
Ja. Ja, de gegevens worden versleuteld op de on-premises server-/client-/SCDPM-machine met AES256 en de gegevens worden verzonden via een beveiligde HTTPS-koppeling.

## Worden de back-upgegevens op Azure ook versleuteld?<br/>
 Ja. De gegevens die naar Azure worden verzonden, blijven versleuteld (at rest). De back-upgegevens worden nooit door Microsoft ontsleuteld. Voor Azure VM-back-ups maakt Azure Backup gebruik van versleuteling van de virtuele machine. Dat betekent dat als de virtuele machine is versleuteld met Azure Disk Encryption of een andere versleutelingstechnologie, Azure Backup die versleuteling gebruikt om uw gegevens te beveiligen. 

## Wat is de minimale lengte van de versleutelingssleutel die wordt gebruikt om de back-upgegevens te versleutelen? <br/>
 De versleutelingssleutel moet minimaal 16 tekens lang zijn.

## Wat gebeurt er als ik de versleutelingssleutel kwijtraak? Kan ik of Microsoft de gegevens herstellen? <br/>
Alleen de klant beschikt over de sleutel die wordt gebruikt om de back-upgegevens te versleutelen. Microsoft bewaart geen kopie in Azure en heeft geen toegang tot de sleutel. Als de klant de sleutel kwijtraakt, kan Microsoft back-upgegevens niet herstellen.

## Hoe kan ik de cachelocatie wijzigen die is opgegeven voor de Azure Backup-agent?<br/>
 Ga sequentieel door de lijst door de onderstaande lijst met opsommingstekens om de cachelocatie te wijzigen.

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

## Waar moet ik de cachemap voor de Azure Backup-agent plaatsen om ervoor te zorgen dat deze naar verwachting werkt?<br/>
De volgende locaties worden niet aanbevolen voor de cachemap:

* Netwerkshare of verwisselbare media: de cachemap moet zich lokaal op de server bevinden waarvan een back-up via een onlineback-up moet worden gemaakt. Netwerklocaties of verwisselbare media zoals USB-stations worden niet ondersteund.
* Offlinevolumes: de cachemap moet online zijn voor de verwacht back-up met de Azure Backup-agent.

## Zijn er kenmerken van de cachemap die niet worden ondersteund?<br/>
 De volgende kenmerken of combinaties van kenmerken worden niet ondersteund voor de cachemap:

* Versleuteld
* Ontdubbeld
* Gecomprimeerd
* Sparse
* Reparsepunt

U kunt de bovenstaande kenmerken beter niet instellen voor de cachemap of de VHD met metagegevens. Als u dit wel doet, werkt de Azure Backup-agent mogelijk niet naar verwachting.

<!--HONumber=Oct16_HO3-->


