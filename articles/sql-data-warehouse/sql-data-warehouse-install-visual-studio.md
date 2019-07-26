---
title: Visual Studio en SSDT voor SQL Data Warehouse installeren | Microsoft Docs
description: Visual Studio en SQL Server Development Tools (SSDT) voor Azure SQL Data Warehouse installeren
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/05/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: aa904ac62ce41cc89945ddfdef2e426143e42bb4
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479480"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Visual Studio en SSDT voor SQL Data Warehouse installeren
Gebruik Visual Studio 2019 om toepassingen te ontwikkelen voor SQL Data Warehouse. Momenteel wordt Visual Studio 2019 SSDT niet ondersteund voor SQL Data Warehouse. 

Met Visual Studio met SSDT kunt u de SQL Server-objectverkenner gebruiken om tabellen, weer gaven, opgeslagen procedures en nog veel meer objecten visueel in uw SQL Data Warehouse te verkennen. Ook kunt u query's uitvoeren.

> [!NOTE]
> SQL Data Warehouse biedt nog geen ondersteuning voor Visual Studio-Database-projecten. Als u periodieke updates voor deze functie wilt ontvangen, kunt u stemmen op [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>Stap 1: Visual Studio installeren
Volg deze koppelingen om Visual Studio te downloaden en te installeren. Als u Visual Studio 2013 of hoger al hebt geïnstalleerd, kunt u door gaan naar stap 2 en SSDT installeren.

1. [Visual Studio downloaden][].
2. Volg de instructies in [Installing Visual Studio][Installing Visual Studio] (Visual Studio installeren) op MSDN en kies de standaardconfiguraties.

## <a name="step-2-install-ssdt"></a>Stap 2: SSDT installeren
Als u SSDT voor Visual Studio wilt installeren, controleert u eerst op een SSDT-update in Visual Studio door de volgende stappen uit te voeren.

1. Klik in Visual Studio op **extra** / **extensies en updates...** / **Updates**
2. Selecteer **Product Updates** (Productupdates) en zoek naar **Microsoft SQL Server Update for database tooling** (Microsoft SQL Server Update voor databasehulpprogramma's)

Als een update niet wordt gevonden, moet de nieuwste versie zijn geïnstalleerd. Als u wilt controleren of SSDT is geïnstalleerd, klikt u op **Help** / **About Microsoft Visual Studio** (Help > Info) en zoekt u in de lijst naar SQL Server Data Tools. Als de optie voor het installeren van Visual Studio niet beschikbaar is, kunt u de [SSDT-Download][SSDT Download] pagina bezoeken om SSDT hand matig te downloaden en te installeren.

## <a name="next-steps"></a>Volgende stappen
Nu u de nieuwste versie van SSDT hebt, bent u klaar om [verbinding te maken][connect] met uw SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio downloaden]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
