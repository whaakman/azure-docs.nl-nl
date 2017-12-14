---
title: Azure MFA-Server gebruiken met AD FS 2.0 | Microsoft Docs
description: Dit is de Azure Multi-Factor Authentication-pagina waarop wordt beschreven hoe u met Azure MFA en AD FS 2.0 aan de slag kunt gaan.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 96168849-241a-4499-a224-d829913caa7e
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: 5a1d3ae76b9fe6e60113dbb1d4157a686e1d809b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-azure-multi-factor-authentication-server-to-work-with-ad-fs-20"></a>Azure Multi-Factor Authentication-server configureren om met AD FS 2.0 te werken
Dit artikel is voor organisaties die zijn gefedereerd met Azure Active Directory en resources die on-premises of in de cloud zijn, willen beveiligen. Beveilig uw resources met de Azure Multi-Factor Authentication-server en configureer de service zodanig dat deze met AD FS werkt en verificatie in twee stappen wordt geactiveerd voor waardevolle eindpunten.

In deze documentatie wordt beschreven hoe u de Azure Multi-Factor Authentication-server gebruikt met AD FS 2.0. Voor meer informatie over AD FS raadpleegt u [Uw cloudresources en on-premises resources beveiligen met behulp van de Azure Multi-Factor Authentication-server met AD FS in Windows Server 2012 R2](multi-factor-authentication-get-started-adfs-w2k12.md).

## <a name="secure-ad-fs-20-with-a-proxy"></a>AD FS 2.0 beveiligen met een proxy
Als u AD FS 2.0 wilt beveiligen met een proxy, installeert u de Multi-Factor Authentication-server op de ADFS-proxyserver.

### <a name="configure-iis-authentication"></a>IIS-verificatie configureren
1. Klik op de Azure Multi-Factor Authentication-server in het menu links op het pictogram **IIS-verificatie**.
2. Klik op het tabblad **Op formulier gebaseerd**.
3. Klik op **Add**.

   <center>![Instellen](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>

4. Als u de gebruikersnaam, het wachtwoord en de domeinvariabelen automatisch wilt detecteren, voert u de aanmeldings-URL (bijvoorbeeld https://sso.contoso.com/adfs/ls) in het dialoogvenster Formulier-gebaseerde website automatisch configureren in en klikt u op **OK**.
5. Schakel het selectievakje **Overeenkomende Azure Multi-Factor Authentication-gebruiker** vereisen in als alle gebruikers zijn of moeten worden geïmporteerd in de server en aan verificatie in twee stappen onderworpen zijn. Als een groot aantal gebruikers nog niet is geïmporteerd in de Azure Multi-Factor Authentication-server en/of vrijgesteld zal zijn van verificatie in twee stappen, laat u het vakje uitgeschakeld.
6. Als de paginavariabelen niet automatisch kunnen worden gedetecteerd, klikt u op **Handmatig opgeven...** in het dialoogvenster Formulier-gebaseerde website automatisch configureren.
7. Voer in het dialoogvenster Formulier-gebaseerde website toevoegen in het veld Indienings-URL de URL naar de aanmeldingspagina van AD FS in (zoals https://sso.contoso.com/adfs/ls) en voer een toepassingsnaam in (optioneel). De naam van de toepassing wordt vermeld in Azure Multi-Factor Authentication-rapporten en kan worden weergegeven in verificatieberichten via sms of mobiele apps.
8. Stel de indeling van de aanvraag in op **POST of GET**.
9. Voer de gebruikersnaamvariabele (ctl00$ContentPlaceHolder1$UsernameTextBox) en wachtwoordvariabele (ctl00$ContentPlaceHolder1$PasswordTextBox) in. Als uw formulier-gebaseerde aanmeldingspagina een tekstvak voor een domein bevat, voert u de domeinvariabele in. Ga in een webbrowser naar de aanmeldingspagina, klik met de rechtermuisknop op de pagina en selecteer **Bron weergeven** om de namen van de invoervakken op de aanmeldingspagina te vinden.
10. Schakel het selectievakje **Overeenkomende Azure Multi-Factor Authentication-gebruiker** vereisen in als alle gebruikers zijn of moeten worden geïmporteerd in de server en aan verificatie in twee stappen onderworpen zijn. Als een groot aantal gebruikers nog niet is geïmporteerd in de Azure Multi-Factor Authentication-server en/of vrijgesteld zal zijn van verificatie in twee stappen, laat u het vakje uitgeschakeld.
    <center>![Instellen](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klik op **Geavanceerd...** om de geavanceerde instellingen te controleren. U kunt onder meer de volgende instellingen configureren:

    - Een aangepast bestand voor de weigeringspagina selecteren
    - Succesvolle verificaties voor de website in de cache opslaan met behulp van cookies
    - Selecteren hoe u de primaire referenties wilt verifiëren

12. Aangezien de AD FS-proxyserver waarschijnlijk niet aan het domein zal worden gekoppeld, kunt u LDAP gebruiken om verbinding te maken met de domeincontroller voor het importeren van gebruikers en pre-authenticatie. Klik in het dialoogvenster Geavanceerde op formulier-gebaseerde website op het tabblad **Primaire authenticatie** en selecteer **LDAP-binding** voor het authenticatietype Pre-authenticatie.
13. Als u klaar bent, klikt u op **OK** om terug te gaan naar het dialoogvenster Formulier-gebaseerde website.
14. Klik op **OK** om het dialoogvenster te sluiten.
15. Zodra de URL- en paginavariabelen zijn gedetecteerd of ingevoerd, worden de websitegegevens weergegeven in het paneel Op formulier gebaseerd.
16. Klik op het tabblad **Systeemeigen module** en selecteer de server, de website waaronder de AD FS-proxy wordt uitgevoerd (zoals 'Standaardwebsite') of de AD FS-proxytoepassing (zoals 'ls' onder 'adfs') om de IIS-invoegtoepassing op het gewenste niveau in te schakelen.
17. Klik op het vak **IIS-verificatie** inschakelen aan de bovenkant van het scherm.

De IIS-authenticatie is nu ingeschakeld.

### <a name="configure-directory-integration"></a>Adreslijstintegratie configureren
U hebt IIS-authenticatie ingeschakeld, maar om de pre-authenticatie voor uw Active Directory (AD) via LDAP uit te voeren, moet u de LDAP-verbinding configureren met de domeincontroller.

1. Kik op het pictogram **Adreslijstintegratie**.
2. Schakel op het tabblad Instellingen het keuzerondje **Specifieke LDAP-configuratie** gebruiken in.

   <center>![Instellen](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>

3. Klik op **Bewerken**.
4. Vul in de velden van het dialoogvenster LDAP-configuratie bewerken de benodigde informatie in om verbinding te maken met de AD-domeincontroller. Beschrijvingen van de velden zijn ook beschikbaar in het Help-bestand voor de Azure Multi-Factor Authentication-server.
5. Test de LDAP-verbinding door te klikken op de knop **Testen**.

   <center>![Instellen](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>

6. Als de LDAP-verbindingstest is geslaagd, klikt u op **OK**.

### <a name="configure-company-settings"></a>Instellingen van het bedrijf configureren
1. Daarna klikt u op het pictogram **Bedrijfsinstellingen** en selecteert u het tabblad **Gebruikersnaamomzetting**.
2. Selecteer het keuzerondje **Het kenmerk Unieke LDAP-id gebruiken voor overeenkomende gebruikersnamen**.
3. Als gebruikers hun gebruikersnaam invoeren in de opmaak ‘domein\gebruikersnaam’, moet de server het domein van de gebruikersnaam kunnen verwijderen bij het maken van de LDAP-query. Dat kan worden gedaan met behulp van een registerinstelling.
4. Open de Register-editor en ga op een 64-bits-server naar HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/Positive Networks/PhoneFactor. Op een 32-bits-server verwijdert u het gedeelte 'Wow6432Node' uit het pad. Maak een DWORD-registersleutel met de naam UsernameCxz_stripPrefixDomain en stel de waarde in op 1. De AD FS-proxy wordt nu beveiligd met Azure Multi-Factor Authentication.

Zorg ervoor dat gebruikers uit Active Directory in de server zijn geïmporteerd. Zie de [sectie Goedgekeurde IP-adressen](#trusted-ips) als u interne IP-adressen op een lijst met toegestane IP-adressen wilt plaatsen, zodat verificatie in twee stappen niet is vereist bij het aanmelden bij de website vanaf die locaties.

<center>![Instellen](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 Direct zonder een proxy
U kunt AD FS beveiligen wanneer de AD FS-proxy niet wordt gebruikt. Installeer de Multi-Factor Authentication-server op de ADFS-server en configureer de server door de volgende stappen uit te voeren:

1. Klik in de Azure Multi-Factor Authentication-server in het menu links op het pictogram **IIS-verificatie**.
2. Klik op het tabblad **HTTP**.
3. Klik op **Add**.
4. Voer in het dialoogvenster Basis-URL toevoegen in het veld Basis-URL de URL in voor de AD FS-website waar de HTTP-verificatie wordt uitgevoerd (zoals https://sso.domain.com/adfs/ls/auth/integrated). Voer dan een toepassingsnaam in (optioneel). De naam van de toepassing wordt vermeld in Azure Multi-Factor Authentication-rapporten en kan worden weergegeven in verificatieberichten via sms of mobiele apps.
5. Pas, indien gewenst, de tijd voor Time-out voor inactiviteit en voor Maximale sessie aan.
6. Schakel het selectievakje **Overeenkomende Azure Multi-Factor Authentication-gebruiker** vereisen in als alle gebruikers zijn of moeten worden geïmporteerd in de server en aan verificatie in twee stappen onderworpen zijn. Als een groot aantal gebruikers nog niet is geïmporteerd in de Azure Multi-Factor Authentication-server en/of vrijgesteld zal zijn van verificatie in twee stappen, laat u het vakje uitgeschakeld.
7. Schakel, indien gewenst, het selectievakje Cookie gebruiken om voltooide authenticaties in de cache op te slaan in.

   <center>![Instellen](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>

8. Klik op **OK**.
9. Klik op het tabblad **Systeemeigen module** en selecteer de server, de website (zoals 'Standaardwebsite') of de AD FS-toepassing (bijvoorbeeld 'ls' onder 'adfs') om de IIS-invoegtoepassing op het gewenste niveau in te schakelen.
10. Klik op het vak **IIS-verificatie** inschakelen aan de bovenkant van het scherm.

AD FS wordt nu beveiligd met Azure Multi-Factor Authentication.

Zorg ervoor dat gebruikers uit Active Directory in de server zijn geïmporteerd. Zie de sectie Goedgekeurde IP-adressen als u interne IP-adressen op een lijst met toegestane IP-adressen wilt plaatsen, zodat verificatie in twee stappen niet is vereist bij het aanmelden bij de website vanaf die locaties.

## <a name="trusted-ips"></a>Goedgekeurde IP-adressen
De goedgekeurde IP-adressen bieden gebruikers de mogelijkheid om Azure Multi-Factor Authentication over te slaan voor websiteverzoeken die afkomstig zijn van bepaalde IP-adressen of subnetten. Als u bijvoorbeeld gebruikers wilt uitsluiten van de verificatie in twee stappen wanneer ze zich aanmelden vanaf het kantoor. Hiervoor geeft u het subnet van de werkplek op als een goedgekeurd IP-adres.

### <a name="to-configure-trusted-ips"></a>Goedgekeurde IP-adressen configureren
1. Klik op het tabblad **Goedgekeurde IP-adressen** in de sectie IIS-verificatie.
2. Klik op de knop **Toevoegen...** te klikken.
3. Wanneer het dialoogvenster Goedgekeurd IP-adres toevoegen wordt weergegeven, selecteert u het keuzerondje **Eén IP-adres**, **IP-bereik** of **Subnet**.
4. Voer het IP-adres, het bereik van IP-adressen of het subnet in dat u op de lijst van goedgekeurde IP-adressen wilt plaatsen. Als u een subnet wilt invoeren, selecteert u het geschikte Netmasker en klikt u op de knop **OK**. Het goedgekeurde IP-adres is nu toegevoegd.

<center>![Instellen](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
