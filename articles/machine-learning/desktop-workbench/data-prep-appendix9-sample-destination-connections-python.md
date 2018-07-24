---
title: Voorbeeld van doelen/uitvoer mogelijk is met het voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een aantal voorbeelden van aangepaste gegevens bestemmingen/uitvoer met gegevensvoorbereiding van Azure Machine Learning
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 2173ff15906940b8628aba3615f31e3f7137e3e2
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216508"
---
# <a name="sample-of-destination-connections-python"></a>Voorbeeld van de doelverbindingen (Python) 
Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).


## <a name="write-to-excel"></a>Schrijven naar Excel 


Schrijven naar Excel is een extra library vereist. Toevoegen van nieuwe tapewisselaars wordt beschreven in het overzicht van uitbreidbaarheid. `openpyxl` is de bibliotheek die u nodig hebt om toe te voegen.

Voordat u naar Excel schrijven, kunnen bepaalde andere wijzigingen nodig zijn. Sommige van de gegevenstypen die worden gebruikt ter gegevensvoorbereiding op de worden niet ondersteund in sommige doel-indelingen. Bijvoorbeeld, als "Error" objecten bestaat, wordt niet ze correct geconverteerd naar Excel. Voordat u probeert te schrijven naar Excel, moet u dus een transformatie 'Vervangen door foutwaarden', waarmee fouten uit alle kolommen wordt verwijderd.

Als alle van de vorige werk is voltooid, schrijft de volgende regel de gegevenstabel naar een enkel blad in een Excel-document. Voeg een transformatie gegevensstroom transformeren (Script). Voer de volgende code in een expressie-sectie.


### <a name="on-windows"></a>In Windows 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

### <a name="on-macosos-x"></a>Voor Mac OS en OS X ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
