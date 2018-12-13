---
title: Azure Backup Server systeemstatus beveiligt en herstelt u bare-metalcomputers
description: Azure Backup Server gebruiken voor back-up van de systeemstatus en bare metal recovery (BMR) beveiligen.
services: backup
author: rayne-wiselman
manager: carmonm
keywords: ''
ms.service: backup
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: raynew
ms.openlocfilehash: 35ab150670cdc27efcedca233928e0c2184aeca6
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52880094"
---
# <a name="back-up-system-state-and-restore-to-bare-metal-with-azure-backup-server"></a>Back-up van systeemstatus en bare-metalcomputers maken met Azure Backup Server herstellen

Azure Backup-Server een back-up van systeemstatus en bare metal recovery (BMR)-beveiliging biedt.

*   **Systeemstatusback-up**: back-ups van besturingssysteembestanden, zodat u herstellen kunt wanneer een computer wordt gestart, maar u systeembestanden en het register, gaan verloren. Een systeemstatusback-up bevat:
    * Domeinlid: opstartbestanden, registratiedatabase van COM +-klasse, register
    * Domeincontroller: Windows Server Active Directory (NTDS), opstartbestanden, registratiedatabase van COM +-klasse, register, systeemvolume (SYSVOL)
    * Computer waarop clusterservices: metagegevens van de Cluster-server
    * Computer waarop certificaatservices: gegevens van het certificaat
* **Bare metal-back-up**: back-ups van besturingssysteembestanden en alle gegevens op essentiële volumes (met uitzondering van gebruikersgegevens). Een BMR back-up bevat per definitie een systeemstatusback-up. Het biedt beveiliging wanneer een computer niet start en u alles moet herstellen.

De volgende tabel geeft een overzicht van wat u kunt een back-up en herstellen. Zie voor gedetailleerde informatie over app-versies die kunnen worden beveiligd met systeemstatus en BMR [wat Azure Backup Server doet een back-up?](backup-mabs-protection-matrix.md).

|Backup|Probleem|Herstellen vanuit back-up van Azure Backup-Server|Herstellen vanaf systeemstatusback-up|BMR|
|----------|---------|---------------------------|------------------------------------|-------|
|**Gegevens uit een bestand**<br /><br />Standaard gegevensback-up<br /><br />BMR/systeemstatusback-up|Verloren bestandsgegevens|J|N|N|
|**Gegevens uit een bestand**<br /><br />Azure Backup-Server back-up van gegevens uit een bestand<br /><br />BMR/systeemstatusback-up|Verloren of beschadigd besturingssysteem|N|J|J|
|**Gegevens uit een bestand**<br /><br />Azure Backup-Server back-up van gegevens uit een bestand<br /><br />BMR/systeemstatusback-up|Verloren server (gegevensvolumes intact)|N|N|J|
|**Gegevens uit een bestand**<br /><br />Azure Backup-Server back-up van gegevens uit een bestand<br /><br />BMR/systeemstatusback-up|Verloren server (gegevensvolumes verloren)|J|Nee|Ja (BMR, gevolgd door normaal herstel van gegevens uit een back-up-bestand)|
|**SharePoint-gegevens**:<br /><br />Azure Backup-Server back-up van Farmgegevens<br /><br />BMR/systeemstatusback-up|Verloren site, lijsten, lijstitems, documenten|J|N|N|
|**SharePoint-gegevens**:<br /><br />Azure Backup-Server back-up van Farmgegevens<br /><br />BMR/systeemstatusback-up|Verloren of beschadigd besturingssysteem|N|J|J|
|**SharePoint-gegevens**:<br /><br />Azure Backup-Server back-up van Farmgegevens<br /><br />BMR/systeemstatusback-up|Herstel na noodgeval|N|N|N|
|Windows Server 2012 R2 Hyper-V<br /><br />Azure Backup-Server back-up van Hyper-V-host of Gast<br /><br />BMR/systeemstatusback-up van host|Verloren virtuele machine|J|N|N|
|Hyper-V<br /><br />Azure Backup-Server back-up van Hyper-V-host of Gast<br /><br />BMR/systeemstatusback-up van host|Verloren of beschadigd besturingssysteem|N|J|J|
|Hyper-V<br /><br />Azure Backup-Server back-up van Hyper-V-host of Gast<br /><br />BMR/systeemstatusback-up van host|Verloren Hyper-V-host (virtuele machines intact)|N|N|J|
|Hyper-V<br /><br />Azure Backup-Server back-up van Hyper-V-host of Gast<br /><br />BMR/systeemstatusback-up van host|Verloren Hyper-V-host (verloren virtuele machines)|N|N|J<br /><br />BMR, gevolgd door normaal herstel van de Azure Backup Server|
|SQL Server/Exchange<br /><br />Back-up van Azure back-upserver van app<br /><br />BMR/systeemstatusback-up|Verloren appgegevens|J|N|N|
|SQL Server/Exchange<br /><br />Back-up van Azure back-upserver van app<br /><br />BMR/systeemstatusback-up|Verloren of beschadigd besturingssysteem|N|Y|J|
|SQL Server/Exchange<br /><br />Back-up van Azure back-upserver van app<br /><br />BMR/systeemstatusback-up|Verloren server (database/transactielogboekbestanden intact)|N|N|J|
|SQL Server/Exchange<br /><br />Back-up van Azure back-upserver van app<br /><br />BMR/systeemstatusback-up|Verloren server (database/transactielogboekbestanden verloren)|N|N|J<br /><br />BMR-herstel, gevolgd door normaal herstel van de Azure Backup Server|

## <a name="how-system-state-backup-works"></a>De werking van systeemstatusback-up

Wanneer een systeemstatusback-up wordt uitgevoerd, wordt back-Server communiceert met Windows Server back-up om aan te vragen van een back-up van de systeemstatus van de server. Back-up-Server en Windows Server back-up gebruiken standaard het station dat de meeste beschikbare vrije ruimte heeft. Informatie over dit station wordt opgeslagen in het bestand PSDataSourceConfig.xml. Dit is het station dat gebruikmaakt van Windows Server back-up voor back-ups.

U kunt het station dat gebruikmaakt van back-upserver voor de systeemstatusback-up aanpassen. Ga op de beveiligde server naar C:\Program Files\Microsoft Data Protection Manager\MABS\Datasources. Open het bestand PSDataSourceConfig.xml voor bewerken. Wijzig de \<FilesToProtect\> waarde voor de stationsletter. Sla het bestand op en sluit het. Als er een beveiligingsgroepset ter bescherming van de systeemstatus van de computer, moet u een consistentiecontrole uitvoeren. Als u een waarschuwing wordt gegenereerd, selecteert u **beveiligingsgroep wijzigen** in de waarschuwing en voltooi de wizard. Voer vervolgens nog een consistentiecontrole uit.

Houd er rekening mee dat als de beveiligingsserver zich in een cluster, is het mogelijk dat een clusterstation wordt geselecteerd als de schijf met de meeste vrije ruimte. Als eigendom van het station is overgeschakeld naar een ander knooppunt en een system state back-up wordt uitgevoerd, wordt het station is niet beschikbaar en wordt de back-up mislukt. In dit scenario wijzigen PSDataSourceConfig.xml om te verwijzen naar een lokaal station.

Windows Server back-up maakt vervolgens een map met de naam WindowsImageBackup in de hoofdmap van de map voor terugzetten. Als Windows Server back-up maakt de back-up, wordt alle gegevens in deze map geplaatst. Wanneer de back-up is voltooid, wordt het bestand overgebracht naar de back-Server-computer. Let op de volgende informatie:

* Deze map en de inhoud ervan worden niet opgeschoond wanneer de back-up of overdracht is voltooid. De beste manier om na te denken hiervan is dat de ruimte wordt gereserveerd voor de volgende keer dat een back-up is voltooid.
* De map wordt gemaakt telkens wanneer een back-up wordt gemaakt. De datum en tijd stempel de tijd van de laatste systeemstatusback-up weer.

## <a name="bmr-backup"></a>BMR-back-up

Voor BMR (inclusief een systeemstatusback-up), wordt de back-uptaak rechtstreeks naar een share opgeslagen op de servercomputer van de back-up. Het is niet opgeslagen in een map op de beveiligde server.

Back-upserver van Windows Server back-up-aanroepen en deelt het replicavolume voor BMR back-up. In dit geval duidelijk niet Windows Server back-up voor het gebruik van het station met de meeste vrije ruimte. In plaats daarvan wordt de share die is gemaakt voor de taak gebruikt.

Wanneer de back-up is voltooid, wordt het bestand overgebracht naar de back-Server-computer. Logboeken worden opgeslagen in C:\Windows\Logs\WindowsServerBackup.

## <a name="prerequisites-and-limitations"></a>Vereisten en beperkingen

-   BMR wordt niet ondersteund voor computers met Windows Server 2003 of voor computers waarop een client-besturingssysteem wordt uitgevoerd.

-   U kunt BMR en systeemstatus voor dezelfde computer in verschillende beveiligingsgroepen niet beveiligen.

-   Een back-up-Server-computer kan zelf voor BMR beveiligen.

-   Beveiliging op korte termijn naar tape (schijf-naar-tape of D2T) wordt niet ondersteund voor BMR. Langdurige opslag op tape (schijf-naar-schijf-naar-tape of D2D2T) wordt ondersteund.

-   Windows Server back-up moet worden geïnstalleerd op de beveiligde computer voor BMR-beveiliging.

-   Voor BMR-beveiliging, in tegenstelling tot voor beveiliging van de systeemstatus, back-up-Server geen schijfruimtevereisten op de beveiligde computer. Back-ups van overgebracht Windows Server back-up rechtstreeks naar de back-up-Server-computer. De overdrachtstaak back-niet wordt weergegeven in de back-upserver **taken** weergeven.

-   Back-upserver reserveert 30 GB aan ruimte op het replicavolume voor BMR. U kunt dit wijzigen op de **Schijftoewijzing** pagina in de wizard beveiligingsgroep wijzigen of met behulp van de cmdlets Get-DatasourceDiskAllocation en Set-DatasourceDiskAllocation PowerShell. Op het herstelpuntvolume vereist BMR-beveiliging ongeveer 6 GB voor een vijf dagen bewaren.
    * Houd er rekening mee dat u de grootte van het replicavolume tot minder dan 15 GB kunt beperken.
    * Back-upserver berekent niet de grootte van de BMR-gegevensbron. Wordt ervan uitgegaan dat 30 GB voor alle servers. Wijzig de waarde op basis van de grootte van BMR-back-ups die u in uw omgeving verwacht. De grootte van een BMR back-up kan ongeveer worden berekend als de som van de gebruikte ruimte op alle essentiële volumes. Essentiële volumes = opstartvolume + systeemvolume + volume die als host fungeert voor systeemstatusgegevens zoals Active Directory.

-   Als u van systeemstatusbeveiliging naar BMR-beveiliging, vereist BMR-beveiliging minder ruimte op de *herstelpuntvolume*. De extra ruimte op het volume is echter niet vrijgemaakt. U kunt de volumegrootte handmatig verkleinen op de **Schijftoewijzing wijzigen** pagina van de wizard beveiligingsgroep wijzigen of met behulp van de cmdlets Get-DatasourceDiskAllocation en Set-DatasourceDiskAllocation PowerShell.

    Als u van systeemstatusbeveiliging naar BMR-beveiliging, vereist BMR-beveiliging meer ruimte op de *replicavolume*. Het volume wordt automatisch uitgebreid. Als u wijzigen van de standaard ruimtetoewijzingen wilt, gebruikt u de Modify-DiskAllocation PowerShell-cmdlet.

-   Als u van BMR-beveiliging naar systeemstatusbeveiliging, moet u meer ruimte op het herstelpuntvolume. Back-upserver mogelijk probeert automatisch te vergroten het volume. Als er onvoldoende ruimte in de opslaggroep, wordt er een fout optreedt.

    Als u van BMR-beveiliging naar systeemstatusbeveiliging, moet u ruimte op de beveiligde computer. Dit is omdat de systeemstatusbeveiliging de replica eerst naar de lokale computer schrijft en brengt deze naar de back-up-Server-computer.

## <a name="before-you-begin"></a>Voordat u begint

1.  **Azure Backup-Server implementeren**. Controleer of de back-up-Server correct is geïmplementeerd. Zie voor meer informatie:
    * [Systeemvereisten voor Azure Backup Server](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites)
    * [Beveiligingsmatrix voor back-Server](backup-mabs-protection-matrix.md)

2.  **Opslag instellen**. U kunt back-upgegevens opslaan op schijf, op tape en in de cloud met Azure. Zie voor meer informatie, [gegevensopslag voorbereiden](https://docs.microsoft.com/system-center/dpm/plan-long-and-short-term-data-storage).

3.  **De beveiligingsagent instellen**. Installeer de beveiligingsagent op de computer die u back wilt-up. Zie voor meer informatie, [de DPM-beveiligingsagent implementeren](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent).

## <a name="back-up-system-state-and-bare-metal"></a>Back-up van systeemstatus en bare metal
Instellen van een beveiligingsgroep, zoals beschreven in [beveiligingsgroepen implementeren](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups). Houd er rekening mee dat u kunt BMR en systeemstatus voor dezelfde computer in verschillende groepen niet beveiligen. Ook, wanneer u BMR selecteert, systeemstatus automatisch ingeschakeld.


1.  Als u wilt de wizard nieuwe beveiligingsgroep maken in de back-up Server Administrator-Console opent, selecteer **Protection** > **acties** > **beveiligingsgroep maken** .

2.  Op de **Type beveiligingsgroep selecteren** pagina, selecteert u **Servers**, en selecteer vervolgens **volgende**.

3.  Op de **groepsleden selecteren** pagina, vouw de computer uit en selecteer dan **BMR** of **systeemstatus**.

    Houd er rekening mee dat u kunt zowel BMR en systeemstatus voor dezelfde computer in verschillende groepen niet beveiligen. Ook, wanneer u BMR selecteert, systeemstatus automatisch ingeschakeld. Zie voor meer informatie, [beveiligingsgroepen implementeren](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups).

4.  Op de **methode voor gegevensbeveiliging selecteren** pagina, selecteert u hoe u wilt voor het afhandelen van back-ups op korte termijn en op lange termijn. Kortetermijnback-up is altijd naar de schijf het eerst met de optie back-up van de schijf op de Azure-cloud met behulp van Azure Backup (korte of lange termijn). Er is een alternatief voor langetermijnback-up naar de cloud voor het instellen van langetermijnback-up naar een zelfstandige apparaat dat of tapewisselaar tapewisselaar die verbonden met de back-upserver.

5.  Op de **Kortetermijndoelen selecteren** pagina, selecteert u hoe u back-up maken naar kortetermijnopslag op schijf wilt:
    1. Voor **bewaartermijn**, selecteert u hoe lang u wilt de gegevens op schijf bewaren. 
    2. Voor **Synchronisatiefrequentie**, selecteert u hoe vaak u wilt uitvoeren van een incrementele back-up naar schijf. Als u niet om in te stellen van een back-upinterval wilt, kunt u controleren de **net vóór een herstelpunt** optie. Back-upserver wordt een snelle, volledige back-up uitgevoerd net voordat elk herstelpunt is gepland.

6.  Als u opslaan van gegevens op tape voor langdurige opslag wilt op de **langetermijndoelen weergeven** pagina, selecteert u hoe lang u tapegegevens wilt bewaren (1-99 jaar). 
    1. Voor **frequentie van back-up**, selecteer hoe vaak back-up naar tape moet worden uitgevoerd. De frequentie is gebaseerd op de bewaartermijn die u hebt geselecteerd:
        * Wanneer de bewaartermijn 1-99 jaar is, kunt u back-ups dagelijks, wekelijks, tweewekelijks, maandelijks, elk kwartaal, elk half jaar of jaarlijks kunt selecteren.
        * Wanneer de bewaartermijn 1-11 maanden is, kunt u back-ups dagelijks, wekelijks, tweewekelijks of maandelijks.
        * Wanneer de bewaartermijn 1-4 weken is, kunt u back-ups dagelijks of wekelijks.

    2. Op de **Tape selecteren en de Details van tapewisselaar** pagina, selecteert u de tape en bibliotheek te gebruiken en of de gegevens moeten worden gecomprimeerd en versleuteld.

7.  Op de **Schijftoewijzing controleren** pagina, Controleer de schijfruimte voor opslag van toepassingen die voor de beveiligingsgroep toegewezen.

    1. **Totale grootte van de gegevens** is de grootte van de gegevens die u back wilt-up.
    2. **Schijfruimte die moet worden ingericht op Azure Backup Server** is de ruimte die back-up-Server wordt aanbevolen voor de beveiligingsgroep. Back-upserver kiest de ideale back-upvolume op basis van de instellingen. U kunt echter de keuzes van de back-upvolume in bewerken **Schijftoewijzingsdetails**. 
    3. Voor werkbelastingen, selecteer de gewenste opslag in de vervolgkeuzelijst. Uw bewerkingen wijzigen de waarden voor **totale opslag** en **vrije opslagruimte** in de **beschikbare schijfopslag** deelvenster. Ingerichte ruimte is de hoeveelheid opslag die back-upserver dat u toevoegen aan het volume stelt, om ervoor te zorgen goede back-ups.

8.  Op de **Replica methode voor maken** pagina, selecteert u hoe u voor het afhandelen van de eerste volledige gegevensreplicatie wilt. Als u ervoor kiest om te repliceren via het netwerk, wordt u aangeraden een tijdstip buiten de piekuren te kiezen. Voor grote hoeveelheden gegevens of netwerkomstandigheden die minder dan optimale zijn, houd rekening met de gegevens offline repliceren met behulp van verwisselbare media.

9. Op de **kiest u opties voor consistentiecontrole** pagina, selecteert u hoe u wilt automatiseren consistentiecontroles. U kunt een controle uitvoeren alleen wanneer replicagegevens is inconsistent, of volgens een schema. Als u het configureren van automatische consistentiecontrole niet wilt, kunt u een handmatige controle uitvoeren op elk gewenst moment. Om uit te voeren een handmatige controle in de **Protection** gebied van de back-up Server Administrator-Console met de rechtermuisknop op de beveiligingsgroep en selecteer vervolgens **consistentiecontrole uitvoeren**.

10. Als u hebt gekozen om de back-up naar de cloud met behulp van Azure Backup, op de **gegevens voor Online beveiliging opgeven** pagina, zorg ervoor dat u de werkbelastingen die u back selecteert wilt-up naar Azure.

11. Op de **Online back-upschema opgeven** pagina, selecteer hoe vaak incrementele back-ups naar Azure wordt uitgevoerd. Back-ups uitvoeren elke dag, week, maand en jaar en selecteer de datum en tijd waarop u deze wilt uitvoeren, kunt u plannen. Back-ups kunnen maximaal twee keer per dag uitgevoerd. Telkens wanneer die een back-up wordt uitgevoerd, is een herstelpunt voor gegevens gemaakt in Azure vanaf de kopie van de back-upgegevens die zijn opgeslagen op schijf op de back-Server.

12. Op de **Online bewaarbeleid opgeven** pagina, selecteert u hoe de herstelpunten die zijn gemaakt op basis van de dagelijkse, wekelijkse, maandelijkse en jaarlijkse back-ups worden bewaard in Azure.

13. Op de **Kies Onlinereplicatie** pagina, selecteert u hoe de eerste volledige replicatie van gegevens plaatsvindt. U kunt repliceren via het netwerk of een offline back-up (offline-seeding). Offline back-up maakt gebruik van de functie Azure Import. Zie voor meer informatie, [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md).

14. Op de **samenvatting** pagina, Controleer uw instellingen. Nadat u hebt geselecteerd **groep maken**, een eerste replica van de gegevens gemaakt. Wanneer replicatie van gegevens is voltooid, op de **Status** pagina, de status van de beveiliging groep is **OK**. Back-up vindt plaats per de beveiliging vervolgens beveiligingsgroepsinstellingen.

## <a name="recover-system-state-or-bmr"></a>Systeemstatus of BMR herstellen
U kunt BMR of systeemstatus naar een netwerklocatie herstellen. Als u een BMR up hebt gemaakt, gebruikt u Windows Recovery Environment (WinRE) op uw systeem te starten en verbinding met het netwerk. Vervolgens gebruikt u Windows Server back-up te herstellen van de netwerklocatie. Als u een systeemstatus up hebt gemaakt, moet u alleen back-up van Windows Server gebruiken om te herstellen van de netwerklocatie.

### <a name="restore-bmr"></a>BMR herstellen
Herstel uitvoeren op de servercomputer van de back-up:

1.  In de **Recovery** deelvenster, zoek de computer die u wilt herstellen en selecteer vervolgens **Bare Metal Recovery**.

2.  Beschikbare herstelpunten worden vet weergegeven in de kalender. Selecteer de datum en tijd voor het herstelpunt dat u wilt gebruiken.

3.  Op de **Type herstelbewerking selecteren** pagina, selecteert u **kopiëren naar een netwerkmap.**

4.  Op de **bestemming opgeven** weergeeft, schakelt waar u de gegevens te kopiëren. Houd er rekening mee dat de geselecteerde bestemming moet er voldoende ruimte. U wordt aangeraden dat u een nieuwe map maken.

5.  Op de **herstelopties opgeven** pagina, selecteert u de beveiligingsinstellingen om toe te passen. Selecteer of u wilt gebruiken, storage area network (SAN)-op basis van momentopnamen van hardware, voor sneller herstel. (Dit is een optie alleen als u een SAN met deze functionaliteit beschikbaar is en de mogelijkheid om te maken en een kloon splitsen zodat deze schrijfbaar. Daarnaast kunnen moeten de beveiligde computer en de back-up-Server-computer zijn verbonden met hetzelfde netwerk.)

6.  Stel meldingsopties. Op de **bevestiging** weergeeft, schakelt **herstellen**.

De sharelocatie instellen:

1.  In de locatie voor terugzetten, gaat u naar de map met de back-up.

2.  Deel de map die zich één niveau boven WindowsImageBackup, zodat de hoofdmap van de gedeelde map de map WindowsImageBackup is. Als u dit niet doet, kan herstel de back-up niet gevonden. Als u wilt verbinding maken met behulp van Windows Recovery Environment (WinRE), moet u een share die u in WinRE met de juiste IP-adres en referenties openen kunt.

Het systeem herstellen:

1.  Start de computer waarop u herstellen van de installatiekopie wilt met behulp van de Windows-DVD voor het systeem die u wilt herstellen.

2.  Op de eerste pagina, Controleer of de taal en landinstellingen. Op de **installeren** weergeeft, schakelt **uw computer herstellen**.

3.  Op de **opties voor Systeemherstel** weergeeft, schakelt **computer met behulp van een eerder gemaakte systeemkopie herstellen**.

4.  Op de **Selecteer een systeemkopieback-up** pagina, selecteert u **een andere systeemkopie herstellen** > **Geavanceerd** > **een systeemkopie zoeken in het netwerk**. Als u een waarschuwing wordt weergegeven, selecteert u **Ja**. Ga in het sharepad, voer de referenties in en selecteer vervolgens het herstelpunt. Hiermee wordt er gescand voor specifieke back-ups die beschikbaar in dat herstelpunt zijn. Selecteer het herstelpunt dat u wilt gebruiken.

5.  Op de **kiezen hoe u de back-up herstellen** weergeeft, schakelt **schijven formatteren en opnieuw partitioneren**. Controleer de instellingen op de volgende pagina. 

6.  Om te beginnen met het herstellen, selecteert u **voltooien**. Opnieuw opstarten is vereist.

### <a name="restore-system-state"></a>Systeemstatus herstellen

Voer het herstel van back-up-Server:

1.  In de **Recovery** deelvenster, zoek de computer die u wilt herstellen en selecteer vervolgens **Bare Metal Recovery**.

2.  Beschikbare herstelpunten worden vet weergegeven in de kalender. Selecteer de datum en tijd voor het herstelpunt dat u wilt gebruiken.

3.  Op de **Type herstelbewerking selecteren** pagina, selecteert u **kopiëren naar een netwerkmap**.

4.  Op de **bestemming opgeven** weergeeft, schakelt waarnaar u wilt kopiëren van de gegevens. Houd er rekening mee dat de geselecteerde bestemming voldoende ruimte moet. U wordt aangeraden dat u een nieuwe map maken.

5.  Op de **herstelopties opgeven** pagina, selecteert u de beveiligingsinstellingen om toe te passen. Selecteer vervolgens of u wilt gebruiken op basis van SAN hardwaremomentopnamen voor sneller herstel. (Dit is een optie alleen als u een SAN met deze functionaliteit en de mogelijkheid om te maken en een kloon om deze beschrijfbaar te splitsen. Daarnaast kunnen moeten de beveiligde computer en de back-up-server zijn verbonden met hetzelfde netwerk.)

6.  Stel meldingsopties. Op de **bevestiging** weergeeft, schakelt **herstellen**.

Windows Server Backup uitvoeren:

1.  Selecteer **acties** > **herstellen** > **deze Server** > **volgende**.

2.  Selecteer **een andere Server**, selecteer de **Type locatie opgeven** pagina en selecteer vervolgens **externe gedeelde map**. Geef het pad naar de map die het herstelpunt bevat.

3.  Op de **Type herstelbewerking selecteren** pagina, selecteert u **systeemstatus**. 

4. Op de **locatie voor Systeemtoestandherstel selecteren** pagina, selecteert u **oorspronkelijke locatie**.

5.  Op de **bevestiging** weergeeft, schakelt **herstellen**. De server opnieuw opstarten na het herstel.

6.  U kunt ook de systeemstatusherstel uitvoeren bij een opdrachtprompt. Om dit te doen, start u Windows Server back-up op de computer die u wilt herstellen. Als u de versie-id bij een opdrachtprompt, typ: ```wbadmin get versions -backuptarget \<servername\sharename\>```

    Start de systeemstatusherstel met behulp van de versie-id. Voer het volgende achter de opdrachtprompt: ```wbadmin start systemstaterecovery -version:<versionidentified> -backuptarget:<servername\sharename>```

    Bevestig dat u wilt het herstel start. Hier ziet u het proces in het opdrachtpromptvenster. Er wordt een herstellogboek gemaakt. De server opnieuw opstarten na het herstel.

