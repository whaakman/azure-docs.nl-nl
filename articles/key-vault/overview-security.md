---
title: Azure Key Vault-beveiliging | Microsoft Docs
description: Toegangsmachtigingen voor Azure Key Vault sleutels en geheimen beheren. Bevat informatie over het model voor verificatie en autorisatie voor Key Vault en over het beveiligen van uw key vault.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: barclayn
Customer intent: As a key vault administrator, I want to learn the options available to secure my vaults
ms.openlocfilehash: 43847b53fbf84fe42be3efdbd647767904a05fb8
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014277"
---
# <a name="azure-key-vault-security"></a>Azure Key Vault-beveiliging

U moet de versleutelingssleutels en geheimen, zoals certificaten, verbindingsreeksen en wachtwoorden in de cloud beveiligen, zodat u met behulp van Azure Key Vault. Aangezien u gevoelige opslaat en bedrijfskritieke gegevens, moet u stappen ondernemen om te maximaliseren van de beveiliging van uw kluizen en de gegevens die erin zijn opgeslagen. In dit artikel komen enkele van de concepten waarmee u rekening houden moet bij het ontwerpen van de beveiliging van uw Azure Key Vault.

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

Wanneer u een key vault in een Azure-abonnement maakt, het automatisch gekoppeld aan de Azure AD-tenant van het abonnement. Iedereen die probeert om te beheren of inhoud ophalen van een kluis moet worden geverifieerd door Azure AD.

- Verificatie wordt de identiteit van de oproepende functie.
- Autorisatie bepaalt welke bewerkingen de aanroeper kunt uitvoeren. Autorisatie in Key Vault maakt gebruik van een combinatie van [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) (RBAC) en het beleid voor toegang van Azure Key Vault.

### <a name="access-model-overview"></a>Overzicht van Access-model

Toegang tot kluizen vindt plaats via twee interfaces of vlakken. Deze vlakken zijn de beheerlaag en de gegevenslaag.

- De *beheerlaag* is waar u Key Vault zelf beheren en het is de interface gebruikt voor het maken en verwijderen van kluizen. U kunt ook lezen van de eigenschappen van de sleutelkluis en toegangsbeleid beheren.
- De *gegevenslaag* kunt u werken met de gegevens die zijn opgeslagen in een key vault. U kunt toevoegen, verwijderen en wijzigen van sleutels, geheimen en certificaten.

Voor toegang tot een key vault in een van beide vlak, moeten alle aanroepers (gebruikers of toepassingen) worden geverifieerd en toegestaan. Beide lagen gebruiken Azure Active Directory (Azure AD) voor verificatie. Het beheervlak op rollen gebaseerd toegangsbeheer (RBAC) wordt gebruikt voor autorisatie, en de gegevenslaag maakt gebruik van een toegangsbeleid voor Key Vault.

Het model van een enkel verificatiemechanisme voor beide lagen heeft een aantal voordelen:

- Organisaties kunnen de toegang tot alle key vaults in hun organisatie centraal beheren.
- Als een gebruiker verlaat, verliest ze direct toegang tot alle key vaults in de organisatie.
- Organisaties kunnen de verificatie aanpassen met behulp van de opties in Azure AD, zoals multi-factor authentication voor extra beveiliging inschakelen

### <a name="managing-administrative-access-to-key-vault"></a>Beheren van beheerderstoegang tot Key Vault

Wanneer u een key vault in een resourcegroep maakt, kunt u toegang beheren met behulp van Azure AD. U verleent gebruikers of groepen van de mogelijkheid voor het beheren van de sleutelkluizen in een resourcegroep. U kunt toegang op het niveau van een bepaald bereik verlenen de juiste RBAC-rollen toe te wijzen. Om toegang te verlenen aan een gebruiker voor het beheren van sleutelkluizen, u een vooraf gedefinieerde toewijzen `key vault Contributor` rol aan de gebruiker op een bepaald bereik. Het niveau van de volgende bereiken kunnen worden toegewezen aan een RBAC-rol:

- **Abonnement**: Een RBAC-rol op het abonnementsniveau van het is van toepassing op alle resourcegroepen en resources in dat abonnement.
- **Resourcegroep**: Een RBAC-rol die is toegewezen op het niveau van de resource is van toepassing op alle resources in die resourcegroep.
- **Specifieke resource**: Een RBAC-rol die is toegewezen aan een specifieke resource, geldt voor die bron. In dit geval heeft de resource een specifieke key vault.

Er zijn verschillende vooraf gedefinieerde rollen. Als een vooraf gedefinieerde rollen niet aansluiten bij uw behoeften, kunt u uw eigen rollen definiëren. Zie voor meer informatie, [RBAC: Ingebouwde rollen](../role-based-access-control/built-in-roles.md).

> [!IMPORTANT]
> Als een gebruiker heeft `Contributor` machtigingen voor de beheerlaag van een sleutelkluis, de gebruiker kan zich toegang verlenen tot de gegevenslaag door in te stellen van een toegangsbeleid voor Key Vault. U moet zorgvuldig beheren wie heeft `Contributor` rollen gebaseerde toegang tot uw key vaults. Zorg ervoor dat alleen gemachtigde personen kunnen openen en beheren van uw sleutelkluizen, sleutels, geheimen en certificaten.

<a id="data-plane-access-control"></a>
### <a name="controlling-access-to-key-vault-data"></a>Beheren van toegang tot Key Vault-gegevens

Toegangsbeleid voor Key Vault verlenen machtigingen afzonderlijk voor sleutels, geheimen of certificaten. U kunt een gebruikerstoegang alleen voor sleutels en geheimen mogen verlenen. Toegangsmachtigingen voor sleutels, geheimen en certificaten worden beheerd op het kluisniveau van de.

> [!IMPORTANT]
> Toegangsbeleid voor Key Vault bieden geen ondersteuning voor gedetailleerde, op objectniveau machtigingen, zoals een specifieke sleutel, het geheim of het certificaat. Wanneer een gebruiker is gemachtigd om te maken en verwijderen van sleutels, ze kunnen deze bewerkingen uitvoeren op alle sleutels in die key vault.

Als u wilt dat het toegangsbeleid van key vault, gebruikt u de [Azure-portal](https://portal.azure.com/), wordt de [Azure CLI](../cli-install-nodejs.md), [Azure PowerShell](/powershell/azureps-cmdlets-docs), of de [Key Vault Management REST API's](https://msdn.microsoft.com/library/azure/mt620024.aspx).

U kunt toegang tot de gegevenslaag beperken met behulp van [virtual network-service-eindpunten voor Azure Key Vault](key-vault-overview-vnet-service-endpoints.md). U kunt configureren [firewalls en virtuele netwerkregels](key-vault-network-security.md) voor een extra beveiligingslaag.

## <a name="network-access"></a>Netwerktoegang

U kunt de blootstelling van uw kluizen verminderen door op te geven welke IP-adressen toegang toe hebben. De service-eindpunten voor virtueel netwerk voor Azure Key Vault kunnen u toegang tot een opgegeven virtuele netwerk te beperken. De eindpunten kunt u toegang tot een lijst met IPv4 (internet protocolversie 4)-adresbereiken beperken. Elke gebruiker die verbinding maken met uw key vault vanuit buiten deze bronnen is toegang geweigerd.

Na de firewall worden regels in feite zijn gebruikers alleen gegevens uit de Sleutelkluis kunnen lezen wanneer hun aanvragen afkomstig uit de toegestane virtuele netwerken of een IPv4-adresbereik zijn. Dit geldt ook voor toegang tot Key Vault vanuit Azure portal. Hoewel gebruikers naar een key vault vanuit Azure portal bladeren kunnen, ze niet mogelijk een lijst met sleutels, geheimen of certificaten als hun clientmachine zich niet in de lijst met toegestane. Dit ook van invloed op de kiezer voor Key Vault met andere Azure-services. Gebruikers mogelijk overzicht van sleutelkluizen, maar niet sleutels, weergeven als de firewall-regels te voorkomen dat de client-computer.

Voor meer informatie over Azure Key Vault netwerk adres revisie [Virtual network-service-eindpunten voor Azure Key Vault](key-vault-overview-vnet-service-endpoints.md)

## <a name="monitoring"></a>Bewaking

Logboekregistratie van Key Vault worden opgeslagen informatie over de activiteiten die worden uitgevoerd voor uw kluis. Logboeken van Key Vault:

- Alle geverifieerde REST API-aanvragen, met inbegrip van mislukte aanvragen
  - Bewerkingen voor de sleutelkluis zelf. Deze bewerkingen zijn onder meer het maken, verwijderen, instellen van toegangsbeleid en bijwerken van de kenmerken van sleutelkluizen, zoals tags.
  - Bewerkingen voor sleutels en geheimen in de key vault, met inbegrip van:
    - Het maken, wijzigen of verwijderen van deze sleutels of geheimen.
    - Ondertekenen, controleren, versleutelen, ontsleutelen, wrapping en uitpakken sleutels, geheimen, en aanbieding sleutels en geheimen (en hun versies) aan.
- Niet-geverifieerde aanvragen die in een 401-respons resulteren. Voorbeelden zijn aanvragen waarvoor geen bearer-token met ongeldige of verlopen, of waarvoor een ongeldig token.

De logboekinformatie voor toegankelijk binnen 10 minuten nadat de sleutelkluis-bewerking. Het is aan u voor het beheren van uw logboeken in uw storage-account. 

- Gebruik standaardmethoden van Azure voor toegangsbeheer om uw logboeken te beveiligen door het aantal gebruikers te beperken dat toegang heeft tot de logboeken.
- Verwijder de logboeken die u niet meer in uw opslagaccount wilt bewaren.

Raadpleeg voor aanbevelingen over het beheren van beveiligde opslag accounts de [Azure Storage-beveiligingshandleiding](../storage/common/storage-security-guide.md)

## <a name="next-steps"></a>Volgende stappen

- [Virtual network-service-eindpunten voor Azure Key Vault](key-vault-overview-vnet-service-endpoints.md)
- [RBAC: Ingebouwde rollen](../role-based-access-control/built-in-roles.md)
- [Virtual network-service-eindpunten voor Azure Key Vault](key-vault-overview-vnet-service-endpoints.md)
