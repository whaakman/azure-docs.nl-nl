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
ms.openlocfilehash: 45365092f5bcd1a8d309c10404a7437c494a8967
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: back-ups maken en herstellen

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services bevat de mogelijkheden van back-up en herstel. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

> [!NOTE]
> Hybride verbindingen zijn geen back-up, ongeacht de editie. U moet uw hybride verbindingen opnieuw maken.


## <a name="before-you-begin"></a>Voordat u begint
* Back-up en herstel mogelijk niet beschikbaar voor alle edities. Zie [BizTalk Services: grafiek van edities](biztalk-editions-feature-chart.md).
* Back-inhoud kan worden hersteld naar dezelfde BizTalk Service of naar een nieuwe BizTalk Service. Voor het herstellen van de BizTalk Service gebruikt dezelfde naam, de bestaande BizTalk-Service moet worden verwijderd en de naam moet beschikbaar zijn. Nadat u een BizTalk Service hebt verwijderd, kan duren langer dan wilden voor dezelfde naam beschikbaar. Als u dezelfde naam beschikbaar niet kunt wachten, zet u een nieuwe BizTalk Service.
* BizTalk Services kunnen worden hersteld naar dezelfde versie of een hogere editie. BizTalk Services is teruggezet naar een lagere versie, wordt uit wanneer de back-up is gemaakt, niet ondersteund.
  
    Bijvoorbeeld, kan een back-up met behulp van de Basic-editie worden hersteld naar de Premium-versie. Een back-up met behulp van de Premium-versie kan niet worden hersteld naar de Standard-editie.
* De getallen EDI-besturingselement worden back-up continuïteit van de getallen besturingselement. Als berichten worden verwerkt na de laatste back-up, herstellen van de inhoud van deze back-up kan leiden tot dubbele besturingselement cijfers.
* Als een batch actieve berichten heeft, de batch verwerkt **voordat** waarop een back-up. Wanneer u een back-up (als de benodigde of geplande) maakt, worden berichten in batches nooit worden opgeslagen. 
  
    **Als u een back-up wordt gemaakt met actieve berichten in een batch, wordt deze berichten worden niet een back-up en zijn daarom verloren.**
* Optioneel: In de Portal BizTalk-Services stop alle beheerbewerkingen.

## <a name="create-a-backup"></a>Maak een back-up
Een back-up kan worden uitgevoerd op elk gewenst moment en volledig door u worden beheerd. Gebruik voor het maken van een back-up de [REST-API voor het beheren van BizTalk Services op Azure](https://msdn.microsoft.com/library/azure/dn232347.aspx).

## <a name="restore"></a>Herstellen
Als u een back-up herstellen, gebruikt u de [REST-API voor het beheren van BizTalk Services op Azure](https://msdn.microsoft.com/library/azure/dn232347.aspx).

### <a name="postrestore"></a>Na het terugzetten van een back-up
De BizTalk Service altijd wordt hersteld een **onderbroken** status. In deze toestand is, kunt u geen configuratiewijzigingen maken voordat de nieuwe omgeving functioneel is, met inbegrip van:

* Als u een BizTalk Service-toepassingen met behulp van de Azure BizTalk Services SDK gemaakt, mogelijk moet u naar de Access Control (ACS)-referenties in die toepassingen werken met de herstelde omgeving bijwerken.
* U herstelt een BizTalk Service om te repliceren van een bestaande BizTalk Service-omgeving. In dit geval als er zijn geconfigureerd in de oorspronkelijke BizTalk Services-portal overeenkomsten die gebruikmaken van een FTP-bronmap mogelijk moet u de overeenkomsten in de herstelde omgeving naar een andere bron FTP-map gebruiken bijwerken. Anders kunnen er twee verschillende overeenkomsten probeert om hetzelfde bericht binnen te halen.
* Als u voor omgevingen met meerdere BizTalk Service hebt hersteld, zorg er dan voor dat u de juiste omgeving in de Visual Studio-toepassingen, PowerShell-cmdlets, REST-API's of Trading Partner Management OM API's zijn gericht.
* Het is verstandig om te configureren van automatische back-ups op de herstelde BizTalk Service-omgeving.

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
Voor het maken van Azure BizTalk Services, gaat u naar [BizTalk Services: inrichten](http://go.microsoft.com/fwlink/p/?LinkID=302280). Ga naar [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197) om te beginnen met het maken van toepassingen.

## <a name="see-also"></a>Zie ook
* [Back-up van BizTalk-Service](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk Service back-up terugzetten](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Developer, Basic, Standard en Premium-edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: inrichten](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: statusgrafiek voor de inrichting](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: de tabbladen Dashboard, Bewaken en Schalen](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: beperking](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: naam en sleutel van verlener](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [De Azure BizTalk Services SDK gaan gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

