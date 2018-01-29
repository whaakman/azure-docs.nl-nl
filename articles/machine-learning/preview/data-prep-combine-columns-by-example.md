---
title: Kolommen door voorbeeld transformatie met behulp van Azure Machine Learning Workbench combineren
description: Het referentiedocument voor de transformatie kolommen door voorbeeld combineren
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 528d7ff5f74948fd1ae7b829d4b2fc38fa2f1109
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="combine-columns-by-example-transformation"></a>Kolommen combineren door voorbeeld transformatie
Deze transformatie kan de gebruiker een nieuwe kolom toevoegen door een combinatie van waarden uit meerdere kolommen. Gebruiker kan een waarde opgeven of vindt u voorbeelden van de combinatie van waarden voor het uitvoeren van deze transformatie. Wanneer de gebruiker ziet u voorbeelden van combinatie, de transformatie wordt verwerkt door dezelfde **door voorbeeld** engine die wordt gebruikt in de **kolom afgeleid door voorbeeld** transformeren.

## <a name="how-to-perform-this-transformation"></a>Het uitvoeren van deze transformatie

Deze transformatie, Ga als volgt te werk:
1. Twee of meer kolommen die u wilt combineren in één kolom selecteren. 
2. Selecteer **kolommen combineren door voorbeeld** van de **transformeert** menu. Of klik met de rechtermuisknop op de kop van een van de geselecteerde kolommen en selecteer **kolommen combineren door voorbeeld** in het contextmenu. De transformatie-Editor wordt geopend en een nieuwe kolom wordt toegevoegd naast de juiste meest geselecteerde kolom. De nieuwe kolom bevat de combinatie van waarden van elkaar gescheiden door een standaardscheidingsteken. Geselecteerde kolommen kunnen worden geïdentificeerd door de selectievakjes uit op de kolomkoppen. Toevoegen en verwijderen van kolommen uit de selectie kunnen worden gedaan met behulp van de selectievakjes uit.
3. U kunt de gecombineerde waarde in de zojuist gemaakte kolom bijwerken. De bijgewerkte waarde wordt gebruikt als voorbeeld voor meer informatie over de transformatie.
4. Klik op **OK** te accepteren van de transformatie.

### <a name="transform-editor-advanced-mode"></a>Editor transformeren: geavanceerde modus

Geavanceerde modus biedt een rijkere ervaring voor het combineren van kolommen. 

Selecteren **scheidingsteken** onder **kolommen combineren door** kan de gebruiker om op te geven tekenreeksen in de **scheidingsteken** in het tekstvak. Tabblad uit vanaf de **scheidingsteken** in het tekstvak voor de resultaten in de gird gegevens bekijken. Druk op **OK** doorvoeren van de transformatie.

Selecteren **voorbeelden** onder **kolommen combineren door** kan gebruiker worden voorbeelden gegeven van de combinatie van waarden. Als u een rij als voorbeeld verhogen, dubbelklik op de rijen in het raster. Typ in de verwachte uitvoer in het tekstvak tegen de gepromoveerde rij. Tabblad uit vanaf de **scheidingsteken** in het tekstvak voor de resultaten in de gird gegevens bekijken. Druk op **OK** doorvoeren van de transformatie. 

Gebruikers kan schakelen tussen de **standaardmodus** en de **geavanceerde modus** door te klikken op de koppelingen in de Editor transformeren.

### <a name="transform-editor-send-feedback"></a>Editor transformeren: Feedback verzenden

Te klikken op de **feedback verzenden** koppelen wordt geopend de **Feedback** dialoogvenster met het Opmerkingen vooraf ingevuld met de gebruiker voorbeelden is opgegeven. Gebruiker moet de inhoud van het opmerkingenvak bekijken en vindt u meer informatie zodat we kunnen het probleem te begrijpen. Als de gebruiker niet wil gegevens delen met Microsoft, gebruiker de vooraf ingevulde bijvoorbeeld gegevens verwijdert voordat u op de **Feedback verzenden** knop. 

### <a name="editing-existing-transformation"></a>Bestaande transformatie bewerken

Gebruikers kunnen bewerken in een bestaande **kolom combineren door voorbeeld** transformeren door te selecteren **bewerken** optie van de transformatie-stap. Te klikken op **bewerken** opent de Editor transformeren in **standaardmodus**. Gebruiker kan invoeren de **geavanceerde modus** door te klikken op de koppeling in de header. De voorbeelden die zijn opgegeven tijdens het maken van de transformatie wordt namelijk hier weergegeven.

## <a name="example-using-separators"></a>Voorbeeld met scheidingstekens

Een komma gevolgd door een spatie als scheidingsteken in dit voorbeeld wordt gebruikt om te combineren de *straat*, *stad*, *status*, en *ZIP* kolommen.

|Straatnaam|Plaats|Status|ZIP|Kolom|
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

## <a name="example-using-by-example"></a>Voorbeeld met door voorbeeld

De waarde in **vet** is opgegeven als voorbeeld.

|Datum|Maand|Jaar|Uur|Minuut|Tweede|Gecombineerde kolom|
|:----|:----|:----|:----|:----|:----|:----|
|13|okt|2016|15|01|23|**13-Oct-2016 15:01:23 PDT**|
|16|okt|2016|16|22|33|16-Oct-2016 15:01:33 PDT|
|17|okt|2016|12|43|12|17-Oct-2016 15:01:12 PDT|
|12|nov|2016|14|22|44|12-Nov-2016 15:01:44 PDT|
|23|nov|2016|01|52|45|23-Nov-2016 15:01:45 PDT|
|16|jan|2017|22|34|56|16-Jan-2016 15:01:56 PDT|
|23|mrt|2017|01|55|25|23-Mar-2016 15:01:25 PDT|
|16|apr|2017|11|34|36|16-Apr-2016 15:01:36 PDT|

