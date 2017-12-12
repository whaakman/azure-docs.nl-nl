---
title: Uitvoer van de stream Analytics Data Lake Store | Microsoft Docs
description: Configuratie van verificatie en autorisatie van een Azure Data Lake Store in een Stream Analytics-taak
keywords: 
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ea5baafa-0054-4c70-973a-6a3a8c6eaffc
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: e2010e86e56c1ce7a98fae97a8f6f00c30b61035
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="stream-analytics-data-lake-store-output"></a>Stream Analytics Data Lake Store-uitvoer
Stream Analytics-taken ondersteunen verschillende uitvoermethoden, een wordt een [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store is een ondernemingsbrede opslagplaats op hyperschaal voor analytische workloads van big data. Data Lake Store kunt u voor het opslaan van gegevens van elke grootte, type en opnamesnelheid snelheid voor operationele en experimentele analyses.

## <a name="authorize-a-data-lake-store-account"></a>Een Data Lake Store-account toestaan
1. Wanneer Data Lake Store als uitvoer in de Azure portal is geselecteerd, wordt u gevraagd naar het gebruik van uw bestaande Data Lake Store toestaan of om aan te vragen toegang tot de Data Lake Store.
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. Als u al toegang naar Data Lake Store, klik op 'Nu autoriseren' en gedurende een korte periode een pagina wordt weergegeven dat aangeeft 'Omleidt naar autorisatie'. De pagina wordt automatisch gesloten en u krijgt de pagina waarmee u de uitvoer van de Data Lake Store configureren.

Als u niet hebt aangemeld voor Data Lake Store, voert u de koppeling 'Nu aanmelden' om de aanvraag niet starten of voert u de [gestart instructies](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>Configureer de eigenschappen van de uitvoer Data Lake Store
Zodra u het Data Lake Store-account dat is geverifieerd hebt, kunt u de eigenschappen configureren voor de uitvoer van uw Data Lake Store. De onderstaande tabel wordt de lijst met namen van eigenschappen en hun beschrijving voor het configureren van de uitvoer van uw Data Lake Store.

<table>
<tbody>
<tr>
<td><B>DE NAAM VAN EIGENSCHAP</B></td>
<td><B>BESCHRIJVING</B></td>
</tr>
<tr>
<td>Uitvoeraliassen</td>
<td>Dit is een beschrijvende naam die is gebruikt in query's om de queryuitvoer naar Data Lake Store.</td>
</tr>
<tr>
<td>Data Lake Store-Account</td>
<td>De naam van het opslagaccount waar u de uitvoer wilt verzenden. U krijgt een lijst van de aangemelde gebruiker toegang tot heeft Data Lake Store-accounts.</td>
</tr>
<tr>
<td>Pad voorvoegsel patroon [<I>optionele</I>]</td>
<td>Het pad dat wordt gebruikt om uw bestanden binnen de opgegeven Data Lake Store-Account te schrijven. <BR>{date} {time}<BR>Voorbeeld 1: Map1/logs / {date} / {time}<BR>Voorbeeld 2: Map1/logs / {date}</td>
</tr>
<tr>
<td>Datum notatie [<I>optionele</I>]</td>
<td>Als de datum-token in het pad van het voorvoegsel wordt gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld. Voorbeeld: Jjjj/MM/DD</td>
</tr>
<tr>
<td>Tijdnotatie [<I>optionele</I>]</td>
<td>Als het token tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de tijdnotatie waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel HH.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Serialisatie-indeling voor uitvoergegevens. JSON, CSV en Avro worden ondersteund.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Als CSV- of JSON-indeling, moet een codering worden opgegeven. De enige ondersteunde coderingsindeling is UTF-8 op dit moment.</td>
</tr>
<tr>
<td>Scheidingsteken</td>
<td>Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn komma, puntkomma, spatie, tab en de verticale balk.</td>
</tr>
<tr>
<td>Indeling</td>
<td>Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer wordt ingedeeld door elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt ingedeeld als een matrix met JSON-objecten.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Vernieuwen van Data Lake Store-autorisatie
Er is momenteel een beperking waar het verificatietoken moet worden handmatig vernieuwd om de 90 dagen voor alle taken met Data Lake Store-uitvoer. Ook moet u uw Data Lake Store-account opnieuw verifiëren als u uw wachtwoord is gewijzigd sinds de taak is gemaakt of laatst geverifieerd. Een symptoom zijn van dit probleem is geen taakuitvoer en een fout in de logboeken van de bewerking opnieuw autorisatie nodig waarmee wordt aangegeven.

U lost dit probleem, stop de actieve taak en gaat u naar de uitvoer van uw Data Lake Store. Klik op de koppeling 'Vernieuwen autorisatie' en gedurende een korte periode een pagina wordt weergegeven die wijzen op 'Omleidt naar autorisatie..'. De pagina wordt automatisch gesloten en als dit lukt, wordt aangegeven 'Autorisatie is vernieuwd'. U vervolgens hoeft te klikken op 'Opslaan' aan de onderkant van de pagina en kan worden voortgezet door de taak in de laatste keer dat kan worden gestopt om gegevensverlies te voorkomen opnieuw te starten.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

