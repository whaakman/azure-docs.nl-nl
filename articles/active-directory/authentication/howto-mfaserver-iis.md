---
title: IIS-verificatie en Azure MFA-Server - Azure Active Directory
description: Leer hoe u IIS-verificatie en Azure Multi-Factor Authentication-server implementeert.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2b7f76211fe810ce1db53e5afaa307d90317464a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057388"
---
# <a name="configure-azure-multi-factor-authentication-server-for-iis-web-apps"></a>Azure Multi-Factor Authentication-server configureren voor IIS-webtoepassingen

Gebruik de IIS-verificatiesectie van de Azure Multi-Factor Authentication-server (MFA) om IIS-verificatie in te schakelen en te configureren voor integratie met Microsoft IIS-webtoepassingen. Met de Azure MFA-server wordt een invoegtoepassing geïnstalleerd voor het filteren van aanvragen die worden ingediend bij de IIS-webserver om Azure Multi-Factor Authentication te kunnen toevoegen. De IIS-invoegtoepassing biedt ondersteuning voor verificatie op basis van formulier en geïntegreerde Windows-verificatie voor HTTP. Goedgekeurde IP-adressen kunnen ook zo worden geconfigureerd dat interne IP-adressen worden vrijgesteld van verificatie met twee factoren.

> [!IMPORTANT]
> Vanaf 1 juli 2019, zal Microsoft MFA-Server niet meer bieden voor nieuwe implementaties. Nieuwe klanten die willen graag meervoudige verificatie van gebruikers vereisen moeten cloud-gebaseerde Azure multi-factor Authentication gebruiken. Bestaande klanten die vóór 1 juli MFA-Server hebben geactiveerd, worden kunnen de nieuwste versie downloaden door toekomstige updates en zoals gebruikelijk activeringsreferenties genereren.

![IIS-verificatie in de MFA-Server](./media/howto-mfaserver-iis/iis.png)

## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>IIS-verificatie samen gebruiken met Azure Multi-Factor Authentication-server

Als u een IIS-webtoepassing wilt beveiligen die gebruikmaakt van verificatie op basis van een formulier, moet u de Azure Multi-Factor Authentication-server op de IIS-webserver installeren en de server volgens de onderstaande procedure configureren:

1. Klik op de Azure Multi-Factor Authentication-server in het menu links op het pictogram IIS-verificatie.
2. Klik op het tabblad **Op formulier gebaseerd**.
3. Klik op **Toevoegen**.
4. Variabelen voor de gebruikersnaam, wachtwoord en automatisch detecteren, voert u de aanmeldings-URL (zoals `https://localhost/contoso/auth/login.aspx`) in het dialoogvenster formulier-gebaseerde Website en klik op **OK**.
5. Schakel het selectievakje **Overeenkomende Multi-Factor Authentication-gebruiker vereisen** in als alle gebruikers zijn of moeten worden geïmporteerd in de server en aan Multi-Factor Authentication moeten worden onderworpen. Als een groot aantal gebruikers nog niet is geïmporteerd op de server en/of vrijgesteld zal zijn van meervoudige verificatie, laat u het vakje uitgeschakeld.
6. Als de paginavariabelen niet automatisch kunnen worden gedetecteerd, klikt u op de **Handmatig opgeven** in het dialoogvenster Formulier-gebaseerde website automatisch configureren.
7. Voer in het dialoogvenster Formulier-gebaseerde website toevoegen in het veld Indienings-URL de URL naar de aanmeldingspagina in en voer een toepassingsnaam in (optioneel). De naam van de toepassing wordt vermeld in Azure Multi-Factor Authentication-rapporten en kan worden weergegeven in verificatieberichten via sms of mobiele apps.
8. Selecteer de juiste aanvraagindeling. Deze is voor de meeste webtoepassingen ingesteld op **POST of GET**.
9. Voer de gebruikersnaamvariabele, de wachtwoordvariabele en de domeinvariabele in (als deze op de aanmeldingspagina worden weergegeven). Ga in een webbrowser naar de aanmeldingspagina, klik met de rechtermuisknop op de pagina en selecteer **Bron weergeven** om de namen van de invoervakken te vinden.
10. Schakel het selectievakje **Overeenkomende Azure Multi-Factor Authentication-gebruiker vereisen** in als alle gebruikers zijn of moeten worden geïmporteerd in de Azure Multi-Factor Authentication-server en aan Multi-Factor Authentication onderworpen zijn. Als een groot aantal gebruikers nog niet is geïmporteerd op de server en/of vrijgesteld zal zijn van meervoudige verificatie, laat u het vakje uitgeschakeld.
11. Klik op **Geavanceerd** om de geavanceerde instellingen te bekijken, zoals:

    - Een aangepast bestand voor de weigeringspagina selecteren
    - Succesvolle verificaties voor de website voor een bepaalde periode in de cache opslaan met behulp van cookies
    - Selecteer of u de primaire referenties wilt verifiëren aan de hand van een Windows-domein, LDAP-adreslijst. of RADIUS-server.

12. Klik op **OK** om terug te gaan naar het dialoogvenster Formulier-gebaseerde website toevoegen.
13. Klik op **OK**.
14. Zodra de URL- en paginavariabelen zijn gedetecteerd of ingevoerd, worden de websitegegevens weergegeven in het paneel Op formulier gebaseerd.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Geïntegreerde Windows-verificatie met Azure Multi-Factor Authentication-server gebruiken

Als u een IIS-webtoepassing wilt beveiligen die gebruikmaakt van geïntegreerde Windows-verificatie voor HTTP, moet u de Azure MFA-server op de IIS-webserver installeren en de server aan de hand van de onderstaande stappen configureren:

1. Klik op de Azure Multi-Factor Authentication-server in het menu links op het pictogram IIS-verificatie.
2. Klik op het tabblad **HTTP**.
3. Klik op **Toevoegen**.
4. Voer in het dialoogvenster basis-URL toevoegen de URL voor de website waar de HTTP-verificatie wordt uitgevoerd (zoals <http://localhost/owa>) en geef een toepassingsnaam in (optioneel). De naam van de toepassing wordt vermeld in Azure Multi-Factor Authentication-rapporten en kan worden weergegeven in verificatieberichten via sms of mobiele apps.
5. Pas de tijd voor time-out voor inactiviteit en voor Maximale sessie aan als de standaardwaarde niet voldoende is.
6. Schakel het selectievakje **Overeenkomende Multi-Factor Authentication-gebruiker vereisen** in als alle gebruikers zijn of moeten worden geïmporteerd in de server en aan Multi-Factor Authentication moeten worden onderworpen. Als een groot aantal gebruikers nog niet is geïmporteerd op de server en/of vrijgesteld zal zijn van meervoudige verificatie, laat u het vakje uitgeschakeld.
7. Schakel, indien gewenst, het selectievakje **Cookie gebruiken om voltooide authenticaties in de cache op te slaan** in.
8. Klik op **OK**.

## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>IIS-invoegtoepassingen inschakelen voor Azure Multi-Factor Authentication-server

Nadat u de URL's en instellingen voor op formulier gebaseerde of HTTP-verificaties hebt ingesteld, selecteert u de locaties waar de IIS-invoegtoepassingen van Azure Multi-Factor Authentication moeten worden geladen en ingeschakeld in IIS. Gebruik de volgende procedure:

1. Indien uitgevoerd op IIS 6, klikt u op het tabblad **ISAPI**. Selecteer de website waaronder die webtoepassing wordt uitgevoerd (bijvoorbeeld de standaardwebsite) om de invoegtoepassing met het ISAPI-filter van Azure Multi-Factor Authentication voor die site in te schakelen.
2. Indien uitgevoerd op IIS 7 of hoger, klikt u op het tabblad **Systeemeigen module**. Selecteer de server, websites of toepassingen om de IIS-invoegtoepassing op de gewenste niveaus in te schakelen.
3. Klik op het vak **IIS-verificatie** inschakelen aan de bovenkant van het scherm. Azure Multi-Factor Authentication is nu de beveiliging van de geselecteerde IIS-toepassing. Zorg ervoor dat gebruikers in de server zijn geïmporteerd.

## <a name="trusted-ips"></a>Goedgekeurde IP-adressen

De goedgekeurde IP-adressen bieden gebruikers de mogelijkheid om Azure Multi-Factor Authentication over te slaan voor websiteverzoeken die afkomstig zijn van bepaalde IP-adressen of subnetten. Zo kunt u gebruikers vrijstellen van Azure Multi-Factor Authentication wanneer zij zich op kantoor aanmelden. Hiervoor geeft u het subnet van de werkplek op als een goedgekeurd IP-adres. Gebruik de volgende procedure als u goedgekeurde IP-adressen wilt configureren:

1. Klik op het tabblad **Goedgekeurde IP-adressen** in de sectie IIS-verificatie.
2. Klik op **Toevoegen**.
3. Wanneer het dialoogvenster Goedgekeurd IP-adres toevoegen wordt weergegeven, selecteert u het keuzerondje **Eén IP-adres**, **IP-bereik** of **Subnet**.
4. Voer het IP-adres, bereik van IP-adressen of subnet die moet worden toegestaan. Als u een subnet wilt invoeren, selecteert u het geschikte Netmasker en klikt u op **OK**.
