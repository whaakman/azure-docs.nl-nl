---
title: Azure MFA-versies en verbruik plannen | Microsoft Docs
description: Informatie over de multi-factor Authentication-client en de verschillende methoden en versies die beschikbaar zijn. Meer informatie over elk plan verbruik
keywords: 
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: joflore
ms.openlocfilehash: af86434e1205d67829fc7079d97a37f013c0f2d8
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="how-to-get-azure-multi-factor-authentication"></a>Het ophalen van Azure multi-factor Authentication

Bij het beveiligen van uw accounts, moet verificatie in twee stappen standaard in uw organisatie. Deze functie is vooral belangrijk voor beheerdersaccounts met toegang tot bronnen uitgebreide. Om deze reden biedt Microsoft basic in twee stappen verificatie functies op Office 365 en Azure beheerders voor geen extra kosten verbonden. Als u wilt upgraden van de functies voor uw beheerders of uitbreiden verificatie in twee stappen voor de rest van uw gebruikers, kunt u Azure multi-factor Authentication kunt aanschaffen. 

Dit artikel wordt uitgelegd dat het verschil tussen de versies die worden aangeboden aan administrators en de volledige versie van Azure MFA. Als u klaar bent voor het implementeren van de volledige Azure MFA biedt, wordt de volgende sectie behandelt implementatie-opties en hoe Microsoft verbruik berekent.


>[!IMPORTANT]
>In dit artikel is bedoeld als een handleiding voor inzicht in de verschillende manieren voor het kopen van Azure multi-factor Authentication. Voor specifieke informatie over prijzen en facturering u altijd moet verwijzen naar de [multi-factor Authentication-pagina met prijzen](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Beschikbare versies van Azure multi-factor Authentication

De volgende tabel beschrijft de verschillen tussen de drie versies van multi-factor authentication-server:

| Versie | Beschrijving |
| --- | --- |
| Multi-factor Authentication voor Office 365 |Deze versie werkt alleen met Office 365-toepassingen en wordt beheerd via de Office 365-portal. Beheerders kunnen [Office 365-resources met verificatie in twee stappen beveiligen](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Deze versie is onderdeel van een Office 365-abonnement. |
| Multi-factor Authentication voor Azure AD-beheerders | De rol globale beheerder in Azure AD-tenants toegewezen gebruikers kunnen inschakelen verificatie in twee stappen voor de accounts van de globale beheerder Azure AD zonder extra kosten.|
| Azure Multi-Factor Authentication | Azure multi-factor Authentication is vaak de 'volledige' versie genoemd, biedt de allerbeste set mogelijkheden. Biedt extra configuratieopties via de [Azure-portal](https://portal.azure.com)geavanceerde rapportage en ondersteuning voor een bereik van lokale en cloudtoepassingen. Azure multi-factor Authentication is opgenomen in [Azure Active Directory Premium-abonnementen](https://www.microsoft.com/cloud-platform/azure-active-directory-features) en [Enterprise Mobility + Beveiligingsplannen](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-pricing), en kan worden geïmplementeerd in de cloud of on-premises. |

## <a name="feature-comparison-of-versions"></a>Vergelijking van functies van versies
De volgende tabel bevat een lijst van de functies die beschikbaar in de verschillende versies van Azure multi-factor Authentication zijn.

> [!NOTE]
> Deze Vergelijkingstabel worden de functies die deel van elke versie van multi-factor Authentication uitmaken besproken. Als u de volledige Azure multi-factor Authentication-service hebt, sommige functies mogelijk niet beschikbaar, afhankelijk van of u gebruiken [MFA in de cloud of MFA lokale](multi-factor-authentication-get-started.md).


| Functie | Multi-factor Authentication voor Office 365 | Multi-factor Authentication voor Azure AD-beheerders | Azure Multi-Factor Authentication |
| --- |:---:|:---:|:---:|
| Beveiligen van Azure AD-beheerdersaccounts met MFA |● |● (alleen globale beheerder van Azure AD-accounts) |● |
| Mobiele app als een tweede factor |● |● |● |
| Telefoonoproep als tweede factor |● |● |● |
| SMS als tweede factor |● |● |● |
| App-wachtwoorden voor clients die geen ondersteuning voor MFA bieden |● |● |● |
| Beheerdercontrole over verificatiemethoden |● |● |● |
| Beveiligen van niet-beheerdersaccounts met MFA |● (alleen voor Office 365-toepassingen) | |● |
| Pincodemodus | | |● |
| Fraudewaarschuwing | | |● |
| MFA-rapporten | | |● |
| Eenmalige toegang | | |● |
| Aangepaste begroeting voor telefoongesprekken | | |● |
| Aangepaste Nummerweergave voor telefoongesprekken | | |● |
| Goedgekeurde IP-adressen | | |● |
| MFA herinneren voor vertrouwde apparaten |● |● |● |
| MFA SDK | | |● (afgeschaft) | 
| MFA voor on-premises toepassingen | | |● |

## <a name="how-to-get-azure-multi-factor-authentication"></a>Het ophalen van Azure multi-factor Authentication
Als u de volledige functionaliteit van Azure multi-factor Authentication dat wilt, zijn er verschillende opties:

### <a name="option-1---mfa-licenses"></a>Optie 1: MFA licenties

Azure multi-factor Authentication-licenties koopt en deze toewijzen aan uw gebruikers in Azure Active Directory. 

Als u deze optie gebruikt, alleen maken een Azure multi-factor Authentication-Provider als u moet de verificatie in twee stappen voor gebruikers die geen licenties opgeven. Anders wordt u mogelijk worden gefactureerd twee keer.

### <a name="option-2---bundled-licenses-that-include-mfa"></a>Optie 2 - gebundeld licenties met MFA

Licenties die deze toewijzen aan uw gebruikers in Azure Active Directory en Azure multi-factor Authentication, zoals Azure Active Directory Premium of Enterprise Mobility + Security, bevatten. 

Als u deze optie gebruikt, moet u een Azure multi-factor Authentication-Provider maken alleen als u moet ook de verificatie in twee stappen voor gebruikers die geen licenties. Anders wordt u mogelijk worden gefactureerd twee keer. 

### <a name="option-3---mfa-consumption-based-model"></a>Optie 3: MFA-model op basis van verbruik

Een Azure multi-factor Authentication-Provider binnen een Azure-abonnement maken. Azure MFA-Providers zijn Azure-resources worden gefactureerd op basis van uw Enterprise Agreement, Azure maandbedrag of creditcard net als alle andere Azure-resources. Deze providers kunnen alleen worden gemaakt in de volledige Azure-abonnementen niet beperkt Azure-abonnementen waarvoor een $-0 uitgavenlimiet. Beperkte abonnementen worden gemaakt wanneer u licenties, zoals in opties 1 en 2 activeert. 

Wanneer u een Azure multi-factor Authentication-Provider gebruikt, zijn er twee modellen beschikbaar die via uw Azure-abonnement worden gefactureerd:  

1. **Per ingeschakelde gebruiker** - voor ondernemingen die in staat wilt stellen verificatie in twee stappen voor een vast aantal werknemers die regelmatig dienen te worden geverifieerd. Facturering per gebruiker is gebaseerd op het aantal gebruikers dat is ingeschakeld voor MFA in uw Azure AD-tenant en uw Azure MFA-Server. Als gebruikers zijn ingeschakeld voor MFA in beide Azure AD en Azure MFA-Server en domein sync (Azure AD Connect) is ingeschakeld, moet we de grotere set van gebruikers tellen. Als synchronisatie van domein is niet ingeschakeld, wordt er de som van alle gebruikers die zijn ingeschakeld voor MFA in Azure AD tellen en Azure MFA-Server. Facturering is naar rato en gemeld aan het systeem Commerce dagelijks. 

  > [!NOTE]
  > Facturering voorbeeld 1: U hebt 5000 gebruikers die zijn ingeschakeld voor MFA vandaag. Het MFA-systeem wordt dit nummer gedeeld door 31 en 161.29 gebruikers rapporten voor die dag. In de toekomst u 15 meer gebruikers, schakelt zodat het systeem MFA 161.77 gebruikers voor die dag rapporten. Aan het einde van de factureringscyclus voegt het totale aantal gebruikers kosten in rekening gebracht op basis van uw Azure-abonnement maximaal rond 5.000. 
  >
  > Facturering voorbeeld 2: U hebt een mengeling van gebruikers met licenties en gebruikers zonder, zodat u een per gebruiker Azure MFA-Provider om het verschil hebt. Er zijn 4.500 Enterprise Mobility + Security-licenties op uw tenant, maar 5000 gebruikers die zijn ingeschakeld voor MFA. Uw Azure-abonnement wordt gefactureerd voor 500 gebruikers, naar rato en dagelijks gerapporteerd als 16.13 gebruikers. 

2. **Per verificatie** - voor ondernemingen die verificatie in twee stappen voor een grote groep met gebruikers hoeven minder vaak verificatie inschakelen. Facturering is gebaseerd op het aantal aanvragen verificatie in twee stappen, ongeacht of deze verificaties slagen of geweigerd. Deze facturering wordt weergegeven op uw van Azure-gebruiksinstructie in de packs van 10 verificaties en dagelijks wordt gerapporteerd. 

  > [!NOTE]
  > Facturering voorbeeld 3: vandaag de dag 3,105 aanvragen voor de verificatie in twee stappen in de Azure MFA-service heeft ontvangen. Uw Azure-abonnement wordt gefactureerd voor 310.5 verificatie packs. 

Het is belangrijk te weten dat u kunt Azure MFA-licenties hebt, maar nog steeds in rekening voor de configuratie op basis van verbruik gebracht ophalen. Als u een per authenticatie Azure MFA-Provider hebt ingesteld, wordt u gefactureerd voor elke aanvraag van de verificatie in twee stappen ook als deze door gebruikers die licenties hebt gedaan. Als u een Azure MFA-Provider per gebruiker ingesteld op een domein dat niet is gekoppeld aan uw Azure AD-tenant, wordt u gefactureerd per ingeschakelde gebruiker zelfs als de gebruikers met licenties in Azure AD. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer prijsinformatie [prijzen van Azure MFA](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Kies of u voor het implementeren van Azure MFA [in de cloud of on-premises](multi-factor-authentication-get-started.md)
