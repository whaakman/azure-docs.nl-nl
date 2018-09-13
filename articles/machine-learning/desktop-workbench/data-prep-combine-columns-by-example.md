---
title: Voorbeeld van de transformatie met behulp van Azure Machine Learning Workbench kolommen combineren
description: Het referentiedocument voor de transformatie 'Kolommen combineren per voorbeeld'
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 621601ad3576aad13f2f71062ee2351cf1a394c8
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644228"
---
# <a name="combine-columns-by-example-transformation"></a>Kolommen combineren per voorbeeld transformatie
Deze transformatie kan de gebruiker een nieuwe kolom toevoegen door een combinatie van waarden uit meerdere kolommen. Gebruiker kan een waarde opgeven of vindt u voorbeelden van gecombineerde waarden om uit te voeren van deze transformatie. Wanneer de gebruiker ziet u voorbeelden van combinatie, de transformatie wordt afgehandeld door hetzelfde **door voorbeeld** -engine die wordt gebruikt in de **kolom afleiden per voorbeeld** transformeren.

## <a name="how-to-perform-this-transformation"></a>Het uitvoeren van deze transformatie

Als u wilt uitvoeren tijdens deze transformatie, de volgende stappen uit:
1. Selecteer twee of meer kolommen die u wilt combineren in één kolom. 
2. Selecteer **kolommen combineren per voorbeeld** uit de **transformeert** menu. Of klik met de rechtermuisknop op de kop van een van de geselecteerde kolommen en selecteer **kolommen combineren per voorbeeld** in het contextmenu. De transformatie-Editor wordt geopend en een nieuwe kolom wordt toegevoegd naast de juiste meest geselecteerde kolom. De nieuwe kolom bevat de gecombineerde waarden worden gescheiden door een standaard-scheidingsteken. Geselecteerde kolommen kunnen worden geïdentificeerd door de selectievakjes in bij de kolomkoppen. Toevoegen en verwijderen van kolommen uit de selectie kunnen worden gedaan met behulp van de selectievakjes in.
3. U kunt de gecombineerde waarde in de zojuist gemaakte kolom bijwerken. De bijgewerkte waarde wordt gebruikt als voorbeeld voor meer informatie over de transformatie.
4. Klik op **OK** de transformatie accepteert.

### <a name="transform-editor-advanced-mode"></a>Editor transformeren: geavanceerde modus

Geavanceerde modus biedt een rijkere ervaring voor het combineren van kolommen. 

Selecteren **scheidingsteken** onder **kolommen combineren door** kan de gebruiker om op te geven tekenreeksen in de **scheidingsteken** in het tekstvak. Tabblad uit vanaf de **scheidingsteken** tekstvak in op de resultaten in de gird gegevens bekijken. Druk op **OK** om door te voeren van de transformatie.

Selecteren **voorbeelden** onder **kolommen combineren door** kan gebruiker vindt u voorbeelden van gecombineerde waarden. Als u wilt promoveren van een rij als een voorbeeld, dubbelklik op de rijen in het raster. Typ in de verwachte uitvoer in het tekstvak in op basis van de gepromoveerde rij. Tabblad uit vanaf de **scheidingsteken** tekstvak in op de resultaten in de gird gegevens bekijken. Druk op **OK** om door te voeren van de transformatie. 

Gebruiker kan schakelen tussen de **Basismodus** en de **geavanceerde modus** door te klikken op de koppelingen in de Editor transformeren.

### <a name="transform-editor-send-feedback"></a>Editor transformeren: Feedback verzenden

Te klikken op de **feedback verzenden** koppeling wordt geopend de **Feedback** dialoogvenster met het Opmerkingen vooraf ingevuld met de voorbeelden-gebruiker is opgegeven. Gebruiker moet de inhoud van het opmerkingenvak bekijken en vindt u meer informatie naar ons beter te begrijpen van het probleem. Als de gebruiker wil geen gegevens delen met Microsoft, gebruiker de vooraf ingevulde bijvoorbeeld gegevens verwijdert voordat u op de **Feedback verzenden** knop. 

### <a name="editing-existing-transformation"></a>Bestaande transformatie bewerken

Een gebruiker kunt bewerken van een bestaande **kolom combineren door voorbeeld** transformeren door te selecteren **bewerken** optie van de transformatie-stap. Te klikken op **bewerken** opent de Editor transformeren in **Basismodus**. Gebruiker kan invoeren de **geavanceerde modus** door te klikken op de koppeling in de header. De voorbeelden die zijn opgegeven tijdens het maken van de transformatie worden er weergegeven.

## <a name="example-using-separators"></a>Voorbeeld met scheidingstekens

Een door komma's gevolgd door een spatie als scheidingsteken in dit voorbeeld wordt gebruikt om te combineren de *straat*, *plaats*, *status*, en *ZIP* kolommen.

|Straatnaam|Plaats|Status|POSTCODE|Kolom|
|:----|:----|:----|:----|:----|
|16011 N.E. 36th manier|REDMOND|WA|98052|16011 N.E. 36th way, REDMOND, WA, 98052|
|16021 N.E. 36th manier|REDMOND|WA|98052|16021 N.E. 36th way, REDMOND, WA, 98052|
|16031 N.E. 36th manier|REDMOND|WA|98052|16031 N.E. 36th way, REDMOND, WA, 98052|
|16041 N.E. 36th manier|REDMOND|WA|98052|16041 N.E. 36th way, REDMOND, WA, 98052|
|16051 N.E. 36th manier|REDMOND|WA|98052|16051 N.E. 36th way, REDMOND, WA, 98052|
|16061 N.E. 36th manier|REDMOND|WA|98052|16061 N.E. 36th way, REDMOND, WA, 98052|
|3460 157th avenue NE|REDMOND|WA|98052|NE in de 3460 157th-avenue, REDMOND, WA, 98052|
|3350 157th Ave N.E.|REDMOND|WA|98052|N.E. in de 3350 157th-Ave, REDMOND, WA, 98052|
|3240 157th avenue N.E.|REDMOND|WA|98052|N.E. in de 3240 157th-avenue, REDMOND, WA, 98052|

## <a name="example-using-by-example"></a>Voorbeeld met behulp van per voorbeeld

De waarde in **vet** is opgegeven als voorbeeld.

|Date|Maand|Jaar|Uur|Minuut|Seconde|Gecombineerde kolom|
|:----|:----|:----|:----|:----|:----|:----|
|13|okt|2016|15|01|23|**13-Oct-2016 15:01:23 PDT**|
|16|okt|2016|16|22|33|16 oktober 2016 15:01:33 PDT|
|17|okt|2016|12|43|12|17 oktober 2016 15:01:12 PDT|
|12|nov|2016|14|22|44|November-12-2016 15:01:44 PDT|
|23|nov|2016|01|52|45|23 november 2016 15:01:45 PDT|
|16|jan|2017|22|34|56|16-Jan-2016 15:01:56 PDT|
|23|mrt|2017|01|55|25|23-maart-2016 15:01:25 PDT|
|16|apr|2017|11|34|36|16 april 2016 15:01:36 PDT|

