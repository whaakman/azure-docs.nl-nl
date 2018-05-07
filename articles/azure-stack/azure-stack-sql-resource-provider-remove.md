---
title: Met behulp van SQL-databases op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u SQL-databases kan implementeren als een service op Azure-Stack en de snelle stappen voor het implementeren van de SQL Server resource provider-adapter.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: c2686a2d5241af46e70263d1827028aa7e9b2138
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="remove-the-sql-resource-provider"></a>Verwijderen van de SQL-resourceprovider

Als u wilt verwijderen van de SQL-resourceprovider, is het essentieel voor Verwijder eerst alle afhankelijkheden:

1. Zorg ervoor dat u het oorspronkelijke implementatiepakket dat u voor deze versie van de adapter SQL resource provider hebt gedownload.

2. Alle gebruikersdatabases moeten worden verwijderd van de resourceprovider. (De databases niet verwijderen van de gegevens.) Deze taak moet worden uitgevoerd door de gebruikers zelf.

3. De beheerder moet de hosting-servers verwijderen van de SQL resource provider-adapter.

4. De beheerder moet de schema's die verwijzen naar de SQL resource provider-adapter verwijderen.

5. De beheerder moet elk SKU's en quota's die gekoppeld aan de SQL resource provider-adapter zijn verwijderen.

6. Opnieuw uit het script voor implementatie met de volgende elementen:
    - De - parameter verwijderen
    - De Azure Resource Manager-eindpunten
    - De DirectoryTenantID
    - De referenties voor de service administrator-account

