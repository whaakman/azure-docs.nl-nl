---
title: JSON transformeren met behulp van Azure Machine Learning Workbench uitvouwen
description: Het referentiedocument voor de transformatie 'Vouw JSON'
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
ms.openlocfilehash: dbda4b7b6d82e8cf1e89dc78ce82efbac08b9933
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646397"
---
# <a name="expand-json-transformation"></a>Vouw de JSON-transformatie
De **Vouw JSON** transformatie kan gebruikers om uit te breiden van een bestaande kolom die geldige JSON-tekst in meerdere kolommen bevat.

## <a name="how-to-perform-this-transformation"></a>Het uitvoeren van deze transformatie

Volg deze stappen voor het uitvoeren van deze transformatie:
1. Selecteer de bronkolom die JSON-tekst bevat.
2. Selecteer **Vouw JSON** uit de **transformeert** menu. Of klik met de rechtermuisknop op de kop van de bronkolom en selecteer **Vouw JSON** in het contextmenu. 
3. Klik op **OK**. 
 
Nieuwe kolommen worden toegevoegd naast de bronkolom. Deze kolommen bevatten eigenschappen van het volgende niveau van de hiërarchie in de JSON-tekst. Sleutel, wordt indien aanwezig, gebruikt om de naam van de bijbehorende kolom te maken. Geneste eigenschappen worden opgeslagen als JSON-tekst die gebruiker iteratief kunt uitvouwen of negeren indien nodig.

## <a name="examples"></a>Voorbeelden

De bron-columnn *klant* is uitgevouwen in twee kolommen *Customer.Name* en *Customer.Phone*.

| Klant                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| {"Naam": "Carrie Dodson' gemaakt,"Phone":"123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| {"Naam": "Leonard Robledo' gemaakt,"Phone":"456-7890-123"} | Leonard Robledo | 456-7890-123   |

