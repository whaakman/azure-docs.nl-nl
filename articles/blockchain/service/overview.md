---
title: Overzicht van Azure Blockchain-Service
description: Overzicht van de Azure Blockchain-Service
services: azure-blockchain
keywords: blockchain
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: a200649493354f1264afb0df4cf74acb4a274017
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406405"
---
# <a name="what-is-azure-blockchain-service"></a>Wat is Azure Blockchain-Service?

Azure Blockchain-Service is een volledig beheerde grootboek-service waarmee gebruikers de mogelijkheid om te laten groeien en gebruiken van blockchain-netwerken op schaal in Azure. Geïntegreerde controle voor zowel beheer van infrastructuur als blockchain netwerk governance bieden, biedt Azure Blockchain-Service:

* Eenvoudig netwerkimplementatie en bewerkingen
* Ingebouwd consortiumbeheer
* Slimme contracten met bekende ontwikkelhulpprogramma's ontwikkelen

Azure Blockchain-Service is ontworpen ter ondersteuning van meerdere grootboek-protocollen. Op dit moment biedt ondersteuning voor de Ethereum [Quorum](https://www.jpmorgan.com/Quorum) grootboek met behulp van de [IBFT](https://github.com/jpmorganchase/quorum/wiki/Quorum-Consensus) consensus-mechanisme.

Deze mogelijkheden vereisen nauwelijks beheer, en alles wordt zonder extra kosten geleverd. U kunt zich richten op de ontwikkeling van Apps en bedrijfslogica in plaats van tijd en middelen voor het beheren van virtuele machines en infrastructuur. U kunt bovendien blijven voor het ontwikkelen van uw toepassing met de open-source hulpprogramma's en het platform van uw keuze voor het leveren van uw oplossingen zonder nieuwe vaardigheden moet aanleren.

## <a name="network-deployment-and-operations"></a>De implementatie en bewerkingen

Implementatie van Azure Blockchain-Service kan worden gedaan via de Azure-portal, Azure CLI, evenals in Visual Studio code met de Azure Blockchain-extensie.  Implementatie wordt vereenvoudigd, met inbegrip van zowel de transactie en de validatie van knooppunten, virtuele netwerken van Azure voor isolatie, evenals service beheerde opslag inrichten.  Bovendien bij het implementeren van een nieuw lid van de blockchain, gebruikers ook maken of toevoegen, een consortium.  Consortiums inschakelen meerdere partijen in verschillende Azure-abonnementen om veilig met elkaar communiceren op een gedeelde blockchain te kunnen.  Deze vereenvoudigde implementatie minder blockchain netwerkimplementatie van dagen tot minuten.

### <a name="performance-and-service-tiers"></a>Prestaties en service-lagen

De Azure Blockchain-Service biedt twee Servicelagen: *Basic* en *Standard*. Elke laag biedt verschillende prestaties en mogelijkheden ter ondersteuning van de basisfunctionaliteit voor ontwikkeling en test workloads tot zeer uitgebreide blockchain-productie-implementaties. Beide lagen zijn ten minste één transactie-knooppunt en één validator knooppunt (basis) of twee validator knooppunten (standaard).

![Prijscategorieën](./media/overview/pricing-tiers.png)

Naast het bieden twee validator knooppunten, de *Standard* laag 2 biedt *vCores* voor elk knooppunt transactie en validator terwijl de Basic-laag een 1 vCore-configuratie biedt.  Door het aanbieden van 2 vCores voor transactie en validatie van knooppunten, kan 1 vCore worden toegewezen aan het grootboek Quorum terwijl de resterende 1 vCore kan worden gebruikt voor andere services met betrekking tot infrastructuur, ervoor te zorgen dat optimale prestaties voor productie blockchain-werkbelastingen. Zie voor meer informatie over prijsinformatie kunt [prijzen voor Azure Blockchain Service](https://azure.microsoft.com/pricing/details/blockchain-service).

### <a name="security-and-maintenance"></a>Beveiliging en onderhoud

U hebt de mogelijkheid extra transactielogboeken knooppunten toevoegen aan uw lid na het inrichten van uw eerste blockchain-lid.  Standaard transactie knooppunten zijn beveiligd via de firewall-regels en moeten worden geconfigureerd om toegang te krijgen.  Bovendien versleutelen alle knooppunten van de transactie van gegevens in beweging via TLS.  Er bestaan meerdere opties voor het beveiligen van transactie knooppunt toegang, met inbegrip van firewall-regels, basisverificatie, toegang tot sleutels, evenals Azure Active Directory-integratie. Zie voor meer informatie, [transactie knooppunten configureren](configure-transaction-nodes.md) en [toegang tot Azure Active Directory configureren](configure-aad.md).

Als een beheerde service, Azure Blockchain-Service zorgt ervoor dat uw blockchain-lid knooppunten worden gevuld met de meest recente host operationele en boekhouding software stack-updates, geconfigureerd voor hoge beschikbaarheid (alleen Standard laag), het elimineren van veel van de DevOps vereist voor traditionele IaaS blockchain-knooppunten.  Zie voor meer informatie over het toepassen van patches en updates, [ondersteunde versies van Azure Blockchain Service grootboek](ledger-versions.md).

### <a name="monitoring-and-logging"></a>Bewaking en registratie

Bovendien de Azure Blockchain-Service biedt uitgebreide metrische gegevens via Azure Monitor-Service bieden inzicht in de knooppunten CPU, geheugen- en opslaggebruik, evenals nuttig inzicht in de activiteit op het netwerk, zoals transacties en -blokken worden, blockchain wachtrijdiepte transactie, evenals actieve verbindingen.  Metrische gegevens kunnen worden aangepast voor weergaven in de inzichten die belangrijk voor uw blockchain-toepassingen zijn.  Drempelwaarden kunnen verder worden gedefinieerd via waarschuwingen zodat gebruikers voor het activeren van acties zoals het verzenden van een e-mail of SMS-bericht, het uitvoeren van een logische App, Azure-functie of verzenden naar een aangepaste definitie webhook.

![Metrische gegevens](./media/overview/metrics.png)

Tot en met Azure Log Analytics, kunnen gebruikers logboeken met betrekking tot het grootboek Quorum of andere verbindingen belangrijke informatie zoals heeft geprobeerd om de transactie-knooppunten te bekijken.

## <a name="built-in-consortium-management"></a>Ingebouwd consortiumbeheer

Bij het implementeren van uw eerste blockchain-lid u deelnemen aan of maak een nieuwe consortium.  Een consortium is een logische groep die wordt gebruikt voor het beheren van de governance en connectiviteit tussen de leden van de blockchain die in een proces dat meerdere partijen transact.  Azure Blockchain-Service biedt ingebouwde governance besturingselementen door middel van vooraf gedefinieerde slimme contracten, die bepalen welke leden van de acties in de consortium kunnen duren.  Deze besturingselementen governance kunnen indien nodig worden aangepast door de beheerder van het consortium. Wanneer u een nieuwe consortium maakt, is uw blockchain-lid de standaard-beheerder van het consortium de mogelijkheid om uit te nodigen andere partijen voor deelname aan uw consortium inschakelen.  U kunt een lid van een consortium worden alleen als u eerder hebt uitgenodigd.  Bij het toevoegen van een consortium, wordt uw blockchain-lid is onderhevig aan de governance-besturingselementen die door de beheerder van het consortium opstellen.

![Consortium management](./media/overview/consortium.png)

Acties zoals het toevoegen en verwijderen van leden van een consortium Consortium is toegankelijk via PowerShell en een REST-API. U kunt een consortium met behulp van algemene interfaces in plaats van het wijzigen en het verzenden van slimme contracten solidity gebaseerde programmatisch beheren. Zie voor meer informatie, [consortium management](consortium.md).

## <a name="develop-using-familiar-development-tools"></a>Ontwikkelen met behulp van vertrouwde ontwikkeling met hulpprogramma 's

Op basis van het Quorum Ethereum open-source grootboek, kunt u toepassingen ontwikkelen voor Azure Blockchain-Service de dezelfde manier zoals u dat wel voor bestaande Ethereum-toepassingen doet. Werken met toonaangevende IT-partners, kan Azure Blockchain Development Kit Visual Studio Code-extensie ontwikkelaars gebruikmaken van vertrouwde hulpprogramma's zoals Truffle Suite slimme contracten samenstellen. Met de extensie Azure Blockchain Development Kit, ontwikkelaars kunnen maken of u verbinding maken met en bestaande consortium zodat u kunt bouwen en implementeren van uw slimme contracten alle vanuit een IDE. Met de extensie Azure Blockchain Visual Studio Code, kunt u maken of verbinding maken met een bestaande consortium zodat u kunt bouwen en implementeren van uw slimme contracten geheel vanuit een IDE. Zie voor meer informatie, [Azure Blockchain Development Kit in de VS Code marketplace](https://aka.ms/vscodebcextension) en de [gebruikershandleiding voor Azure Blockchain Development Kit](https://aka.ms/vscodebcextensionwiki ).

## <a name="support-and-feedback"></a>Ondersteuning en feedback

Hulp nodig hebt of feedback?

* Ga naar de [Azure blockchainblog](https://azure.microsoft.com/blog/topics/blockchain/), [Microsoft Tech-Community](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain), en [Azure Blockchain-forum](https://social.msdn.microsoft.com/Forums/home?forum=azureblockchain).
* Als u feedback wilt geven of een nieuwe functie wilt aanvragen, maakt u een vermelding via [UserVoice](https://feedback.azure.com/forums/921130-azure-blockchain-service).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg een snelstartgids om te beginnen, of meer informatie van deze resources.
* [Een blockchain-lid met de Azure portal maken](create-member.md) of [maken van een blockchain-lid met behulp van Azure CLI]()
* Zie voor kostenvergelijkingen en calculators, de [pagina met prijzen](https://azure.microsoft.com/pricing/details/blockchain-service).
* Bouw uw eerste app met de [Azure Blockchain Development Kit](https://github.com/Azure-Samples/blockchain-devkit)
* Azure Blockchain VSCode-extensie [gebruikershandleiding](https://github.com/Microsoft/vscode-azure-blockchain-ethereum/wiki)
