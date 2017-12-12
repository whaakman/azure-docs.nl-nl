---
title: Bestaande NPS-servers gebruiken om aan te bieden mogelijkheden van Azure MFA | Microsoft Docs
description: De Network Policy Server-extensie voor Azure multi-factor Authentication is een eenvoudige oplossing cloud-gebaseerde in twee stappen vericiation mogelijkheden toevoegen aan uw bestaande infrastructuur voor verificatie.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: 8c3ceb8a455ea32ebbd1393aa2484b8b8c898ef8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>Uw bestaande NPS-infrastructuur integreren met Azure multi-factor Authentication

De Network Policy Server (NPS)-extensie voor Azure MFA biedt een voor de verificatie-infrastructuur met behulp van uw bestaande servers cloud-gebaseerde MFA mogelijkheden. Met de extensie NPS kunt u telefoongesprek, tekstbericht of verificatie via de telefoon-app toevoegen aan uw bestaande authenticatiestroom zonder te installeren, configureren en onderhouden van nieuwe servers. 

Deze uitbreiding is gemaakt voor organisaties die beveiligen van VPN-verbindingen willen zonder de Azure MFA-Server implementeren. De NPS-extensie fungeert als een adapter tussen RADIUS- en cloud-gebaseerde Azure MFA voor een tweede factor van verificatie voor federatieve of gebruikers gesynchroniseerde.

Wanneer u de NPS-extensie voor Azure MFA, is de authenticatiestroom omvat de volgende onderdelen: 

1. **NAS/VPN-Server** ontvangt verzoeken van VPN-clients en converteert deze naar RADIUS-aanvragen naar de NPS-servers. 
2. **NPS-Server** maakt verbinding met Active Directory uit te voeren van de primaire verificatie voor de RADIUS-aanvragen en dit lukt, geeft u de aanvraag aan eventuele geïnstalleerde uitbreidingen.  
3. **NPS-extensie** een aanvraag voor Azure MFA voor de secundaire verificatie wordt geactiveerd. Als het antwoord wordt ontvangen door de uitbreiding en als de MFA-controle is geslaagd, de verificatieaanvraag is voltooid door de NPS-server bieden beveiligingstokens die een claim MFA opnemen, uitgegeven door Azure STS.  
4. **Azure MFA** communiceert met Azure Active Directory om het ophalen van gegevens van de gebruiker en de secundaire authenticatie met behulp van een verificatiemethode die is geconfigureerd voor de gebruiker wordt uitgevoerd.

Het volgende diagram ziet u deze op hoog niveau authenticatiestroom voor aanvraag: 

![Stroomdiagram voor verificatie](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Uw implementatie plannen

De NPS-extensie verwerkt automatisch redundantie, dus u een speciale configuratie hoeft.

U kunt zoveel Azure MFA-functionaliteit van NPS-servers als u nodig hebt. Als u meerdere servers installeert, moet u een clientcertificaat verschil voor elk van deze. Een certificaat voor elke server maakt, wordt u kunt elke cert afzonderlijk bijwerken en u geen zorgen over uitvaltijd in al uw servers.

VPN-servers routeren verificatieaanvragen, zodat ze moeten rekening houden met de nieuwe Azure MFA-functionaliteit van NPS-servers.

## <a name="prerequisites"></a>Vereisten

De NPS-extensie is bedoeld voor gebruik met uw bestaande infrastructuur. Zorg ervoor dat u hebt de volgende vereisten voordat u begint.

### <a name="licenses"></a>Licenties

De NPS-extensie voor Azure MFA is beschikbaar voor klanten met [licenties voor Azure multi-factor Authentication](multi-factor-authentication.md) (meegeleverd bij Azure AD Premium, EMS of een zelfstandige licentie MFA). Op basis van verbruik licenties voor Azure MFA zoals per gebruiker of per authenticatie licenties zijn niet compatibel met de NPS-extensie. 

### <a name="software"></a>Software

Windows Server 2008 R2 SP1 of hoger.

### <a name="libraries"></a>Bibliotheken

Deze bibliotheken worden automatisch geïnstalleerd met de extensie.

-   [Visual C++ Redistributable Packages for Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory-Module voor Windows PowerShell versie 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

De Microsoft Azure Active Directory-Module voor Windows PowerShell is geïnstalleerd, als deze nog niet aanwezig is, via een configuratiescript die u als onderdeel van het installatieproces uitvoert. Er is niet nodig voor het installeren van deze module tevoren als deze nog niet is geïnstalleerd.

### <a name="azure-active-directory"></a>Azure Active Directory

Iedereen met behulp van de NPS-uitbreiding moet worden gesynchroniseerd naar Azure Active Directory met Azure AD Connect en moet zijn geregistreerd voor MFA.

Wanneer u de extensie installeert, moet u de directory-ID en de Administrator-referenties voor uw Azure AD-tenant. U vindt de ID van uw directory in de [Azure-portal](https://portal.azure.com). Meld u aan als beheerder, selecteer de **Azure Active Directory** pictogram aan de linkerkant, selecteer vervolgens **eigenschappen**. Kopieer de GUID in de **map-ID** vak en op te slaan. U kunt deze GUID als de tenant-ID gebruiken bij het installeren van de NPS-extensie.

![De ID van uw Directory onder de eigenschappen van de Azure Active Directory zoeken](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a>Uw omgeving voorbereiden

Voordat u de extensie NPS installeert, die u wilt u de omgeving voor het afhandelen van het verificatieverkeer voorbereiden.

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a>De NPS-rol op een domein-server inschakelen

De NPS-server maakt verbinding met Azure Active Directory en verifieert de MFA-aanvragen. Kies een server voor deze rol. Het is raadzaam om het kiezen van een server die aanvragen van andere services kunnen niet worden verwerkt omdat de extensie NPS genereert fouten voor alle aanvragen die niet RADIUS zijn. De NPS-server moet worden ingesteld als de primaire en secundaire authentication-server voor uw omgeving. proxy RADIUS-aanvragen naar een andere server niet.

1. Open op uw server, de **Wizard Functies toevoegen en onderdelen** in het menu Serverbeheer Quickstart.
2. Kies **op basis van functie of onderdeel gebaseerde installatie** voor uw installatietype.
3. Selecteer de **Network Policy and Access Services** serverfunctie. Een venster kan van de vereiste onderdelen voor het uitvoeren van deze rol weergegeven.
4. Met de wizard worden voortgezet totdat de pagina bevestiging. Selecteer **installeren**.

Nu dat u een aangewezen voor NPS-server hebt, moet u deze server voor het afhandelen van binnenkomende RADIUS-aanvragen van de VPN-oplossing ook configureren.

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a>Uw VPN-oplossing om te communiceren met de NPS-server configureren

Afhankelijk van welke VPN-oplossing u variëren de stappen voor het configureren van uw RADIUS-verificatiebeleid. Deze instelling om te verwijzen naar de RADIUS-NPS-server is geconfigureerd.

### <a name="sync-domain-users-to-the-cloud"></a>Synchronisatie van domeingebruikers aan de cloud

Deze stap is mogelijk al voltooid voor uw tenant, maar het is handig om te controleren of Azure AD Connect uw databases onlangs is gesynchroniseerd.

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **Azure AD Connect**
3. Controleer of de synchronisatiestatus van uw **ingeschakeld** en dat de laatste synchronisatie minder dan een uur geleden.

Als u wilt ere van een nieuwe ronde van synchronisatie ons de instructies in [Azure AD Connect-synchronisatie: Scheduler](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).

### <a name="determine-which-authentication-methods-your-users-can-use"></a>Bepalen welke verificatiemethoden die uw gebruikers gebruiken kunnen.

Er zijn twee factoren die van invloed zijn op welke verificatiemethoden zijn beschikbaar bij de implementatie van een NPS-extensie:

1. Het algoritme gebruikt tussen de RADIUS-client (VPN, Netscaler-server, of andere) en de NPS-servers.
   - **PAP** ondersteunt de verificatiemethoden van Azure MFA in de cloud: telefoonoproep, eenzijdige SMS-bericht, mobiele-appmelding en verificatiecode mobiele app.
   - **CHAPv2** en **EAP** telefoongesprek en mobiele-appmelding ondersteunen.
2. De invoer methoden die de clienttoepassing (VPN, Netscaler-server, of andere) kan verwerken. Bijvoorbeeld, heeft de VPN-client een manier om toe te staan van de gebruiker te typen in een verificatiecode uit een tekst- of mobiele app?

Wanneer u de extensie NPS implementeert, moet u deze factoren gebruiken om te beoordelen welke methoden zijn beschikbaar voor uw gebruikers. Als de RADIUS-client PAP ondersteunt, maar de client UX geen invoervelden voor een verificatiecode, vervolgens zijn telefoongesprek en mobiele-appmelding de twee ondersteunde opties.

U kunt [uitschakelen van niet-ondersteunde verificatiemethoden](multi-factor-authentication-whats-next.md#selectable-verification-methods) in Azure.

### <a name="enable-users-for-mfa"></a>Gebruikers voor MFA inschakelen

Voordat u de volledige uitbreiding van NPS implementeren, moet u MFA inschakelen voor de gebruikers die u wilt uitvoeren van verificatie in twee stappen. Meer onmiddellijk om te testen de extensie als u deze implementeert, moet u minstens één test-account dat volledig is geregistreerd voor multi-factor Authentication.

Gebruik de volgende stappen uit om een testaccount gestart:
1. Aanmelden bij [https://aka.ms/mfasetup](https://aka.ms/mfasetup) met een testaccount. 
2. Volg de aanwijzingen voor het instellen van een verificatiemethode.
3. Maak een beleid voor voorwaardelijke toegang of [wijzigen van de gebruikersstatus](multi-factor-authentication-get-started-user-states.md) verificatie in twee stappen vereist voor de testaccount. 

Uw gebruikers moeten ook als volgt te werk om in te schrijven voordat ze kunnen worden geverifieerd met de NPS-extensie.

## <a name="install-the-nps-extension"></a>De NPS-uitbreiding installeren

> [!IMPORTANT]
> Installeer de NPS-extensie op een andere server dan het toegangspunt VPN.

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a>Download en installeer de NPS-extensie voor Azure MFA

1.  [Download de NPS-extensie](https://aka.ms/npsmfa) uit het Microsoft Download Center.
2.  Het binaire bestand kopiëren naar de Network Policy Server die u wilt configureren.
3.  Voer *setup.exe* en volg de installatie-instructies. Als er fouten optreden, moet u controleren dat de twee bibliotheken van de sectie vereisten zijn geïnstalleerd.

### <a name="run-the-powershell-script"></a>Het PowerShell-script uitvoeren

Het installatieprogramma maakt een PowerShell-script op deze locatie: `C:\Program Files\Microsoft\AzureMfa\Config` (waarbij C:\ is voor uw installatiestation). Dit PowerShell-script voert de volgende handelingen uit:

-   Een zelfondertekend certificaat maken.
-   De openbare sleutel van het certificaat naar de service-principal over Azure AD koppelt.
-   Sla het certificaat in het certificaatarchief van de lokale computer.
-   Toegang verlenen tot de persoonlijke sleutel van het certificaat voor netwerk-gebruiker.
-   Start opnieuw op de NPS.

Tenzij u wilt gebruiken van uw eigen certificaten (in plaats van de zelfondertekende certificaten die het PowerShell-script genereert), voer de PowerShell-Script om de installatie te voltooien. Als u de extensie op meerdere servers installeert, moet elk criterium een eigen certificaat hebben.

1. Voer Windows PowerShell als beheerder.
2. Wijzig de mappen.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. Voer de PowerShell-script dat is gemaakt door het installatieprogramma.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. Aanmelden bij Azure AD als een beheerder.
5. PowerShell wordt u gevraagd om uw tenant-ID. Gebruik de GUID van de Directory-ID die u hebt gekopieerd uit de Azure-portal in de sectie vereisten.
6. PowerShell toont een bericht wanneer het script is voltooid.  

Herhaal deze stappen uit op eventuele aanvullende NPS-servers die u instellen wilt voor taakverdeling.

>[!NOTE]
>Als u uw eigen certificaten gebruiken in plaats van genereren van certificaten met de PowerShell-script, ervoor zorgen dat ze naar de NPS-naamconventie uitgelijnd. De onderwerpnaam moet **CN =\<TenantID\>, OU = Microsoft NPS extensie**. 

## <a name="configure-your-nps-extension"></a>De NPS-uitbreiding configureren

Deze sectie bevat overwegingen bij het ontwerpen en suggesties voor geslaagde implementaties voor NPS-extensie.

### <a name="configuration-limitations"></a>Configuratie-beperkingen

- De NPS-extensie voor Azure MFA omvat geen hulpprogramma's voor gebruikers en -instellingen van MFA-Server migreren naar de cloud. Daarom is het raadzaam met de extensie voor nieuwe implementaties in plaats van bestaande implementatie. Als u de uitbreiding van een bestaande implementatie gebruikt, worden uw gebruikers hebben om uit te voeren bewijs-up opnieuw uit om te vullen van de details van MFA in de cloud.  
- De NPS-extensie gebruikt de UPN van de lokale Active directory voor het identificeren van de gebruiker op de Azure MFA voor het uitvoeren van de secundaire Auth. De extensie kan worden geconfigureerd voor het gebruik van een andere id zoals alternatieve aanmeldings-ID of aangepaste Active Directory-veld dan UPN. Zie [geavanceerde configuratieopties voor de NPS-extensie voor multi-factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) voor meer informatie.
- Niet alle versleuteling protocollen ondersteunen alle verificatiemethoden voor.
   - **PAP** ondersteunt telefoongesprek, eenzijdige SMS-bericht, mobiele-appmelding en verificatiecode mobiele app
   - **CHAPv2** en **EAP** ondersteuning voor telefoongesprekken en meldingen voor mobiele Apps

### <a name="control-radius-clients-that-require-mfa"></a>Besturingselement RADIUS-clients die MFA vereisen

Als u MFA voor een RADIUS-client met behulp van de NPS-extensie inschakelen, worden alle verificaties voor deze client om uit te voeren MFA vereist. Als u wilt inschakelen van MFA voor bepaalde RADIUS-clients, maar andere niet, kunt u twee NPS-servers configureren en installeren van de extensie op slechts één van beide. Configureer de RADIUS-clients die u wilt Vereis MFA voor het verzenden van aanvragen naar de NPS-server geconfigureerd met de extensie en andere RADIUS-clients naar de NPS-server niet is geconfigureerd met de extensie.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Voorbereiden voor gebruikers die niet zijn geregistreerd voor MFA

Als u gebruikers die niet zijn geregistreerd voor MFA hebt, kunt u bepalen wat er gebeurt wanneer ze proberen te verifiëren. Gebruik de registerinstelling *REQUIRE_USER_MATCH* in het registerpad *HKLM\Software\Microsoft\AzureMFA* het gedrag van de functie te bepalen. Deze instelling heeft een configuratie voor één optie:

| Sleutel | Waarde | Standaard |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | WAAR/ONWAAR | Niet ingesteld (equivalent op TRUE) |

Het doel van deze instelling is om te bepalen wat te doen wanneer een gebruiker niet is ingeschreven voor MFA. Wanneer de sleutel bestaat niet, is niet ingesteld of is ingesteld op TRUE, en de gebruiker niet is ingeschreven, wordt de uitbreiding voor de MFA-controle is mislukt. Wanneer de sleutel is ingesteld op FALSE en de gebruiker niet is ingeschreven, wordt de authenticatie voortgezet zonder dat u MFA uitvoert. Als een gebruiker is ingeschreven in MFA, moeten ze verifiëren met MFA zelfs als REQUIRE_USER_MATCH is ingesteld op FALSE.

U kunt deze sleutel maken en ingesteld op FALSE terwijl uw gebruikers voorbereiding zijn en kunnen niet allemaal worden ingeschreven voor Azure MFA nog. Omdat de sleutel, gebruikers die niet zijn geregistreerd voor MFA kunnen aanmelden, moet u deze sleutel verwijderen voordat u doorgaat naar productie.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>Hoe kan ik controleren of het certificaat van de client is geïnstalleerd, zoals verwacht?

Zoekt u naar het zelfondertekende certificaat dat is gemaakt door het installatieprogramma in het certificaatarchief en controleer of de persoonlijke sleutel is gemachtigd om de gebruiker **netwerkservice**. Het certificaat heeft een onderwerpnaam van **CN \<tenantid\>, OU = Microsoft NPS-uitbreiding**

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>Hoe kan ik dat mijn client het certificaat gekoppeld aan mijn tenants in Azure Active Directory is controleren?

Open PowerShell-opdrachtprompt en voer de volgende opdrachten:

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Deze opdrachten afdrukken alle certificaten die uw tenant koppelen aan uw exemplaar van de NPS-extensie in de PowerShell-sessie. Zoek uw certificaat door uw client-certificaat exporteren als een 'Base64-gecodeerd X.509(.cer)'-bestand zonder de persoonlijke sleutel en vergelijken met de lijst van PowerShell.

Geldige-van en geldig-totdat tijdstempels die in leesbaar zijn, kunnen worden gebruikt voor het filteren van de hand liggende misfits als de opdracht meer dan één certificaat retourneert.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Waarom wordt mijn aanvragen waarbij ADAL-token optreedt?

Deze fout kan worden veroorzaakt door een bepaalde reden. Volg deze stappen om op te lossen:

1. De NPS-server opnieuw.
2. Controleer of dat die cert client is geïnstalleerd, zoals verwacht.
3. Controleer of het certificaat is gekoppeld aan uw tenant op Azure AD.
4. Controleer of dat deze https://login.microsoftonline.com/ toegankelijk is vanaf de server met de extensie is.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Waarom faalt verificatie met een fout in HTTP-logboeken met de mededeling dat de gebruiker niet is gevonden

Controleer of dat AD Connect wordt uitgevoerd en dat de gebruiker aanwezig is in zowel Windows Active Directory en Azure Active Directory.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Waarom zie ik HTTP-fouten in Logboeken verbinden met alle mijn verificaties mislukt?

Controleer of https://adnotifications.windowsazure.com bereikbaar is vanaf de server waarop de NPS-extensie wordt uitgevoerd.


## <a name="next-steps"></a>Volgende stappen

- Alternatieve-id's voor aanmelding configureren of stelt u een lijst met uitzonderingen voor IP-adressen die verificatie in twee stappen in mag niet uitvoeren [geavanceerde configuratieopties voor de NPS-extensie voor multi-factor Authentication](nps-extension-advanced-configuration.md)

- Meer informatie over het integreren van [extern bureaublad-Gateway](nps-extension-remote-desktop-gateway.md) en [VPN-servers](nps-extension-vpn.md) met behulp van de NPS-uitbreiding

- [Resolve error messages from the NPS extension for Azure Multi-Factor Authentication](multi-factor-authentication-nps-errors.md) (Foutberichten van de NPS-extensie voor Azure Multi-Factor Authentication oplossen)
