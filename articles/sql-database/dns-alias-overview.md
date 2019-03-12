---
title: DNS-alias voor Azure SQL Database | Microsoft Docs
description: Uw toepassingen kunnen verbinding maken met een alias voor de naam van uw Azure SQL Database-server. Ondertussen kunt u de SQL-Database de alias verwijst naar op elk moment en om te testen en verder vereenvoudigen.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: genemi,ayolubek, jrasnick
manager: craigg
ms.date: 03/07/2019
ms.openlocfilehash: 60f00418a2e3fcf2ac88dfc351e07ca8c3ac802a
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57574607"
---
# <a name="dns-alias-for-azure-sql-database"></a>DNS-alias voor Azure SQL Database

Azure SQL-Database heeft een Domain Name System (DNS)-server. PowerShell en REST-API's accepteren [aanroepen voor het maken en beheren van DNS-aliassen](#anchor-powershell-code-62x) voor de naam van uw SQL-Database-server.

Een *DNS-alias* kan worden gebruikt in plaats van de naam van de Azure SQL Database-server. Client-programma's kunnen u de alias gebruiken in hun verbindingsreeksen. De DNS-server biedt alias een NAT-laag die uw client-programma's naar andere servers omleiden kunt. Deze laag reserveonderdelen de moeilijkheden dat hoeft op te zoeken en bewerken van alle clients en hun verbindingsreeksen.

Veelvoorkomende toepassingen voor een DNS-alias zijn onder andere de volgende gevallen:

- Maak een gemakkelijk te onthouden naam voor een Azure SQL-Server.
- Tijdens het ontwikkelen van initiële, kan uw alias verwijzen naar een test SQL Database-server. Wanneer de toepassing live gaat, kunt u de alias om te verwijzen naar de productieserver wijzigen. De overgang van test naar productie vereist wijzigingen in de configuraties niet meerdere clients die verbinding met de database-server maken.
- Stel dat de enige database in uw toepassing wordt verplaatst naar een andere SQL-Database-server. Hier kunt u de alias wijzigen zonder dat de configuraties van verschillende clients wijzigen.

## <a name="domain-name-system-dns-of-the-internet"></a>Domain Name System (DNS) van het Internet

Het Internet, is afhankelijk van de DNS-server. De DNS-server zet uw beschrijvende namen in de naam van uw Azure SQL Database-server.

## <a name="scenarios-with-one-dns-alias"></a>Scenario's met een DNS-alias

Stel dat u wilt overschakelen van uw systeem naar een nieuwe Azure SQL Database-server. In het verleden had u nodig om te zoeken en elke verbindingsreeks in elke clientprogramma bijwerken. Maar nu, als de verbindingsreeksen een DNS-alias gebruiken, alleen de eigenschap van een alias moet worden bijgewerkt.

De DNS-alias-functie van Azure SQL Database kunt u de volgende scenario's:

### <a name="test-to-production"></a>Test naar productie

Wanneer u begint met het ontwikkelen van de client-programma's, hebben ze een DNS-alias gebruiken in hun verbindingsreeksen. U maakt de eigenschappen van de alias-punt met een testversie van uw Azure SQL Database-server.

Later opnieuw wanneer het nieuwe systeem live in productie gaat, kunt u de eigenschappen van de alias om te verwijzen naar de productie SQL Database-server bijwerken. Er is geen wijziging naar de client-programma's die nodig zijn.

### <a name="cross-region-support"></a>Regio-overschrijdende ondersteuning

Een herstel na noodgevallen mogelijk uw SQL Database-server verplaatsen naar een andere geografische regio. Voor een systeem dan een DNS-alias is gebruikt kan de noodzaak om te zoeken en bijwerken van de verbindingsreeksen voor alle clients worden vermeden. U kunt in plaats daarvan een alias om te verwijzen naar de nieuwe SQL-Database-server die nu als host fungeert voor de database bijwerken.

## <a name="properties-of-a-dns-alias"></a>Eigenschappen van een DNS-alias

De volgende eigenschappen zijn van toepassing op elke DNS-alias voor uw SQL Database-server:

- *Unieke naam op:* De naam van elke alias die u maakt is uniek zijn voor alle Azure SQL Database-servers, net zoals de servernamen van de zijn.
- *Server is vereist:* Een DNS-server kan geen alias worden gemaakt, tenzij deze verwijst naar exact één server en de server moet al bestaan. Een bijgewerkte alias moet altijd verwijzen naar exact één bestaande server.
  - Wanneer u een SQL-databaseserver neerzet, wordt in de Azure-systeem ook alle DNS-aliassen die naar de server verwijzen komt.
- *Niet gekoppeld aan andere regio's:* DNS-aliassen zijn niet gebonden aan een regio. Een DNS-aliassen kunnen worden bijgewerkt om te verwijzen naar een Azure SQL Database-server die zich in elke geografische regio bevinden.
  - Echter, wanneer u een alias om te verwijzen naar een andere server bijwerkt, beide servers moeten bestaan in dezelfde Azure *abonnement*.
- *Machtigingen:* Voor het beheren van een DNS-alias, moet de gebruiker hebben *Inzender voor Server* machtigingen of hoger. Zie voor meer informatie, [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../role-based-access-control/overview.md).

## <a name="manage-your-dns-aliases"></a>Beheer van uw DNS-aliassen

Zowel REST-API's en PowerShell-cmdlets zijn beschikbaar waarmee u kunt voor het programmatisch beheren van uw DNS-aliassen.

### <a name="rest-apis-for-managing-your-dns-aliases"></a>REST API's voor het beheren van uw DNS-aliassen

<!-- TODO
??2 "soon" in the following live sentence, is not the best situation.
TODO update this subsection very soon after REST API docu goes live.
Dev = Magda Bojarska
Comment as of:  2018-01-26
-->

De documentatie voor de REST API's is beschikbaar in de buurt van de volgende locatie:

- [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/)

De REST API's kunnen ook worden weergegeven in GitHub op:

- [Azure SQL Database-server, DNS-alias REST-API 's](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/sql/resource-manager/Microsoft.Sql/preview/2017-03-01-preview/serverDnsAliases.json)

<a name="anchor-powershell-code-62x"/>

#### <a name="powershell-for-managing-your-dns-aliases"></a>PowerShell voor het beheren van uw DNS-aliassen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell-cmdlets zijn beschikbaar die de REST-API's aanroepen.

Een voorbeeld van PowerShell-cmdlets die worden gebruikt voor het beheren van DNS-aliassen wordt beschreven in:

- [PowerShell voor DNS-Alias naar Azure SQL Database](dns-alias-powershell.md)

De cmdlets die in het codevoorbeeld gebruikt zijn de volgende:

- [New-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Hiermee maakt u een nieuwe DNS-alias in het systeem van de service Azure SQL Database. De alias verwijst naar Azure SQL Database-server 1.
- [Get-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): Ophalen en weergeven van alle DNS-aliassen die zijn toegewezen aan de SQL-database-server 1.
- [Set-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): Hiermee wijzigt u de naam van de server die de alias is geconfigureerd om te verwijzen naar, van server 1 naar 2 van de SQL-database-server.
- [Remove-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): Verwijder de DNS-alias van de SQL-database-server 2, met behulp van de naam van de alias.

## <a name="limitations-during-preview"></a>Beperkingen voor de Preview-versie

Een DNS-server heeft momenteel kan alias de volgende beperkingen:

- *Vertraging van maximaal 2 minuten:* Het duurt maximaal 2 minuten voor een DNS-alias worden bijgewerkt of verwijderd.
  - Ongeacht een korte vertraging stopt de alias onmiddellijk clientverbindingen met de oude server verwijzen.
- *DNS-zoekopdracht:* Voor het alleen bindende manier om te controleren welke server die een opgegeven DNS-Server alias naar verwijst is door te voeren op dit moment een [DNS-zoekactie](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup).
- *[Tabelcontrole wordt niet ondersteund](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md):* U kunt een DNS-alias niet gebruiken op een Azure SQL Database-server die is *tabelcontrole* ingeschakeld voor een database.
  - Tabelcontrole is afgeschaft.
  - Het is raadzaam dat u naar verplaatsen [controlefunctie voor blobs](sql-database-auditing.md).

## <a name="related-resources"></a>Gerelateerde resources

- [Overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md), met inbegrip van herstel na noodgevallen.

## <a name="next-steps"></a>Volgende stappen

- [PowerShell voor DNS-Alias naar Azure SQL Database](dns-alias-powershell.md)
