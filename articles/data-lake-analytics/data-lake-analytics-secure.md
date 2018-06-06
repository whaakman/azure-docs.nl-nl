---
title: Beveiligen van Azure Data Lake Analytics voor meerdere gebruikers
description: Informatie over het configureren van meerdere gebruikers voor het uitvoeren van taken in Azure Data Lake Analytics.
ms.service: data-lake-analytics
services: data-lake-analytics
author: matt1883
ms.author: mahi
manager: kfile
editor: jasonwhowell
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: 1d92e6d0e619584dedcbc9a66450c25dd1ac8b75
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701399"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Gebruikerstoegang tot informatie over de taak om informatie over de taak te configureren in Azure Data Lake Analytics 

In Azure Data Lake Analytics kunt u meerdere gebruikersaccounts of service-principals kunt gebruiken om uit te voeren taken. 

De gebruikers moeten kunnen worden gelezen van de inhoud van de taak mappen in volgorde voor dezelfde gebruikers om te zien van de voor gedetailleerde taakinformatie. De taak mappen bevinden zich in `/system/` directory. 

Als de benodigde machtigingen zijn niet geconfigureerd, kan de gebruiker een fout wordt weergegeven: `Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>Configureer de gebruikerstoegang tot informatie over de taak

U kunt de **Wizard gebruiker toevoegen** voor het configureren van de ACL's op de mappen. Zie voor meer informatie [een nieuwe gebruiker toevoegen](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Als u meer gedetailleerde controle of moet de machtiging script vervolgens de mappen beveiligen als volgt:

1. Verleen **uitvoeren** machtigingen (via een access ACL) voor de hoofdmap:
   - /
   
2. Verleen **uitvoeren** en **lezen** machtigingen (via een ACL-toegang en een standaard ACL) voor de mappen die de taak mappen bevatten. Bijvoorbeeld: voor een specifieke taak die is uitgevoerd op 25 mei 2018, deze mappen moeten worden geopend:
   - / System
   - / system/jobservice
   - /System/jobservice/jobs
   - /System/jobservice/Jobs/Usql
   - /System/jobservice/Jobs/Usql/2018
   - /System/jobservice/Jobs/Usql/2018/05
   - /System/jobservice/Jobs/Usql/2018/05/25
   - /System/jobservice/Jobs/Usql/2018/05/25/11
   - /System/jobservice/Jobs/Usql/2018/05/25/11/01
   - systeem/jobservice/taken/Usql/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Volgende stappen
[Een nieuwe gebruiker toevoegen](data-lake-analytics-manage-use-portal.md#add-a-new-user)
