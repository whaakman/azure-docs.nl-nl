---
title: Vouw de JSON-transformatie die met behulp van Azure Machine Learning Workbench
description: Het referentiedocument voor de transformatie 'JSON Vouw'
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 57f461f9a39e8069654985d86bfe3ae23dd0855a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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

| De klant                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| {{'Name': 'Carrie Dodson' gemaakt, "Phone": "123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| {{'Name': 'Leonard Robledo' gemaakt, "Phone": "456 7890 123"} | Leonard Robledo | 456-7890-123   |

