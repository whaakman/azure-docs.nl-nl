---
title: PaaS-implementaties beveiligen | Microsoft Docs
description: " Begrijp de beveiligingsmogelijkheden PaaS ten opzichte van andere cloud servicemodellen en informatie over aanbevolen procedures voor het beveiligen van uw Azure-PaaS-implementatie. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: techlake
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 4629e0ab6bbc9554128a923e92b269df79446b18
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="securing-paas-deployments"></a>PaaS-implementaties beveiligen

Dit artikel bevat informatie die u helpt:

- De beveiligingsmogelijkheden van toepassingen in de cloud hosten begrijpen
- Evalueren van de beveiligingsmogelijkheden van platform als een service (PaaS) ten opzichte van andere servicemodellen cloud
- Wijzigen van de focus van de beveiliging van een centraal netwerk in een benadering identiteit gericht perimeter-beveiliging
- Algemene PaaS beveiligingsaanbevelingen implementeren

## <a name="cloud-security-advantages"></a>Beveiligingsvoordelen van de cloud
Er zijn beveiligingsvoordelen wordt in de cloud. In een on-premises omgeving organisaties waarschijnlijk hebben voldaan verantwoordelijkheden en beperkte bronnen beschikbaar om te investeren in de beveiliging, die wordt gemaakt van een omgeving waarin aanvallers kunnen gebruikmaken van beveiligingsproblemen voor alle lagen.

![Beveiligingsvoordelen van de vormgeving van de cloud][1]

Organisaties kunnen hun detectie van dreigingen en reactietijden verbeteren met behulp van de provider cloud-gebaseerde beveiligingsmogelijkheden en cloud intelligence.  Doordat de verantwoordelijkheden van de cloudprovider, kunnen organisaties meer beveiliging dekking, waardoor ze opnieuw toewijzen van resources van de beveiliging en budget dat u wilt de prioriteiten van andere bedrijven krijgen.

## <a name="division-of-responsibility"></a>Deling van verantwoordelijkheid
Het is belangrijk om te begrijpen van de deling van verantwoordelijkheid tussen u en Microsoft. On-premises eigenaar van de hele stack, maar als u naar de cloud verplaatsen bepaalde verantwoordelijkheden overbrengen naar Microsoft. De volgende verantwoordelijkheid matrix bevat de gedeelten van de stack in een SaaS PaaS en IaaS-implementatie die u verantwoordelijk bent voor en Microsoft is verantwoordelijk voor.

![Verantwoordelijkheid zones][2]

Voor alle cloud-implementatietypen die eigenaar u van uw gegevens en identiteiten. U bent zelf verantwoordelijk voor het beveiligen van de beveiliging van uw gegevens en identiteiten, lokale bronnen en de cloud-onderdelen u controle (dit verschilt per servicetype).

Taken die altijd door u, ongeacht het type implementatie behouden, zijn:

- Gegevens
- Eindpunten
- Account
- Toegangsbeheer

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Beveiligingsvoordelen van een PaaS-cloud-servicemodel
Met behulp van dezelfde verantwoordelijkheid matrix, bekijken we de beveiligingsmogelijkheden van een Azure-PaaS-implementatie tegenover on-premises.

![Beveiligingsvoordelen van PaaS][3]

Aan de onderkant van de stack, de fysieke infrastructuur vanaf vermindert Microsoft algemene risico's en verantwoordelijkheden. Omdat de Microsoft cloud voortdurend wordt bewaakt door Microsoft, is het moeilijk zijn voor aanvallen. Niet dat u voor een aanvaller te oefenen van de Microsoft cloud als doel. Tenzij de aanvaller veel geld en bronnen, de aanvaller heeft is het waarschijnlijk naar een ander doel op.  

Er is geen verschil tussen een PaaS-implementatie en on-premises in het midden van de stack. Aan de toepassingslaag en de account en toegang management-laag hebt u vergelijkbare risico's. In de volgende sectie voor stappen van dit artikel wordt helpen we u aan aanbevolen procedures voor het verwijderen van of deze risico's te minimaliseren.

Aan de bovenkant van de stack, gegevensbeheer en rights management, u rekening houden met een risico die kunnen worden verholpen door Sleutelbeheer. (Key management wordt beschreven in de aanbevolen procedures.) Sleutelbeheer is een extra verantwoordelijkheid, hebt u gebieden in een PaaS-implementatie die u niet langer hoeft te beheren zodat u resources naar Sleutelbeheer verplaatsen kunt.

De Azure-platform biedt u ook sterk DDoS-bescherming met behulp van verschillende technologieën op het netwerk. Alle soorten DDoS-bescherming-methoden op basis van een netwerk hebben echter hun beperkingen op basis van per koppeling en per datacenter. De invloed van grote DDoS-aanvallen worden voorkomen, kunt u profiteren van de Azure core cloud-capaciteit van zodat u snel en automatisch uit te schalen om te beschermen tegen DDoS-aanvallen. Gaan we meer informatie over hoe u dit in de artikelen aanbevolen procedures doen kunt.

## <a name="modernizing-the-defenders-mindset"></a>De defender denkt modernisering
Met PaaS komen implementaties van een verschuiving van uw algehele benadering voor de beveiliging. U verplaatst van het besturingselement alles zelf verantwoordelijkheid delen met Microsoft hoeven.

Een ander belangrijk verschil tussen PaaS en traditionele on-premises implementaties, is een nieuwe weergave van wat het perimeternetwerk van de primaire beveiliging definieert. In het verleden hebben de primaire lokale beveiliging perimeter is uw netwerk en de meeste lokale beveiliging ontwerpen gebruiken het netwerk als de primaire beveiliging pivot. Voor PaaS-implementaties zijn u beter afgehandeld door de identiteit van de primaire beveiliging perimeter overwegen.

## <a name="identity-as-the-primary-security-perimeter"></a>Identiteit als de primaire beveiliging perimeter
Een van de vijf belangrijke kenmerken van cloudcomputing ruime netwerktoegang, waardoor de netwerk-georiënteerde gegevensclassificatie minder relevant is. Het doel van veel van cloudcomputing om toe te staan gebruikers toegang krijgen tot netwerkbronnen ongeacht de locatie. Voor de meeste gebruikers zal hun locatie worden ergens op het Internet.

De volgende afbeelding ziet hoe het perimeternetwerk van de beveiliging van een netwerkverbinding heeft ontwikkeld op een perimeternetwerk identiteit. Beveiliging wordt minder over uw netwerk te beschermen en meer informatie over uw gegevens beschermen, evenals de beveiliging van uw apps en gebruikers beheren. Het belangrijkste verschil is dat u beveiliging dichter push wilt tot wat belangrijk is voor uw bedrijf.

![Identiteit als nieuwe beveiliging perimeter][4]

In eerste instantie opgegeven Azure PaaS-services (bijvoorbeeld webrollen en Azure SQL) weinig of geen traditioneel netwerk perimeter-beveiliging. Het is begrepen dat van het element doel was worden blootgesteld aan Internet (Webrol) en dat de verificatie de nieuwe perimeter (bijvoorbeeld BLOB of Azure SQL biedt).

Moderne beveiligingsprocedures wordt ervan uitgegaan dat de adversary de netwerkverbinding is geschonden. Daarom hebt moderne verdediging procedures verplaatst naar de identiteit. Organisaties moeten een perimeternetwerk beveiliging op basis van identiteit met sterke verificatie en autorisatie hygiëne (aanbevolen procedures) maken.

## <a name="recommendations-for-managing-the-identity-perimeter"></a>Aanbevelingen voor het beheren van het perimeternetwerk identiteit

De principes en patronen voor de netwerkverbinding zijn al beschikbaar jarenlang. Daarentegen is de branche relatief minder ervaring met het gebruik van de identiteit als de primaire beveiliging perimeter. Met die gezegd, hebben we voldoende ervaring om enkele algemene aanbevelingen die in het veld zijn beproefde en van toepassing op bijna alle PaaS-services verzameld.

De volgende ziet u een algemene best practices benadering voor het beheren van het perimeternetwerk van uw identiteit.

- **Uw sleutels of referenties niet verliezen** sleutels en referenties beveiligen is essentieel voor het beveiligen van PaaS-implementaties. Sleutels en referenties verliezen is een veelvoorkomend probleem. Een goede oplossing is het gebruik van gecentraliseerd waarin sleutels en geheimen in hardware security modules (HSM) kunnen worden opgeslagen. Azure biedt u een HSM in de cloud met [Azure Key Vault](../key-vault/key-vault-whatis.md).
- **Plaats geen referenties en andere geheime informatie in de broncode of GitHub** alleen slechter dan onbevoegden verliezen uw sleutels en referenties heeft er toegang toe hebben. Aanvallers kunnen profiteren van bot technologieën voor het vinden van sleutels en geheimen die zijn opgeslagen in de code-opslagplaatsen zoals GitHub. Plaats niet sleutel en geheimen in deze openbare broncodeopslagplaatsen.
- **Beveiligen van uw VM-beheerinterfaces van hybride PaaS- en IaaS** IaaS en PaaS-services worden uitgevoerd op virtuele machines (VM's). Afhankelijk van het type van service verschillende beheerinterfaces beschikbaar zijn die u extern beheren van deze virtuele machines rechtstreeks inschakelen. Extern beheer protocollen zoals [Secure Shell Protocol (SSH)](https://en.wikipedia.org/wiki/Secure_Shell), [Remote Desktop Protocol (RDP)](https://support.microsoft.com/kb/186607), en [externe PowerShell](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) kan worden gebruikt. In het algemeen is het raadzaam dat u niet de mogelijkheid direct externe toegang naar virtuele machines van het Internet. Indien beschikbaar, moet u alternatieve methoden zoals het gebruik van virtueel particulier netwerk in een Azure-netwerk. Als alternatieve methoden zijn niet beschikbaar, zorg ervoor dat u complexe wachtwoordzinnen en indien beschikbaar, tweeledige verificatie (zoals [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)).
- **Sterke verificatie en autorisatie platforms gebruiken**

  - Federatieve identiteiten in Azure AD in plaats van een aangepaste gebruiker winkels gebruiken. Wanneer u een federatieve identiteiten gebruikt, u profiteren van een benadering op basis van het platform en u het beheer van bevoegde identiteiten delegeren aan uw partners. Een benadering van federatieve identiteiten is vooral belangrijk in scenario's wanneer werknemers worden beëindigd en die informatie moet worden weergegeven met meerdere systemen voor identiteits- en autorisatie.
  - Gebruik platform opgegeven mechanismen voor verificatie en autorisatie in plaats van aangepaste code. De reden is dat het ontwikkelen van aangepaste verificatiecode foutgevoelig kan worden. De meeste van uw ontwikkelaars zijn niet beveiliging deskundigen en waarschijnlijk niet op de hoogte van de eigenaardigheden en de meest recente ontwikkelingen in de verificatie en autorisatie. Commerciële code (bijvoorbeeld van Microsoft) is vaak grote schaal beveiliging gecontroleerd.
  - Meervoudige verificatie gebruiken. Multi-factor authentication-server is de huidige standaard voor verificatie en autorisatie, omdat de zwakke punten beveiliging inherent aan de gebruikersnaam en wachtwoord typen verificatie wordt voorkomen. Toegang tot zowel de Azure management (portal/externe PowerShell)-interfaces en klantgerichte services moet worden ontworpen en geconfigureerd voor gebruik [Azure multi-factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md).
  - Gebruik standaard verificatieprotocollen, zoals OAuth2- en Kerberos. Deze protocollen zijn uitgebreid onderzocht en waarschijnlijk worden geïmplementeerd als onderdeel van uw platformbibliotheken voor verificatie en autorisatie.

## <a name="next-steps"></a>Volgende stappen
In dit artikel is gericht op beveiligingsvoordelen van een Azure-PaaS-implementatie. Vervolgens leert u aanbevolen procedures voor het beveiligen van uw PaaS-web- en mobile-oplossingen. We beginnen met Azure App Service, Azure SQL Database en Azure SQL Data Warehouse. Artikelen over de aanbevolen procedures voor andere Azure-services beschikbaar komen, worden koppelingen vermeld in de volgende lijst:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Azure SQL Database en Azure SQL datawarehouse](security-paas-applications-using-sql.md)
- Azure Storage
- Azure REDIS-Cache
- Azure Service Bus
- Web Application Firewalls

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
