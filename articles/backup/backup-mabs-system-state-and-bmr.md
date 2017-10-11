---
title: Azure Backup-Server systeemstatus beveiligt en herstelt u bare-metal | Microsoft Docs
description: Azure Backup-Server gebruiken om back-up van de systeemstatus en bare metal recovery (BMR) bescherming te bieden.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
keywords: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.targetplatform: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: markgal,masaran
ms.openlocfilehash: 30f70a702d7d9a3e1196c04096708c035e406607
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-system-state-and-restore-to-bare-metal-with-azure-backup-server"></a>Back-up van systeemstatus en bare metal met Azure Backup-Server herstellen

Azure Backup-Server maakt een back-up van systeemstatus en bare metal recovery (BMR) beschermt.

*   **Systeemstatusback-up**: back-ups van besturingssysteembestanden, zodat u herstellen kunt wanneer een computer wordt gestart, maar systeembestanden en het register gaan verloren. Een systeemstatusback-up bevat:
    * Lid van domein: bestanden, registratiedatabase van COM +-klasse, register opstarten
    * Domeincontroller: Windows Server Active Directory (NTDS), bestanden, registratiedatabase van COM +-klasse, register, systeemvolume (SYSVOL) opstarten
    * Computer met clusterservices: clusterservermetagegevens
    * Computer die certificaatservices uitvoert: gegevens van het certificaat
* **Bare metal-back-up**: back-ups van besturingssysteembestanden en alle gegevens op essentiële volumes (met uitzondering van gebruikersgegevens). Een BMR back-up bevat per definitie een systeemstatusback-up. Het biedt beveiliging wanneer een computer start niet en er alles moet herstellen.

De volgende tabel geeft een overzicht van wat u kunt back-up en herstellen. Zie voor gedetailleerde informatie over app-versies die kunnen worden beveiligd met systeemstatus en BMR [wat Azure Backup-Server doet een back-up?](backup-mabs-protection-matrix.md).

|Back-up|Probleem|Herstellen vanuit back-up van Azure Backup-Server|Herstellen van een systeemstatusback-up|BMR|
|----------|---------|---------------------------|------------------------------------|-------|
|**Bestandsgegevens**<br /><br />Regelmatig back-up<br /><br />BMR/systeemstatus|Verloren bestandsgegevens|J|N|N|
|**Bestandsgegevens**<br /><br />Azure Backup-Server back-up van gegevens uit een bestand<br /><br />BMR/systeemstatus|Verloren of beschadigd besturingssysteem|N|J|J|
|**Bestandsgegevens**<br /><br />Azure Backup-Server back-up van gegevens uit een bestand<br /><br />BMR/systeemstatus|Verloren server (gegevensvolumes intact)|N|N|J|
|**Bestandsgegevens**<br /><br />Azure Backup-Server back-up van gegevens uit een bestand<br /><br />BMR/systeemstatus|Verloren server (gegevensvolumes verloren)|J|Nee|Ja (BMR, gevolgd door normaal herstel van back-up bestandsgegevens)|
|**SharePoint-gegevens**:<br /><br />Azure Backup-Server back-up van Farmgegevens<br /><br />BMR/systeemstatus|Verloren site, lijsten, lijstitems, documenten|J|N|N|
|**SharePoint-gegevens**:<br /><br />Azure Backup-Server back-up van Farmgegevens<br /><br />BMR/systeemstatus|Verloren of beschadigd besturingssysteem|N|J|J|
|**SharePoint-gegevens**:<br /><br />Azure Backup-Server back-up van Farmgegevens<br /><br />BMR/systeemstatus|Herstel na noodgevallen|N|N|N|
|Windows Server 2012 R2 Hyper-V<br /><br />Azure Backup-Server back-up van de Hyper-V-host of Gast<br /><br />BMR/systeemstatusback-up van host|Verloren VM|J|N|N|
|Hyper-V<br /><br />Azure Backup-Server back-up van de Hyper-V-host of Gast<br /><br />BMR/systeemstatusback-up van host|Verloren of beschadigd besturingssysteem|N|J|J|
|Hyper-V<br /><br />Azure Backup-Server back-up van de Hyper-V-host of Gast<br /><br />BMR/systeemstatusback-up van host|Verloren Hyper-V-host (virtuele machines intact)|N|N|J|
|Hyper-V<br /><br />Azure Backup-Server back-up van de Hyper-V-host of Gast<br /><br />BMR/systeemstatusback-up van host|Verloren Hyper-V-host (virtuele machines verloren)|N|N|J<br /><br />BMR, gevolgd door normaal herstel van de Azure Backup-Server|
|SQL Server/Exchange<br /><br />Back-up van Azure back-upserver van app<br /><br />BMR/systeemstatus|Verloren app-gegevens|J|N|N|
|SQL Server/Exchange<br /><br />Back-up van Azure back-upserver van app<br /><br />BMR/systeemstatus|Verloren of beschadigd besturingssysteem|N|Y|J|
|SQL Server/Exchange<br /><br />Back-up van Azure back-upserver van app<br /><br />BMR/systeemstatus|Verloren server (database-transactie logboeken intact)|N|N|J|
|SQL Server/Exchange<br /><br />Back-up van Azure back-upserver van app<br /><br />BMR/systeemstatus|Verloren server (database-transactie logboeken verloren)|N|N|J<br /><br />BMR-herstel, gevolgd door normaal herstel van de Azure Backup-Server|

## <a name="how-system-state-backup-works"></a>De werking van de systeemstatusback-up

Wanneer een systeemstatusback-up wordt uitgevoerd, wordt de back-Server communiceert met Windows Server back-up voor het aanvragen van een back-up van de systeemstatus van de server. Gebruik het station dat de meeste beschikbare vrije ruimte heeft standaard back-up-Server en Windows Server back-up. Informatie over dit station is opgeslagen in het bestand PSDataSourceConfig.xml. Dit is het station dat gebruikmaakt van Windows Server back-up voor back-ups.

U kunt het station dat gebruikmaakt van back-upserver voor de systeemstatusback-up. Ga op de beveiligde server naar C:\Program Files\Microsoft Data Protection Manager\MABS\Datasources. Open het bestand PSDataSourceConfig.xml om te bewerken. Wijzig de \<FilesToProtect\> waarde voor de stationsletter. Sla en sluit het bestand. Als er een beveiligingsgroepset ter bescherming van de systeemstatus van de computer, moet u een consistentiecontrole uitvoeren. Als u een waarschuwing wordt gegenereerd, selecteert u **beveiligingsgroep wijzigen** in de waarschuwing en voltooi de wizard. Voer vervolgens nog een consistentiecontrole uit.

Houd er rekening mee dat als de beveiligingsserver zich in een cluster, is het mogelijk dat een clusterstation wordt geselecteerd als de schijf met de meeste vrije ruimte. Als eigendom van het station is overgeschakeld naar een ander knooppunt en een systeem status back-up wordt uitgevoerd, wordt het station is niet beschikbaar en wordt de back-up is mislukt. In dit scenario PSDataSourceConfig.xml om te verwijzen naar een lokaal station te wijzigen.

Windows Server back-up maakt vervolgens een map met de naam WindowsImageBackup in de hoofdmap van de map terugzetten. Als Windows Server back-up als de back-up wordt gemaakt, wordt alle gegevens in deze map geplaatst. Wanneer de back-up is voltooid, wordt het bestand overgebracht naar de back-Server-computer. Houd rekening met de volgende informatie:

* Deze map en de inhoud ervan worden niet opgeschoond wanneer de back-up of overdracht is voltooid. De beste manier om na te denken hiervan is dat de ruimte wordt gereserveerd voor de volgende keer dat een back-up is voltooid.
* De map wordt gemaakt telkens wanneer een back-up wordt gemaakt. De datum en tijd stempel geven de tijd van de laatste systeemstatusback-up.

## <a name="bmr-backup"></a>BMR-back-up

Voor BMR (met inbegrip van een systeemstatusback-up), wordt de back-uptaak rechtstreeks naar een share opgeslagen op de computer van de back-upserver. Het is niet opgeslagen in een map op de beveiligde server.

Back-upserver roept Windows Server back-up en deelt het replicavolume voor BMR back-up. In dit geval duidelijk niet Windows Server back-up voor het gebruik van het station met de meeste vrije ruimte. In plaats daarvan wordt de share die is gemaakt voor de taak gebruikt.

Wanneer de back-up is voltooid, wordt het bestand overgebracht naar de back-Server-computer. Logboeken worden opgeslagen in C:\Windows\Logs\WindowsServerBackup.

## <a name="prerequisites-and-limitations"></a>Vereisten en beperkingen

-   BMR wordt niet ondersteund voor computers met Windows Server 2003 of voor computers waarop een client-besturingssysteem wordt uitgevoerd.

-   U beveiligen staat BMR en systeemstatus voor dezelfde computer in verschillende beveiligingsgroepen niet.

-   Een back-up-Server-computer beveiligen niet zelf voor BMR.

-   Beveiliging op korte termijn naar tape (schijf naar tape of D2T) wordt niet ondersteund voor BMR. Langdurige opslag op tape (schijf-naar-schijf-naar-tape of D2D2T) wordt ondersteund.

-   Voor BMR-beveiliging, moet Windows Server back-up worden geïnstalleerd op de beveiligde computer.

-   Voor BMR-beveiliging, in tegenstelling tot voor beveiliging van de systeemstatus, back-up-Server heeft geen geen schijfruimtevereisten op de beveiligde computer. Back-ups van overdraagt Windows Server Backup rechtstreeks naar de back-up-Server. De overdrachtstaak back-niet wordt weergegeven in de back-upserver **taken** weergeven.

-   Back-upserver reserveert 30 GB aan ruimte op het replicavolume voor BMR. U kunt dit wijzigen op de **Schijftoewijzing** pagina in de wizard beveiligingsgroep wijzigen of met behulp van de cmdlets Get-DatasourceDiskAllocation en Set-DatasourceDiskAllocation PowerShell. Op het herstelpuntvolume vereist BMR-beveiliging ongeveer 6 GB voor een bewaarperiode van vijf dagen.
    * Houd er rekening mee dat u kunt het replicavolume niet tot minder dan 15 GB reduceren.
    * Back-upserver berekent niet de grootte van de BMR-gegevensbron. Er wordt vanuit gegaan 30 GB voor alle servers. Wijzig de waarde op basis van de grootte van BMR-back-ups die u verwacht in uw omgeving dat. De grootte van een BMR back-up kan ongeveer worden berekend als de som van de gebruikte ruimte op alle essentiële volumes. Essentiële volumes = opstartvolume + systeemvolume + volume die als host fungeert voor systeemstatusgegevens zoals Active Directory.

-   Als u van systeemstatusbeveiliging naar BMR-beveiliging, vereist BMR-beveiliging minder ruimte op de *herstelpuntvolume*. De extra ruimte op het volume is echter niet vrijgemaakt. U kunt de volumegrootte handmatig verkleinen op de **Schijftoewijzing wijzigen** van de wizard beveiligingsgroep wijzigen of via de Get-DatasourceDiskAllocation en Set-DatasourceDiskAllocation PowerShell-cmdlets.

    Als u van systeemstatusbeveiliging naar BMR-beveiliging, vereist BMR-beveiliging meer ruimte op de *replicavolume*. Het volume wordt automatisch uitgebreid. Als u de standaardruimtetoewijzingen wijzigen wilt, gebruikt u de Modify-DiskAllocation PowerShell-cmdlet.

-   Als u van BMR-beveiliging naar systeemstatusbeveiliging wijzigt, moet u meer ruimte op het herstelpuntvolume. Back-upserver wellicht proberen het volume niet automatisch vergroten. Als er onvoldoende ruimte in de opslaggroep, wordt er een fout optreedt.

    Als u van BMR-beveiliging naar systeemstatusbeveiliging wijzigt, moet u ruimte op de beveiligde computer. Dit is omdat de systeemstatusbeveiliging de replica eerst naar de lokale computer schrijft en vervolgens overdraagt naar de back-up-Server.

## <a name="before-you-begin"></a>Voordat u begint

1.  **Azure Backup-Server implementeren**. Controleer of de back-upserver correct is geïmplementeerd. Zie voor meer informatie:
    * [Systeemvereisten voor Azure Backup-Server](http://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites)
    * [Matrix van back-Server-beveiliging](backup-mabs-protection-matrix.md)

2.  **Opslag instellen**. U kunt back-upgegevens opslaan op schijf op de tape en in de cloud met Azure. Zie voor meer informatie [voorbereiden gegevensopslag](https://docs.microsoft.com/system-center/dpm/plan-long-and-short-term-data-storage).

3.  **De beveiligingsagent instellen**. De beveiligingsagent installeren op de computer die u back wilt-up. Zie voor meer informatie [implementeren van de DPM-beveiligingsagent](http://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent).

## <a name="back-up-system-state-and-bare-metal"></a>Back-up van systeemstatus en bare metal
Instellen van een beveiligingsgroep, zoals beschreven in [implementeren beveiligingsgroepen](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups). Houd er rekening mee dat u de status van BMR en systeemstatus voor dezelfde computer in verschillende groepen niet beveiligen. Ook als u BMR selecteert, wordt systeemstatus automatisch ingeschakeld.


1.  Selecteer om de wizard nieuwe beveiligingsgroep maken in de back-up Server Administrator-Console opent, **beveiliging** > **acties** > **beveiligingsgroep maken**.

2.  Op de **Type beveiligingsgroep selecteren** pagina **Servers**, en selecteer vervolgens **volgende**.

3.  Op de **groepsleden selecteren** pagina, vouw de computer uit en selecteer vervolgens **BMR** of **systeemstatus**.

    Houd er rekening mee dat u de status van zowel BMR en systeemstatus voor dezelfde computer in verschillende groepen niet beveiligen. Ook als u BMR selecteert, wordt systeemstatus automatisch ingeschakeld. Zie voor meer informatie [implementeren beveiligingsgroepen](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups).

4.  Op de **methode voor gegevensbeveiliging selecteren** pagina, selecteert u hoe u wilt back-up van de korte en lange termijn te verwerken. Kortetermijnback-up is altijd naar de schijf het eerst met de optie om een back-up van de schijf naar de Azure-cloud met behulp van Azure Backup (korte of lange). Een alternatief voor langetermijnback-up naar de cloud is het instellen van langetermijnback-up naar een zelfstandige apparaat of tape tapewisselaar die verbonden met de back-upserver.

5.  Op de **Kortetermijndoelen selecteren** pagina, selecteert u hoe u wilt back-ups naar kortetermijnopslag op schijf:
    1. Voor **bewaartermijn**, selecteer hoe lang u wilt behouden van de gegevens op schijf. 
    2. Voor **Synchronisatiefrequentie**, selecteert u hoe vaak u wilt uitvoeren van een incrementele back-up naar schijf. Als u niet wilt dat een back-interval instellen, kunt u controleren de **net vóór een herstelpunt** optie. Back-upserver wordt een snelle volledige back-up uitgevoerd net voordat elk herstelpunt is gepland.

6.  Als u opslaan van gegevens op tape voor langdurige opslag wilt op de **langetermijndoelen weergeven** te selecteren hoe lang u tapegegevens wilt bewaren (1-99 jaar). 
    1. Voor **frequentie van back-up**, selecteer hoe vaak back-up naar tape moet worden uitgevoerd. De frequentie is gebaseerd op de bewaartermijn die u hebt geselecteerd:
        * Wanneer de bewaartermijn 1-99 jaar is, kunt u back-ups dagelijks, wekelijks, tweewekelijks, maandelijks, elk kwartaal, elk half jaar of jaarlijks selecteren.
        * Wanneer de bewaartermijn 1-11 maanden is, kunt u back-ups dagelijks, wekelijks, tweewekelijks of maandelijks worden gemaakt.
        * Wanneer de bewaartermijn 1-4 weken is, kunt u back-ups dagelijks of wekelijks.

    2. Op de **Details selecteren Tape en bibliotheek** pagina, selecteert u de tape en bibliotheek te gebruiken en Hiermee wordt aangegeven of gegevens moeten worden gecomprimeerd en versleuteld.

7.  Op de **Schijftoewijzing controleren** pagina, Controleer de opslaggroepschijfruimte die voor de beveiligingsgroep toegewezen.

    1. **Totale grootte van de gegevens** is de grootte van de gegevens die u back wilt-up.
    2. **Schijfruimte die moeten worden ingericht op Azure Backup-Server** is de ruimte die back-upserver voor de beveiligingsgroep aanbeveelt. Back-upserver kiest het ideaal back-volume op basis van de instellingen. U kunt echter de back-upvolume keuzes in bewerken **schijf van de gegevens van schijfopslagtoewijzing**. 
    3. Voor werkbelastingen, selecteer de gewenste opslag in de vervolgkeuzelijst. De waarden voor uw bewerkingen wijzigen **totale opslag** en **vrije opslagruimte** in de **beschikbaar schijfopslag** deelvenster. Underprovisioned ruimte is de hoeveelheid opslagruimte die back-upserver dat u toevoegen aan het volume stelt, om ervoor te zorgen goede back-ups.

8.  Op de **methode voor maken van selecteren Replica** pagina, selecteert u hoe u wilt verwerken van de initiële volledige gegevensreplicatie. Als u kiest voor repliceren over het netwerk, wordt u aangeraden een tijdstip buiten de piekuren te kiezen. Voor grote hoeveelheden gegevens of netwerkomstandigheden die minder dan optimale, houd rekening met de gegevens offline repliceren met behulp van verwijderbare media.

9. Op de **kiest u opties voor consistentiecontrole** pagina, selecteert u hoe u wilt automatiseren consistentiecontroles. U kunt kiezen om te controleren alleen wanneer gegevens van de replica inconsistent is, of volgens een schema. Als u niet voor het configureren van automatische consistentiecontrole wilt, kunt u een handmatige controle uitvoeren op elk gewenst moment. Uitvoeren van een handmatige controle in de **beveiliging** gebied van de back-up Server Administrator-Console met de rechtermuisknop op de beveiligingsgroep en selecteer vervolgens **consistentiecontrole uitvoeren**.

10. Als u hebt geselecteerd voor back-up naar de cloud met behulp van Azure Backup op de **gegevens voor Online beveiliging opgeven** pagina, zorg ervoor dat u de werkbelastingen die u back selecteert wilt-up naar Azure.

11. Op de **Online back-upschema opgeven** pagina Selecteer hoe vaak incrementele back-ups naar Azure wordt uitgevoerd. Back-ups uitvoeren elke dag, week, maand en jaar en selecteer de datum en tijd waarop ze moeten worden uitgevoerd, kunt u plannen. Back-ups kunnen maximaal twee keer per dag plaatsvinden. Telkens wanneer die een back-up wordt uitgevoerd, wordt een gegevens-herstelpunt gemaakt in Azure via de kopie van de back-upgegevens opgeslagen op de schijf van de back-upserver.

12. Op de **Online bewaarbeleid opgeven** te selecteren hoe de herstelpunten die zijn gemaakt van de dagelijkse, wekelijkse, maandelijkse en jaarlijkse back-ups blijven behouden in Azure.

13. Op de **Onlinereplicatie kiezen** te selecteren hoe de initiële volledige replicatie van gegevens plaatsvindt. U kunt repliceren via het netwerk of komen een offline back-up (offline seeding). Offline back-ups maakt gebruik van de functie Azure Import. Zie voor meer informatie [Offline back-werkstroom in Azure Backup](backup-azure-backup-import-export.md).

14. Op de **samenvatting** pagina, Controleer uw instellingen. Nadat u hebt geselecteerd **groep maken**, initiële replicatie van de gegevens zich voordoet. Wanneer replicatie is voltooid, op de **Status** pagina, de beveiligingsstatus van de groep is **OK**. Back-up vindt plaats per de beveiliging vervolgens groepsinstellingen.

## <a name="recover-system-state-or-bmr"></a>Systeemstatus- of BMR herstellen
U kunt BMR of systeemstatus herstellen naar een netwerklocatie. Als u hebt een back-up BMR, gebruikt u Windows Recovery Environment (WinRE) op uw systeem starten en verbinding te maken met het netwerk. Vervolgens gebruikt u Windows Server back-up te herstellen van de netwerklocatie. Als u hebt een back-up van systeemstatus, alleen gebruik van Windows Server back-up te herstellen van de netwerklocatie.

### <a name="restore-bmr"></a>BMR herstellen
Herstel uitvoeren op de servercomputer van de back-up:

1.  In de **herstel** deelvenster, zoek de computer die u wilt herstellen en selecteer vervolgens **Bare Metal Recovery**.

2.  Beschikbare herstelpunten worden vet weergegeven in de kalender. Selecteer de datum en tijd voor het herstelpunt dat u wilt gebruiken.

3.  Op de **Type herstelbewerking selecteren** pagina **kopiëren naar een netwerkmap.**

4.  Op de **bestemming opgeven** pagina waar u de gegevens te kopiëren. Houd er rekening mee dat het geselecteerde doel moet zijn onvoldoende ruimte. U wordt aangeraden dat u een nieuwe map maken.

5.  Op de **herstelopties opgeven** pagina, selecteer de gewenste instellingen. Selecteer of u wilt gebruiken, storage area network (SAN)-gebaseerde hardwaremomentopnamen voor sneller herstel. (Dit is een optie alleen als u een SAN met deze functionaliteit beschikbaar en de mogelijkheid voor het maken en het splitsen van een kloon zodat deze beschrijfbaar. Bovendien moeten de beveiligde computer en de servercomputer van de back-up worden verbonden met hetzelfde netwerk.)

6.  Meldingsopties instellen. Op de **bevestiging** pagina **herstellen**.

De sharelocatie instellen:

1.  Ga naar de map met de back-up op de locatie herstellen.

2.  De map die zich één niveau boven WindowsImageBackup, zodat de hoofdmap van de gedeelde map is de map WindowsImageBackup delen. Als u dit doet, kan herstel de back-up niet gevonden. Als u wilt verbinding maken met behulp van Windows Recovery Environment (WinRE), moet u een share die u in WinRE met de juiste IP-adres en referenties openen kunt.

Het systeem herstellen:

1.  Start de computer waarop u herstellen van de installatiekopie wilt met behulp van de Windows-DVD voor het systeem die u wilt herstellen.

2.  Controleer of de taal en landinstellingen op de eerste pagina. Op de **installeren** pagina **uw computer herstellen**.

3.  Op de **opties voor Systeemherstel** pagina **herstellen van de computer met behulp van een installatiekopie die u eerder hebt gemaakt**.

4.  Op de **Selecteer een systeemkopieback-up** pagina **een systeemkopie selecteren** > **Geavanceerd** > **zoeken naar een andere systeemkopie herstellen in het netwerk**. Als u een waarschuwing wordt weergegeven, selecteert u **Ja**. Ga in het sharepad, voer de referenties in en selecteer vervolgens het herstelpunt. Dit scant naar specifieke back-ups die beschikbaar in dat herstelpunt zijn. Selecteer het herstelpunt dat u wilt gebruiken.

5.  Op de **kiezen hoe de back-up terugzetten** pagina **schijven formatteren en opnieuw partitioneren**. Controleer de instellingen op de volgende pagina. 

6.  Om te beginnen met het herstel, selecteer **voltooien**. Opnieuw opstarten is vereist.

### <a name="restore-system-state"></a>Systeemstatus herstellen

Voer het herstel in de back-upserver van:

1.  In de **herstel** deelvenster, zoek de computer die u wilt herstellen en selecteer vervolgens **Bare Metal Recovery**.

2.  Beschikbare herstelpunten worden vet weergegeven in de kalender. Selecteer de datum en tijd voor het herstelpunt dat u wilt gebruiken.

3.  Op de **Type herstelbewerking selecteren** pagina **kopiëren naar een netwerkmap**.

4.  Op de **bestemming opgeven** pagina waarnaar u wilt kopiëren van de gegevens. Houd er rekening mee dat de geselecteerde bestemming voldoende ruimte nodig. U wordt aangeraden dat u een nieuwe map maken.

5.  Op de **herstelopties opgeven** pagina, selecteer de gewenste instellingen. Selecteer vervolgens of u wilt gebruiken op basis van SAN hardwaremomentopnamen voor sneller herstel. (Dit is een optie alleen als u een SAN met deze functionaliteit en de mogelijkheid om te maken en een kloon om deze beschrijfbaar te splitsen. Daarnaast biedt moeten de beveiligde computer en de back-up-server worden verbonden met hetzelfde netwerk.)

6.  Meldingsopties instellen. Op de **bevestiging** pagina **herstellen**.

Windows Server back-up uitvoeren:

1.  Selecteer **acties** > **herstellen** > **deze Server** > **volgende**.

2.  Selecteer **een andere Server**, selecteer de **Type locatie opgeven** pagina en selecteer vervolgens **externe gedeelde map**. Geef het pad naar de map waarin het herstelpunt.

3.  Op de **Type herstelbewerking selecteren** pagina **systeemstatus**. 

4. Op de **locatie voor Systeemtoestandherstel selecteren** pagina **oorspronkelijke locatie**.

5.  Op de **bevestiging** pagina **herstellen**. De server opnieuw opstarten na het terugzetten.

6.  U kunt ook de systeemstatusherstel uitvoeren bij een opdrachtprompt. U doet dit door Windows Server back-up te starten op de computer die u wilt herstellen. Als u de versie-id bij een opdrachtprompt, typ:```wbadmin get versions -backuptarget \<servername\sharename\>```

    De versie-id gebruiken het systeemstatusherstel te starten. Voer het volgende achter de opdrachtprompt:```wbadmin start systemstaterecovery -version:<versionidentified> -backuptarget:<servername\sharename>```

    U wilt bevestigen dat het herstel te starten. Hier ziet u het proces in het opdrachtpromptvenster. Er wordt een herstellogboek gemaakt. De server opnieuw opstarten na het terugzetten.

