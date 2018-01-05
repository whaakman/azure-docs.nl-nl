---
title: Vouw de JSON-transformatie die met behulp van Azure Machine Learning Workbench
description: Het referentiedocument voor de transformatie 'JSON Vouw'
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
ms.openlocfilehash: 614f4422aa987fc32dcce62826bb2477473fdc32
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="expand-json-transformation"></a>Vouw de JSON-transformatie
De **Vouw JSON** transformatie kan gebruikers uit te breiden van een bestaande kolom die geldig JSON-tekst in meerdere kolommen bevat.

## <a name="how-to-perform-this-transformation"></a>Het uitvoeren van deze transformatie

Volg deze stappen voor het uitvoeren van deze transformatie:
1. Selecteer de bronkolom die JSON-tekst bevat.
2. Selecteer **Vouw JSON** van de **transformeert** menu. Of klik met de rechtermuisknop op de kop van de bronkolom en selecteer **Vouw JSON** in het contextmenu. 
3. Klik op **OK**. 
 
Nieuwe kolommen worden toegevoegd naast de bronkolom. Deze kolommen bevatten eigenschappen van het volgende niveau van de hiërarchie in de JSON-tekst. Sleutel, wordt indien aanwezig, gebruikt om de naam van de bijbehorende kolom te maken. Eigenschappen van geneste blijven behouden als JSON-tekst die gebruiker iteratief kunt uitvouwen of negeren indien nodig.

## <a name="examples"></a>Voorbeelden

De bron-columnn *klant* in twee kolommen is uitgevouwen *Customer.Name* en *Customer.Phone*.

| Klant                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| {{'Name': 'Carrie Dodson' gemaakt, "Phone": "123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| {{'Name': 'Leonard Robledo' gemaakt, "Phone": "456 7890 123"} | Leonard Robledo | 456-7890-123   |

