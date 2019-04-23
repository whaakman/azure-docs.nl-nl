---
title: SSL-beëindiging met Key Vault-certificaten
description: Meer informatie over hoe u Azure-toepassingsgateway kunt integreren met Key Vault voor certificaten die zijn gekoppeld aan de HTTPS-functionaliteit listeners.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/19/2019
ms.author: victorh
ms.openlocfilehash: 9ab1a42578081d4e6537f221e3f3a8f4c7babde0
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013716"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>SSL-beëindiging met Key Vault-certificaten

[Azure Key Vault](../key-vault/key-vault-whatis.md) is een platform beheerde geheime store kunt u uw geheimen, sleutels en SSL-certificaten beveiligen. Application Gateway ondersteunt de integratie met Key Vault (in openbare preview) voor certificaten die zijn gekoppeld aan de HTTPS-functionaliteit listeners. Deze ondersteuning is beperkt tot de v2-SKU van Application Gateway.

> [!IMPORTANT]
> De Application Gateway Key Vault-integratie is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Er zijn twee modellen voor SSL-beëindiging met deze preview-versie:

- U kunt SSL-certificaten die zijn gekoppeld aan de listener expliciet opgeven. Dit is het traditionele model van het SSL-certificaten wordt doorgegeven aan de toepassingsgateway voor SSL-beëindiging.
- U kunt desgewenst een verwijzing naar een bestaande Key Vault-certificaat of geheim tijdens HTTPS ingeschakeld listener maken.

Er zijn veel voordelen ten opzichte van Key Vault-integratie, met inbegrip van:

- Betere beveiliging omdat SSL-certificaten niet rechtstreeks worden afgehandeld door het ontwikkelingsteam van toepassing. Integratie met Key Vault kunt een afzonderlijke beveiligingsteam kunt inrichten, beheren van de levenscyclus en juiste machtigen Toepassingsgateways voor toegang van certificaten die zijn opgeslagen in Key Vault selecteren.
- Ondersteuning voor het importeren van bestaande certificaten in Key Vault of Key Vault-API's maken en beheren van nieuwe certificaten met een van de vertrouwde partners van de Key Vault gebruiken.
- Ondersteuning voor certificaten die zijn opgeslagen in Key Vault voor het automatisch vernieuwen.

Application Gateway ondersteunt momenteel alleen de certificaten voor de software gevalideerd. Hardware security module (HSM) gevalideerd certificaten worden niet ondersteund. Wanneer Application Gateway is geconfigureerd voor het gebruik van Key Vault-certificaten, worden de instanties ophalen van het certificaat uit Key Vault en lokaal installeren voor SSL-beëindiging. Key Vault controleren de exemplaren ook periodiek op een interval van 24 uur om op te halen van een vernieuwde versie van het certificaat, als deze bestaat. Als een bijgewerkt certificaat wordt gevonden, wordt automatisch het SSL-certificaat is gekoppeld aan de HTTPS-Listener gedraaid.

## <a name="how-it-works"></a>Hoe werkt het?

Integratie met Key Vault vereist het configuratieproces van drie stappen:

1. **Door gebruiker toegewezen beheerde identiteit maken**

   U moet maken of opnieuw gebruiken van een bestaande gebruiker beheerde identiteit die Application Gateway maakt gebruik van certificaten ophalen uit Key Vault namens toegewezen. Zie voor meer informatie, [wat is beheerde identiteiten voor Azure-resources?](../active-directory/managed-identities-azure-resources/overview.md) Deze stap maakt u een nieuwe identiteit in Azure AD-tenant die wordt vertrouwd door het abonnement dat wordt gebruikt voor het maken van de identiteit.
1. **Key Vault configureren**

   U moet vervolgens een van beide importeren of maak een nieuw certificaat in Key Vault gebruikt door toepassingen die worden uitgevoerd via Application Gateway. Een Key Vault-geheim is opgeslagen als zonder wachtwoord base 64 gecodeerde PFX-bestand kan ook worden gebruikt in deze stap. Met behulp van een certificaattype heeft de voorkeur vanwege automatische vernieuwing mogelijkheden beschikbaar voor objecten van het type certificaat in Key Vault. Wanneer een certificaat of het geheim is gemaakt, toegangsbeleid moeten worden gedefinieerd in de Key Vault om toe te staan van de identiteit waarvoor moet worden verleend *ophalen* toegang tot het ophalen van de geheime sleutel.

1. **Application Gateway configureren**

   Als de vorige twee stappen zijn voltooid, kunt u deze kunt inrichten of wijzigen van een bestaande toepassingsgateway voor het gebruik van de gebruiker toegewezen beheerde identiteit. U ook configureren SSL-certificaat voor de HTTP-listener om te verwijzen naar de volledige URI van Key Vault van certificaat of geheime id

![Key Vault-certificaten](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Volgende stappen

[Configureren van SSL-beëindiging met Key Vault-certificaten met behulp van Azure PowerShell](configure-keyvault-ps.md).