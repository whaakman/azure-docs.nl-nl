---
title: RADIUS-verificatie en Azure MFA-Server - Azure Active Directory
description: Leer hoe u RADIUS-verificatie en Azure Multi-Factor Authentication-server implementeert.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/31/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17e040492b1d986215aeb77ea14ebff53946f78e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056010"
---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>RADIUS-verificatie integreren met Azure Multi-Factor Authentication-server

RADIUS is een standaardprotocol om verificatieaanvragen te accepteren en te verwerken. De Azure Multi-Factor Authentication-server kan optreden als een RADIUS-server. Plaats de server hiervoor tussen uw RADIUS-client (VPN-apparaat) en het verificatiedoel om verificatie in twee stappen toe te voegen. Het verificatiedoel kan Active Directory, een LDAP-adreslijst of een andere RADIUS-server zijn. Azure Multi-Factor Authentication (MFA) werkt alleen als u de Azure MFA-server zo configureert dat deze kan communiceren met zowel de clientservers als het verificatiedoel. De Azure MFA-server accepteert aanvragen van een RADIUS-client, controleert referenties met behulp van het verificatiedoel, voegt Azure Multi-Factor Authentication toe en stuurt een antwoord terug naar de RADIUS-client. De verificatieaanvraag slaagt alleen als zowel de primaire verificatie als de Azure Multi-Factor Authentication slaagt.

> [!IMPORTANT]
> Vanaf 1 juli 2019, zal Microsoft MFA-Server niet meer bieden voor nieuwe implementaties. Nieuwe klanten die willen graag meervoudige verificatie van gebruikers vereisen moeten cloud-gebaseerde Azure multi-factor Authentication gebruiken. Bestaande klanten die vóór 1 juli MFA-Server hebben geactiveerd, worden kunnen de nieuwste versie downloaden door toekomstige updates en zoals gebruikelijk activeringsreferenties genereren.

> [!NOTE]
> De MFA-server ondersteunt alleen PAP (Password Authentication Protocol) en MSCHAPv2 (Microsoft Challenge-Handshake Authentication Protocol) RADIUS-protocollen wanneer deze als RADIUS-server fungeert.  Andere protocollen zoals EAP (Extensible Authentication Protocol) kunnen worden gebruikt wanneer de MFA-server fungeert als een RADIUS-proxy naar een andere RADIUS-server die ondersteuning biedt voor dat protocol.
>
> In deze configuratie werken eenzijdige SMS- en OATH-tokens niet omdat de MFA-server geen geslaagd RADIUS Challenge-antwoord met alternatieve protocollen kan starten.

![RADIUS-verificatie in de MFA-Server](./media/howto-mfaserver-dir-radius/radius.png)

## <a name="add-a-radius-client"></a>Een RADIUS-client toevoegen

Installeer de Azure Multi-Factor Authentication-server op een Windows-server als u RADIUS-verificatie wilt configureren. Als u een Active Directory-omgeving hebt, moet de server zijn toegevoegd aan het domein in het netwerk. Gebruik de volgende procedure om de Azure Multi-Factor Authentication-server te configureren:

1. Klik in de Azure Multi-Factor Authentication-server in het menu links op het pictogram RADIUS-authenticatie.
2. Schakel het selectievakje **RADIUS-verificatie inschakelen** in.
3. Wijzig de verificatie- en accountingpoorten op het tabblad Clients als de Azure MFA RADIUS-service naar RADIUS-aanvragen op niet-standaardpoorten moet luisteren.
4. Klik op **Toevoegen**.
5. Voer het IP-adres in van het apparaat dat of de server die moet worden geverifieerd bij de Azure Multi-Factor Authentication-server, en voeg (optioneel) een toepassingsnaam en een gedeeld geheim toe.

   De naam van de toepassing wordt opgenomen in rapporten en kan worden weergegeven in verificatieberichten via sms of in mobiele apps.

   Het gedeelde geheim moet op de Azure Multi-Factor Authentication-server en het apparaat/de server hetzelfde zijn.

6. Schakel het selectievakje **Overeenkomende Multi-Factor Authentication-gebruiker vereisen** in als alle gebruikers zijn geïmporteerd op de server en aan meervoudige verificatie moeten worden onderworpen. Als een groot aantal gebruikers nog niet is geïmporteerd op de server of zijn vrijgesteld van verificatie in twee stappen, laat u het selectievakje uitgeschakeld.
7. Schakel het selectievakje **Alternatief OATH-token inschakelen** in als u OATH-wachtwoordcodes van mobiele verificatie-apps wilt gebruiken als terugvaloptie.
8. Klik op **OK**.

Herhaal stap 4 tot en met 8 om het gewenste aantal extra RADIUS-clients toe te voegen.

## <a name="configure-your-radius-client"></a>De RADIUS-client configureren

1. Klik op het tabblad **Doel**.
   * Als de Azure MFA-Server is geïnstalleerd op een server domein in een Active Directory-omgeving, selecteert u **Windows-domein**.
   * Selecteer **LDAP-binding** als gebruikers moeten worden geverifieerd aan de hand van een LDAP-adreslijst.
      Selecteer het pictogram Adreslijstintegratie en bewerk de LDAP-configuratie op het tabblad Instellingen om ervoor te zorgen dat de server verbinding kan maken met uw adreslijst. In de [handleiding voor LDAP-proxyconfiguratie](howto-mfaserver-dir-ldap.md) vindt u instructies voor de configuratie van LDAP.
   * Als gebruikers moeten worden geverifieerd op basis van een andere RADIUS-server, selecteert u **RADIUS-server (s)** .
1. Klik op **toevoegen** als u de server waarop de Azure MFA-server de RADIUS-aanvragen een volmacht geeft, wilt configureren.
1. Voer in het dialoogvenster RADIUS-server toevoegen het IP-adres van de RADIUS-server en een gedeeld geheim in.

   Het gedeelde geheim moet op de Azure Multi-Factor Authentication-server en de RADIUS-server hetzelfde zijn. Wijzig de verificatiepoort en accountingpoort als door de RADIUS-server andere poorten worden gebruikt.

1. Klik op **OK**.
1. Voeg de Azure MFA-server toe als een RADIUS-client op de andere RADIUS-server, opdat toegangsverzoeken afkomstig van de Azure MFA-server kunnen worden verwerkt. Gebruik hetzelfde gedeelde geheim dat ook op de Azure Multi-Factor Authentication-server is geconfigureerd.

Herhaal deze stappen om meer RADIUS-servers toe te voegen. Met de knoppen **Omhoog** en **Omlaag** kunt u de volgorde configureren waarin de Azure MFA-server de servers aanroept.

De configuratie van de Azure Multi-Factor Authentication-server is hiermee voltooid. De server luistert nu naar de geconfigureerde poorten voor RADIUS-toegangsverzoeken van de geconfigureerde clients.

## <a name="radius-client-configuration"></a>RADIUS-clientconfiguratie

Als u de RADIUS-client wilt configureren, gebruikt u de volgende richtlijnen:

* Configureer uw toestel/server voor verificatie via RADIUS bij het IP-adres van de Azure Multi-Factor Authentication-server, die als de RADIUS-server fungeert.
* Gebruik hetzelfde gedeelde geheim dat eerder is geconfigureerd.
* Stel de time-out voor RADIUS in op 30 tot 60 seconden, zodat er voldoende tijd is om de referenties van de gebruiker te valideren, verificatie in twee stappen uit te voeren, de reactie daarop te ontvangen en vervolgens te reageren op de RADIUS-toegangsaanvraag.

## <a name="next-steps"></a>Volgende stappen

Lees hier meer over [de integratie van RADIUS-verificatie](howto-mfa-nps-extension.md) als u Azure Multi-Factor Authentication in de cloud gebruikt. 
