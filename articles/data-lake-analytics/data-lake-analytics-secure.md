---
title: Beveiliging van Azure Data Lake Analytics voor meerdere gebruikers
description: Informatie over het configureren van meerdere gebruikers voor het uitvoeren van taken in Azure Data Lake Analytics.
ms.service: data-lake-analytics
services: data-lake-analytics
author: matt1883
ms.author: mahi
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: c6b86e25602f36896855d2593952609904396879
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051581"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Gebruikerstoegang tot informatie over de taak om informatie over taak te configureren in Azure Data Lake Analytics 

In Azure Data Lake Analytics kunt u meerdere gebruikersaccounts of service-principals om taken uit te voeren. 

In de volgorde voor dezelfde gebruikers om te zien van de informatie over gedetailleerde taak, moeten de gebruikers kunnen het lezen van de inhoud van de taak-mappen. De taak mappen bevinden zich in `/system/` directory. 

Als de vereiste machtigingen zijn niet geconfigureerd, kan de gebruiker ziet de foutmelding: `Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>Gebruikerstoegang tot informatie over taak configureren

U kunt de **Wizard gebruiker toevoegen** voor het configureren van de ACL's op de mappen. Zie voor meer informatie, [een nieuwe gebruiker toevoegen](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Als u meer nodig hebt nauwkeurige controle of de noodzaak om een script van de machtigingen, klikt u vervolgens de mappen beveiligen als volgt:

1. Verleen **uitvoeren** machtigingen (via een toegang ACL) voor de hoofdmap:
   - /
   
2. Verleen **uitvoeren** en **lezen** machtigingen (via een toegangs-ACL- en een standaard-ACL) voor de mappen die de taak mappen bevatten. Bijvoorbeeld, voor een specifieke taak die is uitgevoerd op 25 mei 2018 wordt moeten deze mappen worden geopend:
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
