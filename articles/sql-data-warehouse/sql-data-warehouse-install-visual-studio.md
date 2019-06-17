---
title: Visual Studio en SSDT voor SQL datawarehouse installeren | Microsoft Docs
description: Visual Studio en SQL Server Development Tools (SSDT) voor Azure SQL Data Warehouse installeren
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/05/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: b2e34f1f72b1b0aa76d4a3031102d052118dae5f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66304117"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Visual Studio en SSDT voor SQL datawarehouse installeren
Gebruik Visual Studio 2019 het ontwikkelen van toepassingen voor SQL Data Warehouse. Op dit moment wordt niet SSDT voor Visual Studio 2019 ondersteund voor SQL Data Warehouse. 

Met behulp van Visual Studio met SSDT, kunt u tabellen, weergaven, opgeslagen procedures en nog veel meer objecten in uw SQL Data Warehouse visueel verkennen met behulp van de SQL Server-Objectverkenner. Ook kunt u query's uitvoeren.

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

U hebt de meest recente versie is geïnstalleerd als een update is niet gevonden. Als u wilt controleren of SSDT is geïnstalleerd, klikt u op **Help** / **About Microsoft Visual Studio** (Help > Info) en zoekt u in de lijst naar SQL Server Data Tools. Als de optie om te installeren niet beschikbaar is via Visual Studio, kunt u via de [SSDT downloaden] [ SSDT Download] pagina om te downloaden en SSDT handmatig installeren.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de nieuwste versie van SSDT hebt, kunt u [verbinding] [ connect] met uw SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Download Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
