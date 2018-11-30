---
title: VPN met Azure MFA integreren met behulp van de Network Policy Server-extensie | Microsoft Docs
description: Uw VPN-infrastructuur integreren met Azure MFA met behulp van de Network Policy Server-extensie voor Microsoft Azure.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: f08bf6a65a93c31d76c5b336cc6ef433153d71c4
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423114"
---
# <a name="integrate-your-vpn-infrastructure-with-azure-mfa-by-using-the-network-policy-server-extension-for-azure"></a>Uw VPN-infrastructuur integreren met Azure MFA met behulp van de Network Policy Server-extensie voor Azure

## <a name="overview"></a>Overzicht

De Network Policy Server (NPS)-extensie voor Azure kan organisaties ter bescherming van de Remote Authentication Dial-In User Service (RADIUS)-clientauthenticatie met behulp van cloud-gebaseerde [Azure multi-factor Authentication (MFA)](howto-mfaserver-nps-rdg.md), waarmee u verificatie in twee stappen.

In dit artikel vindt u instructies voor het NPS-infrastructuur integreren met MFA met behulp van de NPS-extensie voor Azure. Dit proces kunt veilige verificatie voor gebruikers die verbinding met uw netwerk maakt met behulp van een VPN. 

Network Policy and Access Services biedt organisaties de mogelijkheid om:

* Toewijzen van een centrale locatie voor het beheer en controle van netwerkaanvragen om op te geven:

    * Wie verbinding kan maken 
    
    * Welke tijdstippen van de dag verbindingen zijn toegestaan 
    
    * De duur van de verbindingen
    
    * Het niveau van beveiliging die clients gebruiken moeten om verbinding te maken

    In plaats van beleidsregels voor elke VPN- of extern bureaublad-Gateway-server opgeeft, doen nadat ze op een centrale locatie. Het RADIUS-protocol wordt gebruikt voor het centrale verificatie, autorisatie en Accounting (AAA). 

* Instellen en afdwingen van statusbeleid voor Network Access Protection (NAP) client om te bepalen of apparaten onbeperkte of beperkte toegang krijgen tot netwerkbronnen.

* Bieden een manier om af te dwingen de verificatie en autorisatie voor toegang met 802. 1 x-compatibele draadloze toegangspunten en Ethernet-switches.   
Zie voor meer informatie, [Network Policy Server](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

Voor betere beveiliging en een hoge mate van naleving, kunnen organisaties NPS geïntegreerd met Azure multi-factor Authentication om ervoor te zorgen dat gebruikers verificatie in twee stappen om verbinding met de virtuele poort op de VPN-server te gebruiken. Voor gebruikers toegang te krijgen, moeten ze bieden hun gebruikersnaam en wachtwoord combinatie en andere informatie die ze beheren. Deze informatie moet worden vertrouwd en worden niet gedupliceerd. Een mobiele-telefoonnummer, een vast aantal of een toepassing op een mobiel apparaat kan bevatten.

Voorafgaand aan de beschikbaarheid van de NPS-extensie voor Azure, klanten die willen implementeren verificatie in twee stappen voor geïntegreerde NPS en MFA omgevingen hoeft te configureren en onderhouden van een afzonderlijke MFA-server in een on-premises omgeving. Dit type verificatie wordt aangeboden door de extern bureaublad-Gateway en Azure multi-factor Authentication-Server met behulp van RADIUS.

Met de NPS-extensie voor Azure, kunnen organisaties RADIUS-clientverificatie beveiligen door het implementeren van een on-premises gebaseerde MFA-oplossing of een MFA-cloud-gebaseerde oplossing.
 
## <a name="authentication-flow"></a>Verificatie-stroom
Wanneer gebruikers verbinding met een virtuele poort op een VPN-server maken, moeten ze eerst worden geverifieerd met behulp van verschillende protocollen. De protocollen voor toestaan het gebruik van een combinatie van gebruikersnaam en wachtwoord en certificaten gebaseerde verificatiemethoden. 

Naast verificatie en hun identiteit te verifiëren, moeten gebruikers de juiste machtigingen inbellen hebben. In implementaties van eenvoudige, inbellen machtigingen waarmee toegang zijn ingesteld rechtstreeks op de Active Directory-gebruikersobjecten. 

![Gebruikerseigenschappen](./media/howto-mfa-nps-extension-vpn/image1.png)

In eenvoudige implementaties elke VPN-server verleent of weigert toegang op basis van beleidsregels die zijn gedefinieerd op de lokale VPN-server.

In implementaties van grotere en beter schaalbaar, worden de beleidsregels die VPN-toegang verlenen of weigeren gecentraliseerd op RADIUS-servers. In dergelijke gevallen wordt de VPN-server fungeert als een access-server (RADIUS-client) die verbindingsaanvragen en account-berichten naar een RADIUS-server stuurt. Voor verbinding met de virtuele poort op de VPN-server, worden gebruikers moeten worden geverifieerd en voldoen aan de voorwaarden die centraal zijn gedefinieerd op RADIUS-servers. 

Wanneer de NPS-extensie voor Azure is geïntegreerd met de NPS, gevolg een stroom verificatie is geslaagd, als volgt:

1. De VPN-server wordt een verificatieaanvraag ontvangt van een VPN-gebruiker, inclusief de gebruikersnaam en het wachtwoord voor het verbinden met een resource, zoals een extern bureaublad-sessie. 

2. Fungeert als een RADIUS-client, de VPN-server de aanvraag converteert naar een RADIUS *-aanvraag voor toegang tot* bericht en verzendt dit (met een versleuteld wachtwoord) naar de RADIUS-server waarop de NPS-extensie is geïnstalleerd. 

3. De combinatie van gebruikersnaam en wachtwoord is geverifieerd in Active Directory. Als de gebruikersnaam of het wachtwoord onjuist is, stuurt de RADIUS-Server een *toegang weigeren* bericht. 

4. Als alle voorwaarden, zoals opgegeven in de NPS-verbindingsaanvraag en het netwerkbeleid wordt voldaan (bijvoorbeeld, de tijd van de dag of groep lidmaatschapsbeperkingen), de NPS-extensie wordt geactiveerd voor een aanvraag voor secundaire verificatie met Azure multi-factor Authentication. 

5. Azure multi-factor Authentication communiceert met Azure Active Directory, haalt gegevens op van de gebruiker en de secundaire authenticatie wordt uitgevoerd met behulp van de methode die geconfigureerd door de gebruiker (cel telefoongesprek, tekstbericht of mobiele app). 

6. Als de MFA-controle geslaagd is, communiceert de Azure multi-factor Authentication het resultaat dat de NPS-extensie.

7. Nadat de verbindingspoging is geverifieerd en geautoriseerd, de installatie van de extensie NPS een RADIUS verzendt *toegang accepteren* bericht naar de VPN-server (RADIUS-client).

8. De gebruiker krijgt toegang tot de virtuele poort op de VPN-server en een versleutelde VPN-tunnel maakt.

## <a name="prerequisites"></a>Vereisten
In deze sectie worden de vereisten die moeten worden voltooid voordat u MFA met extern bureaublad-Gateway integreren kunt. Voordat u begint, moet u de volgende vereisten hebt voldaan:

* VPN-infrastructuur
* Network Policy and Access Services-rol
* Azure multi-factor Authentication-licentie
* Windows Server-software
* Bibliotheken
* Azure Active Directory (Azure AD) gesynchroniseerd met on-premises Active Directory 
* Azure Active Directory-GUID-ID

### <a name="vpn-infrastructure"></a>VPN-infrastructuur
In dit artikel wordt ervan uitgegaan dat u hebt een werkende VPN-infrastructuur die gebruikmaakt van Microsoft Windows Server 2016 en de VPN-server is momenteel niet geconfigureerd voor verbindingsaanvragen wilt doorsturen naar een RADIUS-server. In het artikel configureert u de VPN-infrastructuur voor het gebruik van een centrale RADIUS-server.

Als u geen een werkende VPN-infrastructuur aanwezig zijn, kunt u snel maken volgt u de instructies in vele zelfstudies van de VPN-instellingen die u kunt vinden op de Microsoft en de sites van derden. 
            
### <a name="the-network-policy-and-access-services-role"></a>De rol Network Policy and Access Services

Network Policy and Access Services biedt de RADIUS-server en client-functionaliteit. In dit artikel wordt ervan uitgegaan dat u de rol Network Policy and Access Services hebt geïnstalleerd op een lidserver of domeincontroller in uw omgeving. In deze handleiding configureert u RADIUS voor de configuratie van een VPN. De functie Network Policy and Access Services installeren op een server *dan* de VPN-server.

Windows Server 2012-service of hoger, Zie voor informatie over het installeren van de rol Network Policy and Access Services [installeren van een NAP-statusbeleidsserver](https://technet.microsoft.com/library/dd296890.aspx). NAP is afgeschaft in Windows Server 2016. Zie voor een beschrijving van aanbevolen procedures voor NPS, met inbegrip van de aanbeveling voor het installeren van NPS op een domeincontroller [aanbevolen procedures voor NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-mfa-license"></a>Azure MFA-licentie

Een licentie is vereist voor Azure multi-factor Authentication en het is beschikbaar via een Azure AD Premium, Enterprise Mobility + Security of een zelfstandige licentie van multi-factor Authentication. Op basis van gebruik licenties voor Azure MFA zoals per gebruiker of per verificatie licenties zijn niet compatibel met de NPS-extensie. Zie voor meer informatie, [over het verkrijgen van Azure multi-factor Authentication](concept-mfa-licensing.md). Voor testdoeleinden kunt u een proefabonnement.

### <a name="windows-server-software"></a>Windows Server-software

De NPS-extensie is vereist voor Windows Server 2008 R2 SP1 of hoger, met de rol Network Policy and Access Services zijn geïnstalleerd. De stappen in deze handleiding zijn uitgevoerd met Windows Server 2016.

### <a name="libraries"></a>Bibliotheken

De volgende bibliotheken worden automatisch geïnstalleerd met de NPS-extensie:

-   [Visual C++ Redistributable Packages for Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory-Module voor Windows PowerShell versie 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Als de Microsoft Azure Active Directory PowerShell-Module nog niet aanwezig is, wordt deze geïnstalleerd met een script voor de configuratie die u als onderdeel van het installatieproces uitvoert. Er is niet nodig voor het installeren van de module vooraf als deze nog niet is geïnstalleerd.

### <a name="azure-active-directory-synced-with-on-premises-active-directory"></a>Azure Active Directory worden gesynchroniseerd met on-premises Active Directory 

Voor het gebruik van de NPS-extensie, moeten on-premises gebruikers worden gesynchroniseerd met Azure Active Directory en ingeschakeld voor MFA. Deze handleiding wordt ervan uitgegaan dat on-premises gebruikers worden gesynchroniseerd met Azure Active Directory via Azure AD Connect. Hieronder vindt u instructies voor het inschakelen van gebruikers voor MFA.

Zie voor meer informatie over Azure AD Connect [uw on-premises directory's integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md). 

### <a name="azure-active-directory-guid-id"></a>Azure Active Directory-GUID-ID 

U moet weten de GUID van de Azure Active Directory voor het installeren van de NPS-extensie. In de volgende sectie vindt u instructies voor het vinden van de GUID van de Azure Active Directory.

## <a name="configure-radius-for-vpn-connections"></a>RADIUS voor VPN-verbindingen configureren

Als u de functie NPS op een lidserver hebt geïnstalleerd, moet u deze configureren om te verifiëren en autoriseren van de VPN-client die aanvragen voor VPN-verbindingen. 

In deze sectie wordt ervan uitgegaan dat u de functie Network Policy and Access Services hebt geïnstalleerd, maar niet voor gebruik in uw infrastructuur configureert zijn.

> [!NOTE]
> Als u al een werkende VPN-server die gebruikmaakt van een gecentraliseerde RADIUS-server voor verificatie hebt, kunt u deze sectie overslaan.
>

### <a name="register-server-in-active-directory"></a>Server registreren in Active Directory
Werkt alleen goed in dit scenario, als moet de NPS-server worden geregistreerd in Active Directory.

1. Open Serverbeheer.

2. Selecteer in Serverbeheer **extra**, en selecteer vervolgens **Network Policy Server**. 

3. In de console van Network Policy Server met de rechtermuisknop op **NPS (lokaal)**, en selecteer vervolgens **server registreren in Active Directory**. Selecteer **OK** twee keer.

    ![Network Policy Server](./media/howto-mfa-nps-extension-vpn/image2.png)

4. Laat de console geopend voor de volgende procedure.

### <a name="use-wizard-to-configure-the-radius-server"></a>Gebruik de wizard voor het configureren van de RADIUS-server
U kunt een standaard (wizard-indeling) of geavanceerde configuratieoptie voor het configureren van de RADIUS-server. In deze sectie wordt ervan uitgegaan dat u de standaardconfiguratie op basis van een wizard-optie.

1. Selecteer in de beheerconsole Network Policy Server **NPS (lokaal)**.

2. Onder **standaardconfiguratie**, selecteer **RADIUS-Server voor inbel- of VPN-verbindingen**, en selecteer vervolgens **configureren van VPN- of inbelverbinding tot stand brengen**.

    ![VPN configureren](./media/howto-mfa-nps-extension-vpn/image3.png)

3. In de **Selecteer inbel- of virtuele particuliere verbindingen netwerktype** venster, selecteer **VPN-verbindingen**, en selecteer vervolgens **volgende**.

    ![Virtueel particulier netwerk](./media/howto-mfa-nps-extension-vpn/image4.png)

4. In de **opgeven inbel- of VPN-Server** venster **toevoegen**.

5. In de **nieuwe RADIUS-client** venster, Geef een beschrijvende naam, voer de naam of IP-adres van de VPN-server en voer vervolgens een gedeeld geheim wachtwoord. Ervoor zorgen dat het gedeelde geheim wachtwoord lange en complexe. Het opnemen, omdat u hebt deze nodig in de volgende sectie.

    ![Nieuwe RADIUS-client](./media/howto-mfa-nps-extension-vpn/image5.png)

6. Selecteer **OK**, en selecteer vervolgens **volgende**.

7. In de **verificatiemethoden configureren** venster, accepteer de standaardinstelling (**Microsoft Encrypted Authentication versie 2 [MS-CHAPv2])** of kies een andere optie en selecteer **volgende** .

    > [!NOTE]
    > Als u Extensible Authentication Protocol (EAP) configureert, moet u Microsoft Challenge Handshake Authentication Protocol (CHAPv2) of Protected Extensible Authentication Protocol (PEAP). Er zijn geen andere EAP wordt ondersteund.
 
8. In de **gebruikersgroepen opgeven** venster **toevoegen**, en selecteer vervolgens een geschikte groep. Als er geen groep is, laat u de selectie leeg om toegang te verlenen aan alle gebruikers.

    ![Het venster gebruikersgroepen opgeven](./media/howto-mfa-nps-extension-vpn/image7.png)

9. Selecteer **Volgende**.

10. In de **IP-Filters opgeven** venster **volgende**.

11. In de **versleutelingsinstellingen opgeven** venster, accepteer de standaardinstellingen, en selecteer vervolgens **volgende**.

    ![Het venster Instellingen voor codering opgeven](./media/howto-mfa-nps-extension-vpn/image8.png)

12. In de **een Realm opgeven** naam van de realm leeg laat, accepteer de standaardinstelling en selecteer vervolgens **volgende**.

    ![Geef een naam van de Realm-venster](./media/howto-mfa-nps-extension-vpn/image9.png)

13. In de **nieuwe voltooien van inbel- of VPN-verbindingen en RADIUS-clients** venster **voltooien**.

    ![Het venster "voltooien nieuwe inbel- of virtuele particuliere netwerkverbindingen en RADIUS-clients"](./media/howto-mfa-nps-extension-vpn/image10.png)

### <a name="verify-the-radius-configuration"></a>Controleer of de RADIUS-configuratie
In deze sectie worden de configuratie die u hebt gemaakt met behulp van de wizard.

1. Op de Network Policy Server, in de NPS (lokaal)-console, vouw **RADIUS-Clients**, en selecteer vervolgens **RADIUS-Clients**.

2. In het detailvenster met de rechtermuisknop op de RADIUS-client die u hebt gemaakt, en selecteer vervolgens **eigenschappen**. De eigenschappen van de RADIUS-client (de VPN-server) moeten zijn, zoals die hier wordt weergegeven:

    ![VPN-eigenschappen](./media/howto-mfa-nps-extension-vpn/image11.png)

3. Selecteer **annuleren**.

4. Op de Network Policy Server, in de NPS (lokaal)-console, vouw **beleid**, en selecteer vervolgens **beleid voor verbindingsaanvragen**. Het beleid voor VPN-verbindingen wordt weergegeven zoals in de volgende afbeelding:

    ![Verbindingsaanvragen](./media/howto-mfa-nps-extension-vpn/image12.png)

5. Onder **beleid**, selecteer **netwerkbeleid**. Hier ziet u een beleid voor virtuele particuliere netwerk (VPN)-verbindingen die lijkt op het beleid wordt weergegeven in de volgende afbeelding:

    ![Beleid voor netwerken](./media/howto-mfa-nps-extension-vpn/image13.png)

## <a name="configure-your-vpn-server-to-use-radius-authentication"></a>De VPN-server voor het gebruik van RADIUS-verificatie configureren
In deze sectie configureert u de VPN-server voor het gebruik van RADIUS-verificatie. De instructies wordt ervan uitgegaan dat u een werkende configuratie van een VPN-server hebt, maar het gebruik van RADIUS-verificatie niet hebt geconfigureerd. Nadat u de VPN-server hebt geconfigureerd, moet u controleren of uw configuratie werkt zoals verwacht.

> [!NOTE]
> Als u al een werkende VPN-server-configuratie die gebruikmaakt van RADIUS-verificatie hebt, kunt u deze sectie overslaan.
>

### <a name="configure-authentication-provider"></a>Provider voor verificatie configureren
1. Open Serverbeheer op de VPN-server.

2. Selecteer in Serverbeheer **extra**, en selecteer vervolgens **Routing and Remote Access**.

3. In de **Routing and Remote Access** venster met de rechtermuisknop op  **\<servernaam > (lokaal)**, en selecteer vervolgens **eigenschappen**.

    ![Het venster Routing and Remote Access](./media/howto-mfa-nps-extension-vpn/image14.png)
 
4. In de  **\<servernaam > (lokaal) eigenschappen** venster de **Security** tabblad. 

5. Op de **Security** tabblad onder **verificatieprovider**, selecteer **RADIUS-verificatie**, en selecteer vervolgens **configureren**.

    ![RADIUS-verificatie](./media/howto-mfa-nps-extension-vpn/image15.png)
 
6. In de **RADIUS-verificatie** venster **toevoegen**.

7. In de **RADIUS-Server toevoegen** venster de volgende handelingen uit:

    a. In de **servernaam** vak, voer de naam of IP-adres van de RADIUS-server die u hebt geconfigureerd in de vorige sectie.

    b. Voor de **gedeeld geheim**, selecteer **wijziging**, en voer vervolgens het gedeelde geheim wachtwoord dat u hebt gemaakt en u eerder hebt genoteerd.

    c. In de **Time-out (seconden)** Selecteer een waarde op tussen **30** via **60**.  
    De time-outwaarde is nodig om voldoende tijd voor het voltooien van de tweede verificatiefactor toestaan.
 
    ![Het venster van de RADIUS-Server toevoegen](./media/howto-mfa-nps-extension-vpn/image16.png)
 
8. Selecteer **OK**.

### <a name="test-vpn-connectivity"></a>VPN-connectiviteit testen
In deze sectie maakt bevestigen u dat de VPN-client is geverifieerd en geautoriseerd door de RADIUS-server wanneer u probeert verbinding maken met de VPN-poort. De instructies wordt ervan uitgegaan dat u van Windows 10 als een VPN-client gebruikmaakt. 

> [!NOTE]
> Als u al een VPN-client als u wilt verbinding maken met de VPN-server en de instellingen hebt opgeslagen hebt geconfigureerd, kunt u de stappen voor het configureren en opslaan van een VPN-verbindingsobject overslaan.
>

1. Selecteer op uw VPN-client-computer, de **Start** knop en selecteer vervolgens de **instellingen** knop.

2. In de **Windows-instellingen** venster **netwerk en Internet**.

3. Selecteer **VPN**.

4. Selecteer **een VPN-verbinding toevoegen**.

5. In de **toevoegen van een VPN-verbinding** venster in de **VPN-provider** Schakel **Windows (ingebouwd)**, vult u de resterende velden, waar nodig, en selecteer vervolgens **Opslaan**. 

    ![Het venster 'Een VPN-verbinding toevoegen'](./media/howto-mfa-nps-extension-vpn/image17.png)
 
6. Ga naar **Configuratiescherm**, en selecteer vervolgens **Netwerkcentrum**.

7. Selecteer **Adapterinstellingen wijzigen**.

    ![Adapterinstellingen wijzigen](./media/howto-mfa-nps-extension-vpn/image18.png)

8. Met de rechtermuisknop op de VPN-verbinding voor netwerk, en selecteer vervolgens **eigenschappen**. 

    ![Eigenschappen van de VPN-netwerk](./media/howto-mfa-nps-extension-vpn/image19.png)

9. Selecteer in het venster van de VPN-eigenschappen, de **Security** tabblad. 

10. Op de **Security** tabblad, zorg ervoor dat alleen **Microsoft CHAP Version 2 (MS-CHAP v2)** is geselecteerd en selecteer vervolgens **OK**.

    ![De optie 'Deze protocollen toestaan'](./media/howto-mfa-nps-extension-vpn/image20.png)

11. Met de rechtermuisknop op de VPN-verbinding en selecteer vervolgens **Connect**.

12. In de **instellingen** venster **Connect**.  
    Een geslaagde verbinding wordt weergegeven in het beveiligingslogboek op de RADIUS-server als gebeurtenis-ID 6272, zoals hier wordt weergegeven:

    ![Het venster Eigenschappen van gebeurtenis](./media/howto-mfa-nps-extension-vpn/image21.png)

## <a name="troubleshooting-radius"></a>Oplossen van problemen met RADIUS

Wordt ervan uitgegaan dat uw VPN-configuratie werkte voordat u de VPN-server geconfigureerd voor het gebruik van een gecentraliseerde RADIUS-server voor verificatie en autorisatie. Als de configuratie is werkt, is het waarschijnlijk dat het probleem wordt veroorzaakt door een onjuiste configuratie van de RADIUS-server of het gebruik van een ongeldige gebruikersnaam of wachtwoord. Bijvoorbeeld, als u de alternatieve UPN-achtervoegsel in de gebruikersnaam gebruikt, kan de aanmeldingspoging mislukken. Gebruik dezelfde accountnaam voor de beste resultaten. 

Voor het oplossen van deze problemen, is een ideale plek om te beginnen het onderzoeken van de gebeurtenislogboeken op de RADIUS-server. Om tijd te zoeken naar gebeurtenissen hebt opgeslagen, kunt u de functie Network Policy and Access-Server aangepaste weergave in Logboeken, zoals hier wordt weergegeven. 'Gebeurtenis-ID 6273' geeft aan dat de gebeurtenissen waar de NPS toegang geweigerd aan een gebruiker. 

![Logboeken](./media/howto-mfa-nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Multi-factor Authentication configureren

Zie de artikelen voor informatie over het configureren van gebruikers voor multi-factor Authentication [hoe u verificatie in twee stappen vereist voor een gebruiker of groep](howto-mfa-userstates.md) en [Mijn account voor verificatie in twee stappen instellen](../user-help/multi-factor-authentication-end-user-first-time.md)

## <a name="install-and-configure-the-nps-extension"></a>Installeren en configureren van de NPS-extensie

Deze sectie vindt u instructies voor het configureren van VPN MFA gebruiken voor clientverificatie met de VPN-server.

Na het installeren en configureren van de NPS-extensie, worden alle RADIUS-clientverificatie die wordt verwerkt door deze server is vereist voor MFA gebruiken. Als al uw VPN-gebruikers niet zijn ingeschreven bij Azure multi-factor Authentication, kunt u het volgende doen:

* Een andere RADIUS-server instellen voor verificatie van gebruikers die niet zijn geconfigureerd voor MFA gebruiken. 

* Maak een register-item waarmee gebruikers uitgedaagd om een tweede verificatiefactor als ze zijn geregistreerd bij Azure multi-factor Authentication. 

Een nieuwe tekenreekswaarde met de naam _REQUIRE_USER_MATCH in HKLM\SOFTWARE\Microsoft\AzureMfa_, en stel de waarde voor *waar* of *False*. 

![De instelling 'Gebruikersovereenkomst vereist'](./media/howto-mfa-nps-extension-vpn/image34.png)
 
Als de waarde is ingesteld op *waar* of is leeg, alle verificatieaanvragen zijn afhankelijk van een MFA-controle. Als de waarde is ingesteld op *False*, MFA uitdagingen alleen voor gebruikers die zijn geregistreerd bij Azure multi-factor Authentication worden uitgegeven. Gebruik de *False* alleen in tests of productie-omgevingen instellen tijdens een periode onboarding.

### <a name="obtain-the-azure-active-directory-guid-id"></a>De Azure Active Directory-GUID-ID ophalen

Als onderdeel van de configuratie van de NPS-extensie dient u beheerdersreferenties en de ID van uw Azure AD-tenant. De ID ophalen door het volgende te doen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder van de Azure-tenant.

2. Selecteer in het linkerdeelvenster de **Azure Active Directory** knop.

3. Selecteer **eigenschappen**.

4. Als uw Azure AD-ID kopiëren, selecteert u de **kopie** knop.
 
    ![De Azure AD-ID](./media/howto-mfa-nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>De NPS-extensie installeren
De NPS-extensie moet worden geïnstalleerd op een server waarop de Network Policy and Access Services-rol geïnstalleerd en die functies als de RADIUS-server in uw ontwerp. Voer *niet* de NPS-extensie installeren op uw extern bureaublad-server.

1. Download de NPS-extensie van [Microsoft Download Center](https://aka.ms/npsmfa). 

2. Kopieer het uitvoerbare bestand voor installatie (*NpsExtnForAzureMfaInstaller.exe*) naar de NPS-server.

3. Dubbelklik op de NPS-server op **NpsExtnForAzureMfaInstaller.exe** en, als u wordt gevraagd, selecteert u **uitvoeren**.

4. In de **NPS-extensie voor Azure MFA Setup** venster, lees de softwarelicentievoorwaarden, selecteer de **ik ga akkoord met de licentievoorwaarden en voorwaarden** selectievakje en selecteer vervolgens **installeren**.

    ![Het venster 'NPS-extensie voor Azure MFA instellen'](./media/howto-mfa-nps-extension-vpn/image36.png)
 
5. In de **NPS-extensie voor Azure MFA Setup** venster **sluiten**.  

    ![Het 'Installatie geslaagd' bevestigingsvenster](./media/howto-mfa-nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-the-nps-extension-by-using-a-powershell-script"></a>Certificaten voor gebruik met de NPS-extensie configureren met behulp van een PowerShell-script
Beveiligde communicatie- en assurance te garanderen, configureren van certificaten voor gebruik door de NPS-extensie. De NPS-onderdelen zijn een Windows PowerShell-script dat een zelfondertekend certificaat voor gebruik met NPS configureert. 

Het script voert de volgende handelingen uit:

* Hiermee maakt u een zelfondertekend certificaat.
* Hiermee koppelt u de openbare sleutel van het certificaat naar de service-principal in Azure AD.
* Slaat het certificaat in het archief van de lokale computer.
* De netwerk-gebruikerstoegang tot de persoonlijke sleutel van het certificaat verleent.
* De NPS-service opnieuw is opgestart.

Als u wilt uw eigen certificaten te gebruiken, moet u de openbare sleutel van uw certificaat koppelen aan de service-principal in Azure AD en enzovoort.

Voor het gebruik van het script, bieden u de extensie met uw beheerdersreferenties voor Azure Active Directory en de Azure Active Directory-tenant-ID die u eerder hebt gekopieerd. Het script uitvoeren op elke NPS-server waarop u de NPS-extensie installeren.

1. Windows PowerShell ook uitvoeren als beheerder.

2. Voer vanaf de PowerShell-opdrachtprompt **cd "c:\Program Files\Microsoft\AzureMfa\Config"**, en selecteer vervolgens Enter.

3. Voer op de volgende opdrachtregel het volgende **.\AzureMfsNpsExtnConfigSetup.ps1**, en selecteer vervolgens Enter. Het script wordt gecontroleerd of de Azure AD PowerShell-module is geïnstalleerd. Als dit niet is geïnstalleerd, installeert u de module met het script voor u.
 
    ![PowerShell](./media/howto-mfa-nps-extension-vpn/image38.png)
 
    Nadat het script wordt gecontroleerd door de installatie van de PowerShell-module, wordt de Azure Active Directory PowerShell-module aanmelden venster weergegeven. 

4. Voer uw beheerdersreferenties voor Azure AD en het wachtwoord en selecteer vervolgens **aanmelden**. 
 
    ![Het PowerShell-aanmelden-venster](./media/howto-mfa-nps-extension-vpn/image39.png)
 
5. Bij de opdrachtprompt, plakt u de tenant-ID die u eerder hebt gekopieerd en selecteer vervolgens Enter. 

    ![Tenant-id](./media/howto-mfa-nps-extension-vpn/image40.png)

    Het script wordt een zelfondertekend certificaat gemaakt en andere wijzigingen in de configuratie wordt uitgevoerd. De uitvoer is net als in de volgende afbeelding:

    ![Zelfondertekend certificaat](./media/howto-mfa-nps-extension-vpn/image41.png)

6. De server opnieuw opstarten.

### <a name="verify-the-configuration"></a>De configuratie controleren
Als u wilt controleren of de configuratie, moet u een nieuwe VPN-verbinding met de VPN-server maken. Nadat u hebt uw referenties zijn ingevoerd voor primaire verificatie, wacht de VPN-verbinding voor de secundaire verificatie te voltooien voordat de verbinding tot stand is gebracht, zoals hieronder wordt weergegeven. 

![Het VPN-instellingen van Windows-venster](./media/howto-mfa-nps-extension-vpn/image42.png)

Als u met de secundaire verificatiemethode die u eerder hebt geconfigureerd in de Azure MFA verifiëren, kunt u bent verbonden met de resource. Als de secundaire verificatie mislukt is, wordt u toegang tot de resource geweigerd. 

De Microsoft Authenticator-app op een Windows Phone levert in het volgende voorbeeld wordt de secundaire verificatie:

![Account verifiëren](./media/howto-mfa-nps-extension-vpn/image43.png)

Nadat u hebt geverifieerd met behulp van de tweede methode, kunt u toegang tot de virtuele poort op de VPN-server zijn verleend. Omdat u een tweede verificatiemethode gebruiken met behulp van een mobiele app op een vertrouwd apparaat zijn vereist, is het aanmeldingsproces veiliger dan als deze nog gebruik van alleen een combinatie van gebruikersnaam en wachtwoord.

### <a name="view-event-viewer-logs-for-successful-sign-in-events"></a>Logboeken voor geslaagde aanmeldingsgebeurtenissen weergeven
Als u wilt weergeven van geslaagde aanmeldingsgebeurtenissen in de Windows-Logboeken, meld u query de Windows-beveiliging op de NPS-server met de volgende PowerShell-opdracht:

    _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL 

![PowerShell security Event Viewer](./media/howto-mfa-nps-extension-vpn/image44.png)
 
U kunt ook het beveiligingslogboek of de Network Policy and Access Services aangepaste weergave, bekijken, zoals hier wordt weergegeven:

![Network Policy Server-logboek](./media/howto-mfa-nps-extension-vpn/image45.png)

Op de server waarop u de NPS-extensie voor Azure multi-factor Authentication hebt geïnstalleerd, vindt u logboeken van de toepassing Logboeken die specifiek zijn voor de extensie op *toepassingen en Services Logs\Microsoft\AzureMfa*. 

    _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL

![Logboeken 'Het aantal gebeurtenissen' deelvenster](./media/howto-mfa-nps-extension-vpn/image46.png)

## <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen
Als de configuratie niet werkt zoals verwacht, probeert op te lossen door te controleren dat de gebruiker is geconfigureerd voor het gebruik van MFA. Laat de gebruiker die verbinding maken met de [Azure-portal](https://portal.azure.com). Als de gebruiker wordt gevraagd om secundaire verificatie en kan worden geverifieerd, kunt u een onjuiste configuratie van MFA als een probleem elimineren.

Als MFA voor de gebruiker werkt is, raadpleegt u de relevante Event Viewer-Logboeken. De logboeken bevatten de beveiligingsgebeurtenis, operationele-Gateway en Azure multi-factor Authentication-logboeken die worden beschreven in de vorige sectie. 

Een voorbeeld van een logboekbestand met een mislukte aanmelding-gebeurtenis (gebeurtenis-ID 6273) wordt hier weergegeven:

![Beveiligingslogboek van een gebeurtenis voor mislukte aanmelding](./media/howto-mfa-nps-extension-vpn/image47.png)

Hier ziet u een gerelateerde gebeurtenis vanuit de Azure multi-factor Authentication-logboekbestanden:

![Azure multi-factor Authentication-Logboeken](./media/howto-mfa-nps-extension-vpn/image48.png)

Als u wilt doen probleemoplossing, raadpleegt u de NPS-database-indeling logboekbestanden waarop de NPS-service is geïnstalleerd. De logboekbestanden worden gemaakt in de _%SystemRoot%\System32\Logs_ map als bestanden met door komma's gescheiden tekst. Zie voor een beschrijving van de logboekbestanden, [interpreteren NPS indeling databaselogboekbestanden](https://technet.microsoft.com/library/cc771748.aspx). 

De items in deze logboekbestanden zijn moeilijk te interpreteren, tenzij u ze naar een werkblad of een database exporteert. Hier vindt u veel Internet Authentication Service (IAS) is online hulpprogramma's om u te helpen u bij het interpreteren van de logboekbestanden te parseren. De uitvoer van een dergelijke downloadbare [shareware toepassing](https://www.deepsoftware.com/iasviewer) wordt hier weergegeven: 

![Shareware bevat toepassing](./media/howto-mfa-nps-extension-vpn/image49.png)

Als u wilt doen op andere problemen oplossen, kunt u een programma voor protocolanalyse zoals Wireshark of [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). De volgende afbeelding van Wireshark ziet u de RADIUS-berichten tussen de VPN-server en de NPS.

![Microsoft Message Analyzer](./media/howto-mfa-nps-extension-vpn/image50.png)

Zie voor meer informatie, [uw bestaande NPS-infrastructuur integreren met Azure multi-factor Authentication](howto-mfa-nps-extension.md). 

## <a name="next-steps"></a>Volgende stappen
[Azure multi-factor Authentication ophalen](concept-mfa-licensing.md)

[Extern bureaublad-gateway en Azure Multi-Factor Authentication-server met behulp van RADIUS](howto-mfaserver-nps-rdg.md)

[Uw on-premises directory's integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md)

