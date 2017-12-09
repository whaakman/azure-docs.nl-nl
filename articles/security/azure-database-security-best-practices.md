---
title: Aanbevolen beveiligingsprocedures voor Azure-database | Microsoft Docs
description: In dit artikel biedt een set met aanbevolen procedures voor beveiliging van de Azure-database.
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: c8f49a778ab5e32108531cab05cab7f7eb092221
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-database-security-best-practices"></a>Aanbevolen beveiligingsprocedures voor Azure-database

Beveiliging is een bovenste probleem bij het beheren van databases en is altijd een prioriteit voor Azure SQL Database. Uw databases nauw kunnen worden beveiligd om te voldoen aan de meest regelgevende of beveiligingsvereisten, zoals HIPAA, ISO 27001/27002 en PCI DSS-niveau 1, onder andere. Een huidige lijst van naleving veiligheidscertificaten is beschikbaar op de [Microsoft Trust Center site](http://azure.microsoft.com/support/trust-center/services/). U kunt ook uw databases in specifieke Azure-datacenters op basis van de regelgeving plaatsen.

In dit artikel bespreken we een verzameling aanbevolen beveiligingsprocedures voor Azure-database. Deze aanbevolen procedures zijn afgeleid van onze ervaring met beveiliging van de Azure-database en de ervaringen van klanten, zoals zelf.

Voor elke beste kunnen worden uitgelegd:

-   Wat de beste handelswijze is
-   Waarom u wilt dat het beste inschakelen
-   Wat zijn het resultaat als u niet de aanbevolen procedure inschakelen
-   Hoe u kunt informatie over het inschakelen van de aanbevolen procedure

In dit artikel Azure Security Best Practices is gebaseerd op een advies consensus- en mogelijkheden van Azure-platform en functie wordt ingesteld als deze bestaan in de tijd die in dit artikel is geschreven. Adviezen en technologieën op den duur veranderen en dit artikel wordt regelmatig gecontroleerd op basis van deze wijzigingen worden bijgewerkt.

Azure-database aanbevolen beveiligingsprocedures besproken in dit artikel zijn onder andere:

-   Firewall-regels gebruiken om te beperken van toegang tot de database
-   Database-verificatie inschakelen
-   Uw gegevens beschermen met versleuteling
-   De gegevens onderweg te beschermen
-   Databasecontrole inschakelen
-   Detectie van dreigingen database inschakelen


## <a name="use-firewall-rules-to-restrict-database-access"></a>Firewall-regels gebruiken om te beperken van toegang tot de database

Microsoft Azure SQL Database levert een relationele-databaseservice voor Azure en andere op internet gebaseerde toepassingen. Beveiliging van toegang tot beheert SQL-Database de toegang met firewallregels beperkende connectiviteit door het IP-adres, zodat gebruikers hun identiteit bewijzen verificatiemechanismen en autorisatiemechanismen gebruikers bepaalde acties en gegevens te beperken. Firewalls verhinderen alle toegang met de databaseserver tot u opgeven welke computers over machtigingen beschikken. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.

![Firewall-regels](./media/azure-database-security-best-practices/azure-database-security-best-practices-Fig1.png)

De Azure SQL Database-service is alleen beschikbaar via TCP-poort 1433. Zorg voor toegang tot een SQL Database vanaf uw computer ervoor dat de firewall van uw clientcomputer uitgaande TCP-communicatie op TCP-poort 1433 toestaat. Als u niet nodig is voor andere toepassingen, binnenkomende verbindingen blokkeren op TCP-poort 1433 met behulp van firewallregels.

Als onderdeel van het verbindingsproces worden verbindingen van virtuele Azure-machines omgeleid naar een ander IP-adres en poort, die uniek zijn voor elke werkrol. Het poortnummer ligt in het bereik van 11000 tot 11999. Zie voor meer informatie over de TCP-poorten, [poorten buiten 1433 voor ADO.NET 4.5 en SQL Database2](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12).

> [!Note]
> Zie [SQL Database-firewallregels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor meer informatie over de firewallregels in SQL Database.

## <a name="enable-database-authentication"></a>Database-verificatie inschakelen
SQL Database ondersteunt twee typen verificatie, SQL-verificatie en Azure Active Directory-verificatie (verificatie met Azure AD).

**SQL-verificatie** wordt aanbevolen in de volgende gevallen:

-   Kunt u SQL Azure ter ondersteuning van omgevingen met gemengde besturingssystemen, waarbij niet alle gebruikers worden geverifieerd door een Windows-domein.
-   Hiermee kunt u SQL Azure ter ondersteuning van oudere toepassingen en toepassingen die worden geleverd door derden waarvoor SQL Server-verificatie.
-   Hiermee kunnen gebruikers verbinding maken via een onbekende of niet-vertrouwde domeinen. Een toepassing waarin klanten tot stand gebrachte verbinding met maken toegewezen bijvoorbeeld SQL Server-aanmeldingen voor het ontvangen van de status van hun orders.
-   Hiermee kunt u SQL Azure ter ondersteuning van webtoepassingen waar gebruikers hun eigen identiteiten maken.
-   Softwareontwikkelaars hun toepassingen distribueren met behulp van de hiërarchie van een complexe machtiging op basis van bekende, vooraf gedefinieerde SQL Server-aanmeldingen.

> [!Note]
> SQL Server-verificatie niet Kerberos veiligheidsprotocol echter niet gebruiken.

Als u **SQL-verificatie** moet:

-   De sterke referenties beheren zelf.
-   Beveiligen van de referenties in de verbindingstekenreeks.
-   De referenties die zijn doorgegeven via het netwerk van de webserver naar de database (potentieel) beveiligen. Zie voor meer informatie [hoe: verbinding maken met SQL Server met behulp van SQL-verificatie in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998300.aspx).

**Azure Active Directory-verificatie** is een mechanisme van verbinding maken met Microsoft Azure SQL Database en [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) met behulp van identiteiten in Azure Active Directory (Azure AD). Azure AD-verificatie, kunt u de identiteit van gebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centrale ID-beheer biedt er één plaats is voor het beheren van databasegebruikers en vereenvoudigt het beheer van machtigingen. Voordelen zijn onder andere:

-   Het biedt een alternatief voor SQL Server-verificatie.
-   Houdt de komst van gebruikersidentiteiten voor databaseservers.
-   Kan wachtwoord draaien op één plaats.
-   Klanten kunnen met behulp van externe (AAD) groepen databasemachtigingen beheren.
-   Deze kunt wachtwoorden moet opslaan voorkomen door het inschakelen van geïntegreerde Windows-verificatie en andere soorten authenticatie wordt ondersteund door Azure Active Directory.
-   Ingesloten databasegebruikers Azure AD-verificatie gebruikt voor het verifiëren van identiteiten op databaseniveau.
-   Azure AD biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met SQL-Database.
-   Azure AD-verificatie ondersteunt AD FS (domeinfederatie) of systeemeigen gebruikerswachtwoord verificatie voor een lokale Azure Active Directory zonder Domeinsynchronisatie.
-   Azure AD ondersteunt verbindingen van SQL Server Management Studio die gebruikmaken van Universal verificatie van Active Directory, waaronder de multi-factor Authentication (MFA). MFA sterke verificatie met een bereik van eenvoudige verificatie-opties bevat: telefoonoproep, tekstbericht, smartcards en pincode of mobiele app-melding. Zie voor meer informatie [SSMS ondersteuning voor Azure AD MFA met SQL-Database en SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication).

De configuratiestappen omvat de volgende procedures om te configureren en gebruiken van Azure Active Directory-verificatie.

-   Maken en Azure AD te vullen.
-   Optioneel: Koppelen of wijzig de active directory die momenteel is gekoppeld aan uw Azure-abonnement.
-   Een Azure Active Directory-beheerder voor Azure SQL-server maken of [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
- De clientcomputers configureren.
-   Ingesloten databasegebruikers maken in de database die is toegewezen aan Azure AD-identiteiten.
-   Verbinding maken met de database met behulp van Azure AD-identiteiten.

U vindt gedetailleerde informatie over [hier](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

## <a name="protect-your-data-using-encryption"></a>Uw gegevens beschermen met versleuteling

[Azure SQL Database transparante gegevensversleuteling (TDE)](https://msdn.microsoft.com/library/dn948096.aspx) helpt beschermen tegen de dreiging van schadelijke activiteiten door te voeren realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogbestanden in rust zonder wijzigingen in de toepassing. TDE versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel, naam van de databaseversleutelingssleutel.

Zelfs als de volledige opslag wordt gecodeerd, is het erg belangrijk voor het versleutelen van uw database zelf ook. Dit is een implementatie van de verdediging in de diepte benadering voor gegevensbescherming. Als u van Azure SQL Database gebruikmaakt en bescherming van gevoelige gegevens zoals creditcard of burgerservicenummers wilt, kunt u databases met FIPS 140-2-gevalideerde 256 bits AES-versleuteling dat voldoet aan de vereisten van veel industrienormen (bijv, HIPAA, PCI) te versleutelen.

Het is belangrijk te begrijpen of bestanden betrekking heeft op [uitbreiding (BPE) van de buffer](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) worden niet versleuteld wanneer een database worden versleuteld met TDE. Moet u versleuteling op bestandsniveau hulpprogramma's zoals [BitLocker](https://technet.microsoft.com/library/cc732774) of de [Encrypting File System (EFS)](https://technet.microsoft.com/library/cc700811.aspx) gerelateerde bestanden voor de BPE.

Sinds een geautoriseerde gebruiker zoals een beveiligingsbeheerder of een databasebeheerder toegang tot de gegevens zelfs als de database is versleuteld met TDE, moet u ook Volg de onderstaande aanbevelingen:

-   Schakel SQL-verificatie op databaseniveau.
-   Gebruik Azure AD-verificatie met [RBAC-rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is).
-   Gebruikers en toepassingen moeten afzonderlijke accounts voor verificatie gebruiken. Deze manier kunt u de machtigingen voor gebruikers en toepassingen beperken en de risico's van schadelijke activiteiten te verminderen.
-   Beveiliging van de database op gebruikersniveau implementeren met behulp van de vaste databaserollen (zoals db_datareader of db_datawriter), of u kunt aangepaste rollen voor uw toepassing expliciete machtigen om objecten in de geselecteerde database maken.

Andere manieren om uw gegevens te versleutelen die u kunt overwegen:

-   [Versleuteling op celniveau](https://msdn.microsoft.com/library/ms179331.aspx) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
-   Versleuteling gebruikt met altijd versleuteld: [altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx) kunnen clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen en nooit onthullen de versleutelingssleutels voor de Database-Engine (SQL-Database of SQL Server). Als gevolg hiervan altijd versleuteld biedt een scheiding tussen degenen die eigenaar zijn van de gegevens (en het kunnen bekijken) en degenen die de gegevens te beheren (maar moet hebben geen toegang).
-   Met behulp van beveiliging: beveiliging op rijniveau kunt klanten toegang tot de rijen in een databasetabel op basis van de kenmerken van de gebruiker (bijv, groep lidmaatschap of -uitvoering context) van een query uit te voeren. Zie [Beveiliging op rijniveau](https://msdn.microsoft.com/library/dn765131) voor meer informatie.

## <a name="protect-data-in-transit"></a>De gegevens onderweg te beschermen
Beveiligen van gegevens die onderweg moet essentieel onderdeel van uw strategie voor gegevensbescherming. Omdat de gegevens wordt heen en weer worden verplaatst vanaf verschillende locaties, is de algemene aanbeveling dat u altijd SSL/TLS-protocollen gebruiken voor het uitwisselen van gegevens op verschillende locaties. In sommige gevallen wilt u mogelijk het hele communicatiekanaal tussen uw on-premises en cloud isoleren infrastructuur met behulp van een virtueel particulier netwerk (VPN).

Gegevens verplaatsen tussen uw on-premises infrastructuur en Azure, moet u passende beveiligingsmaatregelen zoals HTTPS- of VPN.

Voor organisaties die nodig zijn om de toegang van meerdere werkstations die zich on-premises naar Azure te beveiligen, gebruikt u [Azure site-naar-site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

Voor organisaties die nodig zijn voor het beveiligen van toegang van afzonderlijke werkstations die zich on-premises of buiten het bedrijf naar Azure, kunt u overwegen [punt-naar-Site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Grotere gegevenssets kan worden verplaatst via een speciale snelle WAN-verbinding, zoals [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Als u kiest voor ExpressRoute, kunt u ook de gegevens op het niveau van de toepassing met behulp van coderen [SSL/TLS](https://support.microsoft.com/kb/257591) of andere protocollen voor extra beveiliging.

Als u met Azure Storage via de Azure-Portal communiceert, worden alle transacties plaatsvinden via HTTPS. [REST API voor Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS kan ook worden gebruikt om te communiceren met [Azure Storage](https://azure.microsoft.com/services/storage/) en [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organisaties die niet voldoen aan het beveiligen van gegevens die onderweg zijn vatbaar voor [man-in-the-middle-aanvallen](https://technet.microsoft.com/library/gg195821.aspx), [afgeluisterd](https://technet.microsoft.com/library/gg195641.aspx) en sessiehijacking. Deze aanvallen kunnen worden de eerste stap bij het toegang krijgen tot vertrouwelijke gegevens.

Voor meer informatie over Azure VPN-optie door te lezen van het artikel [Planning en ontwerp voor VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

## <a name="enable-database-auditing"></a>Databasecontrole inschakelen
Controle van een exemplaar van SQL Server Database Engine of een individuele database omvat het vastleggen van gebeurtenissen die op de Database-Engine plaatsvinden en bij te houden. SQL Server audit maakt u server audits die server audit specificaties voor niveau gebeurtenissen van de server en database audit specificaties voor database niveau gebeurtenissen kunnen bevatten. Controlegebeurtenissen kunnen worden geschreven naar de gebeurtenislogboeken of bestanden controleren.

Er zijn verschillende niveaus van controle voor SQL Server, afhankelijk van de overheid of standaarden vereisten voor uw installatie. SQL Server Audit biedt de hulpprogramma's en processen die u hebben moet tot inschakelen, opslaan en audits op verschillende server en database-objecten weergeven.

[Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij uw Azure Storage-account.

Dankzij controles kunt u zorgen voor naleving van wet- en regelgeving, krijgt u inzicht in de activiteit in uw database en in de afwijkingen en discrepanties die kunnen wijzen op problemen voor het bedrijf of vermoedelijke schendingen van de beveiliging.

Controle kunt en voldoen aan de nalevingsstandaards vereenvoudigt maar niet garanderen dat.

Lees het artikel voor meer informatie over het controleren van de database en hoe deze in te schakelen, [inschakelen van controle en detectie van bedreigingen op SQL-servers in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="enable-database-threat-detection"></a>Detectie van dreigingen database inschakelen
Detectie van dreigingen SQL kunt u om te detecteren en op mogelijke bedreigingen reageert wanneer deze zich voordoen doordat beveiligingswaarschuwingen op vreemde activiteiten worden gedetecteerd. U ontvangt een waarschuwing bij verdachte databaseactiviteiten, mogelijke beveiligingsproblemen en SQL-injectieaanvallen, evenals database afwijkende toegangspatronen. Detectie van dreigingen SQL waarschuwingen Geef details op van de verdachte activiteit en actie voor het onderzoeken en het risico dat het beste.

SQL-injectie is bijvoorbeeld een van de algemene Web application beveiligingsproblemen op het Internet worden gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers te profiteren van de toepassing zwakke plekken in het injecteren schadelijke SQL-instructies in de toepassing invoervelden, schendingen veroorzaken of wijzigen van gegevens in de database.

Voor meer informatie over het instellen van de detectie van dreigingen voor uw database in de Azure portal zien [SQL Database met detectie van dreigingen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Bovendien detectie van dreigingen SQL waarschuwingen met integreert [Azure Security Center](https://azure.microsoft.com/services/security-center/). U wordt van harte uitgenodigd deze gedurende 60 dagen gratis te proberen.

Lees het artikel voor meer informatie over de detectie van dreigingen Database en hoe deze in te schakelen, [inschakelen van controle en detectie van bedreigingen op SQL-servers in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="conclusion"></a>Conclusie
Azure-Database is een databaseplatform robuuste met een volledige reeks beveiligingsfuncties die voldoen aan veel organisatie- en regelgeving nalevingsvereisten. U kunt gegevens beschermen door de fysieke toegang tot uw gegevens beheren en het gebruik van een groot aantal opties voor beveiliging van gegevens op het file-, kolom- of rijniveau met transparante gegevensversleuteling, -Celcodering of beveiliging. Altijd kan versleutelde ook bewerkingen op de versleutelde gegevens, het proces van toepassingsupdates vereenvoudigen. Op zijn beurt biedt toegang tot de controlelogboeken van de activiteit van de SQL-Database u de informatie die u nodig hebt, zodat u weet hoe en wanneer gegevens worden geopend.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over firewallregels, [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).
- Zie [Aanmeldingen beheren](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) voor meer informatie over gebruikers en aanmeldingen.
- Zie voor een zelfstudie [beveiligen van uw Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
