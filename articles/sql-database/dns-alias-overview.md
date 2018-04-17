---
title: DNS-alias voor Azure SQL Database | Microsoft Docs
description: Uw toepassingen kunnen verbinden met een alias voor de naam van uw Azure SQL Database-server. Ondertussen kunt u de SQL-Database de alias die verwijst naar op elk gewenst moment, enzovoort testen mogelijk te maken.
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: DNS alias
ms.topic: article
ms.date: 02/05/2018
ms.reviewer: genemi;ayolubek
ms.author: dmalik
ms.openlocfilehash: 1793f44026db55d17fabde5fb92bc46795109418
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="dns-alias-for-azure-sql-database"></a>DNS-alias voor Azure SQL Database

Azure SQL-Database heeft een Domain Name System (DNS)-server. PowerShell en REST-API's accepteren [aanroepen maken en beheren van DNS-aliassen](#anchor-powershell-code-62x) voor de naam van uw SQL-Database.

Een *DNS-alias* kan worden gebruikt in plaats van de naam van de Azure SQL Database-server. Client's kunnen de alias gebruiken in hun verbindingsreeksen. De DNS-server biedt alias een NAT-laag die u uw clientprogramma's naar andere servers omleiden kunt. Deze laag reserveonderdelen de moeilijkheden hoeft te zoeken en alle clients en de verbindingsreeks bewerken.

Veelvoorkomende toepassingen voor een DNS-alias zijn onder andere de volgende gevallen:

- Maak een gemakkelijk te onthouden naam voor een Azure SQL Server.
- Je alias kunt tijdens de eerste ontwikkeling verwijzen naar een test SQL Database-server. Wanneer de toepassing live gaat, kunt u de alias voor het verwijzen naar de productieserver wijzigen. De overgang van test naar productie vereist wijzigingen in de configuraties geen verschillende clients die verbinding met de databaseserver maken.
- Stel dat de enige database in uw toepassing wordt verplaatst naar een andere SQL-Database-server. Hier kunt u de alias wijzigen zonder te wijzigen van de configuraties van verschillende clients.

#### <a name="domain-name-system-dns-of-the-internet"></a>Domain Name System (DNS) van het Internet

Het Internet, is afhankelijk van de DNS-server. De DNS-server zet de beschrijvende namen in de naam van uw Azure SQL Database-server.

## <a name="scenarios-with-one-dns-alias"></a>Scenario's met een DNS-alias

Stel dat u wilt overschakelen van uw systeem naar een nieuwe Azure SQL Database-server. In het verleden u nodig hebt om te zoeken en elke verbindingsreeks in elke clientprogramma bijwerken. Maar nu als de verbindingsreeksen een DNS-alias gebruiken, alleen een alias-eigenschap moet worden bijgewerkt.

De functie DNS-alias van Azure SQL Database kunt u de volgende scenario's:

#### <a name="test-to-production"></a>Test naar productie

Wanneer u begint met het ontwikkelen van de client-programma's, hebben ze een DNS-alias gebruiken in hun verbindingsreeksen. U kunt de eigenschappen van het punt alias maakt met een testversie van uw Azure SQL Database-server.

Later wanneer het nieuwe systeem live in productie gaat, kunt u de eigenschappen van de alias die verwijzen naar de productieserver SQL-Database bijwerken. Er is geen wijziging in de client-programma's nodig.

#### <a name="cross-region-support"></a>Regio-overschrijdende-ondersteuning

Een noodherstel mogelijk uw SQL Database-server verplaatsen naar een ander geografische regio. Voor een systeem dan gebruik van een DNS-alias, is kan de noodzaak om te zoeken en bijwerken van de verbindingsreeksen voor alle clients worden vermeden. U kunt in plaats daarvan een alias voor het verwijzen naar de nieuwe SQL-databaseserver die nu als host fungeert voor uw database bijwerken.




## <a name="properties-of-a-dns-alias"></a>Eigenschappen van een DNS-alias

De volgende eigenschappen van toepassing op elke DNS-alias voor uw SQL Database-server:

- *Unieke naam:* de aliasnaam van elke die u maakt is uniek voor alle Azure SQL Database-servers, net zoals server namen zijn.

- *Server is vereist:* A-DNS-alias kan niet worden gemaakt, tenzij deze verwijst naar exact één server en de server moet al bestaan. Een bijgewerkte alias moet altijd verwijzen naar één bestaande server.
    - Wanneer u een SQL Database-server verwijderen, wordt het systeem Azure ook alle DNS-aliassen die naar de server verwijzen verwijderd.

- *Niet is gebonden aan een willekeurige regio:* DNS-aliassen zijn niet gekoppeld aan een regio. DNS-aliassen kunnen worden bijgewerkt om te verwijzen naar een Azure SQL Database-server die zich in een geografische regio bevindt.
    - Bij het bijwerken van een alias voor het verwijzen naar een andere server beide servers moeten echter bestaan in dezelfde Azure *abonnement*.

- *Machtigingen:* voor het beheren van een DNS-alias, moet de gebruiker hebben *Server Inzender* machtigingen of hoger. Zie voor meer informatie [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../role-based-access-control/overview.md).





## <a name="manage-your-dns-aliases"></a>Beheren van uw DNS-aliassen

Zowel de REST-API's en de PowerShell-cmdlets zijn beschikbaar waarmee u kunt uw DNS-aliassen programmatisch te beheren.

#### <a name="rest-apis-for-managing-your-dns-aliases"></a>REST API's voor het beheren van uw DNS-aliassen

<!-- TODO
??2 "soon" in the following live sentence, is not the best situation.
TODO update this subsection very soon after REST API docu goes live.
Dev = Magda Bojarska
Comment as of:  2018-01-26
-->

De documentatie voor de REST API's is beschikbaar in de buurt van de volgende locatie:
- [Azure SQL Database REST-API](https://docs.microsoft.com/rest/api/sql/)

De REST API's kunnen ook worden weergegeven in GitHub op:
- [Azure SQL Database-server, DNS-alias REST-API 's](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/sql/resource-manager/Microsoft.Sql/preview/2017-03-01-preview/serverDnsAliases.json)

<a name="anchor-powershell-code-62x"/>

#### <a name="powershell-for-managing-your-dns-aliases"></a>PowerShell voor het beheren van uw DNS-aliassen

PowerShell-cmdlets zijn beschikbaar die het REST-API's aanroepen.

Een voorbeeld van PowerShell-cmdlets worden gebruikt voor het beheren van DNS-aliassen wordt beschreven op:
- [PowerShell voor DNS-Alias naar Azure SQL Database](dns-alias-powershell.md)


De cmdlets gebruikt in het voorbeeld zijn de volgende:
- [Nieuwe AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/New-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): een nieuwe DNS-alias maakt in het systeem van de service Azure SQL Database. De alias die verwijst naar Azure SQL Database-server 1.
- [Get-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): ophalen en weergeven van alle DNS-aliassen die zijn toegewezen aan SQL-database-server 1.
- [Set-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Set-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Hiermee wijzigt u de naam van de server die de alias die is geconfigureerd om te verwijzen naar, vanaf server 1 naar 2 van de SQL-database-server.
- [Verwijder AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Remove-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): DNS-alias van de SQL-database-server 2, verwijderen met behulp van de naam van de alias.


De voorgaande cmdlets zijn toegevoegd aan de **AzureRM.Sql** module beginnen met moduleversie 5.1.1.




## <a name="limitations-during-preview"></a>Beperkingen tijdens de preview

Een DNS-server heeft momenteel kan alias de volgende beperkingen:

- *Vertraging van maximaal 2 minuten:* die nodig is tot twee minuten om een DNS-alias worden bijgewerkt of verwijderd.
    - Ongeacht een korte vertraging stopt de alias onmiddellijk clientverbindingen met de oude server verwijst.

- *DNS-lookup:* de alleen bindende manier om te controleren welke server die een opgegeven DNS-Server alias naar verwijst door te voeren op dit moment een [DNS-zoekopdracht](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup).

- *[Controle van de tabel wordt niet ondersteund](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md):* u een DNS-alias niet gebruiken op een Azure SQL Database-server die is *tabel controle* ingeschakeld voor een database.
    - Controle van de tabel is afgeschaft.
    - Het is raadzaam dat u naar verplaatsen [Auditingfunctie voor blobs](sql-database-auditing.md).




## <a name="related-resources"></a>Gerelateerde resources

- [Overzicht van zakelijke continuïteit met Azure SQL Database](sql-database-business-continuity.md), met inbegrip van herstel na noodgevallen.



## <a name="next-steps"></a>Volgende stappen

- [PowerShell voor DNS-Alias naar Azure SQL Database](dns-alias-powershell.md)

