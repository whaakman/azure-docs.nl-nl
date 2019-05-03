---
title: Overzicht van Azure Blockchain-Service voor ontwikkelaars
description: Inleiding op het ontwikkelen van oplossingen voor Azure Blockchain-Service.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 388a5d8c80c3e2462602959e9d5cbc1452974d1f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027898"
---
# <a name="azure-blockchain-service-development-overview"></a>Overzicht van Azure Blockchain-Service voor ontwikkelaars

Met Azure Blockchain-Service, kunt u consortium blockchain-netwerken om in te schakelen zakelijke scenario's zoals activa bijhouden, digitale token, betrokkenheid en beloning, financiële toeleveringsketen en herkomst maken. In dit artikel bevat een inleiding tot Azure Blockchain-Service-overzicht voor ontwikkelaars en belangrijke onderwerpen voor het implementeren van blockchain voor enterprise.

## <a name="client-connection-to-azure-blockchain-service"></a>Clientverbinding met Azure Blockchain-Service

Er zijn verschillende soorten clients voor blockchain netwerken, inclusief volledige knooppunten, lichte knooppunten en externe clients. Azure Blockchain-Service bouwt een blockchain-netwerk met knooppunten. U kunt verschillende clients gebruiken als uw gateway met Azure Blockchain-Service voor blockchain-ontwikkeling. Azure Blockchain-Service biedt basisverificatie of toegangssleutel als een eindpunt in ontwikkeling. Hieronder vindt u populaire clients kunt u verbinding maken.

### <a name="metamask"></a>MetaMask

MetaMask is een browser gebaseerde wallet (externe client), RPC-client en basic-contract explorer. In tegenstelling tot andere portefeuilles browser MetaMask injects een web3-exemplaar in de browser JavaScript-context, fungeert als een RPC-client die verbinding met een verscheidenheid aan Ethereum blockchains maakt (*mainnet*, *Ropsten testnet*, *Kovan testnet*, lokale RPC-knooppunt, enz.). U kunt aangepaste RPC eenvoudig verbinding maken met Azure Blockchain-Service en start blockchain-ontwikkeling met behulp van Remix instellen.

### <a name="geth"></a>Geth

Geth is de opdrachtregelinterface voor het uitvoeren van een volledige Ethereum-knooppunt in Go geïmplementeerd. U hoeft te worden uitgevoerd volledige knooppunt maar een interactieve console waarmee een JavaScript-runtime-omgeving die een JavaScript-API om te kunnen werken met Azure Blockchain-Service kunnen starten.

## <a name="development-framework-configuration"></a>Ontwikkeling framework-configuratie

Voor het ontwikkelen van geavanceerde bedrijfstoepassingen blockchain-oplossingen, is een raamwerk voor ontwikkelaars nodig om verbinding maken met verschillende blockchain-netwerken, slimme contract levenscyclus beheren, tests automatiseren, slimme contract met scripts implementeren en verschaffen van een interactieve console.

Truffle is een populaire blockchain-webontwikkelingsframework te schrijven, compileren, implementeren en testen van gedecentraliseerde toepassingen op Ethereum blockchains. U kunt ook Truffle beschouwen als een framework waarmee wordt geprobeerd slimme contract ontwikkeling en webontwikkeling traditionele probleemloos kunt integreren.

Zelfs de kleinste project communiceert met ten minste twee blockchain-knooppunten: Een van de ontwikkelaar van de machine en de andere voor het netwerk waar de ontwikkelaar van hun toepassing implementeert. Bijvoorbeeld, de belangrijkste openbaar Ethereum-netwerk of een Azure Blockchain-Service. Truffle biedt een systeem voor het beheren van de compilatie ingeschakeld en met implementatie artefacten voor elk netwerk en op een manier die vereenvoudigt de implementatie van de toepassing uiteindelijk doet. Zie voor meer informatie [Snelstart: Truffle gebruiken voor verbinding met een een netwerk met Azure Blockchain Service](connect-truffle.md).

## <a name="ethereum-quorum-private-transaction"></a>Persoonlijke Ethereum Quorum-transactie

Quorum is een protocol op basis van een Ethereum gedistribueerd grootboek met transactie plus contract privacy en nieuwe consensus-mechanismen. Belangrijke verbeteringen via Go-Ethereum zijn onder andere:

* Privacy - Quorum biedt ondersteuning voor persoonlijke transacties en persoonlijke contracten via openbare en persoonlijke status scheiding en maakt gebruik van peer-to-peer versleutelde berichten worden uitgewisseld voor gerichte overdracht van persoonlijke gegevens voor netwerk-deelnemers.
* Alternatieve methoden van het Consensus - in de zonder noodzaak voor het bewijs van werk- of bewijs van inzet consensus in een permissioned netwerk. Quorum biedt meerdere consensus mechanismen die zijn ontworpen voor consortium ketens zoals echter en IBFT.  Azure Blockchain-Services maakt gebruik van het mechanisme voor IBFT consensus.

* Peer-rollen - knooppunt en peer machtigingen beheren met behulp van slimme contracten, ervoor te zorgen dat alleen bekende partijen kan deelnemen aan het netwerk
* Betere prestaties - Quorum biedt betere prestaties dan openbare Geth

Zie [zelfstudie: Verzenden van een transactie met Azure Blockchain Service](send-transaction.md) voor een voorbeeld van een persoonlijke transactie.

## <a name="block-explorers"></a>Blok explorers

Blok explorers zijn online blockchain-browsers die inhoud afzonderlijke blokkeren, adres transactiegegevens en geschiedenis weergeven. Basic blok informatie is beschikbaar via Azure Monitor in Azure Blockchain-Service, maar als u meer gedetailleerde informatie tijdens de ontwikkeling, blokkeren explorers kunnen nuttig zijn.  Er zijn een populaire open-source-blok explorers die u kunt gebruiken. Hier volgt een lijst met geblokkeerde explorers die met Azure Blockchain-Service werken:

* [Azure Blockchain-Service Explorer](https://web3labs.com/azure-offer) van Web3 Labs
* [BlockScout](https://github.com/Azure-Samples/blockchain/blob/master/ledger/template/ethereum-on-azure/technology-samples/blockscout/README.md)

## <a name="tps-measurement"></a>TPS meting

Zoals blockchain wordt gebruikt in meer zakelijke scenario's, transacties per seconde (TPS) snelheid is belangrijk om te voorkomen dat de knelpunten en inefficiëntie system. Hoge transactietarieven kunnen lastig zijn te onderhouden binnen een gedecentraliseerde blockchain. Een nauwkeurige TPS-metingen kan worden beïnvloed door diverse factoren, zoals serverthread, de grootte van de transactie-wachtrij, netwerklatentie en beveiliging. Als u nodig hebt voor het meten van TPS snelheid tijdens de ontwikkeling, een populaire open-source-hulpprogramma is [ChainHammer](https://github.com/drandreaskrueger/chainhammer).

## <a name="next-steps"></a>Volgende stappen

[Snelstart: Truffle gebruiken voor verbinding met een een Blockchain-Service van Azure-netwerk](connect-truffle.md)
