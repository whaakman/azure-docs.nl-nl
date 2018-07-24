---
title: Maken en herstellen van een back-up in BizTalk Services | Microsoft Docs
description: BizTalk Services bevat back-up en herstel. Informatie over het maken en herstellen van een back-up en te bepalen wat wordt een back-up. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 90cf2d0ddbba47a856bf1299a101c5185873b5d8
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214409"
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: back-ups maken en herstellen

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services omvat mogelijkheden voor back-up en herstel. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

> [!NOTE]
> Hybride verbindingen zijn niet back-ups, ongeacht de editie. U moet uw hybride verbindingen opnieuw maken.


## <a name="before-you-begin"></a>Voordat u begint
* Back-up en herstel mogelijk niet beschikbaar voor alle edities. Zie [BizTalk Services: grafiek van edities](biztalk-editions-feature-chart.md).
* Back-inhoud kan worden hersteld naar dezelfde BizTalk Service of naar een nieuwe BizTalk Service. Als u wilt herstellen van de BizTalk-Service met dezelfde naam, de bestaande BizTalk-Service moeten worden verwijderd en de naam mag niet beschikbaar. Nadat u een BizTalk Service hebt verwijderd, duurt het langer dan wilde voor dezelfde naam beschikbaar is. Als u niet kunt op dezelfde naam wachten beschikbaar is, zet u een nieuwe BizTalk Service.
* BizTalk Services kunnen worden hersteld naar dezelfde versie of een hogere editie. BizTalk Services is teruggezet naar een lagere versie, wordt uit wanneer de back-up is gemaakt, niet ondersteund.
  
    Bijvoorbeeld, kan een back-up met behulp van de Basic-editie worden hersteld naar de Premium-editie. Een back-up met behulp van de Premium-editie kan niet worden hersteld naar de Standard-editie.
* De EDI-controlenummers back-ups continuïteit van de controlenummers. Als berichten worden verwerkt na de laatste back-up, kan de inhoud van deze back-up herstellen dubbele controlenummers veroorzaken.
* Als een batch actieve berichten heeft, verwerking van de batch **voordat** waarop een back-up wordt uitgevoerd. Bij het maken van een back-up (als vereiste of geplande), worden berichten in batches worden nooit opgeslagen. 
  
    **Als u een back-up is gemaakt met actieve berichten in een batch, wordt deze berichten worden niet ondersteund en zijn daarom verloren gaan.**
* Optioneel: In de BizTalk Services-Portal, niet alle beheerbewerkingen.

## <a name="create-a-backup"></a>Een back-up maken
Een back-up kan worden uitgevoerd op elk gewenst moment en volledig wordt beheerd door u. U kunt een back-up maken met de [REST-API voor het beheren van BizTalk Services op Azure](https://msdn.microsoft.com/library/azure/dn232347.aspx).

## <a name="restore"></a>Herstellen
Als u een back-up herstellen, gebruikt u de [REST-API voor het beheren van BizTalk Services op Azure](https://msdn.microsoft.com/library/azure/dn232347.aspx).

### <a name="postrestore"></a>Na het herstellen van een back-up
De BizTalk Service altijd hersteld in een **onderbroken** staat. In deze staat, kunt u eventuele wijzigingen in de configuratie voordat de nieuwe omgeving functioneel is, met inbegrip van:

* Als u een BizTalk Service-toepassingen met behulp van de Azure BizTalk Services SDK hebt gemaakt, moet u mogelijk de Access Control (ACS)-referenties in die toepassingen werken met de herstelde omgeving bijwerken.
* U herstellen een BizTalk Service als u wilt repliceren van een bestaande BizTalk Service-omgeving. In dit geval, als er zijn geconfigureerd in de oorspronkelijke BizTalk Services-portal overeenkomsten die gebruikmaken van een bron FTP-map, moet u mogelijk om bij te werken van de overeenkomsten in de herstelde omgeving om een andere bron FTP-map te gebruiken. Anders kunnen er twee verschillende overeenkomsten probeert om op te halen van hetzelfde bericht.
* Als u hebt hersteld als u wilt dat meerdere BizTalk Service-omgevingen, zorg er dan voor dat u zich richten op de juiste omgeving in de Visual Studio-toepassingen, PowerShell-cmdlets, REST-API's of Trading Partner Management OM API's.
* Het is raadzaam om te configureren van automatische back-ups op de herstelde BizTalk Service-omgeving.

## <a name="what-gets-backed-up"></a>Wat wordt een back-up
Wanneer een back-up wordt gemaakt, de volgende items zijn back-up gemaakt:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Back-ups van items</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Services-Portal</strong></td>
</tr> 
<tr>
<td>Configuratie en Runtime</td> 
<td>
<ul>
<li>Details van de partner en het profiel</li>
<li>Overeenkomsten</li>
<li>Aangepaste assembly's die zijn geïmplementeerd</li>
<li>Bruggen geïmplementeerd</li>
<li>Certificaten</li>
<li>Transformaties geïmplementeerd</li>
<li>Pijplijnen</li>
<li>Sjablonen gemaakt en opgeslagen in de BizTalk Services-Portal</li>
<li>X12 ST01 en GS01 toewijzingen</li>
<li>Controlenummers (EDI)</li>
<li>AS2-bericht MIC-waarden</li>
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
<li>De gegevens van de SSL-certificaat</li>
<li>Wachtwoord voor SSL-certificaat</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk Service-instellingen</td> 
<td>
<ul>
<li>Schaalaanpassingsaantal eenheid</li>
<li>Editie</li>
<li>Productversie</li>
<li>Regio/Datacenter</li>
<li>Access Control Service (ACS)-naamruimte en de sleutel</li>
<li>Bijhouden van de tekenreeks voor databaseverbinding</li>
<li>Tekenreeks opslagaccountverbinding voor archivering</li>
<li>Tekenreeks opslagaccountverbinding voor bewaking</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Extra Items</strong></td>
</tr> 
<tr>
<td>Traceringsdatabase</td> 
<td>Wanneer de BizTalk Service wordt gemaakt, worden de details van de Database bijhouden ingevoerd, met inbegrip van de Azure SQL Database-Server en de naam van de Database bijhouden. Het bijhouden van Database wordt niet automatisch back-ups.
<br/><br/>
<strong>Belangrijk</strong><br/>
Als de Database bijhouden is verwijderd en de behoeften van de database is hersteld, moet een eerdere back-up bestaan. Als u een back-up niet bestaat, zijn de Database bijhouden en de bijbehorende gegevens niet hersteld. In dit geval maakt u een nieuwe Database voor het bijhouden van met de naam van de dezelfde database. Geo-replicatie wordt aanbevolen.</td>
</tr> 
</table>

## <a name="next"></a>Volgende
Voor het maken van Azure BizTalk Services, gaat u naar [BizTalk Services: inrichten](http://go.microsoft.com/fwlink/p/?LinkID=302280). Ga naar [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197) om te beginnen met het maken van toepassingen.

## <a name="see-also"></a>Zie ook
* [Back-up van BizTalk-Service](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk-Service te herstellen vanuit back-up](http://go.microsoft.com/fwlink/p/?LinkID=325582)
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

