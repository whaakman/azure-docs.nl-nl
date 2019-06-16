---
title: SSL-beëindiging met Azure Key Vault-certificaten
description: Meer informatie over hoe u Azure Application Gateway kunt integreren met Key Vault voor certificaten die zijn gekoppeld aan de HTTPS-functionaliteit listeners.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: victorh
ms.openlocfilehash: 18af315c58c838a7237acfbcc32f622a0edbd3b3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827632"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>SSL-beëindiging met Key Vault-certificaten

[Azure Key Vault](../key-vault/key-vault-whatis.md) is een platform beheerde geheim opslaan die u kunt gebruiken om geheimen, sleutels en SSL-certificaten te beveiligen. Azure Application Gateway ondersteunt de integratie met Key Vault (in openbare preview) voor certificaten die zijn gekoppeld aan de HTTPS-functionaliteit listeners. Deze ondersteuning is beperkt tot de v2-SKU van Application Gateway.

> [!IMPORTANT]
> Integratie van Application Gateway met Key Vault is momenteel in openbare preview. Deze preview wordt aangeboden zonder een service-level agreement (SLA) en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Deze openbare preview biedt twee modellen voor SSL-beëindiging:

- U kunt SSL-certificaten die zijn gekoppeld aan de listener expliciet opgeven. Dit model is de traditionele manier om door te geven van SSL-certificaten aan de toepassingsgateway voor SSL-beëindiging.
- U kunt desgewenst een verwijzing naar een bestaande Key Vault-certificaat of het geheim opgeven bij het maken van een listener van een HTTPS-functionaliteit.

Integratie van Application Gateway met Key Vault biedt veel voordelen, waaronder:

- Betere beveiliging, omdat het SSL-certificaten worden niet rechtstreeks worden verwerkt door het ontwikkelingsteam van toepassing. Integratie kan een afzonderlijke security team:
  * Toepassingsgateways instellen.
  * Application gateway levenscycli beheren.
  * Machtigingen verlenen aan geselecteerde Toepassingsgateways voor toegang tot de certificaten die zijn opgeslagen in uw key vault.
- Ondersteuning voor het importeren van bestaande certificaten in uw key vault. Of gebruik van Key Vault-API's maken en beheren van nieuwe certificaten met een van de vertrouwde partners van de Key Vault.
- Ondersteuning voor automatische vernieuwing van certificaten die zijn opgeslagen in uw key vault.

Application Gateway ondersteunt momenteel alleen certificaten software gevalideerd. Hardware security module (HSM)-gevalideerde certificaten worden niet ondersteund. Nadat Application Gateway is geconfigureerd voor het gebruik van Key Vault-certificaten, wordt de exemplaren van het certificaat niet ophalen uit Key Vault en lokaal installeren voor SSL-beëindiging. Key Vault pollen de exemplaren ook met 24-uurs intervallen om op te halen van een vernieuwde versie van het certificaat, indien aanwezig. Als een bijgewerkt certificaat wordt gevonden, wordt automatisch het SSL-certificaat is gekoppeld aan de HTTPS-listener gedraaid.

## <a name="how-integration-works"></a>Hoe werkt de integratie

Integratie van Application Gateway met Key Vault is het configuratieproces van drie stappen vereist:

1. **Maken van een gebruiker toegewezen beheerde identiteit**

   U maakt of een bestaande gebruiker toegewezen beheerde identiteit, die Application Gateway maakt gebruik van certificaten ophalen uit Key Vault namens opnieuw. Zie [Wat zijn beheerde identiteiten voor Azure-resources?](../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie. Deze stap maakt u een nieuwe identiteit in de Azure Active Directory-tenant. De identiteit wordt vertrouwd door het abonnement dat wordt gebruikt om de identiteit.

1. **Uw key vault configureren**

   U vervolgens een bestaand certificaat importeren of een nieuwe maken in uw key vault. Het certificaat wordt gebruikt door toepassingen die worden uitgevoerd via de application gateway. In deze stap kunt u ook een key vault-geheim dat wordt opgeslagen als een wachtwoord te hebben, base 64-gecodeerd PFX-bestand gebruiken. Het is raadzaam om met behulp van een certificaattype vanwege de mogelijkheid voor automatisch verlengen dat beschikbaar is voor objecten van het type certificaat in de key vault. Nadat u een certificaat of een geheim hebt gemaakt, definieert u beleid voor toegang in de key vault om toe te staan van de identiteit waarvoor moet worden verleend *ophalen* toegang tot de geheime sleutel.

1. **De toepassingsgateway configureren**

   Nadat u de twee voorgaande stappen hebt voltooid, kunt u instellen of wijzigen van een bestaande toepassingsgateway voor het gebruik van de gebruiker toegewezen beheerde identiteit. U kunt ook configureren met SSL-certificaat voor de HTTP-listener om te verwijzen naar de volledige URI van de Key Vault-certificaat of geheime ID.

   ![Key vault-certificaten](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Volgende stappen

[SSL-beëindiging met Key Vault-certificaten configureren met behulp van Azure PowerShell](configure-keyvault-ps.md)
