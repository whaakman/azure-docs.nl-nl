---
author: vhorne
ms.service: dns
ms.topic: include
ms.date: 11/25/2018
ms.author: victorh
ms.openlocfilehash: 9cc650cea17acb8d89933c819c4ca60e2c459d1c
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52330895"
---
Afzender beleid framework (SPF) records worden gebruikt om op te geven welke e-mailservers e-mail namens een domeinnaam kunnen verzenden. Juiste configuratie van de SPF-records is belangrijk om te voorkomen dat ontvangers uw e-mailadres als ongewenst gemarkeerd.

De DNS-RFC's een nieuwe SPF-recordtype ter ondersteuning van dit scenario oorspronkelijk is geïntroduceerd. Ter ondersteuning van oudere naamservers, mogen ze ook het gebruik van het recordtype TXT om op te geven van de SPF-records. Deze onduidelijkheid heeft geleid tot verwarring, dit is opgelost door [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1). Er wordt aangegeven dat de SPF-records moeten worden gemaakt met behulp van het recordtype TXT. Ook wordt aangegeven dat de SPF-recordtype is afgeschaft.

**SPF-records worden ondersteund door Azure DNS en moeten worden gemaakt met behulp van het recordtype TXT.** Het verouderde SPF-recordtype wordt niet ondersteund. Wanneer u [een DNS-zonebestand importeren](../articles/dns/dns-import-export.md), een SPF-records die gebruikmaken van de SPF-recordtype worden geconverteerd naar het recordtype TXT.
