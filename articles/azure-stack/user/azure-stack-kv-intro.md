---
title: Kennismaking met Azure Stack Key Vault | Microsoft Docs
description: Informatie over hoe Azure Stack Key Vault sleutels en geheimen beheren
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 70f1684a-3fbb-4cd1-bf29-9f9882e98fe9
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/05/2019
ms.author: sethm
ms.openlocfilehash: 40a748895038dbadea7d12d79268f89796a4b428
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54061498"
---
# <a name="introduction-to-key-vault-in-azure-stack"></a>Inleiding tot Key Vault in Azure Stack

## <a name="prerequisites"></a>Vereisten

* U moet zich abonneren op een aanbieding met inbegrip van de Azure Key Vault-service.  
* [PowerShell is geconfigureerd voor gebruik met Azure Stack](azure-stack-powershell-configure-user.md).

## <a name="key-vault-basics"></a>Grondbeginselen van Key Vault

Key Vault in Azure Stack helpt bescherming van de cryptografische sleutels en geheimen die toepassingen en services in de cloud gebruiken. Met Key Vault, u kunt sleutels en geheimen versleutelen, zoals:

* Verificatiesleutels
* Opslagaccountsleutels
* Sleutels voor gegevenscodering
* PFX-bestanden
* Wachtwoorden

Key Vault stroomlijnt het beheerproces voor sleutels en zorgt dat u de controle houdt over de sleutels waarmee uw gegevens toegankelijk zijn en worden versleuteld. Ontwikkelaars kunnen binnen enkele minuten sleutels voor ontwikkel- en testdoeleinden maken en deze vervolgens probleemloos migreren naar productiesleutels. Administrators voor rapportbeveiliging kunnen verlenen (en intrekken) machtigingen voor sleutels, indien nodig.

Iedereen met een Azure Stack-abonnement kunt maken en gebruiken van sleutelkluizen. Hoewel Key Vault voordelen biedt voor ontwikkelaars en beveiligingsadministrators, kan de operator die wordt beheerd van andere Azure Stack-services voor een organisatie kunt implementeren en beheren. Bijvoorbeeld, maken de Azure Stack operator kan zich aanmelden met een Azure Stack-abonnement, een kluis voor de organisatie waarin sleutels opslaan en vervolgens worden die verantwoordelijk is voor deze operationele taken:

* Maak of importeer een sleutel of geheim.
* Intrekken of verwijderen van een sleutel of geheim.
* Toestaan dat gebruikers of toepassingen toegang tot de key vault, zodat ze kunnen beheren of. de sleutels en geheimen gebruiken.
* Sleutelgebruik configureren (bijvoorbeeld ondertekenen of versleutelen).

De operator kan ontwikkelaars vervolgens voorzien Uniform Resource-id's (URI) om aan te roepen via hun toepassingen. Operators kunnen ook administrators voor rapportbeveiliging met logboekinformatie sleutelgebruik opgeven.

Ontwikkelaars kunnen de sleutels ook rechtstreeks beheren door gebruik te maken van API's. Zie de Key Vault developer's guide voor meer informatie.

## <a name="scenarios"></a>Scenario's

De volgende scenario's wordt beschreven hoe u Key Vault kunt voldoen aan de behoeften van ontwikkelaars en beveiligingsadministrators.

### <a name="developer-for-an-azure-stack-application"></a>Ontwikkelaar voor een Azure Stack-toepassing

**Probleem:** Ik wil een toepassing schrijven voor Azure Stack die gebruikmaakt van sleutels voor ondertekening en versleuteling. Ik wil deze externe sleutels zijn van mijn toepassing, zodat de oplossing geschikt is voor een toepassing die geografisch wordt gedistribueerd.

**Overzicht:** Sleutels worden opgeslagen in een kluis en aangeroepen door een URI wanneer dat nodig is.

### <a name="developer-for-software-as-a-service-saas"></a>SaaS-ontwikkelaar (Software as a Service)

**Probleem:** Ik wil niet de verantwoordelijk of potentiële aansprakelijkheid voor de sleutels en geheimen van mijn klant. Ik wil dat klanten hun sleutels beheren en zodat ik mij concentreren kan op hetgeen waar ik het beste in ben, namelijk het leveren van de belangrijkste softwarefuncties.

**Overzicht:** Klanten kunnen hun eigen sleutels importeren in Azure Stack en deze vervolgens te beheren.

### <a name="chief-security-officer-cso"></a>Chief Security Officer (CSO)

**Probleem:** Ik wil ervoor zorgen dat mijn organisatie de controle heeft over de levenscyclus van de sleutel en het sleutelgebruik kan bewaken.

**Overzicht:** Key Vault is zo ontworpen dat Microsoft uw sleutels niet kan zien of ophalen. Wanneer een toepassing cryptografische bewerkingen uitvoeren moet met behulp van de sleutels van de klant, Key Vault maakt gebruik van de sleutels namens de toepassing. De toepassing de sleutels van de klant niet te zien. Hoewel we meerdere Azure Stack-services en resources gebruiken, kunt u de sleutels kunt beheren vanaf één locatie in Azure Stack. De kluis biedt één interface, ongeacht het aantal kluizen dat u hebt in Azure Stack, welke regio's ze ondersteuning en welke toepassingen ze worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

* [Key Vault in Azure Stack door de portal beheren](azure-stack-kv-manage-portal.md)  
* [Key Vault in Azure Stack beheren met behulp van PowerShell](azure-stack-kv-manage-powershell.md)

