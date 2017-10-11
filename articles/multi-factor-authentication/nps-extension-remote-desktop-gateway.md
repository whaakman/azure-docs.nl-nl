---
title: Integratie met extern bureaublad-Gateway met de extensie Azure MFA NPS | Microsoft Docs
description: Dit artikel wordt beschreven in uw infrastructuur voor extern bureaublad-gatewayserver integreren met Azure MFA met behulp van de Network Policy Server (NPS)-uitbreiding voor Microsoft Azure.
services: active-directory
keywords: Azure MFA integreren van extern bureaublad-Gateway, Azure Active Directory, uitbreiding van Network Policy Server
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
ms.openlocfilehash: 6ff9a341b31e5005949dcc0ecb2591060269846e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
#  <a name="integrate-your-remote-desktop-gateway-infrastructure-using-the-network-policy-server-nps-extension-and-azure-ad"></a>De infrastructuur van uw extern bureaublad-Gateway met de extensie van Network Policy Server (NPS) en Azure AD integreren

Dit artikel bevat informatie voor het integreren van uw infrastructuur voor extern bureaublad-Gateway met Azure multi-factor Authentication (MFA) met behulp van de Network Policy Server (NPS)-uitbreiding voor Microsoft Azure. 

De Service NPS (Network Policy)-extensie voor Azure klanten kunnen te beschermen Remote Authentication Dial-In User Service (RADIUS)-clientverificatie met Azure de cloud-gebaseerde [multi-factor Authentication (MFA)](multi-factor-authentication.md). Deze oplossing biedt verificatie in twee stappen voor het toevoegen van een tweede beveiligingslaag aan gebruikersaanmeldingen en transacties.

In dit artikel bevat stapsgewijze instructies voor het integreren van de NPS-infrastructuur met Azure MFA met behulp van de NPS-extensie voor Azure. Hierdoor veilige verificatie voor gebruikers die zich willen aanmelden bij een extern bureaublad-Gateway. 

De Network Policy and Access Services (NPS) biedt organisaties de mogelijkheid het volgende doen:
* Definieer centrale locatie voor het beheer en controle verzoeken van het netwerk door op te geven die een verbinding kunt maken, welke momenten van de dag verbindingen zijn toegestaan, de duur van verbindingen en het niveau van beveiliging die clients moeten gebruiken om verbinding te, enzovoort. In plaats van deze beleidsregels geven op elke server VPN- of -Gateway van de extern bureaublad (RD), kunnen deze beleidsregels eenmaal worden opgegeven op een centrale locatie. Het RADIUS-protocol biedt gecentraliseerd verificatie, autorisatie en Accounting (AAA). 
* Instellen en afdwingen van statusbeleid voor Network Access Protection (NAP) client om te bepalen of apparaten onbeperkte of beperkte toegang krijgen tot netwerkbronnen.
* De mogelijkheid om af te dwingen, verificatie en autorisatie voor toegang op 802. 1 x-compatibele draadloze toegangspunten en Ethernet-switches.    

Normaal gesproken organisaties gebruik NPS (RADIUS) om te vereenvoudigen en centraliseer het beheer van VPN-beleid. Veel organisaties worden echter ook de NPS gebruiken om te vereenvoudigen en centraliseer het beheer van extern bureaublad bureaublad Verbindingsautorisatiebeleid (RD CAP's). 

Organisaties kunnen ook NPS integreren met Azure MFA om betere beveiliging en een hoge mate van naleving te bieden. Dit zorgt ervoor dat gebruikers de verificatie in twee stappen voor aanmelding bij de extern bureaublad-Gateway maken. Voor gebruikers toegang te krijgen, moet deze de combinatie van gebruikersnaam en wachtwoord met informatie die de gebruiker in het besturingselement heeft opgeven. Deze informatie moet worden vertrouwd en eenvoudig niet wordt gedupliceerd, zoals een mobiele telefoonnummer, een vast, een toepassing op een mobiel apparaat, enzovoort.

Voorafgaand aan de beschikbaarheid van de NPS-extensie voor Azure moest-klanten die willen implementeren verificatie in twee stappen voor geïntegreerde NPS en Azure MFA-omgevingen configureren en onderhouden van een afzonderlijke MFA-Server in de on-premises-omgeving, zoals beschreven in [ Extern bureaublad-Gateway en Azure multi-factor Authentication-Server met behulp van RADIUS](multi-factor-authentication-get-started-server-rdg.md).

De beschikbaarheid van de NPS-extensie voor Azure biedt organisaties nu kiezen voor het implementeren van een on-premises op basis van MFA oplossing of een MFA-cloud-gebaseerde oplossing voor veilige verificatie van de RADIUS-client.

## <a name="authentication-flow"></a>Verificatiestroom

Gebruikers krijgen toegang tot netwerkbronnen via een extern bureaublad-Gateway, moeten voldoen aan de voorwaarden van een extern bureaublad-Verbindingsverificatiebeleid (RD CAP) en een extern bureaublad-bronautorisatiebeleid (RD RAP). RD CAP's opgeven die verbinding maken met extern bureaublad-Gateways is geautoriseerd. RD RAP's opgeven voor de netwerkbronnen, zoals externe bureaubladen of externe apps, die de gebruiker is toegestaan verbinding maken met via de RD-Gateway. 

Een extern bureaublad-Gateway kan worden geconfigureerd voor gebruik van een centraal beleid-archief voor RD CAP's. RD RAP's niet een centraal beleid gebruiken zoals ze worden verwerkt op de RD-Gateway. Een voorbeeld van een extern bureaublad-Gateway geconfigureerd voor gebruik van een centrale beleidsarchief voor RD CAP's is een RADIUS-client naar een andere NPS-server die als het centrale beleidsarchief fungeert.

Wanneer de NPS-extensie voor Azure is geïntegreerd met de NPS- en extern bureaublad-Gateway, is de geslaagde authenticatiestroom als volgt:

1. De extern bureaublad-gatewayserver ontvangt een verificatieaanvraag van een extern bureaublad-gebruiker verbinding maken met een resource, zoals een extern bureaublad-sessie. De extern bureaublad-gatewayserver fungeert als een RADIUS-client, converteert de aanvraag naar een RADIUS-Access-Request-bericht en verzendt het bericht naar de RADIUS-(NPS)-server waarop de NPS-uitbreiding is geïnstalleerd. 
2. De combinatie van gebruikersnaam en wachtwoord wordt geverifieerd in Active Directory en de gebruiker is geverifieerd.
3. Indien wordt voldaan aan de voorwaarden zoals opgegeven in de NPS-verbindingsaanvraag en het netwerkbeleid (bijvoorbeeld de tijd van de dag of groep lidmaatschap beperkingen), de NPS-extensie activeert een aanvraag voor de secundaire verificatie met Azure MFA. 
4. Azure MFA communiceert met Azure AD, haalt de details van de gebruiker en de secundaire authenticatie via de methode die is geconfigureerd door de gebruiker (SMS-bericht, mobiele app, enzovoort) wordt uitgevoerd. 
5. Bij voltooiing van de uitdaging MFA communiceert Azure MFA het resultaat naar de NPS-extensie.
6. De NPS-server waarop de uitbreiding is geïnstalleerd verzendt een RADIUS-Access-Accept-bericht voor het beleid RD CAP's naar de extern bureaublad-gatewayserver.
7. De gebruiker krijgt toegang tot de aangevraagde netwerkbron via de RD-Gateway.

## <a name="prerequisites"></a>Vereisten
In deze sectie beschrijft de vereisten die nodig zijn voordat de integratie van Azure MFA met extern bureaublad-Gateway. Voordat u begint, moet u de volgende vereisten hebben voldaan.  

* Infrastructuur voor extern bureaublad-Services (RDS)
* Azure MFA-licentie
* Windows Server-software
* De rol en Network Policy and Access Services (NPS)
* Azure AD gesynchroniseerd met on-premises AD dat 
* GUID-ID van Azure Active Directory

### <a name="remote-desktop-services-rds-infrastructure"></a>Infrastructuur voor extern bureaublad-Services (RDS)
U moet beschikken over een werkende extern bureaublad-Services (RDS)-infrastructuur. Als u dit niet doet, wordt u kunt deze infrastructuur snel maken in Azure met behulp van de volgende snel starten-sjabloon: [implementatie van extern bureaublad Sessieverzameling maken](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment). 

Volg de stappen voor het implementeren van een desgewenst handmatig maken van een on-premises RDS-infrastructuur snel voor testdoeleinden. 
**Meer informatie**: [RDS implementeren met Azure snel starten](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure) en [Basic RDS infrastructuur implementeren](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure). 

### <a name="licenses"></a>Licenties
Vereist, is een licentie voor Azure MFA, die beschikbaar via een Azure AD Premium, Enterprise Mobility plus Security (EMS) of een MFA-abonnement is. Zie voor meer informatie [het ophalen van Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md). Voor testdoeleinden kunt u een proefabonnement.

### <a name="software"></a>Software
De NPS-uitbreiding vereist Windows Server 2008 R2 SP1 of hoger als de NPS-functieservice die is geïnstalleerd. De stappen in deze sectie zijn uitgevoerd met behulp van Windows Server 2016.

### <a name="network-policy-and-access-services-nps-role"></a>De rol en Network Policy and Access Services (NPS)
De functieservice NPS biedt de RADIUS-server en client-functionaliteit, evenals een netwerktoegangsbeleid health-service. Deze functie moet worden geïnstalleerd op ten minste twee computers in uw infrastructuur: de extern bureaublad-Gateway en een andere lidserver of domeincontroller. De functie is standaard al aanwezig op de computer geconfigureerd als de extern bureaublad-Gateway.  U moet ook installeren de functie NPS op ten minste op een andere computer, zoals een lidserver of domeincontroller.

Voor informatie over het installeren van de NPS-rolservice service WindowsServer 2012 of ouder, Zie [installeren van een NAP-statusbeleidsserver](https://technet.microsoft.com/library/dd296890.aspx). Zie voor een beschrijving van best practices voor NPS, met inbegrip van de aanbeveling om NPS installeert op een domeincontroller [Best Practices voor NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory worden gesynchroniseerd met de lokale Active Directory 
Voor het gebruik van de NPS-extensie, zijn on-premises gebruikers gesynchroniseerd met Azure AD en ingeschakeld voor MFA. Deze sectie wordt ervan uitgegaan dat de on-premises gebruikers zijn gesynchroniseerd met Azure AD via AD Connect. Voor informatie over Azure AD connect, Zie [uw on-premises adreslijsten integreren met Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>GUID-ID van Azure Active Directory
U moet weten de GUID van de Azure AD voor de installatie van NPS. Hieronder vindt u instructies voor het vinden van de GUID van de Azure AD.

## <a name="configure-multi-factor-authentication"></a>Multi-factor Authentication configureren 
Deze sectie geeft instructies voor het integreren van Azure MFA met extern bureaublad-Gateway. Als beheerder, moet u de Azure MFA-service configureren voordat gebruikers hun apparaten meerledige of toepassingen zichzelf kunnen registreren.

Volg de stappen in [aan de slag met Azure multi-factor Authentication in de cloud](multi-factor-authentication-get-started-cloud.md) MFA inschakelen voor uw Azure AD-gebruikers. 

### <a name="configure-accounts-for-two-step-verification"></a>Accounts configureren voor verificatie in twee stappen
Wanneer een account is ingeschakeld voor MFA, u kunt zich niet in naar bronnen die worden bepaald door de MFA beleid totdat u een vertrouwd apparaat moet worden gebruikt voor de tweede verificatiefactor hebt geconfigureerd hebt geverifieerd met behulp van verificatie in twee stappen.

Volg de stappen in [wat Azure multi-factor Authentication betekent voor mij?](./end-user/multi-factor-authentication-end-user.md) om te begrijpen en uw apparaten goed geconfigureerd voor MFA met uw gebruikersaccount.

## <a name="install-and-configure-nps-extension"></a>Installeren en configureren van NPS-uitbreiding
Deze sectie geeft instructies voor het configureren van RDS-infrastructuur naar Azure MFA gebruiken voor clientverificatie met extern bureaublad-Gateway.

### <a name="acquire-azure-active-directory-guid-id"></a>ID van Azure Active Directory-GUID verkrijgen

Als onderdeel van de configuratie van de NPS-extensie moet u beheerdersreferenties en de Azure AD-ID opgeven voor uw Azure AD-tenant. De volgende stappen ziet u het ophalen van de tenant-ID.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder van de Azure-tenant.
2. Selecteer in de navigatiebalk links de **Azure Active Directory** pictogram.
3. Selecteer **eigenschappen**.
4. Klik op de blade eigenschappen naast de map-ID, de **kopie** pictogram, zoals hieronder aangegeven, de ID naar Klembord kopiëren.

 ![Eigenschappen](./media/nps-extension-remote-desktop-gateway/image1.png)

### <a name="install-the-nps-extension"></a>De NPS-uitbreiding installeren
Installeer de NPS-extensie op een server met de serverrol Network Policy and Access Services (NPS) is geïnstalleerd. Dit fungeert als de RADIUS-server voor uw ontwerp. 

>[!Important]
> Zorg ervoor dat u niet de NPS-extensie op uw server voor extern bureaublad-Gateway installeert.
> 

1. Download de [NPS extensie](https://aka.ms/npsmfa). 
2. Kopieer het setup-bestand (NpsExtnForAzureMfaInstaller.exe) naar de NPS-server.
3. Dubbelklik op de NPS-server op **NpsExtnForAzureMfaInstaller.exe**. Als u wordt gevraagd, klikt u op **uitvoeren**.
4. In de NPS-extensie voor het dialoogvenster Azure MFA, controleert u de softwarelicentievoorwaarden controleren **ik ga akkoord met de licentievoorwaarden en voorwaarden**, en klik op **installeren**.
 
  ![Azure MFA-instellingen](./media/nps-extension-remote-desktop-gateway/image2.png)

5. Klik op Sluiten in de NPS-extensie voor Azure MFA-dialoogvenster. 

  ![NPS-extensie voor Azure MFA](./media/nps-extension-remote-desktop-gateway/image3.png)

### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Configureren van certificaten voor gebruik met de NPS-extensie met een PowerShell-script
Vervolgens moet u om certificaten te configureren voor gebruik door de NPS-extensie om ervoor te zorgen beveiligde communicatie en zekerheid. De NPS-onderdelen zijn Windows PowerShell-script waarmee een zelfondertekend certificaat voor gebruik met NPS worden geconfigureerd. 

Het script voert de volgende handelingen uit:

* Een zelfondertekend certificaat gemaakt
* Koppelt de openbare sleutel van certificaat voor service-principal over Azure AD
* Het certificaat dat is opgeslagen in het archief van de lokale computer
* Verleent toegang tot de persoonlijke sleutel van het certificaat voor de netwerkgebruiker
* Network Policy Server-service wordt opnieuw gestart

Als u wilt uw eigen certificaten te gebruiken, moet u het publiek van uw certificaat aan het principe van de service op Azure AD te koppelen, enzovoort.

Voor het gebruik van het script, voorzien van de extensie Azure AD-beheerdersreferenties en de Azure AD-tenant-ID die u eerder hebt gekopieerd. Het script uitvoeren op elke NPS-server waarop u de NPS-uitbreiding hebt geïnstalleerd. Ga daarna als volgt te werk:

1. Open een administratieve Windows PowerShell-prompt.
2. Typ het volgende achter de PowerShell-prompt **cd 'c:\Program Files\Microsoft\AzureMfa\Config'**, en druk op **ENTER**.
3. Type _.\AzureMfsNpsExtnConfigSetup.ps1_, en druk op **ENTER**. Het script wordt gecontroleerd of de Azure Active Directory PowerShell-module is geïnstalleerd. Als niet geïnstalleerd, installeert u de module met het script voor u.

  ![Azure AD PowerShell](./media/nps-extension-remote-desktop-gateway/image4.png)
  
4. Nadat het script wordt gecontroleerd door de installatie van de PowerShell-module, wordt het dialoogvenster Azure Active Directory PowerShell-module weergegeven. Voer in het dialoogvenster uw Azure AD-referenties en het wachtwoord in en klikt u op **aanmelden**.

  ![Powershell-account openen](./media/nps-extension-remote-desktop-gateway/image5.png)

5. Wanneer u wordt gevraagd, plak de tenant-ID die u eerder naar het Klembord hebt gekopieerd en druk op **ENTER**.

  ![Tenant-ID invoeren](./media/nps-extension-remote-desktop-gateway/image6.png)

6. Het script maakt een zelfondertekend certificaat en andere configuratiewijzigingen uitvoert. De uitvoer moet zoals de afbeelding hieronder wordt weergegeven.

  ![Zelfondertekend certificaat](./media/nps-extension-remote-desktop-gateway/image7.png)

## <a name="configure-nps-components-on-remote-desktop-gateway"></a>NPS-onderdelen in extern bureaublad-Gateway configureren
In deze sectie configureert u de extern bureaublad-Gateway-Verbindingsautorisatiebeleid en andere instellingen voor RADIUS.

De authenticatiestroom is vereist dat RADIUS-berichten worden uitgewisseld tussen de extern bureaublad-Gateway en de NPS-server waarop de NPS-Server is geïnstalleerd. Dit betekent dat moet u de instellingen voor RADIUS-client configureren voor zowel extern bureaublad-Gateway en de NPS-server waarop de NPS-uitbreiding is geïnstalleerd. 

### <a name="configure-remote-desktop-gateway-connection-authorization-policies-to-use-central-store"></a>Extern bureaublad-gatewayserver Verbindingsautorisatiebeleid voor het gebruik van centrale opslag configureren
Extern bureaublad Verbindingsautorisatiebeleid (RD CAP's), geef de vereisten voor de verbinding met extern bureaublad-gatewayserver. Extern bureaublad Verbindingsautorisatiebeleid kan lokaal worden opgeslagen (standaard) of ze kunnen worden opgeslagen in een centraal RD CAP-archief waarop NPS wordt uitgevoerd. Om de integratie van Azure MFA met RDS configureren, moet u het gebruik van een centrale store opgeven.

1. Open op de extern bureaublad-gatewayserver **Serverbeheer**. 
2. Klik op het menu **extra**, wijs **extern bureaublad-Services**, en klik vervolgens op **extern bureaublad-gatewaybeheer**.

  ![Externe bureaubladservices](./media/nps-extension-remote-desktop-gateway/image8.png)

3. Met de rechtermuisknop in de RD-Gateway-Manager  **\[servernaam\] (lokaal)**, en klik op **eigenschappen**.

  ![Servernaam](./media/nps-extension-remote-desktop-gateway/image9.png)

4. Selecteer in het dialoogvenster Eigenschappen de **RD CAP** tabblad archief.
5. Selecteer op het tabblad archief van extern bureaublad **centrale Server met NPS**. 
6. In de **Voer een naam of IP-adres voor de server met NPS** veld, typt u het IP-adres of de servernaam van de server waarop u de NPS-uitbreiding hebt geïnstalleerd.

  ![Voer de naam of IP-adres](./media/nps-extension-remote-desktop-gateway/image10.png)
  
7. Klik op **Add**.
8. In de **gedeelde geheim** in het dialoogvenster voert u een gedeeld geheim en klik vervolgens op **OK**. Zorg ervoor dat u dit gedeelde geheim vastlegt en veilig opslaan van de record.

 >[!NOTE]
 >Gedeeld geheim wordt gebruikt om een vertrouwensrelatie tussen de RADIUS-servers en clients. Een lange en complexe wachtwoord maken.
 >

 ![Gedeeld geheim](./media/nps-extension-remote-desktop-gateway/image11.png)

9. Klik op **OK** om het dialoogvenster te sluiten.

### <a name="configure-radius-timeout-value-on-remote-desktop-gateway-nps"></a>RADIUS-time-outwaarde op extern bureaublad-Gateway NPS configureren
Verificatie in twee stappen uitvoeren om te controleren of er voldoende tijd om gebruikers referenties te valideren is, antwoorden ontvangen en reageren op RADIUS-berichten, is het nodig is om aan te passen van de RADIUS-time-outwaarde.

1. Klik op de extern bureaublad-gatewayserver, in Serverbeheer op **extra**, en klik vervolgens op **Network Policy Server**. 
2. In de **NPS (lokaal)** console, vouw **RADIUS-Clients en Servers**, en selecteer **externe RADIUS-Server**.

 ![Externe RADIUS-Server](./media/nps-extension-remote-desktop-gateway/image12.png)

3. Dubbelklik in het detailvenster op **groep TS-GATEWAYSERVER**.

 >[!NOTE]
 >Deze groep van RADIUS-Server is gemaakt toen u de centrale server geconfigureerd voor NPS-beleid. De RD-Gateway stuurt RADIUS-berichten naar deze server of de groep servers als meer dan één in de groep.
 >

4. In de **eigenschappen TS GATEWAY SERVERGROEP** dialoogvenster Selecteer de IP-adres of de naam van de NPS-server die u hebt geconfigureerd voor het opslaan van RD CAP's en klik vervolgens op **bewerken**. 

 ![Groep TS-gatewayserver](./media/nps-extension-remote-desktop-gateway/image13.png)

5. In de **RADIUS-Server bewerken** selecteert u de **Load Balancing** tabblad.
6. In de **Load Balancing** tabblad, in de **verwijderd van het aantal seconden zonder reactie voordat de aanvraag wordt beschouwd als** veld, wijzigt u de standaardwaarde van 3 op een waarde tussen 30 en 60 seconden.
7. In de **aantal seconden tussen aanvragen wanneer de server wordt geïdentificeerd als niet beschikbaar** veld, wijzigt u de standaardwaarde van 30 seconden op een waarde die gelijk is aan of groter dan de waarde die u hebt opgegeven in de vorige stap.

 ![RADIUS-Server bewerken](./media/nps-extension-remote-desktop-gateway/image14.png)

8.  Klik twee keer op OK om de dialoogvensters te sluiten.

### <a name="verify-connection-request-policies"></a>Controleer of u beleid voor verbindingsaanvragen 
Bij het configureren van de RD-Gateway met een centrale beleidsarchief voor Verbindingsautorisatiebeleid, wordt de RD-Gateway standaard geconfigureerd voor het doorsturen van CAP aanvragen naar de NPS-server. De NPS-server met de Azure MFA-extensie is geïnstalleerd, verwerkt de RADIUS-toegangsaanvraag. De volgende stappen laten zien hoe het standaardbeleid voor verbindingsaanvragen controleren. 

1. Vouw op de RD-Gateway in de console NPS (lokaal) **beleid**, en selecteer **beleid voor verbindingsaanvragen**.
2. Met de rechtermuisknop op **verbinding beleid**, en dubbelklik op **TS GATEWAY-AUTORISATIEBELEID**.
3. In de **TS GATEWAY-AUTORISATIEBELEID eigenschappen** in het dialoogvenster, klikt u op de **instellingen** tabblad.
4. Op **instellingen** tabblad onder verbindingsaanvraag, klikt u op **verificatie**. RADIUS-client is geconfigureerd voor doorsturen van aanvragen voor verificatie.

 ![Verificatie-instellingen](./media/nps-extension-remote-desktop-gateway/image15.png)
 
5. Klik op **annuleren**. 

## <a name="configure-nps-on-the-server-where-the-nps-extension-is-installed"></a>NPS configureren op de server waarop de NPS-uitbreiding is geïnstalleerd
De NPS-server waarop de NPS-uitbreiding is geïnstalleerd moet kunnen voor het uitwisselen van RADIUS-berichten met de NPS-server op de extern bureaublad-Gateway. Om deze uitwisseling van berichten, moet u de NPS-onderdelen configureren op de server waarop de NPS-extensie-service is geïnstalleerd. 

### <a name="register-server-in-active-directory"></a>Server registreren in Active Directory
De NPS-server moet worden geregistreerd in Active Directory een correcte werking in dit scenario.

1. Open **Serverbeheer**.
2. Klik in Serverbeheer op **extra**, en klik vervolgens op **Network Policy Server**. 
3. In de console Network Policy Server met de rechtermuisknop op **NPS (lokaal)**, en klik vervolgens op **server registreren in Active Directory**. 
4. Klik op **OK** twee keer.

 ![Server registreren in AD](./media/nps-extension-remote-desktop-gateway/image16.png)

5. Laat de console geopend voor de volgende procedure.

### <a name="create-and-configure-radius-client"></a>Maak en configureer de RADIUS-client 
De extern bureaublad-Gateway moet worden geconfigureerd als een RADIUS-client naar de NPS-server. 

1. Op de NPS-server waarop de NPS-uitbreiding is geïnstalleerd, in de **NPS (lokaal)** console, met de rechtermuisknop op **RADIUS-Clients** en klik op **nieuw**.

 ![Nieuwe RADIUS-Clients](./media/nps-extension-remote-desktop-gateway/image17.png)

2. In de **nieuwe RADIUS-client** dialoogvenster Geef een beschrijvende naam, zoals _Gateway_, en het IP-adres of de DNS-naam van de extern bureaublad-gatewayserver. 
3. In de **gedeeld geheim** en de **Bevestig het gedeelde geheim** velden, Voer hetzelfde geheim dat u eerder hebt gebruikt.

 ![Naam en adres](./media/nps-extension-remote-desktop-gateway/image18.png)

4. Klik op **OK** te sluiten van het dialoogvenster Nieuwe RADIUS-client.

### <a name="configure-network-policy"></a>Netwerkbeleid configureren
Intrekken van dat de NPS-server met de Azure MFA-extensie is het aangewezen centraal beleidsarchief voor de verbinding autorisatie beleid (CAP). Daarom moet u een LIMIET op de NPS-server te autoriseren geldige verbindingen aanvragen implementeren.  

1. Vouw in de console NPS (lokaal) **beleid**, en klik op **netwerkbeleid**.
2. Met de rechtermuisknop op **verbindingen met andere toegangsservers**, en klik op **beleid dubbele**. 

 ![Dubbele beleid](./media/nps-extension-remote-desktop-gateway/image19.png)

3. Met de rechtermuisknop op **kopie van verbindingen met andere toegangsservers**, en klik op **eigenschappen**.

 ![Eigenschappen van het netwerk](./media/nps-extension-remote-desktop-gateway/image20.png)

4. In de **kopie van verbindingen met andere toegangsservers** in het dialoogvenster beleidsnaam, Voer in een geschikte naam zoals **RDG_CAP**. Controleer **beleid ingeschakeld**, en selecteer **toegang verlenen**. Selecteer desgewenst in het Type toegang tot het netwerk, **extern bureaublad-Gateway**, of u kunt dit als laten **onbepaald**.

 ![Kopie van verbindingen](./media/nps-extension-remote-desktop-gateway/image21.png)

5.  Klik op de **beperkingen** tabblad en Controleer **clients toestaan verbinding zonder een verificatiemethode te onderhandelen te**.

 ![Clients toestaan verbinding te](./media/nps-extension-remote-desktop-gateway/image22.png)

6. Klik desgewenst op de **voorwaarden** tabblad en voorwaarden die moeten worden voldaan om de verbinding worden geautoriseerd, bijvoorbeeld, lidmaatschap van een specifieke windowsgroep toevoegen.

 ![Voorwaarden](./media/nps-extension-remote-desktop-gateway/image23.png)

7. Klik op **OK**. Wanneer u wordt gevraagd om de bijbehorende Help-onderwerp weer te geven, klikt u op **Nee**.
8. Zorg ervoor dat het nieuwe beleid boven aan de lijst, dat het beleid is ingeschakeld, en dat toegang wordt verleend.

 ![Netwerkbeleid](./media/nps-extension-remote-desktop-gateway/image24.png)

## <a name="verify-configuration"></a>Configuratie controleren
Om te controleren of de configuratie, moet u de extern bureaublad-Gateway aanmelden met een geschikte RDP-client. Zorg dat u een account gebruiken dat is toegestaan door uw Verbindingsautorisatiebeleid en is ingeschakeld voor Azure MFA. 

Als weergeven in de onderstaande afbeelding, kunt u de **Remote Desktop Web Access** pagina.

![Extern bureaublad-webtoegang](./media/nps-extension-remote-desktop-gateway/image25.png)

Nadat het is uw referenties invoeren voor primaire verificatie, wordt in het dialoogvenster Extern bureaublad-verbinding een status van de externe verbinding tot stand brengen, zoals hieronder wordt weergegeven. 

Als u verificatie is met de secundaire verificatie-methode die u eerder hebt geconfigureerd in de Azure MFA, kunt u bent verbonden met de resource. Als de secundaire verificatie niet lukt, wordt u toegang tot bron geweigerd. 

![Externe verbinding tot stand brengen](./media/nps-extension-remote-desktop-gateway/image26.png)

In het volgende voorbeeld wordt de verificator-app op een Windows phone gebruikt om de secundaire verificatie te bieden.

![Accounts](./media/nps-extension-remote-desktop-gateway/image27.png)

Nadat u bent geverifieerd met de secundaire authenticatiemethode, kunt u bent aangemeld bij de extern bureaublad-Gateway als normaal. Het proces aanmelden is echter veiliger dan het anders zou zijn omdat u een methode van de secundaire verificatie via een mobiele app op een vertrouwd apparaat gebruiken moet.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Logboeken voor geslaagde aanmeldingsgebeurtenissen weergeven
Als u wilt de gebeurtenissen voor geslaagde aanmelding in Windows logboeken bekijken, kunt u de volgende Windows PowerShell-opdracht voor het opvragen van de logboeken van de Windows Terminal Services en Windows-beveiliging uitgeven.

Query uitvoeren op gebeurtenissen voor geslaagde aanmelding in de operationele logboeken van de Gateway _(gebeurtenis logboeken\logboeken toepassingen en Services Logs\Microsoft\Windows\TerminalServices-Gateway\Operational_, gebruik de volgende opdrachten:

* _Get-WinEvent - logboeknaam Microsoft-Windows-TerminalServices-Gateway/operationeel_ | waar {$_.ID - eq '300'} | FL 
* Deze opdracht geeft Windows-gebeurtenissen die de gebruiker worden voldaan aan de beleidsvereisten autorisatie van resource (RD RAP) en heeft toegang gekregen weergeven.

![Logboeken weergeven](./media/nps-extension-remote-desktop-gateway/image28.png)

* _Get-WinEvent - logboeknaam Microsoft-Windows-TerminalServices-Gateway/operationeel_ | waar {$_.ID - eq "200"} | FL
* Deze opdracht worden de gebeurtenissen die worden weergegeven wanneer de gebruiker verbinding autorisatievereisten voldaan.

![Verbindingsverificatie](./media/nps-extension-remote-desktop-gateway/image29.png)

U kunt dit logboek en filter ook bekijken op de gebeurtenis-id's, 300 en 200. Om te vragen van geslaagde aanmeldingsgebeurtenissen worden geregistreerd in de logboeken van de beveiliging, gebruik de volgende opdracht:

* _Get-WinEvent - logboeknaam beveiliging_ | waar {$_.ID - eq '6272'} | FL 
* Met deze opdracht kan worden uitgevoerd op de centrale NPS of de extern bureaublad-gatewayserver. 

![Geslaagde aanmeldingsgebeurtenissen](./media/nps-extension-remote-desktop-gateway/image30.png)

U kunt ook het beveiligingslogboek of de Network Policy and Access Services aangepaste weergave, bekijken, zoals hieronder wordt weergegeven:

![Network Policy and Access Services](./media/nps-extension-remote-desktop-gateway/image31.png)

Op de server waar u de NPS-extensie voor Azure MFA hebt geïnstalleerd, vindt u Event Viewer-logboeken specifiek zijn voor de uitbreiding op _toepassingen en Services Logs\Microsoft\AzureMfa_. 

![Toepassing van de logboeken](./media/nps-extension-remote-desktop-gateway/image32.png)

## <a name="troubleshoot-guide"></a>Problemen met de gids

Als de configuratie niet werkt zoals verwacht, is de eerste plaats om te starten om op te lossen om te controleren of de gebruiker is geconfigureerd voor het gebruik van Azure MFA. Een gebruiker verbinding maken met de [Azure-portal](https://portal.azure.com). Als gebruikers wordt gevraagd om secundaire verificatie en kunnen worden geverifieerd, kunt u een onjuiste configuratie van Azure MFA elimineren.

Als u Azure MFA is voor de gebruiker (s) werkt, moet u de relevante gebeurtenislogboeken bekijken. Het gaat hierbij om de gebeurtenis voor beveiliging, operationele Gateway en Azure MFA-logboeken die worden beschreven in de vorige sectie. 

Hieronder volgt een voorbeeld van uitvoer van beveiligingslogboek met een mislukte aanmeldingsgebeurtenis (gebeurtenis-ID 6273).

![Mislukte aanmeldingsgebeurtenis](./media/nps-extension-remote-desktop-gateway/image33.png)

Hieronder vindt u een bijbehorende gebeurtenis uit de AzureMFA Logboeken:

![Azure MFA-logboek](./media/nps-extension-remote-desktop-gateway/image34.png)

Uitvoeren van geavanceerde opties oplossen, raadpleegt u de NPS-indeling databaselogboekbestanden waarop de NPS-service is geïnstalleerd. Deze logboekbestanden worden gemaakt in _%SystemRoot%\System32\Logs_ map als CSV-bestanden. 

Voor een beschrijving van deze logboekbestanden, Zie [interpreteren NPS indeling databaselogboekbestanden](https://technet.microsoft.com/library/cc771748.aspx). De vermeldingen in deze logboekbestanden kunnen lastig zijn om te interpreteren zonder ze te importeren in een database of een werkblad. Helpt u bij het interpreteren van de logboekbestanden kunt u verschillende IAS parsers online vinden. 

De onderstaande afbeelding ziet u de uitvoer van een dergelijke downloadbare [shareware toepassing](http://www.deepsoftware.com/iasviewer). 

![Shareware app](./media/nps-extension-remote-desktop-gateway/image35.png)

Ten slotte voor extra opties oplossen, kunt u een programma voor protocolanalyse, dergelijke [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). 

De afbeelding hieronder van Microsoft Message Analyzer netwerkverkeer gefilterd op RADIUS-protocol met de naam van de gebruiker ziet **Contoso\AliceC**.

![Microsoft Message Analyzer](./media/nps-extension-remote-desktop-gateway/image36.png)

## <a name="next-steps"></a>Volgende stappen
[Azure Multi-Factor Authentication gebruiken](multi-factor-authentication-versions-plans.md)

[Extern bureaublad-gateway en Azure Multi-Factor Authentication-server met behulp van RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Uw on-premises adreslijsten integreren met Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)
