---
title: PaaS-implementaties beveiligen | Microsoft Docs
description: " Informatie over de beveiligingsvoordelen van PaaS ten opzichte van andere service-modellen in de cloud en informatie over aanbevolen procedures voor het beveiligen van uw Azure PaaS-implementatie. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: techlake
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: da5d59aaaea8e6186609eb5f3419fba5e67d4279
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "42056866"
---
# <a name="securing-paas-deployments"></a>PaaS-implementaties beveiligen

Dit artikel bevat informatie die u helpt:

- Informatie over de beveiligingsvoordelen van hosting van toepassingen in de cloud
- Evalueren van de beveiligingsvoordelen van het platform als een service (PaaS) en een andere cloud service-modellen
- De focus van de beveiliging niet wijzigen van een centraal netwerk in een aanpak die perimeter-identiteit gebaseerde beveiliging
- Algemene PaaS best practices voor aanbevelingen voor beveiliging implementeren

## <a name="cloud-security-advantages"></a>Voordelen van cloud-beveiliging
Er zijn beveiligingsvoordelen wordt in de cloud. In een on-premises-omgeving, kunnen organisaties hebben nog niet vervulde verantwoordelijkheden en beperkte resources beschikbaar te investeren in de beveiliging, die zorgt voor een omgeving waarin aanvallers kunnen gebruikmaken van beveiligingsproblemen op alle lagen.

![Beveiligingsvoordelen van het cloudtijdperk][1]

Organisaties kunnen de detectie van bedreigingen en de reactietijden verbeteren met behulp van een provider cloud-gebaseerde beveiligingsmogelijkheden en intelligentie van de cloud.  Doordat de verantwoordelijkheden van de cloudprovider krijgt organisaties meer beveiliging dekking, zodat ze voor het toewijzen van resources van de beveiliging en het budget aan andere prioriteiten in uw bedrijf.

## <a name="division-of-responsibility"></a>Deling van verantwoordelijkheid
Het is belangrijk om te begrijpen van de verdeling van de verantwoordelijkheid tussen u en Microsoft. On-premises eigenaar van de hele stack, maar wanneer u naar de cloud bepaalde verantwoordelijkheden overbrengen naar Microsoft. Verantwoordelijkheid in de volgende matrix ziet u de gebieden van de stack in een SaaS, PaaS en IaaS-implementatie waarbij u bent verantwoordelijk voor en Microsoft is verantwoordelijk voor.

![Verantwoordelijkheid zones][2]

Voor alle cloud-implementatietypen, u bent eigenaar van uw gegevens en identiteiten. U bent verantwoordelijk voor het beveiligen van de beveiliging van uw gegevens en identiteiten, on-premises bronnen en de cloudonderdelen u controle (dit is afhankelijk van het servicetype).

Taken die altijd door u, ongeacht het type van de implementatie behouden, zijn:

- Gegevens
- Eindpunten
- Account
- Toegangsbeheer

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Beveiligingsvoordelen van een PaaS cloud service-model
Met behulp van dezelfde verantwoordelijkheid matrix, de beveiligingsvoordelen van een Azure PaaS-implementatie plaats van on-premises laten we kijken.

![Beveiligingsvoordelen van PaaS][3]

Aan de onderkant van de stack, de fysieke infrastructuur, vanaf vermindert Microsoft algemene risico's en -verantwoordelijkheden. Omdat de Microsoft-cloud voortdurend door Microsoft bewaakt wordt, is het moeilijk zijn om aan te vallen. Het zinvol niet zijn voor een aanvaller na te streven van Microsoft cloud als een doel. Als de aanvaller veel geld en bronnen, de aanvaller heeft is het waarschijnlijk om door te gaan naar een ander doel.  

Er is geen verschil tussen een PaaS-implementatie en on-premises in het midden van de stack. Op het niveau van de toepassing en een beheerlaag voor de account- en toegangsbeheer hebt u soortgelijke risico's. In de volgende sectie voor stappen van dit artikel wordt helpen we u aan de aanbevolen procedures voor het verwijderen van of het minimaliseren van deze risico's.

Aan de bovenkant van de stack, gegevensbeheer en rights management uitvoeren u op een risico's die kan worden opgelost door Sleutelbeheer. (Key management wordt beschreven in de aanbevolen procedures.) Sleutelbeheer is een extra verantwoordelijkheid, hebt u gebieden in een PaaS-implementatie die u niet langer hoeft te beheren zodat u resources naar Sleutelbeheer verplaatsen kunt.

Het Azure-platform biedt u ook sterk DDoS-beveiliging met behulp van verschillende technologieën op het netwerk. Alle typen van DDoS protection-methoden op het netwerk hebben echter hun beperkingen op basis van per-koppeling en per datacenter. Om te voorkomen van de impact van grote DDoS-aanvallen, kunt u profiteren van Azure core cloud mogelijkheden van het inschakelen van u om snel en automatisch de schaal vergroten tot verdediging tegen DDoS-aanvallen. Gaan we meer informatie over hoe u dit in de aanbevolen procedures-artikelen doen kunt.

## <a name="modernizing-the-defenders-mindset"></a>Modernisering van de Verdediger assume ')
Met PaaS komen implementaties een verschuiving van de algehele gang voor beveiliging. U schuiven hoeft voor het beheren van alles zelf verantwoordelijkheid delen met Microsoft.

Een ander belangrijk verschil tussen PaaS en traditionele on-premises implementaties, is een nieuwe weergave van de primaire beveiligingsperimeter bepaald. In het verleden was de beveiligingsperimeter primair on-premises is uw netwerk en de meeste on-premises beveiliging ontwerpen gebruiken het netwerk als de primaire beveiliging pivot. Voor PaaS-implementaties, u betere dienstverlening door de identiteit van de primaire beveiligingsperimeter overweegt.

## <a name="identity-as-the-primary-security-perimeter"></a>De identiteit als de primaire beveiligingsperimeter
Een van de vijf belangrijke kenmerken van cloudcomputing is ruime netwerktoegang, waardoor de netwerk-georiënteerde minder relevante overweegt. Het doel van veel van cloudcomputing om toe te staan gebruikers toegang krijgen tot resources, ongeacht de locatie. Voor de meeste gebruikers gaat de locatie zich ergens op het Internet.

De volgende afbeelding ziet u hoe de beveiligingsperimeter zich heeft ontwikkeld van een perimeter van het netwerk een perimeter-identiteit. Beveiliging wordt minder over het verdedigen van uw netwerk en meer informatie over uw gegevens beschermen, evenals de beveiliging van uw apps en gebruikers beheren. Het belangrijkste verschil is dat u pushen dichter bij de beveiliging wilt op wat belangrijk is voor uw bedrijf is.

![De identiteit als nieuwe beveiligingsperimeter][4]

In eerste instantie wordt Azure PaaS-services (bijvoorbeeld webrollen en Azure SQL) opgegeven weinig of geen traditionele netwerk perimeter-beveiliging. Het is duidelijk dat van het element doel is om te worden blootgesteld aan Internet (Webrol) en dat de verificatie de nieuwe omheining (bijvoorbeeld BLOB of Azure SQL biedt).

Moderne beveiligingsprocedures wordt ervan uitgegaan dat de kwaadwillende persoon de netwerkperimeter heeft geschonden. Moderne defense procedures zijn daarom verplaatst naar de identiteit. Organisaties moeten een perimeternetwerk identiteit gebaseerde beveiliging met sterke verificatie en autorisatie hygiëne (aanbevolen procedures) maken.

## <a name="recommendations-for-managing-the-identity-perimeter"></a>Aanbevelingen voor het beheren van de perimeter identiteit

Beginselen en patronen voor de netwerkverbinding zijn beschikbaar voor tientallen jaren. Daarentegen is de branche relatief minder ervaring met het gebruik van identiteit als de primaire beveiligingsperimeter. Met die al zei, hebben we voldoende ervaring voor enkele algemene aanbevelingen die in het veld hebben bewezen en van toepassing op bijna alle PaaS-services verzameld.

De volgende ziet u een algemene aanbevolen procedures-benadering voor het beheren van uw perimeternetwerk identiteit.

- **Uw sleutels of referenties niet verliezen** sleutels en referenties beveiligen is van essentieel belang voor het beveiligen van PaaS-implementaties. Verlies van sleutels en referenties is een veelvoorkomend probleem. Een goede oplossing is het gebruik van een gecentraliseerde oplossing waarbij de sleutels en geheimen kunnen worden opgeslagen in hardware security modules (HSM). Azure biedt u een HSM in de cloud met [Azure Key Vault](../key-vault/key-vault-whatis.md).
- **Plaats geen referenties en andere geheimen in de broncode of GitHub** het enige wat slechter dan een onbevoegde partij verlies van uw sleutels en referenties heeft er toegang toe hebben. Aanvallers zijn kunnen profiteren van bot technologieën om sleutels en geheimen in broncodeopslagruimte zoals GitHub te vinden. Plaats niet sleutel en geheimen in deze openbare broncodeopslagplaatsen.
- **Beveiligen van uw VM-beheerinterfaces van hybride PaaS en IaaS-services** IaaS en PaaS-services worden uitgevoerd op virtuele machines (VM's). Afhankelijk van het type service, verschillende beheerinterfaces beschikbaar zijn die u op afstand beheren van deze VM's rechtstreeks inschakelen. Extern beheer protocollen zoals [Secure Shell Protocol (SSH)](https://en.wikipedia.org/wiki/Secure_Shell), [Remote Desktop Protocol (RDP)](https://support.microsoft.com/kb/186607), en [externe PowerShell](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) kan worden gebruikt. In het algemeen is het raadzaam dat u niet rechtstreekse externe toegang tot VM's vanaf het Internet inschakelt. Als deze beschikbaar is, moet u alternatieve methoden, zoals het gebruik van virtueel particulier netwerk in een Azure-netwerk. Als er alternatieve methoden zijn niet beschikbaar, zorg ervoor dat u complexe wachtwoordzinnen en indien beschikbaar, tweeledige verificatie (zoals [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md)).
- **Sterke verificatie en autorisatie platforms te gebruiken**

  - Federatieve identiteiten in Azure AD in plaats van aangepaste gebruikersopslag gebruiken. Wanneer u federatieve identiteiten, u profiteren van een benadering op basis van het platform en u het beheer van bevoegde identiteiten delegeren aan uw partners. Een federatieve identiteit aanpak is vooral belangrijk in scenario's wanneer werknemers worden beëindigd en die gegevens moeten worden weerspiegeld in meerdere systemen voor identiteit en autorisatie.
  - Platform beschikbare verificatie en autorisatie-mechanismen gebruiken in plaats van aangepaste code. De reden is dat het ontwikkelen van aangepaste verificatiecode op te geven foutgevoelig kan zijn. De meeste van uw ontwikkelaars zijn niet beveiligingsexperts en waarschijnlijk niet op de hoogte van de finesses en de meest recente ontwikkelingen in de verificatie en autorisatie. Commerciële code (bijvoorbeeld van Microsoft) is vaak veel security gecontroleerd.
  - Meervoudige verificatie gebruiken. Meervoudige verificatie is de huidige standaard voor verificatie en autorisatie omdat wordt voorkomen de gebruikersnaam en wachtwoord typen verificatie intrinsieke security zwakke punten dat. Toegang tot zowel de Azure-beheer (portal/externe PowerShell)-interfaces en klantgerichte services moet worden ontworpen en geconfigureerd voor het gebruik van [Azure multi-factor Authentication (MFA)](../active-directory/authentication/multi-factor-authentication.md).
  - Gebruik standaard verificatieprotocollen, zoals OAuth2 en Kerberos. Deze protocollen zijn uitgebreid peer gecontroleerd en worden waarschijnlijk geïmplementeerd als onderdeel van uw platform-bibliotheken voor verificatie en autorisatie.

## <a name="next-steps"></a>Volgende stappen
In dit artikel richten we ons op beveiligingsvoordelen van een Azure PaaS-implementatie. Hierna leert u aanbevolen procedures voor het beveiligen van uw PaaS-web- en mobiele oplossingen. We beginnen met Azure App Service, Azure SQL Database en Azure SQL Data Warehouse. Artikelen over aanbevolen procedures voor andere Azure-services beschikbaar komen, worden koppelingen worden opgegeven in de volgende lijst:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Azure SQL Database en Azure SQL datawarehouse](security-paas-applications-using-sql.md)
- [Azure Storage](security-paas-applications-using-storage.md)
- Azure REDIS-Cache
- Azure Service Bus
- Web Application Firewalls

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
