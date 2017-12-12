---
title: Azure MFA-Server configureren voor maximale beschikbaarheid | Microsoft Docs
description: Implementeer meerdere exemplaren van Azure multi-factor Authentication-Server in de configuraties die maximale beschikbaarheid.
services: multi-factor-authentication
keywords: Azure MFA
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: c2f9a2422ea5a47edd80948073c7f7a6cb0d0945
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-azure-multi-factor-authentication-server-for-high-availability"></a>Azure multi-factor Authentication-Server configureren voor hoge beschikbaarheid

Om te zorgen voor hoge beschikbaarheid met uw Azure-Server MFA-implementatie, moet u meerdere MFA-servers implementeren. Deze sectie bevat informatie over een ontwerp voor taakverdeling voor uw doelen voor hoge beschikbaarheid in uw Azure MFS Server-implementatie.

## <a name="mfa-server-overview"></a>Overzicht van de MFA-Server

De architectuur van de service Azure MFA-Server omvat diverse onderdelen, zoals wordt weergegeven in het volgende diagram:

 ![Architectuur van MFA-Server](./media/mfa-server-high-availability/mfa-ha-architecture.png)

Een MFA-Server is een Windows-Server die de Azure multi-factor Authentication software is geïnstalleerd. Het MFA-Server-exemplaar moet worden geactiveerd door de MFA-Service in Azure naar de functie. Meer dan één MFA-Server worden geïnstalleerd op lokale.

De eerste MFA-Server die is geïnstalleerd, is de master MFA-Server bij de activering door de Service Azure MFA standaard. De master MFA-server heeft een beschrijfbare kopie van de database PhoneFactor.pfdata. Volgende installaties van exemplaren van MFA-Server worden slaves genoemd. De MFA-slaves hebben gerepliceerde alleen-lezen kopie van de database PhoneFactor.pfdata. MFA servers repliceren gegevens door middel van Remote Procedure Call (RPC). Alle servers van MFA moet gezamenlijk zijn verbonden met het domein of zelfstandige om gegevens te repliceren.

MFA master en slave MFA-Servers communiceren met de MFA-Service wanneer tweeledige verificatie vereist is. Bijvoorbeeld, wanneer een gebruiker probeert toegang te krijgen tot een toepassing waarvoor tweeledige verificatie, de gebruiker eerst geverifieerd door een id-provider, zoals Active Directory (AD).

Na een geslaagde authenticatie met AD communiceert de MFA-Server met de MFA-Service. De MFA-Server wacht op een melding van de MFA-Service wilt toestaan of weigeren van de gebruikerstoegang tot de toepassing.

Als de hoofd-MFA-server offline gaat, worden verificaties nog steeds kunnen worden verwerkt, maar bewerkingen waarvoor wijzigingen in de MFA-database kunnen niet worden verwerkt. (Voorbeelden zijn: het toevoegen van gebruikers, selfservice PINCODE wijzigen en het wijzigen van gebruikersgegevens)

## <a name="deployment"></a>Implementatie

Houd rekening met de volgende belangrijke punten voor taakverdeling Azure MFA-Server en de bijbehorende onderdelen.

* **Als u hoge beschikbaarheid met behulp van RADIUS-standaard**. Als u van Azure MFA-Servers als RADIUS-servers gebruikmaakt, kunt u mogelijk een MFA-Server configureren als een RADIUS-doel voor primaire verificatie en andere Azure MFA-Servers als secundaire verificatie doelen. Maar deze methode als u de maximale beschikbaarheid niet mogelijk praktische omdat u tot een time-outperiode moet worden vertoond wachten moet als verificatie bij het doel van de primaire verificatie mislukt voordat u op het doel van de secundaire verificatie kan worden geverifieerd. Het is efficiënter om taken te verdelen van de RADIUS-verkeer tussen de RADIUS-client en de RADIUS-Servers (in dit geval de Azure MFA-Servers die fungeren als RADIUS-servers) zodat u de RADIUS-clients configureren met één URL die u kunt kunt naar verwijzen.
* **Moet handmatig verhogen MFA slaves**. Als de hoofd Azure MFA-server offline gaat, blijven de secundaire Servers voor Azure MFA om MFA-aanvragen te verwerken. Echter, totdat een master MFA-server beschikbaar is, beheerders niet kunt gebruikers toevoegen of wijzigen van MFA-instellingen en gebruikers kunnen geen wijzigingen aanbrengen in de gebruikersportal. Promoveren van een slave MFA-functie master is altijd een handmatig proces.
* **Afscheidbaarheid van onderdelen**. De Azure MFA-Server bestaat uit verschillende onderdelen die kunnen worden geïnstalleerd op hetzelfde exemplaar van Windows Server of op verschillende exemplaren. Deze onderdelen zijn de Gebruikersportal, Mobile App Web Service en de AD FS-adapter (agent). Deze Afscheidbaarheid stelt u de Web Application Proxy gebruiken voor het publiceren van de Gebruikersportal en de Mobile App Web Server uit het perimeternetwerk. Een dergelijke configuratie wordt toegevoegd aan de algehele beveiliging van uw ontwerp, zoals wordt weergegeven in het volgende diagram. De MFA-Gebruikersportal en de Mobile App Web Server kan ook worden geïmplementeerd in HA taakverdeling configuraties.

   ![MFA-Server met een perimeternetwerk](./media/mfa-server-high-availability/mfasecurity.png)

* **Eenmalig wachtwoord (OTP) via SMS (aka eenzijdige SMS) vereist het gebruik van een tijdelijke sessies als verkeer taakverdeling**. SMS in één richting is een verificatieoptie die ervoor zorgt de MFA-Server dat voor het verzenden van de gebruikers een tekstbericht met een OTP. De gebruiker voert de OTP in een prompt venster de MFA-controle voltooien. Als u taakverdeling van Azure MFA-Servers, moet de server waarop de aanvraag van de eerste verificatie wordt uitgevoerd de server waarmee het OTP-bericht van de gebruiker ontvangen. Als een andere MFA-Server de OTP-antwoord ontvangt, wordt de verificatievraag mislukt. Zie voor meer informatie [één keer wachtwoord via SMS toegevoegd aan Azure MFA-Server](https://blogs.technet.microsoft.com/enterprisemobility/2015/03/02/one-time-password-over-sms-added-to-azure-mfa-server).
* **Taakverdeling implementaties van de Gebruikersportal en de webservice voor mobiele App moet een tijdelijke sessies**. Als u weet taakverdeling de MFA-Gebruikersportal en de webservice voor mobiele App, moet elke sessie op dezelfde server blijven.

## <a name="high-availability-deployment"></a>Hoge beschikbaarheid-implementatie

Het volgende diagram toont een volledige op HA taakverdeling implementatie van Azure MFA en de bijbehorende onderdelen, samen met ADFS ter referentie.

 ![Azure MFA-Server HA-implementatie](./media/mfa-server-high-availability/mfa-ha-deployment.png)

Let op de volgende items voor het navenant genummerde gebied van het voorgaande diagram.

1. De twee Azure MFA-Servers (MFA1 en MFA2) gelijkmatig verdeeld zijn (mfaapp.contoso.com) en zijn geconfigureerd voor het gebruik van een statische poort (4443) om de database PhoneFactor.pfdata te repliceren. De Web Service SDK is geïnstalleerd op elk van de MFA-Server-communicatie inschakelen via TCP-poort 443 met de ADFS-servers. De MFA-servers worden geïmplementeerd in een stateless configuratie van taakverdeling. Echter, als u OTP via SMS gebruiken wilt, moet u stateful taakverdeling.
   ![Azure MFA-Server - appserver HA](./media/mfa-server-high-availability/mfaapp.png)

   > [!NOTE]
   > Omdat de RPC dynamische poorten gebruikt, is het niet raadzaam openen firewalls tot het bereik van dynamische poorten die RPC kan gebruiken. Als u een firewall hebt **tussen** uw MFA-toepassingsservers, moet u de MFA-Server om te communiceren op een statische poort voor het replicatieverkeer tussen slave en masterservers en open deze poort op uw firewall configureren. U kunt de statische poort afdwingen door het maken van een DWORD-registerwaarde op ```HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Positive Networks\PhoneFactor``` aangeroepen ```Pfsvc_ncan_ip_tcp_port``` en de waarde in te stellen op een beschikbare statische poort. Verbindingen zijn altijd wordt geïnitieerd door de slave MFA Servers naar het hoofdniveau, statische poort is alleen vereist op de master, maar omdat u een lager niveau bevindende om te worden van het model op elk gewenst moment promoveren kunt, moet u de statische poort instellen op alle Servers met MFA.

2. De twee servers gebruiker Portal/MFA mobiele App (MFA-UP-MAS1 en MFA-UP-MAS2) worden verdeeld een **stateful** configuration (mfa.contoso.com). Intrekken dat een tijdelijke sessies zijn vereist voor de load balancer het MFA-Gebruikersportal en de mobiele App Service.
   ![Azure MFA-Server - Gebruikersportal en de mobiele App Service HA](./media/mfa-server-high-availability/mfaportal.png)
3. De ADFS-Server-farm is evenredig verdeeld en gepubliceerd op Internet via AD FS-proxy's in het perimeternetwerk taakverdeling. Elke AD FS-Server maakt gebruik van de AD FS-agent te communiceren met de Azure MFA-Servers via één taakverdeling URL (mfaapp.contoso.com) via TCP-poort 443.

## <a name="next-steps"></a>Volgende stappen

* [Azure MFA-Server installeren en configureren](multi-factor-authentication-get-started-server.md)
