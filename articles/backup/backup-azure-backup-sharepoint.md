---
title: DPM/Azure Backup server-beveiliging van een SharePoint-farm naar Azure
description: In dit artikel biedt een overzicht van DPM/Azure Backup server-beveiliging van een SharePoint-farm naar Azure
services: backup
author: adigan
manager: Nkolli1
ms.service: backup
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: adigan
ms.openlocfilehash: b3b4d42d9a48d02639019f815cbf4fca15060771
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49946042"
---
# <a name="back-up-a-sharepoint-farm-to-azure"></a>Een back-up maken in Azure van een SharePoint-farm
U back-up van een SharePoint-farm naar Microsoft Azure met behulp van System Center Data Protection Manager (DPM) op ongeveer dezelfde manier dat u back-up van gegevens van andere bronnen. Azure Backup biedt flexibiliteit in de back-upschema maken het dagelijkse, wekelijkse, maandelijkse of jaarlijkse back-up verwijst en biedt u de bewaarperiode beleidsopties voor verschillende back-uppunten. DPM biedt de mogelijkheid voor het opslaan van kopieën van de lokale schijf voor snelle doelstellingen voor hersteltijd (RTO) en voor het opslaan van kopieën naar Azure voor het bewaren van voordelige, op de lange termijn.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint ondersteunde versies en verwante scenario's voor gegevensbescherming
Azure Backup voor DPM ondersteunt de volgende scenario's:

| Workload | Versie | SharePoint-implementatie | Type DPM-implementatie | DPM - System Center 2012 R2 | Beveiliging en herstel |
| --- | --- | --- | --- | --- | --- |
| SharePoint |SharePoint 2013, SharePoint 2010, SharePoint 2007, SharePoint 3.0 |SharePoint geïmplementeerd als een fysieke server of Hyper-V-/ VMware virtuele machine <br> -------------- <br> Sqlalwayson |Fysieke server of on-premises Hyper-V virtuele machine |Ondersteunt back-up naar Azure vanaf updatepakket 5 |Opties voor herstel van SharePoint-Farm beschermen: farm voor herstel, database en bestand of lijstitem van herstelpunten van de schijf.  Farm en databaseherstel uit Azure herstelpunten. |

## <a name="before-you-start"></a>Voordat u begint
Er zijn enkele dingen die u nodig hebt om te bevestigen voordat u back-up van een SharePoint-farm naar Azure.

### <a name="prerequisites"></a>Vereisten
Voordat u doorgaat, zorgt u ervoor dat u voldoet aan alle de [vereisten voor het gebruik van Microsoft Azure Backup](backup-azure-dpm-introduction.md#prerequisites-and-limitations) om workloads te beschermen. Sommige taken voor vereisten zijn: een back-upkluis maken, download de kluisreferenties, Azure Backup Agent installeren en DPM/Azure Backup-Server registreren bij de kluis.

### <a name="dpm-agent"></a>DPM-agent
De DPM-beveiligingsagent moet zijn geïnstalleerd op de server die wordt uitgevoerd, SharePoint, de servers waarop SQL Server en alle andere servers die deel van de SharePoint-farm uitmaken. Zie voor meer informatie over het instellen van de beveiligingsagent [Setup-beveiligingsagent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).  De enige uitzondering hierop is dat u de agent op een server één web-front-end (WFE installeren). DPM moet de agent op één WFE-server alleen om te fungeren als toegangspunt voor de beveiliging.

### <a name="sharepoint-farm"></a>SharePoint-farm
Voor elke 10 miljoen items in de farm, moet er ten minste 2 GB aan ruimte op het volume waar de DPM-map zich bevindt. Deze ruimte is vereist voor het genereren van de catalogus. Genereren van de catalogus wordt voor DPM om specifieke items (siteverzamelingen, sites, lijsten, documentbibliotheken, mappen, afzonderlijke documenten en lijstitems) te herstellen, een lijst van de URL's die zijn opgenomen in elke inhoudsdatabase gemaakt. U kunt de lijst met URL's weergeven in het deelvenster herstelbaar item in de **Recovery** taakgebied van de DPM Administrator-Console.

### <a name="sql-server"></a>SQL Server
DPM wordt uitgevoerd als lokale systeemaccount. Als u wilt back-up van SQL Server-databases, heeft DPM sysadmin-machtigingen op dat account nodig voor de server waarop SQL Server wordt uitgevoerd. NT AUTHORITY\SYSTEM ingesteld op *sysadmin* op de server waarop SQL Server wordt uitgevoerd voordat u een back-up.

Als de SharePoint-farm SQL Server-databases die zijn geconfigureerd met SQL Server-aliassen heeft, installeert u de SQL Server-clientonderdelen op de front-endwebserver die DPM worden beveiligd.

### <a name="sharepoint-server"></a>SharePoint Server
Prestaties zijn afhankelijk van veel factoren zoals de grootte van de SharePoint-farm, als algemene richtlijn kunt één DPM-server beveiligen een 25 TB SharePoint-farm.

### <a name="dpm-update-rollup-5"></a>DPM-updatepakket 5
Om te beginnen met beveiliging van een SharePoint-farm naar Azure, moet u DPM met Update Rollup 5 of hoger installeren. Updatepakket 5 biedt de mogelijkheid om een SharePoint-farm naar Azure als de farm met behulp van SQL AlwaysOn is geconfigureerd.
Voor meer informatie, Zie de blog post introductie van [DPM Update Rollup 5](http://blogs.technet.com/b/dpm/archive/2015/02/11/update-rollup-5-for-system-center-2012-r2-data-protection-manager-is-now-available.aspx)

### <a name="whats-not-supported"></a>Wat wordt er niet ondersteund
* DPM die worden beveiligd met een SharePoint-farm biedt geen bescherming zoekindexen of toepassingsservicedatabases. U moet de beveiliging van deze databases afzonderlijk configureren.
* DPM biedt geen back-up van SharePoint SQL Server-databases die worden gehost op Servershares van scale-out bestandsserver (SOFS).

## <a name="configure-sharepoint-protection"></a>SharePoint-beveiliging configureren
Voordat u DPM kunt u SharePoint beveiligen, moet u de SharePoint VSS Writer-service (WSS Writer-service) configureren met behulp van **ConfigureSharePoint.exe**.

U vindt **ConfigureSharePoint.exe** in de map [DPM-installatiepad] \bin op de front-endwebserver. Dit hulpprogramma bevat de beveiligingsagent met de referenties voor de SharePoint-farm. U voert deze op één WFE-server. Als u meerdere WFE-servers hebt, selecteert u slechts één wanneer u een beveiligingsgroep configureert.

### <a name="to-configure-the-sharepoint-vss-writer-service"></a>De SharePoint VSS Writer-service configureren
1. Op de WFE-server bij een opdrachtprompt, gaat u naar [DPM-installatielocatie] \bin\
2. Voer ConfigureSharePoint - EnableSharePointProtection.
3. Voer de referenties van de farmbeheerder. Dit account moet lid zijn van de lokale groep Administrators op de WFE-server. Als de farmbeheerder geen lokale beheerder van de volgende machtigingen op de WFE-server verlenen:
   * Geef de WSS_Admin_WPG-groep volledige controle naar de DPM-map (% Program Files%\Microsoft Data Protection Manager\DPM).
   * De WSS_Admin_WPG-groep lezen toegang verlenen tot de DPM-registersleutel (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).

> [!NOTE]
> U moet ConfigureSharePoint.exe opnieuw wanneer er sprake is van een wijziging in de referenties van de SharePoint-farm uit te voeren.
> 
> 

## <a name="back-up-a-sharepoint-farm-by-using-dpm"></a>Back-up van een SharePoint-farm met behulp van DPM
Nadat u hebt DPM en de SharePoint-farm zoals hierboven is geconfigureerd, kunt u SharePoint beveiligd door DPM.

### <a name="to-protect-a-sharepoint-farm"></a>Een SharePoint-farm beveiligen
1. Uit de **Protection** tabblad van de DPM Administrator-Console klikt u op **nieuw**.
    ![Nieuw tabblad van de beveiliging](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)
2. Op de **Type beveiligingsgroep selecteren** pagina van de **nieuwe beveiligingsgroep maken** wizard, selecteer **Servers**, en klik vervolgens op **volgende**.
   
    ![Type beveiligingsgroep selecteren](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. Op de **groepsleden selecteren** scherm, schakel het selectievakje in voor de SharePoint-server die u wilt beveiligen en klikt u op **volgende**.
   
    ![Groepsleden selecteren](./media/backup-azure-backup-sharepoint/select-group-members2.png)
   
   > [!NOTE]
   > Met de DPM-agent is geïnstalleerd, ziet u de server in de wizard. DPM toont ook de structuur. Omdat u ConfigureSharePoint.exe hebt uitgevoerd, wordt DPM communiceert met de SharePoint VSS Writer-service en de bijbehorende SQL Server-databases en herkent de structuur van SharePoint-farm, de bijbehorende inhoud databases, en alle bijbehorende items.
   > 
   > 
4. Op de **methode voor gegevensbeveiliging selecteren** pagina, typ de naam van de **beveiligingsgroep**, en selecteer de gewenste *beveiligingsmethodes*. Klik op **Volgende**.
   
    ![Methode voor gegevensbeveiliging selecteren](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)
   
   > [!NOTE]
   > De schijf-methode voor gegevensbeveiliging helpt om te voldoen aan de doelstellingen voor korte moment van herstel. Azure is een efficiënte, langlopende bescherming-doel in vergelijking met tapes. Zie voor meer informatie, [gebruik Azure Backup om uw tape-infrastructuur te vervangen](https://azure.microsoft.com/documentation/articles/backup-azure-backup-cloud-as-tape/)
   > 
   > 
5. Op de **Kortetermijndoelen opgeven** pagina, selecteer de gewenste **bewaartermijn** en als u wilt dat back-ups te identificeren.
   
    ![Korte-termijndoelstellingen opgeven](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)
   
   > [!NOTE]
   > Omdat het herstel is vaak vereist voor gegevens die minder dan vijf dagen oud is, we een bewaartermijn van vijf dagen op schijf geselecteerd en ervoor gezorgd dat de back-up uitgevoerd tijdens niet-productie-uren, in dit voorbeeld.
   > 
   > 
6. Bekijk de opslaggroep schijfruimte toegewezen voor de beveiligingsgroep en klik vervolgens op **volgende**.
7. Voor elke beveiligingsgroep wijst DPM schijfruimte voor het opslaan en beheren van replica's. DPM moet op dit moment een kopie van de geselecteerde gegevens maken. Selecteer hoe en wanneer u wilt dat de replica is gemaakt en klik vervolgens op **volgende**.
   
    ![Methode voor het maken van replica selecteren](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)
   
   > [!NOTE]
   > Om ervoor te zorgen dat het netwerkverkeer wordt getroffen, selecteert u een tijdstip buiten productie-uren.
   > 
   > 
8. DPM zorgt ervoor dat de integriteit van gegevens door het uitvoeren van consistentiecontrole op de replica. Er zijn twee opties beschikbaar. Kunt u een planning wordt uitgevoerd, consistentiecontroles of consistentiecontroles automatisch op de replica door DPM kan worden uitgevoerd wanneer deze inconsistent wordt. Selecteer de gewenste optie en klik vervolgens op **volgende**.
   
    ![Consistentiecontrole](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. Op de **gegevens voor Online beveiliging opgeven** pagina, selecteert u de SharePoint-farm die u wilt beveiligen, en klik vervolgens op **volgende**.
   
    ![DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. Op de **Online back-upschema opgeven** pagina, selecteert u uw voorkeursplanning en klik vervolgens op **volgende**.
    
    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)
    
    > [!NOTE]
    > DPM biedt een maximum van twee dagelijkse back-ups naar Azure op verschillende tijdstippen. Azure Backup kunt ook bepalen voor het bedrag van WAN-bandbreedte die kan worden gebruikt voor back-ups in de piek- en daluren met behulp van [netwerkbeperking voor Azure-back-up](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).
    > 
    > 
11. Afhankelijk van het back-upschema die u hebt geselecteerd op de **Online bewaarbeleid opgeven** pagina, selecteert u het bewaarbeleid voor dagelijkse, wekelijkse, maandelijkse en jaarlijkse back-uppunten.
    
    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)
    
    > [!NOTE]
    > DPM maakt gebruik van een zogenaamde grootvader-vader-zoon bewaarschema waarin een andere bewaarbeleid worden gekozen voor verschillende back-uppunten.
    > 
    > 
12. Net als bij de schijf, een eerste verwijzing punt replica moet worden gemaakt in Azure. De gewenste optie voor het maken van een eerste back-up naar Azure, en klik vervolgens op **volgende**.
    
    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. Controleer de geselecteerde instellingen op de **samenvatting** pagina en klik vervolgens op **groep maken**. U ziet een bericht weergegeven nadat de beveiligingsgroep is gemaakt.
    
    ![Samenvatting](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-dpm"></a>Herstellen van een SharePoint-item van de schijf met behulp van DPM
In het volgende voorbeeld wordt de *herstellen van SharePoint-item* per ongeluk is verwijderd en moeten worden hersteld.
![DPM SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. Open de **DPM Administrator-Console**. Alle SharePoint-farms die zijn beveiligd door DPM worden weergegeven in de **Protection** tabblad.
   
    ![DPM SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. Als u wilt beginnen met het herstellen van het item, selecteer de **Recovery** tabblad.
   
    ![DPM SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. U kunt zoeken naar SharePoint voor *herstellen van SharePoint-item* verwijzen met behulp van een zoekopdracht op basis van een jokerteken binnen een herstel bereik.
   
    ![DPM SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Selecteer het juiste herstelpunt in de zoekresultaten, met de rechtermuisknop op het item en selecteer vervolgens **herstellen**.
5. U kunt ook door verschillende herstelpunten bladeren en selecteren van een database of het item dat u wilt herstellen. Selecteer **datum > hersteltijd**, en selecteer vervolgens de juiste **Database > SharePoint-farm > herstelpunt > Item**.
   
    ![DPM SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. Met de rechtermuisknop op het item en selecteer vervolgens **herstellen** openen de **Herstelwizard**. Klik op **Volgende**.
   
    ![Selectie voor herstel controleren](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. Selecteer het type herstel dat u wilt uitvoeren, en klik vervolgens op **volgende**.
   
    ![Hersteltype](./media/backup-azure-backup-sharepoint/select-recovery-type.png)
   
   > [!NOTE]
   > De selectie van **herstellen naar oorspronkelijke** in het voorbeeld herstelt u het item naar de oorspronkelijke SharePoint-site.
   > 
   > 
8. Selecteer de **herstelproces** die u wilt gebruiken.
   
   * Selecteer **herstellen zonder herstelfarm** als de SharePoint-farm is niet gewijzigd en is hetzelfde als het herstelpunt dat wordt hersteld.
   * Selecteer **herstellen met behulp van een herstelfarm** als de SharePoint-farm is gewijzigd sinds het herstelpunt is gemaakt.
     
     ![Herstelproces](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. Een gefaseerde installatie locatie van SQL Server-exemplaar als u wilt herstellen van de database tijdelijk, en bieden een staging-bestandsshare op de DPM-server en de server waarop SharePoint voor het herstellen van het item wordt uitgevoerd.
   
    ![Fasering Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)
   
    DPM voegt de inhoud van de database die als host voor de SharePoint-item naar de tijdelijke SQL Server-exemplaar fungeert. Van de inhoud van de database, de DPM-server herstelt van het item en plaatst deze in de tijdelijke bestandslocatie op de DPM-server. Het herstelde item dat is nu op de faseringslocatie van de DPM-server moet worden geëxporteerd naar de faseringslocatie op de SharePoint-farm.
   
    ![Fasering Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. Selecteer **herstelopties opgeven**, en beveiligingsinstellingen toepassen op de SharePoint-farm of de beveiligingsinstellingen van het herstelpunt dat van toepassing. Klik op **Volgende**.
    
    ![Opties voor herstel](./media/backup-azure-backup-sharepoint/recovery-options.png)
    
    > [!NOTE]
    > U kunt het gebruik van netwerkbandbreedte te beperken. Dit minimaliseert van invloed op de productieserver tijdens de productie-uren.
    > 
    > 
11. Controleer de samenvattingsinformatie en klik vervolgens op **herstellen** om te beginnen met herstel van het bestand.
    
    ![Samenvatting van herstel](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Selecteer nu de **bewaking** tabblad de **DPM Administrator-Console** om weer te geven de **Status** van het herstel.
    
    ![Herstelstatus](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)
    
    > [!NOTE]
    > Het bestand is nu hersteld. U kunt de SharePoint-site om te controleren of het herstelde bestand vernieuwen.
    > 
    > 

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>Een SharePoint-database herstellen van Azure met behulp van DPM
1. Als u wilt herstellen van een SharePoint-inhoudsdatabase, blader door verschillende herstelpunten (zoals eerder beschreven) en selecteer het herstelpunt dat u wilt herstellen.
   
    ![DPM SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. Dubbelklik op de SharePoint-herstelpunt om weer te geven van de beschikbare informatie van de SharePoint-catalogus.
   
   > [!NOTE]
   > Omdat de SharePoint-farm is beveiligd voor langdurige bewaarperioden in Azure, is geen catalogusgegevens (metagegevens) beschikbaar op de DPM-server. Wanneer een SharePoint-inhoudsdatabase point-in-time worden hersteld moet, moet u als gevolg hiervan de SharePoint-farm opnieuw catalogiseren.
   > 
   > 
3. Klik op **opnieuw catalogiseren**.
   
    ![DPM SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)
   
    De **Cloud opnieuw catalogiseren** statusvenster wordt geopend.
   
    ![DPM SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)
   
    Nadat de catalogiseren is voltooid, verandert de status in *succes*. Klik op **Sluiten**.
   
    ![DPM SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. Klik op de SharePoint-object wordt weergegeven in de DPM **Recovery** tabblad om op te halen van de structuur van de inhoud van de database. Met de rechtermuisknop op het item en klik vervolgens op **herstellen**.
   
    ![DPM SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Op dit moment, volgt u de [recovery stappen eerder in dit artikel](#restore-a-sharepoint-item-from-disk-using-dpm) een SharePoint-inhoudsdatabase om schijf te herstellen.

## <a name="faqs"></a>Veelgestelde vragen
Vraag: welke versies van DPM ondersteuning voor SQL Server 2014 en SQL 2012 (SP2)?<br>
A: DPM 2012 R2 met updatepakket 4 ondersteunt zowel.

V: kan ik een SharePoint-item naar de oorspronkelijke locatie herstellen als SharePoint is geconfigureerd met behulp van SQL AlwaysOn (met beveiliging op schijf)?<br>
A: Ja, het item kan worden hersteld naar de oorspronkelijke SharePoint-site.

V: kan ik een SharePoint-database naar de oorspronkelijke locatie herstellen als SharePoint met behulp van SQL AlwaysOn is geconfigureerd?<br>
A: omdat SharePoint-databases zijn geconfigureerd in de SQL AlwaysOn, kan ze niet worden gewijzigd, tenzij de beschikbaarheidsgroep wordt verwijderd. DPM kan niet als gevolg hiervan, een database herstellen naar de oorspronkelijke locatie. U kunt een SQL Server-database naar een ander exemplaar van SQL Server herstellen.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over DPM-beveiliging van SharePoint - Zie [- Videoserie met DPM-beveiliging van SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)
* Beoordeling [releaseopmerkingen voor System Center 2012 - Data Protection Manager](https://technet.microsoft.com/library/jj860415.aspx)
* Beoordeling [opmerkingen bij de Release voor Data Protection Manager in System Center 2012 SP1](https://technet.microsoft.com/library/jj860394.aspx)

