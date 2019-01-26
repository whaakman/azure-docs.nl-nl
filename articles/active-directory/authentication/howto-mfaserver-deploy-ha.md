---
title: Azure MFA-Server configureren voor hoge beschikbaarheid | Microsoft Docs
description: Meerdere instanties van Azure multi-factor Authentication-Server in configuraties die zorgen voor hoge beschikbaarheid implementeren.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.openlocfilehash: 0ce14b3d78b020bbb8872661351794e82944205e
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55081837"
---
# <a name="configure-azure-multi-factor-authentication-server-for-high-availability"></a>Azure multi-factor Authentication-Server configureren voor maximale beschikbaarheid

Voor het bereiken van hoge beschikbaarheid met uw Azure-Server MFA-implementatie, moet u meerdere MFA-servers implementeren. In deze sectie bevat informatie over een ontwerp met load balancing om uw doelen voor hoge beschikbaarheid in uw Azure-MFS-serverimplementatie.

## <a name="mfa-server-overview"></a>Overzicht van de MFA-Server

De architectuur van de service Azure MFA-Server bestaat uit verschillende onderdelen, zoals wordt weergegeven in het volgende diagram:

 ![Architectuur van MFA-Server](./media/howto-mfaserver-deploy-ha/mfa-ha-architecture.png)

Een MFA-Server is een Windows-Server die de Azure multi-factor Authentication software is geïnstalleerd. Het MFA-Server-exemplaar moet worden geactiveerd door de MFA-Service in Azure naar functie. Meer dan één MFA-Server worden geïnstalleerd on-premises.

De eerste MFA-Server die is geïnstalleerd, is de master MFA-Server bij de activering door de Azure MFA-Service standaard. De master MFA-server heeft een beschrijfbare kopie van de database PhoneFactor.pfdata. Volgende installaties van exemplaren van MFA-Server staat bekend als onderliggende niveaus. De MFA-onderliggende niveaus hebben een gerepliceerde alleen-lezen kopie van de database PhoneFactor.pfdata. MFA-servers repliceren met behulp van Remote Procedure Call (RPC) informatie. Alle MFA-servers moeten gezamenlijk lid zijn van een domein of zelfstandige om gegevens te repliceren.

Zowel MFA hoofd- en onderliggende MFA-Servers communiceren met de MFA-Service als tweeledige verificatie vereist is. Bijvoorbeeld, wanneer een gebruiker probeert toegang te krijgen tot een toepassing waarbij de verificatie met twee factoren, wordt de gebruiker eerst worden geverifieerd door een id-provider, zoals Active Directory (AD).

Na een succesvolle verificatie met AD communiceert de MFA-Server met de MFA-Service. De MFA-Server wacht op een melding van de MFA-Service wilt toestaan of weigeren van de gebruikerstoegang tot de toepassing.

Als de hoofd-MFA-server offline gaat, worden verificaties nog steeds kunnen worden verwerkt, maar bewerkingen waarvoor wijzigingen in de MFA-database kunnen niet worden verwerkt. (Voorbeelden zijn onder meer: het toevoegen van gebruikers, selfservice PINCODE wijzigen en wijzigen van gebruikersgegevens)

## <a name="deployment"></a>Implementatie

Houd rekening met de volgende belangrijke punten voor taakverdeling van Azure MFA-Server en de bijbehorende onderdelen.

* **Met behulp van RADIUS-standaard voor hoge beschikbaarheid**. Als u van Azure MFA-Servers als RADIUS-servers gebruikmaakt, kunt u mogelijk een MFA-Server configureren als een RADIUS-doel voor primaire verificatie en andere Azure MFA-Servers als de doelen van de secundaire verificatie. Maar deze methode voor hoge beschikbaarheid mogelijk niet praktisch omdat u tot een time-outperiode wachten moet moet plaatsvinden wanneer verificatie op het doel van de primaire verificatie is mislukt voordat u op het doel van de secundaire verificatie kan worden geverifieerd. Het is efficiënter voor taakverdeling tussen de RADIUS-verkeer tussen de RADIUS-client en de RADIUS-Servers (in dit geval de Azure MFA-Servers die fungeren als RADIUS-servers) zodat u de RADIUS-clients met één URL die ze configureren kunt naar kunnen verwijzen.
* **Moet handmatig promoveren MFA onderliggende niveaus**. Als de master Azure MFA-server offline gaat, blijven de secundaire Servers voor Azure MFA om MFA-aanvragen te verwerken. Echter, totdat een master MFA-server beschikbaar is, beheerders kunnen geen gebruikers toevoegen of wijzigen van de MFA-instellingen en kunnen gebruikers niet met behulp van de gebruikersportal wijzigingen aanbrengen. Promoveren van een MFA is ondergeschikt is aan de rol altijd een handmatig proces.
* **Afscheidbaarheid van onderdelen**. De Azure MFA-Server bestaat uit verschillende onderdelen die kunnen worden geïnstalleerd op hetzelfde exemplaar van Windows Server of op verschillende exemplaren. Deze onderdelen zijn onder andere de Gebruikersportal, webservice voor mobiele App en de AD FS-adapter (agent). Deze Afscheidbaarheid maakt het mogelijk is de Web Application Proxy gebruiken om te publiceren van de Gebruikersportal en mobiele App-webserver uit het perimeternetwerk. Een dergelijke configuratie wordt toegevoegd aan de algehele beveiliging van uw ontwerp, zoals wordt weergegeven in het volgende diagram. De Gebruikersportal van MFA en Mobile App Web Server kan ook worden geïmplementeerd in HA met load balancing-configuraties.

   ![MFA-Server met een perimeternetwerk](./media/howto-mfaserver-deploy-ha/mfasecurity.png)

* **Eenmalig wachtwoord (OTP) via SMS (ook wel eenrichtings-SMS) vereist het gebruik van tijdelijke sessies als verkeer met load balancing**. SMS in één richting is een optie voor verificatie dat ervoor zorgt de MFA-Server dat voor het verzenden van de gebruikers een tekstbericht met een OTP. De gebruiker voert de OTP in een venster op de MFA-controle voltooien. Als u taakverdeling Azure MFA-Servers, moet de server waarop de initiële verificatie-aanvraag wordt uitgevoerd de server die het OTP-bericht van de gebruiker ontvangt. Als een andere MFA-Server de OTP-antwoord ontvangt, wordt de verificatiecontrole mislukt. Zie voor meer informatie, [één keer wachtwoord via SMS toegevoegd aan Azure MFA-Server](https://blogs.technet.microsoft.com/enterprisemobility/2015/03/02/one-time-password-over-sms-added-to-azure-mfa-server).
* **Met Load Balancing implementaties van de Gebruikersportal en de webservice voor mobiele App moet tijdelijke sessies**. Als u bent load balancing de MFA-Gebruikersportal en de webservice voor mobiele Apps, moet elke sessie om te blijven op dezelfde server.

## <a name="high-availability-deployment"></a>Implementatie met hoge beschikbaarheid

Het volgende diagram toont een volledige-HA met load balancing implementatie van Azure MFA en de bijbehorende onderdelen, samen met AD FS ter referentie.

 ![Azure MFA Server HA-implementatie](./media/howto-mfaserver-deploy-ha/mfa-ha-deployment.png)

Houd er rekening mee de volgende items voor de overeenkomstige genummerde gebieden van het vorige diagram.

1. De twee Azure MFA-Servers (MFA1 en MFA2) worden verdeeld (mfaapp.contoso.com) en zijn geconfigureerd voor het gebruik van een statische poort (4443) om de database PhoneFactor.pfdata te repliceren. De webservice-SDK is geïnstalleerd op elk van de MFA-Server voor de communicatie via TCP-poort 443 met de AD FS-servers. De MFA-servers zijn geïmplementeerd in een staatloze met load balancing-configuratie. Echter, als u OTP via SMS gebruiken wilt, moet u stateful taakverdeling.
   ![Azure MFA-Server - HA-App-server](./media/howto-mfaserver-deploy-ha/mfaapp.png)

   > [!NOTE]
   > Omdat de RPC dynamische poorten gebruikt, wordt het niet aanbevolen om te openen firewalls tot het bereik van dynamische poorten die RPC kan gebruiken. Als u een firewall hebt **tussen** uw MFA-toepassingsservers, moet u de MFA-Server om te communiceren op een statische poort voor het replicatieverkeer tussen de onderliggende en master-servers en die poort openen in uw firewall configureren. U kunt afdwingen dat de statische poort door het maken van een DWORD-registerwaarde op ```HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Positive Networks\PhoneFactor``` met de naam ```Pfsvc_ncan_ip_tcp_port``` en als de waarde instelt op een beschikbare statische poort. Verbindingen zijn altijd wordt geïnitieerd door de onderliggende MFA-Servers aan het model, de statische poort is alleen vereist op de hoofddoelserver, maar omdat u een onderliggend niveau om te worden van het model op elk gewenst moment verhogen kunt, moet u de statische poort ingesteld op alle MFA-Servers.

2. De twee mobiele App van gebruiker Portal/MFA-servers (MFA-UP-MAS1 en MFA-UP-MAS2) worden gelijkmatig verdeeld een **stateful** configuratie (mfa.contoso.com). Intrekken dat tijdelijke sessies zijn vereist voor de MFA-Gebruikersportal en mobiele App Service te verdelen.
   ![Azure MFA-Server - Gebruikersportal en mobiele App Service HA](./media/howto-mfaserver-deploy-ha/mfaportal.png)
3. De AD FS-serverfarm is evenredig verdeeld en gepubliceerd op Internet via AD FS-proxy's in het perimeternetwerk met load balancing. Elke AD FS-Server maakt gebruik van de AD FS-agent om te communiceren met de Azure MFA-Servers met behulp van een eenmalige load balancing-URL (mfaapp.contoso.com) via TCP-poort 443.

## <a name="next-steps"></a>Volgende stappen

* [Azure MFA-Server installeren en configureren](howto-mfaserver-deploy.md)
