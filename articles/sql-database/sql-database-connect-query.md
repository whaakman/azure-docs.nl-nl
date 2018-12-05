---
title: Snelstartgidsen voor verbinding maken met Azure SQL-database en uitvoeren van query's| Microsoft Docs
description: In deze snelstartgidsen voor Azure SQL Database wordt beschreven hoe u verbinding maakt met een Azure SQL-database en vervolgens een query uitvoert.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: ab1f96b2348c24a746aaa7bdd670f396884ee3f5
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52870194"
---
# <a name="quickstarts-azure-sql-database-connect-and-query"></a>Snelstarts: verbinding maken met Azure SQL-database en query's uitvoeren

Het volgende document bevat koppelingen naar voorbeelden van Azure die laten zien hoe u verbinding maakt met een Azure SQL-database en vervolgens een query uitvoert. Het bevat ook enkele aanbevelingen voor Transport Layer Security.

## <a name="quickstarts"></a>Snelstartgidsen

| |  |
|---|---|
|[SQL Server Management Studio](sql-database-connect-query-ssms.md)|In deze Quick Start ziet u hoe u SSMS gebruikt om verbinding te maken met een Azure SQL-database en vervolgens Transact-SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen.|
|[Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)|In deze quickstart wordt uitgelegd hoe u Azure Data Studio gebruikt om verbinding te maken met een Azure SQL-database, waarna u met behulp van Transact-SQL-instructies (T-SQL) de TutorialDB maakt die wordt gebruikt in de zelfstudies voor Azure Data Studio.|
|[Azure Portal](sql-database-connect-query-portal.md)|In deze quickstart ziet u hoe u de queryeditor gebruikt om verbinding te maken met een SQL-database en vervolgens Transact-SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen.|
|[Visual Studio Code](sql-database-connect-query-vscode.md)|In deze Quick Start ziet u hoe u Visual Studio Code gebruikt om verbinding te maken met een Azure SQL-database en vervolgens Transact-SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen.|
|[.NET met Visual Studio](sql-database-connect-query-dotnet-visual-studio.md)|In deze snelstart wordt gedemonstreerd hoe u .NET Framework gebruikt om een C#-programma te maken met Visual Studio dat verbinding maakt met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om gegevens te doorzoeken.|
|[.NET Core](sql-database-connect-query-dotnet-core.md)|In deze snelstart wordt gedemonstreerd hoe u .NET Core gebruikt in Windows/Linux/macOS om een C#-programma te maken dat verbinding maakt met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om een query uit te voeren voor de gegevens.|
|[Go](sql-database-connect-query-go.md)|In deze snelstart ziet u hoe u Go kunt gebruiken om verbinding te maken met een Azure SQL-database. Bovendien worden er Transact-SQL-instructies voor het doorzoeken en wijzigen van gegevens beschreven.|
|[Java](sql-database-connect-query-java.md)|In deze snelstart wordt gedemonstreerd hoe u Java gebruikt om verbinding te maken met een Azure SQL-database, en hoe u vervolgens Transact-SQL-instructies gebruikt om een query uit te voeren voor gegevens.|
|[Node.js](sql-database-connect-query-nodejs.md)|In deze snelstart wordt gedemonstreerd hoe u Node.js gebruikt om een programma te maken dat verbinding maakt met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om een query uit te voeren voor gegevens.|
|[PHP](sql-database-connect-query-php.md)|In deze snelstart wordt gedemonstreerd hoe u PHP gebruikt om een programma te maken dat verbinding maakt met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om gegevens te doorzoeken.|
|[Python](sql-database-connect-query-python.md)|In deze snelstart wordt gedemonstreerd hoe u Python gebruikt om verbinding te maken met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om een query uit te voeren voor de gegevens. |
|[Ruby](sql-database-connect-query-ruby.md)|In deze snelstart wordt gedemonstreerd hoe u Ruby gebruikt om een programma te maken dat verbinding maakt met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om gegevens te doorzoeken.|
|||

## <a name="tls-considerations-for-sql-database-connectivity"></a>TLS-overwegingen voor de connectiviteit van SQL Database
Transport Layer Security (TLS) wordt gebruikt door alle stuurprogramma's die Microsoft aanbiedt of ondersteunt voor het maken van verbinding met Azure SQL Database. Er is geen speciale configuratie nodig. Voor alle verbindingen met SQL Server of Azure SQL Database raden we het aan om de volgende configuraties of een equivalent daarvan in te stellen voor alle toepassingen:

 - **Versleutelen = Aan**
 - **TrustServerCertificate = Uit**

Sommige systemen gebruiken andere, maar wel vergelijkbare sleutelwoorden voor deze configuratiesleutelwoorden. Deze configuraties zorgen ervoor dat het clientstuurprogramma de identiteit controleert van het TLS-certificaat dat afkomstig is van de server.

We raden u ook aan om TLS 1.1 en 1.0 op de client uit te schakelen als u moet voldoen aan de Payment Card Industry - Data Security Standard (PCI-DSS).

Stuurprogramma's die niet van Microsoft zijn, maken mogelijk niet standaard gebruik van TLS. Dit kan een factor zijn bij het maken van verbinding met Azure SQL Database. Bij toepassingen met ingesloten stuurprogramma's is het mogelijk niet toegestaan om deze verbindingsinstellingen te beheren. Wij raden u aan om de beveiliging van zulke stuurprogramma’s en toepassingen te controleren voordat u ze gebruikt op systemen die interactie hebben met gevoelige gegevens.

## <a name="next-steps"></a>Volgende stappen

Ga naar [Connectiviteitsarchitectuur van Azure SQL Database](sql-database-connectivity-architecture.md) voor informatie over connectiviteitsarchitectuur.