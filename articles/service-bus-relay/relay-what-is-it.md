---
title: Overzicht van Azure Relay en nuttige eigenschappen | Microsoft Docs
description: Overzicht van Azure Relay
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: 77ee85db0bcc701514a1a98da9405a79d658d49d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-relay"></a>Wat is Azure Relay?

De Azure Relay-service maakt het uitvoeren van hybride toepassingen mogelijk doordat u services die zich in een bedrijfsnetwerk bevinden, veilig kunt blootstellen aan de openbare cloud zonder dat u een firewallverbinding hoeft te openen en zonder dat wijzigingen in de infrastructuur van een bedrijfsnetwerk zijn vereist. Met Relay wordt een groot aantal verschillende transportprotocollen en webservicestandaarden ondersteund.

De Relay-service biedt ondersteuning voor traditioneel verkeer in één richting, aanvraag-/antwoordverkeer en peer-to-peerverkeer. De service ondersteunt tevens gebeurtenisdistributie via internet voor scenario's voor publiceren/abonneren en bidirectionele socket-communicatie voor verbeterde point-to-point-efficiëntie. 

In het patroon voor doorgestuurde gegevensoverdracht maakt een on-premises service verbinding met de Relay-service via een uitgaande poort en wordt een bidirectionele socket voor communicatie gemaakt die is gekoppeld aan een bepaald rendezvous-adres. De client kan vervolgens met de on-premises service communiceren door verkeer te verzenden naar de Relay-service die gericht is op het rendezvous-adres. De Relay-service stuurt gegevens vervolgens door naar de on-premises service via een bidirectionele socket die is toegewezen aan elke client. De client heeft geen rechtstreekse verbinding met de on-premises service nodig en hoeft niet te weten waar de service zich bevindt. Voor de on-premises service is niet vereist dat poorten voor inkomend verkeer zijn geopend in de firewall.

De belangrijkste mogelijkheden met Relay zijn bidirectionele, uit de buffer gehaalde communicatie over de netwerkgrenzen heen, met TCP-beperking, eindpuntdetectie, connectiviteitsstatus en overlappende eindpuntbeveiliging. De mogelijkheden van Relay verschillen van die van integratietechnologieën op netwerkniveau zoals VPN. Dit komt omdat Relay kan worden toegepast op een enkele computer, terwijl VPN-technologie veel ingrijpender is omdat de netwerkomgeving hierbij moet worden gewijzigd.

Azure Relay heeft twee functies:

1. [Hybride verbindingen](#hybrid-connections): maakt gebruik van websockets op basis van open standaarden, waardoor scenario's met meerdere platforms worden ingeschakeld.
2. [WCF-relays](#wcf-relays): maakt gebruik van WCF (Windows Communication Foundation) om externe procedureaanroepen in te schakelen. WCF-relay is de oude relay-aanbieding die veel klanten mogelijk al gebruiken met hun WCF-programmeringsmodellen.

Zowel met Hybride verbindingen als met WCF-relays wordt een beveiligde verbinding ingeschakeld naar activa die bestaan binnen een bedrijfsnetwerk. Of u de ene of de andere methode gebruikt is afhankelijk van uw specifieke behoeften, zoals wordt beschreven in de volgende tabel:

|  | WCF-relay | Hybride verbindingen |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET Framework** |x |x |
| **JavaScript/NodeJS** | |x |
| **Open protocol op basis van standaarden** | |x |
| **Meerdere RPC-programmeringsmodellen** | |x |

## <a name="hybrid-connections"></a>Hybride verbindingen

De functie [Hybride verbindingen van Azure Relay](relay-hybrid-connections-protocol.md) is een veilige open-protocolversie van de bestaande Relay-functies en kan op elk platform en in elke taal worden geïmplementeerd. De enige vereiste hiervoor zijn de standaardmogelijkheden van WebSocket, die expliciet de WebSocket-API omvat in veelgebruikte webbrowsers. Hybride verbindingen zijn gebaseerd op HTTP en WebSockets.

### <a name="service-history"></a>Servicegeschiedenis

De functie Hybride verbindingen vervangt de vorige functie, genaamd BizTalk Services, die is gebouwd op de Azure Service Bus WCF-relay. De mogelijkheden van de nieuwe hybride verbindingen zijn een aanvulling op de bestaande WCF-relay. De mogelijkheden van deze twee services bestaan naast elkaar in de Azure Relay-service. De twee services delen een gateway, maar zijn verder afzonderlijke implementaties.

## <a name="wcf-relays"></a>WCF-relays

De WCF-Relay werkt voor het volledige .NET Framework (NETFX) en voor WCF. U start de verbinding tussen uw on-premises service en de Relay-service met een reeks WCF-relaybindingen. Achter de schermen worden de Relay-bindingen toegewezen aan nieuwe transportbindingselementen die zijn ontworpen om WCF-kanaalonderdelen te maken die kunnen worden geïntegreerd met de Service Bus in de cloud.

## <a name="architecture-processing-of-incoming-relay-requests"></a>Architectuur: verwerken van inkomende relay-aanvragen
Wanneer een client een verzoek naar de [Azure Relay](/azure/service-bus-relay/)-service verzendt, wordt het via de Azure Load Balancer doorgestuurd naar een van de gateway-knooppunten. Als de aanvraag een aanvraag voor luisteren is, maakt het gateway-knooppunt een nieuwe relay. Als de aanvraag een verbindingsaanvraag is voor een specifieke relay, verzendt het gateway-knooppunt de verbindingsaanvraag door naar het gateway-knooppunt dat eigenaar is van de relay. Het gateway-knooppunt dat eigenaar is van de relay verzendt een ontmoetingsaanvraag naar de luisterende client om een tijdelijk kanaal te maken naar het gateway-knooppunt dat de verbindingsaanvraag heeft ontvangen.

Wanneer de relay-verbinding tot stand is gebracht, kunnen de clients berichten uitwisselen via het gateway-knooppunt dat wordt gebruikt voor de ontmoeting.

![Verwerken van inkomende WCF Relay-aanvragen](./media/relay-what-is-it/ic690645.png)

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)

