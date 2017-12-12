---
title: Azure Sleutelkluis Stack Inleiding | Microsoft Docs
description: Meer informatie over hoe Azure-Stack Sleutelkluis sleutels en geheimen beheert
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 70f1684a-3fbb-4cd1-bf29-9f9882e98fe9
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/04/2017
ms.author: mabrigg
ms.openlocfilehash: a50a03e70ccf014a8a9d33e0f177febed560853f
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="introduction-to-key-vault-in-azure-stack"></a>Inleiding tot de Sleutelkluis in Azure-Stack

## <a name="prerequisites"></a>Vereisten 

* U moet zich abonneren op een aanbieding met de Azure Sleutelkluis-service.  
* [PowerShell is geconfigureerd voor gebruik met Azure-Stack](azure-stack-powershell-configure-user.md).
 
## <a name="key-vault-basics"></a>Sleutelkluis-basisbeginselen
Sleutelkluis in Azure-Stack kunt beschermen cryptografische sleutels en geheimen die cloud toepassingen en services gebruiken. Met behulp van de Sleutelkluis u kan sleutels en geheimen versleutelen, zoals:
   * Verificatiesleutels 
   * Toegangscodes voor opslag
   * De gegevensversleutelingssleutels
   * PFX-bestanden
   * Wachtwoorden

Key Vault stroomlijnt het beheerproces voor sleutels en zorgt dat u de controle houdt over de sleutels waarmee uw gegevens toegankelijk zijn en worden versleuteld. Ontwikkelaars kunnen binnen enkele minuten sleutels voor ontwikkel- en testdoeleinden maken en deze vervolgens probleemloos migreren naar productiesleutels. Beveiligingsadministrator kunnen wanneer dit nodig is machtigingen aan sleutels verlenen (en intrekken).

Iedereen met een Azure-Stack-abonnement kunt maken en gebruiken van sleutelkluizen. Hoewel Sleutelkluis voordelen biedt voor ontwikkelaars en beveiligingsadministrators, kan de operator die andere Azure-Stack-services voor een organisatie beheert kunt implementeren en beheren. Bijvoorbeeld, maken de Azure-Stack operator kunt aanmelden met een Azure-Stack-abonnement een kluis voor de organisatie waarin sleutels opslaan en vervolgens worden die verantwoordelijk is voor deze operationele taken:

* Maken of importeren van een sleutel of geheim.
* Intrekken of verwijderen van een sleutel of geheim.
* Gebruikers of toepassingen voor toegang tot de sleutelkluis, zodat ze kunnen beheren of gebruiken van de sleutels en geheimen autoriseren.
* Sleutelgebruik configureren (bijvoorbeeld ondertekenen of versleutelen).

De operator kan ontwikkelaars vervolgens met de Uniform Resource-id's (URI) aan te roepen vanuit hun toepassingen bieden. Operators kunnen ook Beveiligingsbeheerders met sleutelgebruik logboekregistratie informatie bieden.

Ontwikkelaars kunnen de sleutels ook rechtstreeks beheren door gebruik te maken van API's. Zie de Sleutelkluis developer's guide voor meer informatie.

## <a name="scenarios"></a>Scenario's
De volgende scenario's wordt beschreven hoe Sleutelkluis u kunt de behoeften van ontwikkelaars en beveiligingsadministrators.

### <a name="developer-for-an-azure-stack-application"></a>Ontwikkelaar voor een Azure-Stack-toepassing
**Probleem:** ik wil een toepassing schrijven voor Azure-Stack die gebruikmaakt van sleutels voor ondertekening en versleuteling. Ik wil deze sleutels worden externe van mijn toepassing, zodat de oplossing geschikt voor een toepassing die geografisch wordt gedistribueerd.

**Overzicht:** sleutels worden opgeslagen in een kluis en aangeroepen door een URI, wanneer deze nodig is.

### <a name="developer-for-software-as-a-service-saas"></a>Ontwikkelaar voor software als een service (SaaS)
**Probleem:** ik wil niet de verantwoordelijk of potentiële aansprakelijkheid voor de sleutels en geheimen van mijn klant. Ik wil dat klanten eigenaar en beheren van hun sleutels, zodat ik mij concentreren kan op hetgeen waar ik beste, namelijk het leveren van de kern van het software-onderdelen.

**Overzicht:** klanten hun eigen sleutels importeren in Azure-Stack en deze vervolgens te beheren. 

### <a name="chief-security-officer-cso"></a>Chief Security Officer (CSO)
**Probleem:** ik wil ervoor zorgen dat mijn organisatie controle over de levenscyclus van de en sleutelgebruik kan controleren.

**Overzicht:** Sleutelkluis is zodanig ontworpen dat Microsoft niet kan zien of extraheren van uw sleutels. Wanneer een toepassing moet om uit te voeren cryptografische bewerkingen met behulp van klanten sleutels, Sleutelkluis maakt gebruik van de sleutels voor de toepassing is. De toepassing krijgt de sleutels van de klant niet te zien. Hoewel we meerdere Azure-Stack-services en bronnen gebruiken, kunt u de sleutels beheren vanaf één locatie in Azure-Stack. De kluis biedt één interface, ongeacht het aantal kluizen hebt u in Azure Stack, welke regio's ze ondersteuning en welke toepassingen ze worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

* [Sleutelkluis in Azure-Stack beheren via de portal](azure-stack-kv-manage-portal.md)  
* [Sleutelkluis in Azure-Stack beheren met PowerShell](azure-stack-kv-manage-powershell.md)

