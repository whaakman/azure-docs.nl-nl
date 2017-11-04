---
title: VPN-integratie met Azure MFA met NPS extensie | Microsoft Docs
description: Dit artikel wordt de VPN-infrastructuur integreren met Azure MFA met behulp van de Network Policy Server (NPS)-uitbreiding voor Microsoft Azure.
services: active-directory
keywords: Azure MFA integreren van VPN-, Azure Active Directory, uitbreiding van Network Policy Server
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 8ac4c5d88e724febf21fe6bcc8ecf939283f361e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="integrate-your-vpn-infrastructure-with-azure-multi-factor-authentication-mfa-using-the-network-policy-server-nps-extension-for-azure"></a>Uw VPN-infrastructuur integreren met Azure multi-factor Authentication (MFA) met de Network Policy Server (NPS)-extensie voor Azure

## <a name="overview"></a>Overzicht

De Network Policy Server (NPS)-extensie voor Azure, waarmee organisaties beschermen Remote Authentication Dial-In User Service (RADIUS)-clientauthenticatie met behulp van cloud-gebaseerde [Azure multi-factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md), waarmee u verificatie in twee stappen.

Dit artikel bevat instructies voor het integreren van de NPS-infrastructuur met Azure MFA met behulp van de NPS-extensie voor Azure verificatie van de beveiligde in twee stappen voor gebruikers die verbinding maken met uw netwerk via een VPN wilt inschakelen. 

De Network Policy and Access Services (NPS) biedt organisaties de volgende mogelijkheden:

* Geef de centrale locatie voor het beheer en controle van netwerkaanvragen om op te geven die verbinding kunnen maken, welke tijdstippen verbindingen zijn toegestaan, de duur van verbindingen en het niveau van beveiliging die clients moeten gebruiken om verbinding te, enzovoort. In plaats van deze beleidsregels op elke server VPN of extern bureaublad (RD) Gateway opgeeft, kunnen deze beleidsregels één keer worden opgegeven in een centrale locatie. De RADIUS-protocol wordt gebruikt voor de centrale verificatie, autorisatie en Accounting (AAA). 
* Instellen en afdwingen van statusbeleid voor Network Access Protection (NAP) client om te bepalen of apparaten onbeperkte of beperkte toegang krijgen tot netwerkbronnen.
* De mogelijkheid om af te dwingen, verificatie en autorisatie voor toegang op 802. 1 x-compatibele draadloze toegangspunten en Ethernet-switches.    

Zie voor meer informatie [Network Policy Server (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

Voor betere beveiliging en bieden hoge mate van naleving, organisaties kunnen worden geïntegreerd NPS met Azure MFA om ervoor te zorgen dat gebruikers verificatie in twee stappen gebruiken om te kunnen verbinding maken met de virtuele poort op de VPN-server. Voor gebruikers toegang te krijgen, moet deze de combinatie van gebruikersnaam en wachtwoord met informatie die de gebruiker in het besturingselement heeft opgeven. Deze informatie moet worden vertrouwd en eenvoudig niet wordt gedupliceerd, zoals een mobiele telefoonnummer, een vast, een toepassing op een mobiel apparaat, enzovoort.

Voorafgaand aan de beschikbaarheid van de NPS-extensie voor Azure moest-klanten die willen implementeren verificatie in twee stappen voor geïntegreerde NPS en Azure MFA-omgevingen configureren en onderhouden van een afzonderlijke MFA-Server in de on-premises-omgeving, zoals beschreven in Extern bureaublad-Gateway en Azure multi-factor Authentication-Server met behulp van RADIUS.

De beschikbaarheid van de NPS-extensie voor Azure biedt organisaties nu kiezen voor het implementeren van een on-premises op basis van MFA oplossing of een MFA-cloud-gebaseerde oplossing voor veilige verificatie van de RADIUS-client.
 
## <a name="authentication-flow"></a>Verificatiestroom
Wanneer een gebruiker verbinding met een virtuele poort op een VPN-server maakt, moeten ze eerst verifiëren met behulp van verschillende protocollen, die het gebruik van een combinatie van gebruikersnaam en wachtwoord en de verificatiemethoden op basis van certificaten toestaan. 

Naast de authenticatie en identiteit te controleren, moeten gebruikers de juiste machtigingen inbellen hebben. In implementaties van eenvoudige, inbellen machtigingen toegewezen die toegang toestaan rechtstreeks op de Active Directory-gebruikersobjecten ingesteld. 

 ![Eigenschappen van de gebruiker](./media/nps-extension-vpn/image1.png)

Elke VPN-server verleent of weigert op basis van beleid gedefinieerd voor elke lokale VPN-server voor eenvoudige implementaties.

In implementaties van grotere en meer schaalbare, de beleidsregels die verlenen of weigeren van de VPN-toegang op RADIUS-servers worden gecentraliseerd. In dit geval wordt fungeert de VPN-server als een access-server (RADIUS-client) die verbindingsaanvragen en account berichten naar een RADIUS-server stuurt. Maak verbinding met de virtuele poort op de VPN-server door gebruikers moeten worden geverifieerd en voldoen aan de voorwaarden die centraal gedefinieerd op RADIUS-servers. 

Wanneer de NPS-extensie voor Azure is geïntegreerd met de NPS, is de geslaagde authenticatiestroom als volgt:

1. De VPN-server ontvangt een verificatieaanvraag van een VPN-gebruiker met de gebruikersnaam en wachtwoord verbinding maken met een resource, zoals een extern bureaublad-sessie. 
2. Fungeert als een RADIUS-client, VPN-server de aanvraag converteert naar een RADIUS-Access-Request-bericht en verzendt het bericht (wachtwoord wordt gecodeerd) naar de RADIUS-(NPS)-server waarop de NPS-uitbreiding is geïnstalleerd. 
3. De combinatie van gebruikersnaam en wachtwoord wordt geverifieerd in Active Directory. Als de gebruikersnaam / wachtwoord onjuist is, stuurt de RADIUS-Server een bericht toegang weigeren. 
4. Indien wordt voldaan aan alle voorwaarden zoals opgegeven in de NPS-verbindingsaanvraag en netwerkbeleid (bijvoorbeeld de tijd van de dag of groep lidmaatschap beperkingen), de NPS-extensie activeert een aanvraag voor de secundaire verificatie met Azure MFA. 
5. Azure MFA communiceert met Azure Active Directory, haalt de details van de gebruiker en de secundaire authenticatie via de methode die is geconfigureerd door de gebruiker (SMS-bericht, mobiele app, enzovoort) wordt uitgevoerd. 
6. Bij voltooiing van de uitdaging MFA communiceert Azure MFA het resultaat naar de NPS-extensie.
7. Nadat de verbindingspoging is geverifieerd en geautoriseerd, verzendt de NPS-server waarop de uitbreiding is geïnstalleerd een RADIUS-Access-Accept-bericht met de VPN-server (RADIUS-client).
8. De gebruiker krijgt toegang tot de virtuele poort op VPN-server en er wordt een versleutelde VPN-tunnel.

## <a name="prerequisites"></a>Vereisten
In deze sectie beschrijft de vereisten die nodig zijn voordat de integratie van Azure MFA met extern bureaublad-Gateway. Voordat u begint, moet u de volgende vereisten hebben voldaan.

* VPN-infrastructuur
* De rol en Network Policy and Access Services (NPS)
* Azure MFA-licentie
* Windows Server-software
* Bibliotheken
* Azure AD gesynchroniseerd met on-premises AD dat 
* GUID-ID van Azure Active Directory

### <a name="vpn-infrastructure"></a>VPN-infrastructuur
In dit artikel wordt ervan uitgegaan dat u hebt een werkende VPN-infrastructuur met behulp van Microsoft Windows Server 2016 aanwezig en de VPN-server is momenteel niet geconfigureerd voor verbindingsaanvragen wilt doorsturen naar een RADIUS-server. U configureert de VPN-infrastructuur voor het gebruik van een centrale RADIUS-server in deze handleiding.

Als u geen een werkende-infrastructuur aanwezig zijn, kunt u deze infrastructuur snel maken door de richtlijnen in talloze zelfstudies met VPN-installatie die u op de Microsoft en derden sites vinden kunt te volgen. 

### <a name="network-policy-and-access-services-nps-role"></a>De rol en Network Policy and Access Services (NPS)

De functieservice NPS biedt de RADIUS-server en clientfunctionaliteit. In dit artikel wordt ervan uitgegaan dat u hebt de NPS-rol geïnstalleerd op een lidserver of domeincontroller in uw omgeving. RADIUS configureert u voor een VPN-configuratie in deze handleiding. De NPS-rolservice installeert op een server _andere_ dan de VPN-server.

Voor informatie over het installeren van de NPS-rolservice service Windows Server 2012 of hoger, Zie [installeren van een NAP-statusbeleidsserver](https://technet.microsoft.com/library/dd296890.aspx). Beleid voor NAP (Network Access) is afgeschaft in Windows Server 2016. Zie voor een beschrijving van best practices voor NPS, met inbegrip van de aanbeveling om NPS installeert op een domeincontroller [Best Practices voor NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-mfa-license"></a>Azure MFA-licentie

Vereist, is een licentie voor Azure MFA, die beschikbaar via een Azure AD Premium, Enterprise Mobility plus Security (EMS) of een MFA-abonnement is. Zie voor meer informatie [het ophalen van Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md). Voor testdoeleinden kunt u een proefabonnement.

### <a name="windows-server-software"></a>Windows Server-software

De NPS-uitbreiding vereist Windows Server 2008 R2 SP1 of hoger als de NPS-functieservice die is geïnstalleerd. De stappen in deze handleiding zijn uitgevoerd met behulp van Windows Server 2016.

### <a name="libraries"></a>Bibliotheken

Deze bibliotheken worden automatisch geïnstalleerd met de extensie.

-   [Visual C++ Redistributable Packages for Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory-Module voor Windows PowerShell versie 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

De Microsoft Azure Active Directory-Module voor Windows PowerShell is geïnstalleerd, als deze nog niet aanwezig is, via een configuratiescript die u als onderdeel van het installatieproces uitvoert. Er is niet nodig voor het installeren van deze module tevoren als deze nog niet is geïnstalleerd.

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory worden gesynchroniseerd met de lokale Active Directory 

Voor het gebruik van de NPS-extensie, zijn on-premises gebruikers gesynchroniseerd met Azure Active Directory en ingeschakeld voor multi-factor Authentication. Deze handleiding wordt ervan uitgegaan dat de on-premises gebruikers zijn gesynchroniseerd met Azure Active Directory met AD Connect. Hieronder vindt u instructies voor het inschakelen van gebruikers voor MFA.
Voor informatie over Azure AD connect, Zie [uw on-premises adreslijsten integreren met Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>GUID-ID van Azure Active Directory 
U moet weten de GUID van de Azure Active Directory voor het installeren van de NPS-extensie. Instructies voor het vinden van de GUID van de Azure Active Directory zijn beschikbaar in de volgende sectie.

## <a name="configure-radius-for-vpn-connections"></a>RADIUS voor VPN-verbindingen configureren

Als u de functie NPS-server hebt geïnstalleerd op een lidserver, moet u configureren om te verifiëren en autoriseren van VPN-client die aanvraag VPN-verbindingen. 

Deze sectie wordt ervan uitgegaan dat u de functie Network Policy Server hebt geïnstalleerd, maar hebben geen deze heeft geconfigureerd voor gebruik in uw infrastructuur.

>[!NOTE]
>Als u al een werkende VPN-server die gebruikmaakt van een gecentraliseerde RADIUS-server voor verificatie, kunt u deze sectie overslaan.
>

### <a name="register-server-in-active-directory"></a>Server registreren in Active Directory
De NPS-server moet worden geregistreerd in Active Directory een correcte werking in dit scenario.

1. Open Serverbeheer.
2. Klik in Serverbeheer op **extra**, en klik vervolgens op **Network Policy Server**. 
3. In de console Network Policy Server met de rechtermuisknop op **NPS (lokaal)**, en klik vervolgens op **server registreren in Active Directory**. Klik op **OK** twee keer.

 ![Network Policy Server](./media/nps-extension-vpn/image2.png)

4. Laat de console geopend voor de volgende procedure.

### <a name="use-wizard-to-configure-radius-server"></a>Gebruik de wizard RADIUS-server configureren
U kunt een standaard (door de wizard gebaseerd) of geavanceerde configuratieoptie voor het configureren van de RADIUS-server. Deze sectie wordt ervan uitgegaan dat het gebruik van de standaardconfiguratie op basis van een wizard-optie.

1. Klik in de console Network Policy Server **NPS (lokaal)**.
2. Selecteer onder configuratie van de standaard **RADIUS-Server voor inbel- of VPN-verbindingen**, en klik vervolgens op **configureren van VPN- of inbelverbindingen**.

 ![VPN configureren](./media/nps-extension-vpn/image3.png)

3. Selecteer op de optie inbel- of virtuele particuliere netwerk verbindingen Type pagina **VPN-verbindingen**, en klik op **volgende**.

 ![Virtueel particulier netwerk](./media/nps-extension-vpn/image4.png)

4. Klik op de pagina opgeven inbel- of VPN-Server op **toevoegen**.
5. In de **nieuwe RADIUS-client** in het dialoogvenster een beschrijvende naam opgeven, voer de naam of IP-adres van de VPN-server en een gedeeld geheim wachtwoord invoeren. Deze gedeelde geheime wachtwoord lange en complexe maken. Als u dit nodig voor de stappen in de volgende sectie, noteert u dit wachtwoord.

 ![Nieuwe RADIUS-Client](./media/nps-extension-vpn/image5.png)

6. Klik op **OK**, en vervolgens **volgende**.
7. Op de **verificatiemethoden configureren** pagina, accepteer de standaardinstelling (Microsoft Encrypted Authentication versie 2 (MS-CHAPv2) of kies een andere optie en klik op **volgende**.

  >[!NOTE]
  >Als u Extensible Authentication Protocol (EAP) configureert, moet u MS-CHAPv2 of PEAP. Er zijn geen andere EAP wordt ondersteund.
 
8. Klik op de pagina gebruikersgroepen opgeven **toevoegen** en selecteer een geschikte groep, indien aanwezig. Anders laat u de selectie leeg om toegang te verlenen voor alle gebruikers.

 ![Gebruikersgroepen opgeven](./media/nps-extension-vpn/image7.png)

9. Klik op **Volgende**.
10. Klik op de pagina IP-Filters opgeven **volgende**.
11. Accepteer de standaardinstellingen op de pagina versleutelingsinstellingen opgeven en klik op **volgende**.

 ![Versleuteling opgeven](./media/nps-extension-vpn/image8.png)

12. Geef de naam van een Realm laat op de naam leeg, accepteer de standaardinstelling en klik op **volgende**.

 ![Een Realm opgeven](./media/nps-extension-vpn/image9.png)

13. Klik op de nieuwe voltooien inbel- of de pagina voor VPN-verbindingen en RADIUS-clients, **voltooien**.

 ![Verbindingen voltooien](./media/nps-extension-vpn/image10.png)

### <a name="verify-radius-configuration"></a>Controleer of de RADIUS-configuratie
Deze sectie beschrijft de configuratie die u hebt gemaakt met de wizard.

1. Vouw op de NPS-server in de console NPS (lokaal), RADIUS-Clients en selecteer **RADIUS-Clients**.
2. In het detailvenster met de rechtermuisknop op de RADIUS-client die u hebt gemaakt met de wizard en klikt u op **eigenschappen**. De eigenschappen voor uw RADIUS-client (de VPN-server) moet overeenkomen met die hieronder wordt weergegeven.

 ![VPN-eigenschappen](./media/nps-extension-vpn/image11.png)

3. Klik op **annuleren**.
4. Vouw op de NPS-server in de console NPS (lokaal) **beleid**, en selecteer **beleid voor verbindingsaanvragen**. Hier ziet u het beleid voor VPN-verbindingen dat lijkt op de onderstaande afbeelding.

 ![Verbindingsaanvragen](./media/nps-extension-vpn/image12.png)

5. Selecteer onder beleidsregels, **netwerkbeleid**. U moet een beleid met virtuele particuliere netwerk (VPN)-verbindingen dat lijkt op de onderstaande afbeelding.

 ![Eigenschappen van het netwerk](./media/nps-extension-vpn/image13.png)

## <a name="configure-vpn-server-to-use-radius-authentication"></a>VPN-Server voor RADIUS-verificatie configureren
In deze sectie configureert u de VPN-server voor RADIUS-verificatie. Deze sectie wordt ervan uitgegaan dat u een werkende configuratie van VPN-server hebt, maar de VPN-server voor RADIUS-verificatie niet hebt geconfigureerd. Nadat de VPN-server zijn geconfigureerd, kunt u bevestigen dat de configuratie werkt zoals verwacht.

>[!NOTE]
>Als u VPN-serverconfiguratie die gebruikmaakt van RADIUS-verificatie hebt, kunt u deze sectie overslaan.
>

### <a name="configure-authentication-provider"></a>Verificatieprovider configureren
1. Open Serverbeheer op de VPN-server.
2. Klik in Serverbeheer op **extra**, en vervolgens **Routing and Remote Access**.
3. In de Routing and Remote Access-console met de rechtermuisknop op  **\[servernaam\] (lokaal)**, en klik vervolgens op **eigenschappen**.

 ![Routering en RAS](./media/nps-extension-vpn/image14.png)
 
4. In de **[servernaam} (lokaal) eigenschappen** in het dialoogvenster, klikt u op de **beveiliging** tabblad. 
5. Op de **beveiliging** tabblad onder verificatieprovider, klikt u op **RADIUS-verificatie**, en vervolgens **configureren**.

 ![RADIUS-verificatie](./media/nps-extension-vpn/image15.png)
 
6. Klik in het dialoogvenster RADIUS-verificatie op **toevoegen**.
7. Voeg in de RADIUS-Server toevoegen, in de servernaam, de naam of het IP-adres van de RADIUS-server die u hebt geconfigureerd in de vorige sectie.
8. Klik in het gedeeld geheim op **wijziging** en voeg het gedeelde geheim wachtwoord die u hebt gemaakt en u eerder hebt genoteerd.
9. De waarde in het vak Time-out (seconden) wijzigen in een waarde tussen **30** en **60**. Dit is nodig om voldoende tijd is voor het voltooien van de tweede verificatiefactor toestaan.
 
 ![RADIUS-Server toevoegen](./media/nps-extension-vpn/image16.png)
 
10. Klik op **OK** totdat u alle dialoogvensters sluiten.

### <a name="test-vpn-connectivity"></a>VPN-verbindingen testen
In deze sectie kunt bevestigen u dat de VPN-client is geverifieerd en gemachtigd door de RADIUS-server wanneer u probeert verbinding maken met VPN-virtuele poort. Deze sectie wordt ervan uitgegaan dat u gebruikmaakt van Windows 10 als een VPN-client. 

>[!NOTE]
>Als u al een VPN-client om verbinding maken met de VPN-server en de instellingen hebt opgeslagen hebt geconfigureerd, kunt u de stappen voor het configureren en opslaan van een VPN-verbindingsobject overslaan.
>

1. Klik op de VPN-client-computer **Start**, en vervolgens **instellingen** (tandwielpictogram pictogram).
2. Klik in de instellingen van venster **netwerk en Internet**.
3. Klik op **VPN**.
4. Klik op **een VPN-verbinding toevoegen**.
5. In een VPN-verbinding toevoegen Windows (ingebouwd) als de VPN-provider opgeven en vervolgens de resterende velden naar gelang van toepassing, en op **opslaan**. 

 ![VPN-verbinding toevoegen](./media/nps-extension-vpn/image17.png)
 
6. Open de **Netwerkcentrum** in het Configuratiescherm.
7. Klik op **Adapterinstellingen wijzigen**.

 ![Adapterinstellingen wijzigen](./media/nps-extension-vpn/image18.png)

8. Met de rechtermuisknop op het netwerk VPN-verbinding en klikt u op Eigenschappen. 

 ![Eigenschappen van VPN-netwerk](./media/nps-extension-vpn/image19.png)

9. Klik in het dialoogvenster van VPN-eigenschappen op de **beveiliging** tabblad. 
10. Zorg ervoor dat alleen op het tabblad Beveiliging **Microsoft CHAP Version 2 (MS-CHAP v2)** is geselecteerd en klik op OK.

 ![Protocollen toestaan](./media/nps-extension-vpn/image20.png)

11. Met de rechtermuisknop op de VPN-verbinding en klikt u op **Connect**.
12. Klik op de pagina instellingen **Connect**.

Een geslaagde verbinding wordt weergegeven in het beveiligingslogboek op RADIUS-server als gebeurtenis-ID 6272, zoals hieronder wordt weergegeven.

 ![Eigenschappen van gebeurtenis](./media/nps-extension-vpn/image21.png)

## <a name="troubleshoot-guide"></a>Problemen met de gids
Wordt ervan uitgegaan dat uw VPN-configuratie werkte voordat u de VPN-server geconfigureerd voor het gebruik van een gecentraliseerde RADIUS-server voor verificatie en autorisatie. In dit geval is het waarschijnlijk dat het probleem kan worden veroorzaakt door een onjuiste configuratie van de RADIUS-Server of het gebruik van een ongeldige gebruikersnaam of wachtwoord. Bijvoorbeeld, als u de alternatieve UPN-achtervoegsel in de gebruikersnaam gebruikt, de aanmeldingspoging mislukken (u moet dezelfde accountnaam voor de beste resultaten gebruiken). 

Raadpleeg de gebeurtenislogboeken voor beveiliging op de RADIUS-server is om deze problemen te een ideaal plaats om te beginnen. Als u wilt opslaan op tijd zoeken naar gebeurtenissen, kunt u de op rollen gebaseerde Network Policy and Access Server aangepaste weergave in Logboeken, zoals u hierna ziet. Gebeurtenis-ID 6273 geeft aan waar de Network Policy Server toegang geweigerd voor een gebruiker gebeurtenissen. 

 ![Logboeken](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Multi-factor Authentication configureren
Deze sectie geeft instructies voor het inschakelen van gebruikers voor MFA en voor het instellen van accounts voor verificatie in twee stappen. 

### <a name="enable-multi-factor-authentication"></a>Multi-Factor Authentication inschakelen
In deze sectie schakelt u Azure AD-accounts voor MFA. Gebruik de **klassieke portal** zodat gebruikers voor MFA. 

1. Open een browser en Ga naar [https://manage.windowsazure.com](https://manage.windowsazure.com). 
2. Meld u als beheerder.
3. Klik in de portal in de navigatiebalk links op **ACTIVE DIRECTORY**.

 ![Standaard-map](./media/nps-extension-vpn/image23.png)

4. Klik in de kolom naam **standaarddirectory** (of een andere map, indien van toepassing).
5. Klik op de pagina Quick Start op **configureren**.

 ![Standaard configureren](./media/nps-extension-vpn/image24.png)

6. Op de pagina configureren Schuif naar beneden en klik in de sectie multi-factor authentication op **service-instellingen beheren**.

 ![MFA-instellingen beheren](./media/nps-extension-vpn/image25.png)
 
7. Controleer de standaardinstellingen van de service op de pagina multi-factor authentication-server en klik vervolgens op **gebruikers**. 

 ![MFA-gebruikers](./media/nps-extension-vpn/image26.png)
 
8. Selecteer de gebruikers die u wilt inschakelen voor MFA en klik op de pagina gebruikers **inschakelen**.

 ![Eigenschappen](./media/nps-extension-vpn/image27.png)
 
9. Wanneer u wordt gevraagd, klikt u op **multi-factor auth inschakelen**.

 ![Schakel MFA in](./media/nps-extension-vpn/image28.png)
 
10. Klik op **Sluiten**. 
11. Vernieuw de pagina. De MFA-status wordt op ingeschakeld gewijzigd.

Zie voor meer informatie over het inschakelen van gebruikers voor multi-factor Authentication [aan de slag met Azure multi-factor Authentication in de cloud](multi-factor-authentication-get-started-cloud.md). 

### <a name="configure-accounts-for-two-step-verification"></a>Accounts configureren voor verificatie in twee stappen
Wanneer een account is ingeschakeld voor MFA, zich gebruikers niet aanmelden bij resources beheerst door het MFA-beleid totdat ze een vertrouwd apparaat moet worden gebruikt voor de tweede verificatiefactor gelet gebruikt verificatie in twee stappen hebt geconfigureerd.

In deze sectie configureert u een vertrouwd apparaat voor gebruik met verificatie in twee stappen. Er zijn diverse opties beschikbaar voor u deze opties, waaronder de volgende configureren:

* **Mobiele app**. U kunt de Microsoft Authenticator-app installeren op een Windows Phone, Android of iOS-apparaat. Afhankelijk van uw organisatie beleid, moet u de app gebruiken in een van de twee modi: meldingen ontvangen voor verificaties (een gestuurd naar het apparaat) of verificatiecode (u zijn vereist voor het invoeren van een bevestigingscode die updates gebruiken elke 30 seconden). 
* **Mobiele telefoon telefoongesprek of tekstbericht**. U kunt ofwel een automatisch telefoongesprek of SMS-bericht ontvangen. Met de optie telefoonoproep beantwoordt het gesprek en drukt u op het teken # om te verifiëren. Met de optie tekst kunt u beantwoord het SMS-bericht of Voer de verificatiecode in de interface voor aanmelden.
* **Office-telefoongesprek**. Dit proces is hetzelfde als die voor geautomatiseerde telefoongesprekken hierboven beschreven.

Volg deze instructies voor het instellen van een apparaat voor de mobiele app gebruiken voor het ontvangen van pushmeldingen voor verificatie.

1. Meld u aan bij [https://aka.ms/mfasetup](https://aka.ms/mfasetup) of een site, zoals [https://portal.azure.com](https://portal.azure.com), waar u vereist verificatie met uw referenties MFA-functionaliteit. 
2. Bij het aanmelden met uw gebruikersnaam en wachtwoord, krijgt u een scherm waarin u voor het instellen van het account voor aanvullende beveiligingsverificatie wordt gevraagd.

 ![Extra beveiliging](./media/nps-extension-vpn/image29.png)

3. Klik op **het nu instellen**.
4. Selecteer op de pagina aanvullende beveiligingsinstellingen verificatie een contact op met type (telefoon voor authenticatie, zakelijke telefoonnummer of mobiele app). Vervolgens selecteert u een land of regio en selecteer een methode. De methode hangt af van de contactpersoon dat die u selecteert. Bijvoorbeeld als u mobiele app kiest, kunt u selecteren of meldingen voor verificatie moet gebruiken of een verificatiecode. Welke stappen volgen wordt ervan uitgegaan dat u kiest **mobiele app** als het type contactgegevens.

 ![Telefoon-verificatie](./media/nps-extension-vpn/image30.png)

5. Selecteer mobiele app, klik op **ontvangen van meldingen voor verificatie**, en vervolgens **instellen**. 

 ![Verificatie van de mobiele App](./media/nps-extension-vpn/image31.png)
 
6. Als u dit nog niet hebt gedaan, moet u de mobiele authenticator-app installeren op uw apparaat. 
7. Volg de instructies in de mobiele app scan de streepjescode gepresenteerd of de gegevens handmatig invoeren en klik vervolgens op **gedaan**.

 ![Mobiele App configureren](./media/nps-extension-vpn/image32.png)

8. Klik op de pagina aanvullende beveiligingsinstellingen verificatie **Contact met mij opnemen** en antwoorden van de melding verzonden naar je apparaat.
9. Voer een telefoonnummer op de pagina aanvullende beveiligingsinstellingen verificatie als u geen toegang meer tot de mobiele app en klik op **volgende**.

 ![Mobiele telefoonnummer](./media/nps-extension-vpn/image33.png)
 
10. Klik op de aanvullende beveiligingsverificatie **gedaan**.

Het apparaat is nu geconfigureerd voor het bieden van een tweede methode voor verificatie. Zie voor meer informatie over het instellen van accounts voor verificatie in twee stappen [Mijn account voor verificatie in twee stappen instellen](./end-user/multi-factor-authentication-end-user-first-time.md).

## <a name="install-and-configure-nps-extension"></a>Installeren en configureren van NPS-uitbreiding

Deze sectie geeft instructies voor het configureren van VPN Azure MFA gebruiken voor clientverificatie met de VPN-Server.

Nadat u installeren en configureren van de NPS-uitbreiding, worden alle RADIUS-clientverificatie die wordt verwerkt door deze server is vereist voor Azure MFA gebruiken. Als niet alle VPN-gebruikers zijn geregistreerd bij Azure MFA, kunt u een andere RADIUS-server instellen om te verifiëren van gebruikers die niet zijn geconfigureerd voor MFA gebruiken. Of u een registervermelding waarmee gebruikers met een handicap om een tweede verificatiefactor alleen als deze zijn ingeschreven in MFA kunt maken. 

Maak een nieuwe tekenreekswaarde met de naam _REQUIRE_USER_MATCH in HKLM\SOFTWARE\Microsoft\AzureMfa_, en stel de waarde waar of ONWAAR RETOURNEERT. 

 ![Gebruikersovereenkomst vereist](./media/nps-extension-vpn/image34.png)
 
Als de waarde is ingesteld op TRUE of niet is ingesteld, wordt alle verificatieaanvragen zijn onderworpen aan een challenge MFA. Als de waarde is ingesteld op FALSE, worden alleen voor gebruikers die zijn ingeschreven bij MFA MFA uitdagingen uitgegeven. Gebruik de FALSE instelling in het testen van of in productieomgevingen gedurende een periode onboarding alleen.

### <a name="acquire-azure-active-directory-guid-id"></a>ID van Azure Active Directory-GUID verkrijgen

Als onderdeel van de configuratie van de NPS-extensie moet u beheerdersreferenties en de Azure Active Directory-ID opgeven voor uw Azure AD-tenant. De onderstaande stappen ziet u het ophalen van de tenant-ID.

1. Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com) als globale beheerder van de Azure-tenant.
2. Klik in het linkernavigatievenster op het **Azure Active Directory** pictogram.
3. Klik op **Eigenschappen**.
4. Als uw map-ID naar het Klembord kopiëren, selecteert u de **kopie** pictogram.
 
 ![Map-ID](./media/nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>De NPS-uitbreiding installeren
De NPS-uitbreiding moet worden geïnstalleerd op een server met Network Policy and Access Services (NPS)-functie is geïnstalleerd en die functies als de RADIUS-server in uw ontwerp. Installeer de NPS-extensie niet op uw extern bureaublad-Server.

1. Download de NPS-extensie van [https://aka.ms/npsmfa](https://aka.ms/npsmfa). 
2. Kopieer het setup-bestand (NpsExtnForAzureMfaInstaller.exe) naar de NPS-server.
3. Dubbelklik op de NPS-server op **NpsExtnForAzureMfaInstaller.exe**. Als u wordt gevraagd, klikt u op **uitvoeren**.
4. Lees de licentievoorwaarden voor software in het dialoogvenster NPS-extensie voor Azure MFA Setup controleren **ik ga akkoord met de licentievoorwaarden en voorwaarden**, en klik op **installeren**.

 ![NPS-uitbreiding](./media/nps-extension-vpn/image36.png)
 
5. Klik in het dialoogvenster NPS-extensie voor Azure MFA Setup **sluiten**.  

 ![Setup is voltooid](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Configureren van certificaten voor gebruik met de NPS-extensie met een PowerShell-script
Beveiligde communicatie en zekerheid te garanderen, moet u om certificaten te configureren voor gebruik door de NPS-extensie. De NPS-onderdelen zijn Windows PowerShell-script waarmee een zelfondertekend certificaat voor gebruik met NPS worden geconfigureerd. 

Het script voert de volgende handelingen uit:

* Een zelfondertekend certificaat gemaakt
* Koppelt de openbare sleutel van certificaat voor service-principal over Azure AD
* Het certificaat dat is opgeslagen in het archief van de lokale computer
* Verleent toegang tot de persoonlijke sleutel van het certificaat voor de gebruiker van het netwerk
* Network Policy Server-service wordt opnieuw gestart

Als u wilt uw eigen certificaten te gebruiken, moet u de openbare sleutel van uw certificaat voor de service-principal over Azure AD te koppelen, enzovoort.
Voor het gebruik van het script bieden u de uitbreiding met uw beheerdersreferenties voor de Azure Active Directory en de Azure Active Directory-tenant-ID die u eerder hebt gekopieerd. Het script uitvoeren op elke NPS-server waarop u de extensie NPS installeert.

1. Open een administratieve Windows PowerShell-prompt.
2. Typ het volgende achter de PowerShell-prompt _cd 'c:\Program Files\Microsoft\AzureMfa\Config'_, en druk op **ENTER**.
3. Type _.\AzureMfsNpsExtnConfigSetup.ps1_, en druk op **ENTER**. 
 * Het script wordt gecontroleerd of de Azure Active Directory PowerShell-module is geïnstalleerd. Als dit niet is geïnstalleerd, installeert u de module met het script voor u.
 
 ![PowerShell](./media/nps-extension-vpn/image38.png)
 
4. Nadat het script wordt gecontroleerd door de installatie van de PowerShell-module, wordt het dialoogvenster Azure Active Directory PowerShell-module weergegeven. Voer in het dialoogvenster uw Azure AD-referenties en het wachtwoord in en klikt u op **aanmelden**. 
 
 ![PowerShell aanmelden](./media/nps-extension-vpn/image39.png)
 
5. Wanneer u wordt gevraagd, plak de tenant-ID die u eerder naar het Klembord hebt gekopieerd en druk op **ENTER**. 

 ![Tenant-id](./media/nps-extension-vpn/image40.png)

6. Het script maakt een zelfondertekend certificaat en andere configuratiewijzigingen uitvoert. De uitvoer lijkt op de afbeelding hieronder wordt weergegeven.

 ![Zelf-ondertekend certificaat](./media/nps-extension-vpn/image41.png)

7. De server opnieuw opgestart.
 
### <a name="verify-configuration"></a>Configuratie controleren
Om te controleren of de configuratie, moet u een nieuwe VPN-verbinding maken met de VPN-server. Nadat het is uw referenties invoeren voor primaire verificatie, wacht de VPN-verbinding voor de secundaire verificatie mislukt voordat de verbinding tot stand is gebracht, zoals hieronder wordt weergegeven. 

 ![Configuratie controleren](./media/nps-extension-vpn/image42.png)

Als u verificatie is met de secundaire verificatiemethode die u eerder hebt geconfigureerd in de Azure MFA, kunt u bent verbonden met de resource. Als de secundaire verificatie niet lukt, wordt u toegang tot de bron geweigerd. 

In het volgende voorbeeld wordt de verificator-app op een Windows phone gebruikt om de secundaire verificatie te bieden.

 ![Het Account verifiëren](./media/nps-extension-vpn/image43.png)

Nadat u bent geverifieerd met behulp van de secundaire methode, kunt u toegang tot de virtuele poort op de VPN-server zijn verleend. Omdat u vereist zijn voor het gebruik van een methode van de secundaire verificatie via een mobiele app op een vertrouwd apparaat, het logboek in proces is echter veiliger dan het zou doen met alleen een gebruikersnaam / wachtwoord combinatie.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Logboeken voor geslaagde aanmeldingsgebeurtenissen weergeven
Als u wilt de geslaagde aanmeldingsgebeurtenissen in Windows logboeken bekijken, kunt u de volgende Windows PowerShell-opdracht voor het beveiligingslogboek van Windows op de NPS-server een query uitgeven.

Gebruik de volgende opdracht om te vragen van geslaagde aanmeldingsgebeurtenissen worden geregistreerd in de logboeken van de beveiliging,
* _Get-WinEvent - logboeknaam beveiliging_ | waar {$_.ID - eq '6272'} | FL 

 ![Beveiliging-Logboeken](./media/nps-extension-vpn/image44.png)
 
U kunt ook het beveiligingslogboek of de Network Policy and Access Services aangepaste weergave, bekijken, zoals hieronder wordt weergegeven:

 ![Beleid voor toegang tot het netwerk](./media/nps-extension-vpn/image45.png)

Op de server waar u de NPS-extensie voor Azure MFA hebt geïnstalleerd, vindt u Event Viewer-logboeken specifiek zijn voor de uitbreiding op **toepassingen en Services Logs\Microsoft\AzureMfa**. 

* _Get-WinEvent - logboeknaam beveiliging_ | waar {$_.ID - eq '6272'} | FL

 ![Aantal gebeurtenissen](./media/nps-extension-vpn/image46.png)

## <a name="troubleshoot-guide"></a>Problemen met de gids
Als de configuratie niet werkt zoals verwacht, is een goede plaats om te starten om op te lossen om te controleren of de gebruiker is geconfigureerd voor het gebruik van Azure MFA. Een gebruiker verbinding maken met [https://portal.azure.com](https://portal.azure.com). Als deze wordt gevraagd om secundaire verificatie en kunnen worden geverifieerd, kunt u een onjuiste configuratie van Azure MFA elimineren.

Als u Azure MFA is voor de gebruiker (s) werkt, moet u de relevante gebeurtenislogboeken bekijken. Het gaat hierbij om de gebeurtenis voor beveiliging, operationele Gateway en Azure MFA-logboeken die worden beschreven in de vorige sectie. 

Hieronder volgt een voorbeeld van uitvoer van beveiligingslogboek met een mislukte aanmeldingsgebeurtenis (gebeurtenis-ID 6273):

 ![Beveiligingslogboek](./media/nps-extension-vpn/image47.png)

Hieronder vindt u een bijbehorende gebeurtenis uit de AzureMFA Logboeken:

 ![Azure MFA-Logboeken](./media/nps-extension-vpn/image48.png)

Uitvoeren van geavanceerde opties oplossen, raadpleegt u de NPS-indeling databaselogboekbestanden waarop de NPS-service is geïnstalleerd. Deze logboekbestanden worden gemaakt in _%SystemRoot%\System32\Logs_ map als CSV-bestanden. Voor een beschrijving van deze logboekbestanden, Zie [interpreteren NPS indeling databaselogboekbestanden](https://technet.microsoft.com/library/cc771748.aspx). 

De items in deze logboekbestanden zijn moeilijk te interpreteren zonder ze te importeren in een database of een werkblad. Hier vindt u een aantal IAS parsers online helpen u bij het interpreteren van de logboekbestanden. Hieronder vindt u de uitvoer van een dergelijke downloadbare [shareware toepassing](http://www.deepsoftware.com/iasviewer): 

 ![Shareware toepassing](./media/nps-extension-vpn/image49.png)

Ten slotte voor extra opties oplossen, kunt u een programma voor protocolanalyse zoals Wireshark of [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). De volgende afbeelding van Wireshark toont de RADIUS-berichten tussen de VPN-server en de NPS-server.

 ![Microsoft Message Analyzer](./media/nps-extension-vpn/image50.png)

Zie voor meer informatie [uw bestaande NPS-infrastructuur integreren met Azure multi-factor Authentication](multi-factor-authentication-nps-extension.md).  

## <a name="next-steps"></a>Volgende stappen
[Azure Multi-Factor Authentication gebruiken](multi-factor-authentication-versions-plans.md)

[Extern bureaublad-gateway en Azure Multi-Factor Authentication-server met behulp van RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Uw on-premises adreslijsten integreren met Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)

