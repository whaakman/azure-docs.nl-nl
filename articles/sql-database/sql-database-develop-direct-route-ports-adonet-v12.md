---
title: Poorten buiten 1433 voor SQL-Database | Microsoft Docs
description: Clientverbindingen van ADO.NET naar Azure SQL Database kunnen u de proxyserver wordt overgeslagen en communiceren rechtstreeks met de database met andere poorten dan 1433.
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
ms.assetid: 3f17106a-92fd-4aa4-b6a9-1daa29421f64
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: 3004edee0096d2e1594679371fb162b392e67f9a
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="ports-beyond-1433-for-adonet-45"></a>Poorten buiten 1433 voor ADO.NET 4.5
Dit onderwerp beschrijft het gedrag van de Azure SQL Database-verbinding voor clients die gebruikmaken van ADO.NET 4.5 of hoger. 

> [!IMPORTANT]
> Zie voor meer informatie over de architectuur van de connectiviteit [Azure SQL Database connectivity architectuur](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Externe tegenover binnen
Voor verbindingen met Azure SQL Database, moet eerst vragen we uw clientprogramma wordt uitgevoerd of *buiten* of *binnen* de grens van de Azure-cloud. De subsecties worden de twee algemene scenario's besproken.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Buiten:* Client wordt uitgevoerd op uw computer
Poort 1433 is de enige poort die moet worden geopend op uw bureaublad computer die als host fungeert voor uw clienttoepassing SQL-Database.

#### <a name="inside-client-runs-on-azure"></a>*Binnen:* Client wordt uitgevoerd op Azure
Wanneer de client wordt uitgevoerd binnen een grens van de Azure-cloud, gebruikt het kunt zogeheten een *direct route* om te communiceren met de Database van SQL server. Nadat een verbinding tot stand is gebracht, verdere interacties tussen de client en de database hebben betrekking op geen Azure SQL Database-Gateway.

De volgorde is als volgt:

1. ADO.NET 4.5 (of hoger) initieert een korte interactie met de Azure-cloud en ontvangt een dynamisch geïdentificeerde poortnummer.
   
   * Het dynamisch geïdentificeerde poortnummer valt binnen het bereik van 11000 11999 of 14000 14999.
2. ADO.NET maakt vervolgens verbinding met de SQL-Database-server rechtstreeks met geen middleware ertussen.
3. Query's rechtstreeks naar de database worden verzonden en resultaten rechtstreeks naar de client worden geretourneerd.

Zorg ervoor dat de poort bereiken van 11000 11999 en 14000-14999 op uw Azure-client-computer beschikbaar voor ADO.NET 4.5 client interactie met de SQL-Database blijven.

* In het bijzonder moet poorten in het bereik zijn van andere uitgaande blockers gratis.
* Op de virtuele machine van Azure de **Windows Firewall met geavanceerde beveiliging** bepaalt de poortinstellingen.
  
  * U kunt de [gebruikersinterface van de firewall](http://msdn.microsoft.com/library/cc646023.aspx) toevoegen van een regel die u opgeeft de **TCP** protocol samen met een poortbereik met de syntaxis, zoals **11000 11999**.

## <a name="version-clarifications"></a>Versie verduidelijkingen
Deze sectie wordt uitleg gegeven over de monikers die naar versies van het product verwijzen. Het bevat ook enkele koppelingen tussen versies tussen producten.

#### <a name="adonet"></a>ADO.NET
* ADO.NET 4.0 ondersteunt het protocol TDS 7.3, maar niet 7.4.
* ADO.NET 4.5 en hoger ondersteunt het 7.4 TDS-protocol.

## <a name="related-links"></a>Verwante koppelingen
* ADO.NET 4.6 is uitgebracht op 20 juli 2015. Er is een aankondiging blog van het .NET-team beschikbaar [hier](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* ADO.NET 4.5 is uitgebracht op 15 augustus 2012. Er is een aankondiging blog van het .NET-team beschikbaar [hier](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  
  * Er is een blogbericht over ADO.NET 4.5.1 beschikbaar [hier](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).
* [Lijst met de versie van de TDS-protocol](http://www.freetds.org/userguide/tdshistory.htm)
* [Overzicht van SQL Database-ontwikkeling](sql-database-develop-overview.md)
* [Azure SQL Database-firewall](sql-database-firewall-configure.md)
* [Procedure: firewall-instellingen configureren op de SQL-Database](sql-database-configure-firewall-settings.md)

