---
title: Visual Studio en SSDT voor SQL datawarehouse installeren | Microsoft Docs
description: Visual Studio en SQL Server Development Tools (SSDT) voor Azure SQL Data Warehouse installeren
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: a2f01424dedb977000d0e4150f4a31c1a9a21cfb
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Visual Studio en SSDT voor SQL datawarehouse installeren
Voor het ontwikkelen van toepassingen voor SQL Data Warehouse, wordt u aangeraden de meest recente versie van Visual Studio met de meest recente versie van SQL Server Data Tools (SSDT).  Visual Studio 2013 Update 5 met SSDT wordt ook ondersteund voor compatibiliteit met eerdere versies.  

Met Visual Studio met SSDT, kunt u de SQL Server Object Explorer gebruiken om visueel verkennen tabellen, weergaven, opgeslagen procedures en veel meer objecten in uw SQL Data Warehouse, evenals een query's uitvoeren.

> [!NOTE]
> SQL Data Warehouse biedt nog geen ondersteuning voor Visual Studio-Database-projecten. Periodieke om updates te ontvangen over deze functie, neem stem [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>Stap 1: Installeer Visual Studio
Volg deze koppelingen wilt downloaden en installeren van Visual Studio. Als u al Visual Studio 2013 hebt of hoger is geïnstalleerd, kunt u doorgaan met stap 2, SSDT installeren.

1. [Visual Studio downloaden][].
2. Ga als volgt de [Visual Studio installeren] [ Installing Visual Studio] leiden op MSDN en kies de standaardconfiguraties.

## <a name="step-2-install-ssdt"></a>Stap 2: SSDT installeren
Als u wilt installeren SSDT voor Visual Studio, eerst controleren op een SSDT-update van Visual Studio met de volgende stappen.

1. Klik in Visual Studio op **extra** / **uitbreidingen en Updates...** / **Updates**
2. Selecteer **Product Updates** (Productupdates) en zoek naar **Microsoft SQL Server Update for database tooling** (Microsoft SQL Server Update voor databasehulpprogramma's)

Als er geen update wordt gevonden, hebt u de meest recente versie al geïnstalleerd.  Als u wilt controleren of SSDT is geïnstalleerd, klikt u op **Help** / **About Microsoft Visual Studio** (Help > Info) en zoekt u in de lijst naar SQL Server Data Tools. De meest recente versie van SSDT is 14.0.60525.0. Als de optie voor het installeren van niet beschikbaar vanuit Visual Studio is, u kunt ook kunt u via de [SSDT downloaden] [ SSDT Download] pagina om te downloaden en SSDT handmatig installeren.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de nieuwste versie van SSDT hebt, bent u klaar om te [verbinding] [ connect] met uw SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio downloaden]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
