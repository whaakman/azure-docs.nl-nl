---
title: Bestaande NPS-servers gebruiken om aan te bieden mogelijkheden voor Azure MFA - Azure Active Directory
description: Cloud-gebaseerde verificatie in twee stappen verificatie mogelijkheden toevoegen aan uw bestaande infrastructuur voor verificatie
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
ms.openlocfilehash: b36b6e513e382e25f7d7038f49e7467a21686a0f
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58311727"
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>Uw bestaande NPS-infrastructuur integreren met Azure multi-factor Authentication

De Network Policy Server (NPS)-extensie voor Azure MFA voegt cloud-gebaseerde MFA functies aan uw infrastructuur voor verificatie met behulp van uw bestaande servers. Met de NPS-extensie kunt u telefonische oproepen, SMS-bericht of verificatie via de telefoon-app toevoegen aan uw bestaande verificatiestroom zonder te installeren, configureren en onderhouden van nieuwe servers. 

Deze extensie is bedoeld voor organisaties die beveiligen van VPN-verbindingen willen zonder de Azure MFA-Server te implementeren. De NPS-extensie fungeert als een adapter tussen RADIUS- en cloud-gebaseerde Azure MFA voor een tweede factor-verificatie voor federatieve of gebruikers gesynchroniseerde.

Wanneer u de NPS-extensie voor Azure MFA, bevat de verificatie-stroom de volgende onderdelen: 

1. **NAS/VPN-Server** ontvangt verzoeken van VPN-clients en converteert deze naar RADIUS-aanvragen naar de NPS-servers. 
2. **NPS-Server** maakt verbinding met Active Directory voor het uitvoeren van de primaire verificatie voor de RADIUS-aanvragen en implementatie is geslaagd, geeft u de aanvraag aan een geïnstalleerde extensies.  
3. **NPS-extensie** een aanvraag voor Azure MFA voor de secundaire verificatie wordt geactiveerd. Nadat de extensie-antwoord is ontvangen, en als de MFA-controle is geslaagd, de verificatieaanvraag is voltooid door te geven van de NPS-server met beveiligingstokens die een MFA-claim bevatten, dat is uitgegeven door Azure STS.  
4. **Azure MFA** communiceert met Azure Active Directory voor het ophalen van gegevens van de gebruiker en de secundaire authenticatie met behulp van een verificatiemethode die is geconfigureerd voor de gebruiker wordt uitgevoerd.

Het volgende diagram illustreert deze aanvraagstroom op hoog niveau verificatie: 

![Stroomdiagram voor verificatie](./media/howto-mfa-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Uw implementatie plannen

De NPS-extensie verwerkt automatisch redundantie, dus u een speciale configuratie hoeft.

U kunt zoveel ingeschakeld met een Azure MFA NPS-servers als u nodig hebt. Als u meerdere servers installeert, moet u een clientcertificaat verschil voor elk incident. Het maken van een certificaat voor elke server betekent dat u kunt elk certificaat afzonderlijk bijwerken, en niet een zorgen over downtime voor al uw servers maken.

VPN-servers routeren verificatieaanvragen, zodat ze nodig hebben om te worden op de hoogte van de nieuwe Azure MFA-functionaliteit NPS-servers.

## <a name="prerequisites"></a>Vereisten

De NPS-extensie is bedoeld om te werken met uw bestaande infrastructuur. Zorg ervoor dat u hebt de volgende vereisten voordat u begint.

### <a name="licenses"></a>Licenties

De NPS-extensie voor Azure MFA is beschikbaar voor klanten met [licenties voor Azure multi-factor Authentication](multi-factor-authentication.md) (meegeleverd bij Azure AD Premium, EMS of een zelfstandige licentie MFA). Op basis van gebruik licenties voor Azure MFA zoals per gebruiker of per verificatie licenties zijn niet compatibel met de NPS-extensie. 

### <a name="software"></a>Software

Windows Server 2008 R2 SP1 of hoger.

### <a name="libraries"></a>Bibliotheken

Deze bibliotheken worden automatisch geïnstalleerd met de extensie.

- [Visual C++ Redistributable Packages for Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
- [Microsoft Azure Active Directory-Module voor Windows PowerShell versie 1.1.166.0](https://www.powershellgallery.com/packages/MSOnline/1.1.166.0)

De Microsoft Azure Active Directory-Module voor Windows PowerShell is geïnstalleerd, als deze nog niet aanwezig is, via een configuratiescript die u als onderdeel van het installatieproces uitvoert. Er is niet nodig voor het installeren van deze module vooraf als deze nog niet is geïnstalleerd.

### <a name="azure-active-directory"></a>Azure Active Directory

Iedereen met behulp van de NPS-extensie moet worden gesynchroniseerd met Azure Active Directory met Azure AD Connect en moet zijn geregistreerd voor MFA.

Wanneer u de extensie installeren, moet u de map-ID en de beheerdersreferenties voor uw Azure AD-tenant. U vindt uw map-ID in de [Azure-portal](https://portal.azure.com). Meld u aan als beheerder, selecteer de **Azure Active Directory** pictogram aan de linkerkant, selecteer vervolgens **eigenschappen**. Kopieer de GUID in de **map-ID** vak en op te slaan. U gebruikt deze GUID als de tenant-ID bij de installatie van de NPS-extensie.

![Uw Directory-ID onder Azure Active Directory-eigenschappen niet vinden](./media/howto-mfa-nps-extension/find-directory-id.png)

### <a name="network-requirements"></a>Netwerkvereisten

De NPS-server moet kunnen communiceren met de volgende URL's via de poorten 80 en 443.

* https://adnotifications.windowsazure.com  
* https://login.microsoftonline.com

## <a name="prepare-your-environment"></a>Uw omgeving voorbereiden

Voordat u de NPS-extensie installeren, die u wilt u de omgeving voor het afhandelen van het verificatieverkeer voorbereiden.

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a>De functie NPS op een domein-server inschakelen

De NPS-server maakt verbinding met Azure Active Directory en verifieert de MFA-aanvragen. Kies één server voor deze rol. U wordt aangeraden kiezen van een server die aanvragen van andere services, wordt niet verwerkt omdat de NPS-extensie genereert fouten voor alle aanvragen die niet van RADIUS. De NPS-server moet worden ingesteld als de primaire en secundaire authentication-server voor uw omgeving. het kan niet worden aanvragen van RADIUS-proxy naar een andere server.

1. Open op uw server, de **functies Wizard Functies toevoegen en** in het menu Serverbeheer Quickstart.
2. Kies **op basis van functie of onderdeel gebaseerde installatie** voor uw installatietype.
3. Selecteer de **Network Policy and Access Services** serverfunctie. Een venster kan weergegeven om u van vereiste onderdelen om uit te voeren van deze rol te informeren.
4. Ga door met de wizard totdat de bevestigingspagina. Selecteer **Installeren**.

Nu dat u een server die is aangewezen voor NPS hebt, moet u ook deze server voor het afhandelen van binnenkomende RADIUS-aanvragen van de VPN-oplossing te configureren.

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a>Uw VPN-oplossing om te communiceren met de NPS-server configureren

Afhankelijk van welke VPN-oplossing u variëren de stappen voor het configureren van uw RADIUS-verificatiebeleid. Dit beleid om te verwijzen naar de NPS RADIUS-server configureert.

### <a name="sync-domain-users-to-the-cloud"></a>Gebruikers van een domein synchroniseren naar de cloud

Deze stap kan al zijn voltooid voor uw tenant, maar het is goed om te controleren of Azure AD Connect onlangs uw databases is gesynchroniseerd.

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **Azure AD Connect**
3. Controleer of de synchronisatiestatus **ingeschakeld** en dat de laatste synchronisatie minder dan een uur geleden.

Als u wilt een vliegende start een nieuwe ronde van synchronisatie ons de instructies in [Azure AD Connect-synchronisatie: Scheduler](../hybrid/how-to-connect-sync-feature-scheduler.md#start-the-scheduler).

### <a name="determine-which-authentication-methods-your-users-can-use"></a>Bepalen welke verificatiemethoden die uw gebruikers kunnen gebruiken

Er zijn twee factoren die invloed hebben op welke verificatiemethoden zijn beschikbaar voor de implementatie van een NPS-extensie:

1. De wachtwoord-versleutelingsalgoritme wordt gebruikt tussen de RADIUS-client (VPN, Netscaler-server, of andere) en de NPS-servers.
   - **PAP** biedt ondersteuning voor alle verificatiemethoden van Azure MFA in de cloud: telefonische oproep, eenzijdige SMS-bericht, mobiele app-meldingen en verificatiecode via mobiele app.
   - **CHAPv2** en **EAP** ondersteuning bellen en mobiele app-meldingen.
2. De invoer methoden die de clienttoepassing (VPN, Netscaler-server, of andere) kunnen worden verwerkt. Bijvoorbeeld, heeft de VPN-client een manier om toe te staan van de gebruiker op het type in een verificatiecode van een tekst- of mobiele app

Wanneer u de NPS-extensie implementeert, gebruikt u deze factoren om te evalueren welke methoden zijn beschikbaar voor uw gebruikers. Als de RADIUS-client PAP ondersteunt, maar de UX-client geen invoervelden voor een verificatiecode, klikt u vervolgens zijn voor bellen en mobiele app-meldingen de twee ondersteunde opties.

U kunt [uitschakelen van niet-ondersteunde verificatiemethoden](howto-mfa-mfasettings.md#verification-methods) in Azure.

### <a name="register-users-for-mfa"></a>Gebruikers registreren voor MFA

Voordat u implementeert en de NPS-extensie gebruiken, moeten gebruikers die nodig zijn voor het uitvoeren van verificatie in twee stappen worden geregistreerd voor MFA. Meer direct, als u wilt testen de extensie als u deze implementeert, moet u ten minste één testaccount dat volledig is geregistreerd voor meervoudige verificatie.

Volg deze stappen om een testaccount aan de slag:
1. Aanmelden bij [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) met een testaccount. 
2. Volg de aanwijzingen voor het instellen van een verificatiemethode.
3. Beleid voor voorwaardelijke toegang aan te maken of [wijzigen van de gebruikersstatus](howto-mfa-userstates.md) verificatie in twee stappen vereist voor de testaccount. 

Uw gebruikers moeten ook als volgt te werk om in te schrijven voordat ze kunnen worden geverifieerd met de NPS-extensie.

## <a name="install-the-nps-extension"></a>De NPS-extensie installeren

> [!IMPORTANT]
> Installeer de NPS-extensie op een andere server dan het VPN-toegangspunt.

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a>Download en installeer de NPS-extensie voor Azure MFA

1. [De NPS-extensie downloaden](https://aka.ms/npsmfa) van het Microsoft Download Center.
2. Kopieer het binaire bestand om de Network Policy Server die u wilt configureren.
3. Voer *setup.exe* en volgt u de installatie-instructies. Als er fouten optreden, moet u controleren dat de twee bibliotheken van de sectie vereisten zijn geïnstalleerd.

### <a name="run-the-powershell-script"></a>Het PowerShell-script uitvoeren

Het installatieprogramma maakt een PowerShell-script op deze locatie: `C:\Program Files\Microsoft\AzureMfa\Config` (waarbij C:\ is voor uw installatiestation). Dit PowerShell-script worden de volgende acties uitgevoerd telkens wanneer die deze wordt uitgevoerd:

- Maak een zelf-ondertekend certificaat.
- De openbare sleutel van het certificaat naar de service-principal in Azure AD koppelen.
- Het certificaat in het archief van de lokale computer cert Store.
- Toegang verlenen tot de persoonlijke sleutel van het certificaat voor netwerk-gebruiker.
- Start opnieuw op de NPS.

Tenzij u wilt dat het gebruik van uw eigen certificaten (in plaats van de zelfondertekende certificaten die het PowerShell-script genereert), voer het PowerShell-Script om de installatie te voltooien. Als u de extensie op meerdere servers installeert, moet elk een eigen certificaat hebben.

1. Windows PowerShell ook uitvoeren als beheerder.
2. Wijzig de mappen.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. De PowerShell-script dat is gemaakt door het installatieprogramma uitvoeren.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. Aanmelden bij Azure AD als een beheerder.
5. PowerShell-prompts voor uw tenant-ID. Gebruik de GUID van de Directory-ID die u hebt gekopieerd vanuit Azure portal in de sectie vereisten.
6. PowerShell toont een bericht weergegeven wanneer het script is voltooid.  

Herhaal deze stappen uit op eventuele aanvullende NPS-servers die u instellen wilt voor de taakverdeling.

> [!NOTE]
> Als u uw eigen certificaten gebruikt in plaats van certificaten met de PowerShell-script genereren, zorg dat deze worden uitgelijnd op de NPS-naamconventie. Naam van de certificaathouder moet **CN =\<TenantID\>, OU = Microsoft NPS-extensie**. 

## <a name="configure-your-nps-extension"></a>Configureren van uw NPS-extensie

Deze sectie bevat overwegingen bij het ontwerpen en suggesties voor geslaagde implementaties voor NPS-extensie.

### <a name="configuration-limitations"></a>Configuratie-beperkingen

- De NPS-extensie voor Azure MFA bevat geen hulpprogramma's voor het migreren van gebruikers en -instellingen van MFA-Server naar de cloud. Daarom is het raadzaam met behulp van de extensie voor nieuwe implementaties in plaats van bestaande implementatie. Als u de extensie op een bestaande implementatie gebruikt, hebben uw gebruikers om uit te voeren proof-up opnieuw uit om te vullen van de MFA-gegevens in de cloud.  
- De NPS-extensie gebruikt de UPN van de on-premises Active directory om te identificeren van de gebruiker op de Azure MFA voor het uitvoeren van de secundaire verificatie De extensie kan worden geconfigureerd voor het gebruik van een andere id, zoals aangepaste Active Directory-veld dan UPN of alternatieve aanmeldings-ID. Zie voor meer informatie het artikel [geavanceerde configuratieopties voor de NPS-extensie voor multi-factor Authentication](howto-mfa-nps-extension-advanced.md).
- Niet alle versleuteling protocollen ondersteunen alle verificatiemethoden.
   - **PAP** biedt ondersteuning voor telefonische oproep, eenzijdige SMS-bericht, mobiele app-meldingen en verificatiecode via mobiele app
   - **CHAPv2** en **EAP** ondersteuning bellen en mobiele app-meldingen

### <a name="control-radius-clients-that-require-mfa"></a>Besturingselement voor RADIUS-clients die MFA vereisen

Als u MFA voor een RADIUS-client met behulp van de NPS-extensie inschakelen, worden alle verificaties voor deze client om uit te voeren MFA vereist. Als u wilt dat het inschakelen van MFA voor bepaalde RADIUS-clients, maar niet voor andere, kunt u twee NPS-servers configureren en installeren van de extensie op slechts één provider. Configureer RADIUS-clients die u wilt vereisen dat MFA voor het verzenden van aanvragen naar de NPS-server geconfigureerd met de extensie en de andere RADIUS-clients naar de NPS-server niet is geconfigureerd met de extensie.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Voorbereiden voor gebruikers die niet zijn geregistreerd voor MFA

Als u gebruikers die niet zijn geregistreerd voor MFA hebt, kunt u bepalen wat er gebeurt wanneer ze proberen te verifiëren. Gebruik de registerinstelling *REQUIRE_USER_MATCH* in het registerpad *HKLM\Software\Microsoft\AzureMFA* om te bepalen het gedrag van de functie. Deze instelling heeft een configuratieoptie voor één:

| Sleutel | Value | Standaard |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | WAAR/ONWAAR | Niet ingesteld (gelijk aan TRUE) |

Het doel van deze instelling is om te bepalen wat er gebeurt wanneer een gebruiker is niet geregistreerd voor MFA. Wanneer de sleutel niet bestaat, is niet ingesteld of is ingesteld op TRUE, en de gebruiker niet is ingeschreven, wordt de extensie voor de MFA-controle is mislukt. Wanneer de sleutel is ingesteld op FALSE en de gebruiker niet is ingeschreven, wordt de authenticatie voortgezet zonder uit te voeren van MFA. Als een gebruiker is geregistreerd bij MFA, ze moeten worden geverifieerd met MFA, zelfs als REQUIRE_USER_MATCH is ingesteld op FALSE.

U kunt deze sleutel maken en deze instelt op FALSE, terwijl uw gebruikers zich onboarding, en kan niet alle worden geregistreerd voor Azure MFA nog. Omdat de sleutel instellen, gebruikers die niet zijn geregistreerd voor MFA kunnen zich aanmeldt, moet u deze sleutel verwijderen voordat u verdergaat met de productie.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>Hoe kan ik controleren of het clientcertificaat is geïnstalleerd, zoals verwacht?

Zoek het zelfondertekende certificaat dat is gemaakt door het installatieprogramma in het certificaatarchief en controleer of de persoonlijke sleutel heeft machtigingen verleend aan gebruiker **netwerkservice**. Het certificaat heeft een naam voor onderwerp van **CN \<tenantid\>, OU = Microsoft NPS-extensie**

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>Hoe kan ik controleren of mijn clientcertificaat gekoppeld aan mijn tenant in Azure Active Directory is?

Open PowerShell-opdrachtprompt en voer de volgende opdrachten uit:

``` PowerShell
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1
```

Deze opdrachten afdrukken alle certificaten in uw tenant koppelen met uw exemplaar van de NPS-extensie in de PowerShell-sessie. Zoek naar uw certificaat door het clientcertificaat exporteren als een 'Base-64 gecodeerde x.509 (.cer)'-bestand zonder de persoonlijke sleutel en vergelijken met de lijst van PowerShell.

De volgende opdracht maakt u een bestand met de naam 'npscertificate' op de schijf "C:" in de indeling cer.

``` PowerShell
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 | select -ExpandProperty "value" | out-file c:\npscertficicate.cer
```

Zodra u deze opdracht uitvoert, gaat u naar de C-schijf naar het bestand en dubbelklik erop. Gaat u naar de details en bladert u omlaag naar "vingerafdruk", de vingerafdruk van het certificaat is geïnstalleerd op de server om deze te vergelijken. De vingerafdrukken van het certificaat moeten overeenkomen met.

Geldige-van en geldig-totdat tijdstempels die in leesbare vorm, kan aan de hand liggende misfits eruit gefilterd als de opdracht meer dan één certificaat retourneert worden gebruikt.

-------------------------------------------------------------

### <a name="why-cant-i-sign-in"></a>Waarom kan ik aanmelden?

Controleer of uw wachtwoord nog niet is verlopen. De NPS-extensie biedt geen ondersteuning voor wijzigen van wachtwoorden als onderdeel van de werkstroom aanmelden. Neem contact op met de IT-afdeling van uw organisatie voor verdere ondersteuning.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Waarom wordt mijn aanvragen mislukt met fout voor ADAL-token?

Deze fout kan worden veroorzaakt door verschillende oorzaken hebben. Volg deze stappen om op te lossen:

1. Uw NPS-server opnieuw opstarten.
2. Controleer of dat dit clientcertificaat is geïnstalleerd, zoals verwacht.
3. Controleer of het certificaat is gekoppeld aan uw tenant in Azure AD.
4. Controleer of https://login.microsoftonline.com/ toegankelijk is vanaf de server waarop de extensie wordt uitgevoerd.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Waarom verificatie mislukt vanwege een fout in HTTP-logboeken met de mededeling dat de gebruiker niet is gevonden?

Controleer of dat AD Connect wordt uitgevoerd en dat de gebruiker aanwezig is in zowel Windows Active Directory en Azure Active Directory.

-------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Waarom zie ik HTTP-verbindingsfouten in Logboeken met mijn verificaties die zijn mislukt?

Controleer of https://adnotifications.windowsazure.com bereikbaar is vanaf de server waarop de NPS-extensie wordt uitgevoerd.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Beheer van de TLS/SSL-protocollen en coderingssuites

Het verdient aanbeveling dat oudere en zwakkere coderingssuites worden uitgeschakeld of verwijderd, tenzij dit vereist door uw organisatie. Informatie over het uitvoeren van deze taak vindt u in het artikel [​​SSL/TLS-protocollen en coderingssuites beheren voor AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-protocols-in-ad-fs)

## <a name="next-steps"></a>Volgende stappen

- Alternatieve id's voor aanmelding configureren en instellen van een lijst met IP-adressen die al dan niet mogen uitvoeren van verificatie in [geavanceerde configuratieopties voor de NPS-extensie voor meervoudige verificatie](howto-mfa-nps-extension-advanced.md)

- Meer informatie over het integreren [extern bureaublad-Gateway](howto-mfa-nps-extension-rdg.md) en [VPN-servers](howto-mfa-nps-extension-vpn.md) met behulp van de NPS-extensie

- [Resolve error messages from the NPS extension for Azure Multi-Factor Authentication](howto-mfa-nps-extension-errors.md) (Foutberichten van de NPS-extensie voor Azure Multi-Factor Authentication oplossen)
