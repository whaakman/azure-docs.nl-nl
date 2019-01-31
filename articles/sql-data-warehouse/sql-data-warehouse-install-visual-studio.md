---
title: Visual Studio en SSDT voor SQL datawarehouse installeren | Microsoft Docs
description: Visual Studio en SQL Server Development Tools (SSDT) voor Azure SQL Data Warehouse installeren
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: e27a3d4f2fe4b8421b637e485d77d6376d15bf9a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55461488"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Visual Studio en SSDT voor SQL datawarehouse installeren
Voor het ontwikkelen van toepassingen voor SQL Data Warehouse, wordt u aangeraden de meest recente versie van Visual Studio met de meest recente versie van SQL Server Data Tools (SSDT).  Visual Studio 2013 Update 5 met SSDT wordt ook ondersteund voor compatibiliteit met eerdere versies.  

Met behulp van Visual Studio met SSDT, kunt u visueel verkennen van tabellen, weergaven, opgeslagen procedures en nog veel meer objecten in uw SQL Data Warehouse, evenals query's uitvoeren met de SQL Server-Objectverkenner.

> [!NOTE]
> SQL Data Warehouse biedt nog geen ondersteuning voor Visual Studio-Database-projecten. Voor het ontvangen van periodieke updates op deze functie, stemmen Neem op [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>Stap 1: Visual Studio installeren
Volg deze koppelingen om te downloaden en installeren van Visual Studio. Als u al Visual Studio 2013 of hoger is geïnstalleerd, kunt u doorgaan met stap 2, SSDT installeren.

1. [Download Visual Studio][].
2. Ga als volgt de [Visual Studio installeren] [ Installing Visual Studio] begeleiden op MSDN en kies de standaardconfiguraties.

## <a name="step-2-install-ssdt"></a>Stap 2: SSDT installeren
Voor het installeren van SSDT voor Visual Studio, eerst controleren op een SSDT-update van Visual Studio door de volgende stappen.

1. Klik in Visual Studio op **extra** / **extensies en Updates...** / **Updates**
2. Selecteer **Product Updates** (Productupdates) en zoek naar **Microsoft SQL Server Update for database tooling** (Microsoft SQL Server Update voor databasehulpprogramma's)

Als er geen update wordt gevonden, hebt u de meest recente versie al geïnstalleerd.  Als u wilt controleren of SSDT is geïnstalleerd, klikt u op **Help** / **About Microsoft Visual Studio** (Help > Info) en zoekt u in de lijst naar SQL Server Data Tools. De meest recente versie van SSDT is 14.0.60525.0. Als de optie voor het installeren van niet beschikbaar is via Visual Studio is, u kunt ook u kunt de [SSDT downloaden] [ SSDT Download] pagina om te downloaden en SSDT handmatig installeren.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de nieuwste versie van SSDT hebt, bent u klaar om te [verbinding] [ connect] met uw SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Download Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
