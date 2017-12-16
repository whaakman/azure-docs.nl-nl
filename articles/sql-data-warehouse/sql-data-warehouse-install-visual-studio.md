---
title: Visual Studio en SSDT voor SQL datawarehouse installeren | Microsoft Docs
description: Visual Studio en SQL Server Development Tools (SSDT) voor Azure SQL Data Warehouse installeren
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 12/14/2017
ms.author: kevin;barbkess
ms.openlocfilehash: e8170eefb2e359719684e08749180a4e7784f9b6
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Visual Studio en SSDT voor SQL datawarehouse installeren
Voor het ontwikkelen van toepassingen voor SQL Data Warehouse, wordt u aangeraden de meest recente versie van Visual Studio met de meest recente versie van SQL Server Data Tools (SSDT).  Visual Studio 2013 Update 5 met SSDT wordt ook ondersteund voor compatibiliteit met eerdere versies.  

Wanneer u Visual Studio met SSDT gebruikt, kunt u de SQL Server-objectverkenner gebruiken om tabellen, weergaven, opgeslagen procedures en nog veel meer objecten in de SQL Data Warehouse visueel te verkennen, en kunt u query's uitvoeren.

> [!NOTE]
> SQL Data Warehouse biedt nog geen ondersteuning voor Visual Studio-Database-projecten.  Deze functie wordt in een toekomstige versie toegevoegd. Periodieke om updates te ontvangen over deze functie, neem stem [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>Stap 1: Installeer Visual Studio
Volg deze koppelingen wilt downloaden en installeren van Visual Studio. Als u al Visual Studio 2013 hebt of hoger is geïnstalleerd, kunt u doorgaan met stap 2, SSDT installeren.

1. [Visual Studio downloaden][].
2. Ga als volgt de [Visual Studio installeren] [ Installing Visual Studio] leiden op MSDN en kies de standaardconfiguraties.

## <a name="step-2-install-ssdt"></a>Stap 2: SSDT installeren
Als u SSDT voor Visual Studio wilt installeren, voert u de volgende stappen uit om in Visual Studio op een SSDT-update te controleren.

1. Klik in Visual Studio op **extra** / **uitbreidingen en Updates...** / **Updates**
2. Selecteer **Product Updates** (Productupdates) en zoek naar **Microsoft SQL Server Update for database tooling** (Microsoft SQL Server Update voor databasehulpprogramma's)

Als er geen update wordt gevonden, hebt u de meest recente versie al geïnstalleerd.  Als u wilt controleren of SSDT is geïnstalleerd, klikt u op **Help** / **About Microsoft Visual Studio** (Help > Info) en zoekt u in de lijst naar SQL Server Data Tools.  De meest recente versie van SSDT is 14.0.60525.0.  Als de optie voor het installeren van niet beschikbaar vanuit Visual Studio is, u kunt ook kunt u via de [SSDT downloaden] [ SSDT Download] pagina om te downloaden en SSDT handmatig installeren.

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
