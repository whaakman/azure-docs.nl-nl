---
title: Maken en herstellen van een back-up in BizTalk Services | Microsoft Docs
description: BizTalk Services omvat back-up en herstel. Meer informatie over het maken en herstellen van een back-up om te bepalen wat opgehaald back-up gemaakt. MABS, WABS
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c55d1ab124441c42101b4ad60924a9ea28231408
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: back-ups maken en herstellen

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services bevat de mogelijkheden van back-up en herstel. Dit onderwerp wordt beschreven hoe u back-up en herstellen van BizTalk Services met behulp van de klassieke Azure portal.

U kunt ook back-up met behulp van BizTalk Services de [REST-API van BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [!NOTE]
> Hybride verbindingen zijn geen back-up, ongeacht de editie. U moet uw hybride verbindingen opnieuw maken.


## <a name="before-you-begin"></a>Voordat u begint
* Back-up en herstel mogelijk niet beschikbaar voor alle edities. Zie [BizTalk Services: grafiek van edities](biztalk-editions-feature-chart.md).
* Met de klassieke Azure portal, kunt u een On Demand back-up maken of een geplande back-up maken. 
* Back-inhoud kan worden hersteld naar dezelfde BizTalk Service of naar een nieuwe BizTalk Service. Voor het herstellen van de BizTalk Service gebruikt dezelfde naam, de bestaande BizTalk-Service moet worden verwijderd en de naam moet beschikbaar zijn. Nadat u een BizTalk Service hebt verwijderd, kan duren langer dan wilden voor dezelfde naam beschikbaar. Als u dezelfde naam beschikbaar niet kunt wachten, zet u een nieuwe BizTalk Service.
* BizTalk Services kunnen worden hersteld naar dezelfde versie of een hogere editie. BizTalk Services is teruggezet naar een lagere versie, wordt uit wanneer de back-up is gemaakt, niet ondersteund.
  
    Bijvoorbeeld, kan een back-up met behulp van de Basic-editie worden hersteld naar de Premium-versie. Een back-up met behulp van de Premium-versie kan niet worden hersteld naar de Standard-editie.
* De getallen EDI-besturingselement worden back-up continuïteit van de getallen besturingselement. Als berichten worden verwerkt na de laatste back-up, herstellen van de inhoud van deze back-up kan leiden tot dubbele besturingselement cijfers.
* Als een batch actieve berichten heeft, de batch verwerkt **voordat** waarop een back-up. Wanneer u een back-up (als de benodigde of geplande) maakt, worden berichten in batches nooit worden opgeslagen. 
  
    **Als u een back-up wordt gemaakt met actieve berichten in een batch, wordt deze berichten worden niet een back-up en zijn daarom verloren.**
* Optioneel: In de Portal BizTalk-Services stop alle beheerbewerkingen.

## <a name="create-a-backup"></a>Maak een back-up
Een back-up kan worden uitgevoerd op elk gewenst moment en volledig door u worden beheerd. Deze sectie vindt u de stappen voor het maken van back-ups met de klassieke Azure portal, met inbegrip van:

[Op aanvraag back-up](#backupnow)

[Een back-up plannen](#backupschedule)

#### <a name="backupnow"></a>Op aanvraag back-up
1. Selecteer in de klassieke Azure portal **BizTalk Services**, en selecteer vervolgens de BizTalk-Service die u wilt back-up.
2. In de **Dashboard** tabblad **Back-up** aan de onderkant van de pagina.
3. Voer de naam van een back-up. Voer bijvoorbeeld *myBizTalkService*door*datum*.
4. Kies een blob storage-account en schakel het selectievakje voor het starten van de back-up.

Zodra de back-up is voltooid, wordt een container met de back-naam die u invoert in het opslagaccount gemaakt. Deze container bevat uw BizTalk Service back-upconfiguratie.

#### <a name="backupschedule"></a>Een back-up plannen
1. Selecteer in de klassieke Azure portal **BizTalk Services**, selecteert u de BizTalk Service-naam die u wilt de back-up plannen en selecteer vervolgens de **configureren** tabblad.
2. Stel de **back-up van de Status** naar **automatische**. 
3. Selecteer de **Opslagaccount** invoeren voor het opslaan van de back-up, de **frequentie** voor het maken van de back-ups, en hoe lang de back-ups behouden (**dagen bewaren**):
   
    ![][AutomaticBU]
   
    **Opmerkingen bij de**     
   
   * In **dagen bewaren**, de retentietijd moet groter zijn dan de back-upfrequentie.
   * Selecteer **altijd ten minste één back-up bewaren**, zelfs als deze na de bewaarperiode valt.
4. Selecteer **Opslaan**.

Wanneer een geplande back-uptaak wordt uitgevoerd, wordt een container (voor het opslaan van back-upgegevens) gemaakt in het opslagaccount dat u hebt ingevoerd. De naam van de container met de naam *naam-/-tijd van de BizTalk Service*. 

Als de BizTalk Service-dashboard toont een **mislukt** status:

![Status van laatste geplande back-up][BackupStatus] 

De koppeling opent de Bewerkingslogboeken van de Management-Services voor het oplossen van. Zie [BizTalk Services: problemen oplossen met bewerkingslogboeken](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Herstellen
U kunt back-ups herstellen vanuit de klassieke Azure portal of vanuit de [herstellen BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582). Deze sectie vindt u de stappen om te herstellen met de klassieke portal.

#### <a name="before-restoring-a-backup"></a>Voordat u een back-up herstellen
* Nieuwe bijhouden, archiveren en te bewaken winkels kunnen worden ingevoerd tijdens het herstellen van een BizTalk Service.
* Dezelfde EDI-Runtime-gegevens is hersteld. De back-up van EDI-Runtime slaat de besturingselement-getallen. De herstelde besturingselement-nummers zijn in volgorde van de tijd van de back-up. Als berichten worden verwerkt na de laatste back-up, herstellen van de inhoud van deze back-up kan leiden tot dubbele besturingselement cijfers.

#### <a name="restore-a-backup"></a>Een back-up terugzetten
1. Selecteer in de klassieke Azure portal **nieuw** > **App Services** > **BizTalk Service** > **herstellen**:
   
    ![Een back-up terugzetten][Restore]
2. In **back-URL**, selecteert u het pictogram van de map en vouw de Azure storage-account waarmee de BizTalk Service configuratieback-up worden opgeslagen. Vouw de container uit en selecteer in het rechterdeelvenster de bijbehorende back-up txt-bestand. 
   <br/><br/>
   Selecteer **Open**.
3. Op de **herstellen BizTalk Service** pagina, voert u een **BizTalk-servicenaam** en controleer of de **domein-URL**, **Edition**, en **regio** voor de herstelde BizTalk Service. **Maak een nieuw exemplaar van SQL database** voor de database bijhouden:
   
    ![][RestoreBizTalkService]
   
    Selecteer de pijl Volgende.
4. Controleer de naam van de SQL-database, de fysieke server waar de SQL-database wordt gemaakt en een gebruikersnaam en wachtwoord invoeren voor die server.

    Als u de editie van SQL database, de grootte en andere eigenschappen configureren wilt, selecteert u **geavanceerde Database-instellingen configureren**. 

    Selecteer de pijl Volgende.

1. Een nieuw opslagaccount maken of voert u een bestaand opslagaccount voor de BizTalk Service.
2. Selecteer het vinkje om te beginnen met terugzetten.

Nadat de herstelbewerking is voltooid, wordt een nieuwe BizTalk Service wordt vermeld in een onderbroken status op de pagina BizTalk Services in de klassieke Azure portal.

### <a name="postrestore"></a>Na het terugzetten van een back-up
De BizTalk Service altijd wordt hersteld een **onderbroken** status. In deze toestand is, kunt u geen configuratiewijzigingen maken voordat de nieuwe omgeving functioneel is, met inbegrip van:

* Als u een BizTalk Service-toepassingen met behulp van de Azure BizTalk Services SDK gemaakt, mogelijk moet u naar de Access Control (ACS)-referenties in die toepassingen werken met de herstelde omgeving bijwerken.
* U herstelt een BizTalk Service om te repliceren van een bestaande BizTalk Service-omgeving. In dit geval als er zijn geconfigureerd in de oorspronkelijke BizTalk Services-portal overeenkomsten die gebruikmaken van een FTP-bronmap mogelijk moet u de overeenkomsten in de herstelde omgeving naar een andere bron FTP-map gebruiken bijwerken. Anders kunnen er twee verschillende overeenkomsten probeert om hetzelfde bericht binnen te halen.
* Als u voor omgevingen met meerdere BizTalk Service hebt hersteld, zorg er dan voor dat u de juiste omgeving in de Visual Studio-toepassingen, PowerShell-cmdlets, REST-API's of Trading Partner Management OM API's zijn gericht.
* Het is verstandig om te configureren van automatische back-ups op de herstelde BizTalk Service-omgeving.

U start de BizTalk Service in de klassieke Azure portal, selecteer de herstelde BizTalk Service en selecteer **hervatten** in de taakbalk. 

## <a name="what-gets-backed-up"></a>Wat wordt een back-up
Wanneer een back-up is gemaakt, de volgende items zijn back-up gemaakt:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Een back-up items</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Services-Portal</strong></td>
</tr> 
<tr>
<td>Configuratie- en Runtime</td> 
<td>
<ul>
<li>Details van partners en -profiel</li>
<li>Partner overeenkomsten</li>
<li>Aangepaste assembly's die zijn geïmplementeerd</li>
<li>Bruggen geïmplementeerd</li>
<li>Certificaten</li>
<li>Transformaties geïmplementeerd</li>
<li>Pijplijnen</li>
<li>Sjablonen gemaakt en opgeslagen in de Portal van BizTalk Services</li>
<li>X12 ST01 en GS01 toewijzingen</li>
<li>Besturingselement cijfers (EDI)</li>
<li>AS2-bericht MIC waarden</li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2">
 <strong>Azure BizTalk Service</strong></td>
</tr> 
<tr>
<td>SSL-certificaat</td> 
<td>
<ul>
<li>Gegevens van SSL-certificaat</li>
<li>Wachtwoord voor SSL-certificaat</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk Service-instellingen</td> 
<td>
<ul>
<li>Aantal Scale-eenheden</li>
<li>Editie</li>
<li>Versie van het product</li>
<li>Regio/Datacenter</li>
<li>Access Control Service (ACS) naamruimte en -sleutel</li>
<li>Tekenreeks voor databaseverbinding bijhouden</li>
<li>Archiveren van de verbindingsreeks voor opslag-account</li>
<li>Bewaking van de verbindingsreeks voor opslag-account</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Extra Items</strong></td>
</tr> 
<tr>
<td>Traceringsdatabase</td> 
<td>Wanneer de BizTalk Service wordt gemaakt, worden de details van de Database bijhouden ingevoerd, met inbegrip van de Azure SQL Database-Server en de naam van de Database bijhouden. De Database bijhouden wordt niet automatisch back-up.
<br/><br/>
<strong>Belangrijk</strong><br/>
Als de Database bijhouden wordt verwijderd en de databasebehoeften is hersteld, moet een eerdere back-up bestaan. Als een back-up niet bestaat, zijn de Database bijhouden en de gegevens niet hersteld. In dit geval maakt u een nieuwe Database voor het bijhouden met dezelfde databasenaam. Geo-replicatie wordt aanbevolen.</td>
</tr> 
</table>

## <a name="next"></a>Volgende
Voor het maken van Azure BizTalk Services in de klassieke Azure portal, gaat u naar [BizTalk Services: inrichten met behulp van Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280). Ga naar [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197) om te beginnen met het maken van toepassingen.

## <a name="see-also"></a>Zie ook
* [Back-up van BizTalk-Service](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk Service back-up terugzetten](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Developer, Basic, Standard en Premium-edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Inrichten met behulp van Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: statusgrafiek voor de inrichting](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: de tabbladen Dashboard, Bewaken en Schalen](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: beperking](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: naam en sleutel van verlener](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [De Azure BizTalk Services SDK gaan gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

