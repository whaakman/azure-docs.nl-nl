---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: e2c8b4e74933df593dbc023dcb886dff80554a1a
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977344"
---
Instellingen voor de container zijn hiërarchisch alle containers op de hostcomputer een gedeelde hiërarchie gebruiken.

U kunt een van de volgende instellingen opgeven:

* [Omgevingsvariabelen](#environment-variable-settings)
* [Opdrachtregelargumenten](#command-line-argument-settings)

Waarden van omgevingsvariabelen overschrijven opdrachtregelargument waarden, die op zijn beurt de standaardwaarden voor de containerinstallatiekopie te overschrijven. Als u verschillende waarden in een omgevingsvariabele en een opdrachtregelargument voor de dezelfde configuratie-instelling opgeeft, wordt de waarde in de omgevingsvariabele wordt gebruikt door de geïnstantieerde container.

|Prioriteit|Locatie instellen|
|--|--|
|1|Omgevingsvariabele| 
|2|Opdrachtregel|
|3|Standaardwaarde van container-installatiekopie|

### <a name="environment-variable-settings"></a>Instellingen voor de omgevingsvariabelen

De voordelen van het gebruik van omgevingsvariabelen zijn:

* Meerdere instellingen kunnen worden geconfigureerd.
* Meerdere containers kunnen dezelfde instellingen gebruiken.

### <a name="command-line-argument-settings"></a>Opdrachtregelargument-instellingen

Het voordeel van het gebruik van opdrachtregelargumenten is dat elke container verschillende instellingen kunt gebruiken.
