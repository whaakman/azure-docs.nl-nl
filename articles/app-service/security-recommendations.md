---
title: Aanbevelingen voor beveiliging voor Azure App Service
description: Aanbevelingen voor beveiliging voor de Azure App Service. Implementatie van deze aanbevelingen verbetert help u te voldoen aan uw verplichtingen beveiliging zoals beschreven in onze wordt in gedeelde verantwoordelijkheid model en de algehele beveiliging voor uw web-app-oplossingen.
services: app-service
author: barclayn
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: barclayn
ms.custom: security-recommendations
ms.openlocfilehash: 53cd2b1dde1158a1c46f798e57911dad110dc87e
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718265"
---
# <a name="security-recommendations-for-app-service"></a>Aanbevelingen voor beveiliging voor App Service

In dit artikel bevat aanbevelingen voor beveiliging voor Azure App Service. Implementatie van deze aanbevelingen verbetert help u te voldoen aan uw verplichtingen beveiliging zoals beschreven in onze wordt in gedeelde verantwoordelijkheid model en de algehele beveiliging voor uw Web-App-oplossingen. Lees voor meer informatie over wat Microsoft doet om te voldoen aan de verantwoordelijkheden van de service-provider, [beveiliging van de Azure-infrastructuur](../security/azure-security-infrastructure.md).

## <a name="general"></a>Algemeen

| Aanbeveling | Opmerkingen |
|-|-|----|
| Blijf op de hoogte | Gebruik de nieuwste versies van ondersteunde platforms, programmeertalen, protocollen en frameworks. |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Aanbeveling | Opmerkingen |
|-|----|
| Anonieme toegang uitschakelen | Tenzij u nodig hebt ter ondersteuning van anonieme aanvragen, anonieme toegang uitschakelen. Zie voor meer informatie over Azure App Service-verificatie-opties [verificatie en autorisatie in Azure App Service](overview-authentication-authorization.md).|
| Verificatie vereisen | Gebruik indien mogelijk de module voor App Service-verificatie in plaats van het schrijven van code voor het afhandelen van verificatie en autorisatie. Zie [verificatie en autorisatie in Azure App Service](overview-authentication-authorization.md). |
| Back-end resources beveiligen met geverifieerde toegang | U kunt gebruikmaken van de identiteit van de gebruiker of een toepassings-id gebruiken om te verifiëren met een back-end-bron. Wanneer u een toepassing identiteit u kiest een [beheerde identiteit](overview-managed-identity.md).
| Verificatie van clientcertificaten vereisen | Verificatie van clientcertificaten verbetert de beveiliging door toe te staan alleen verbindingen van clients die kunnen worden geverifieerd met behulp van certificaten die u opgeeft. |

## <a name="data-protection"></a>Gegevensbescherming

| Aanbeveling | Opmerkingen |
|-|-|
| HTTP omleiden naar HTTPs | Clients kunnen standaard verbinding met de web-apps met behulp van zowel HTTP of HTTPS. U wordt aangeraden HTTP omleiden naar HTTPs omdat HTTPS het SSL/TLS-protocol wordt gebruikt voor een beveiligde verbinding, die beide is versleuteld en geverifieerd. |
| Versleutelen van communicatie met Azure-resources | Als uw app verbinding maakt met Azure-resources, zoals [SQL-Database](https://azure.microsoft.com/services/sql-database/) of [Azure Storage](/azure/storage/), de verbinding, blijft het in Azure. Omdat de verbinding via het gedeelde netwerken in Azure verloopt, moet u altijd alle communicatie te versleutelen. |
| Vereisen mogelijk het meest recente TLS-versie | Omdat de nieuwe Azure App Service-apps 2018 gebruik van TLS 1.2. Nieuwere versies van TLS bevatten verbeteringen in de beveiliging ten opzichte van oudere protocolversies. |
| FTPS gebruiken | App Service biedt ondersteuning voor FTP- en FTPS voor het implementeren van uw bestanden. FTPS gebruiken in plaats van FTP indien mogelijk. Wanneer één of beide van deze protocollen niet gebruikt worden, moet u [uit te schakelen](deploy-ftp.md#enforce-ftps). |
| Toepassingsgegevens beveiligen | Toepassingsgeheimen, zoals databasereferenties, API-tokens of persoonlijke sleutels niet worden opgeslagen in uw code of configuratie bestanden. De algemeen geaccepteerde benadering is voor toegang tot deze als [omgevingsvariabelen](https://wikipedia.org/wiki/Environment_variable) met het standaardpatroon in uw taal naar keuze. In Azure App Service, kunt u omgevingsvariabelen via [app-instellingen](web-sites-configure.md) en [verbindingsreeksen](web-sites-configure.md). App-instellingen en verbindingsreeksen worden versleuteld opgeslagen in Azure. De app-instellingen worden ontsleuteld alleen voordat wordt opgenomen in het procesgeheugen van uw app wanneer de app wordt gestart. De versleutelingssleutels worden regelmatig gedraaid. U kunt ook uw Azure App Service-app met integreren [Azure Key Vault](/azure/key-vault/) voor het beheer van geavanceerde geheimen. Door [toegang tot de Key Vault met een beheerde identiteit](../key-vault/tutorial-web-application-keyvault.md), uw App Service-app veilig toegang krijgen tot de geheimen die u nodig hebt. |

## <a name="networking"></a>Netwerken

| Aanbeveling | Opmerkingen |
|-|-|
| Statische IP-beperkingen gebruiken | Azure App Service in Windows kunt u een lijst met IP-adressen die zijn toegestaan voor toegang tot uw app te definiëren. De lijst met toegestane kan afzonderlijke IP-adressen of een bereik van IP-adressen die zijn gedefinieerd door een subnetmasker. Zie voor meer informatie, [Azure App Service statische IP-adresbeperkingen](app-service-ip-restrictions.md).  |
| Gebruik de geïsoleerde prijscategorie | Alle lagen, met uitzondering van de geïsoleerde prijscategorie, Voer uw apps op de gedeelde netwerkinfrastructuur in Azure App Service uit. De geïsoleerde laag biedt u volledige netwerkisolatie door het uitvoeren van de apps in een toegewezen [App Service-omgeving](environment/intro.md). Een App Service-omgeving wordt uitgevoerd in uw eigen exemplaar van [Azure Virtual Network](/azure/virtual-network/).|
| Beveiligde verbindingen gebruiken bij toegang tot on-premises resources | U kunt [hybride verbindingen](app-service-hybrid-connections.md), [integratie van virtuele netwerken](web-sites-integrate-with-vnet.md), of [App Service-omgeving](environment/intro.md) verbinding maken met on-premises bronnen. |
| Kwetsbaarheid voor aanvallen te inkomend netwerkverkeer | Netwerkbeveiligingsgroepen kunnen u het aantal beschikbaar gestelde eindpunten beheren en beperken van toegang tot het netwerk. Zie voor meer informatie, [hoe voor inkomend verkeer beheren op een App Service Environment](environment/app-service-app-service-environment-control-inbound-traffic.md). |

## <a name="monitoring"></a>Bewaking

| Aanbeveling | Opmerkingen |
|-|-|
|Gebruik Azure Security Center standard-laag | [Azure Security Center](../security-center/security-center-app-services.md) is geïntegreerd met Azure App Service. Het kan evaluaties worden uitgevoerd en doet aanbevelingen voor beveiliging. |

## <a name="next-steps"></a>Volgende stappen

Neem contact op met de provider van uw toepassing om te zien of er aanvullende beveiligingsvereisten zijn. Zie voor meer informatie over het ontwikkelen van veilige toepassingen [Secure Development-documentatie](../security/abstract-develop-secure-apps.md).
