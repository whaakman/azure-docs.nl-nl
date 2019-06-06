---
title: Richtlijnen voor herstel na noodgevallen voor Azure Data Lake Analytics
description: Informatie over het plannen van herstel na noodgevallen voor uw Azure Data Lake Analytics-accounts.
services: data-lake-analytics
author: MikeRys
ms.author: mrys
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ea1d4020aa9be23b4839690ae0b386d35bce8a23
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66498887"
---
# <a name="disaster-recovery-guidance-for-azure-data-lake-analytics"></a>Richtlijnen voor herstel na noodgevallen voor Azure Data Lake Analytics

Azure Data Lake Analytics is een service op aanvraag voor het uitvoeren van analysetaken, om de analyse van big data te vereenvoudigen. U hoeft geen hardware te implementeren, configureren en optimaliseren, maar kunt query's schrijven om uw data te transformeren en waardevolle inzichten te verkrijgen. De analyseservice kan taken van elke schaal onmiddellijk verwerken, door de hoeveelheid rekenkracht die u nodig hebt aan te passen. U betaalt alleen voor de tijd die nodig is voor het uitvoeren van uw taak, wat het gebruik van deze service zeer rendabel maakt. In dit artikel bevat richtlijnen voor het beveiligen van uw taken tegen zeldzame regiobrede storingen of onbedoelde verwijderingen.

## <a name="disaster-recovery-guidance"></a>Richtlijnen voor herstel na noodgevallen

Wanneer u Azure Data Lake Analytics, is het van cruciaal belang voor u het voorbereiden van uw eigen noodherstelplan. In dit artikel begeleidt u voor het bouwen van een herstelplan na noodgevallen. Er zijn aanvullende bronnen waarmee u uw eigen plan maken:
- [Herstel na fouten en noodgevallen voor Azure-toepassingen](/azure/architecture/reliability/disaster-recovery)
- [Technische richtlijnen voor flexibiliteit van Azure](/azure/architecture/reliability)

## <a name="best-practices-and-scenario-guidance"></a>Aanbevolen procedures en richtlijnen voor scenario

U kunt een terugkerende U-SQL-taak uitvoeren in een ADLA-account in een regio die leest en schrijft U-SQL-tabellen, evenals de niet-gestructureerde gegevens.  Na een noodgeval voorbereiden door deze stappen:

1. ADLA en ADLS-accounts maken in de secundaire regio die wordt gebruikt tijdens een storing.

   > [!NOTE]
   > Sinds de accountnamen wereldwijd uniek zijn, gebruikt u een consistente schematische naam die aangeeft welk account secundaire.

2. Voor niet-gestructureerde gegevens door te verwijzen naar [richtlijnen voor herstel na noodgevallen voor gegevens in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)

3. Voor gestructureerde gegevens die zijn opgeslagen in ADLA-tabellen en -databases, kopieën van de van metagegevensartefacten, zoals databases, tabellen, tabelfuncties en assembly's te maken. U moet deze artefacten periodiek opnieuw te synchroniseren wanneer er wijzigingen optreden in de productieomgeving. Nieuw geplaatste gegevens heeft bijvoorbeeld naar de secundaire regio worden gerepliceerd door de gegevens kopiëren en in de tweede tabel invoegen.

   > [!NOTE]
   > De namen van deze objecten zijn gericht op de secundaire-account en zijn niet globaal uniek zijn, zodat ze kunnen dezelfde namen als in de primaire productie-account hebben.

Tijdens een storing moet u uw scripts bijwerken zodat de invoerpaden naar het secundaire eindpunt verwijst. Vervolgens de gebruikers hun taken indienen bij de ADLA account in de secundaire regio. De uitvoer van de taak wordt vervolgens naar de ADLA en ADLS-account in de secundaire regio worden geschreven.

## <a name="next-steps"></a>Volgende stappen

[Richtlijnen voor herstel na noodgevallen voor gegevens in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)
