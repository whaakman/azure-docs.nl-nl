---
title: Automatiseer de inrichting van apps die SCIM gebruiken in Azure Active Directory | Microsoft Docs
description: Azure Active Directory kan automatisch inrichten van gebruikers en groepen voor elke toepassing of identiteit store die door een webservice is fronted met de interface die is gedefinieerd in de specificatie van het protocol SCIM
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: barbkess
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;seohack1
ms.openlocfilehash: 13491b10096e651b40a83e072057250e856d4ef1
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54469525"
---
# <a name="using-system-for-cross-domain-identity-management-scim-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a>Met behulp van systeem voor meerdere domeinen Identity Management (SCIM) voor het automatisch inrichten van gebruikers en groepen uit Azure Active Directory voor toepassingen

## <a name="overview"></a>Overzicht
Azure Active Directory (Azure AD) kunt automatisch inrichten van gebruikers en groepen naar elke toepassing of identiteit store die door een webservice met de interface is fronted gedefinieerd in de [systeem voor meerdere domeinen Identity Management (SCIM) 2.0-protocol specificatie](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory kunt verzenden aanvragen voor het maken, wijzigen of verwijderen toegewezen gebruikers en groepen met de webservice. De webservice kan deze aanvragen vervolgens vertalen naar bewerkingen op de doel-id-store. 

>[!IMPORTANT]
>Het gedrag van de implementatie van Azure AD SCIM het laatst is bijgewerkt op 18 December 2018. Zie voor meer informatie over wat er nieuw [SCIM 2.0-protocol naleving van de Azure AD-gebruiker Provisioning-service](application-provisioning-config-problem-scim-compatibility.md).

![][0]
*Afbeelding 1: Inrichten van Azure Active Directory naar een store identiteit via een webservice*

Deze mogelijkheid kan worden gebruikt in combinatie met de functie 'bring your own app' in Azure AD. Deze mogelijkheid kan eenmalige aanmelding en automatisch gebruikers inrichten voor toepassingen die door een webservice SCIM zijn fronted.

Er zijn twee gebruiksscenario's voor het gebruik van SCIM in Azure Active Directory:

* **Inrichten van gebruikers en groepen met toepassingen die ondersteuning bieden voor SCIM** -toepassingen met SCIM 2.0 ondersteunen en bearer-tokens van OAuth gebruiken voor verificatie met Azure AD zonder configuratie werkt.
  
* **Het bouwen van uw eigen oplossing voor toepassingen die ondersteuning voor andere op API gebaseerde inrichting** -voor niet-SCIM toepassingen, kunt u een SCIM-eindpunt voor de omzetting tussen de Azure AD SCIM-eindpunt en een API biedt ondersteuning voor de toepassing voor maken inrichten van gebruikers. Voor hulp bij het ontwikkelen van een eindpunt SCIM, zijn er bibliotheken en codevoorbeelden waarin u ziet u hoe u een eindpunt SCIM bieden en vertalen SCIM berichten Common Language Infrastructure (CLI).  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Inrichten van gebruikers en groepen met toepassingen die SCIM ondersteunen
Azure AD kan worden geconfigureerd om automatisch inrichten toegewezen gebruikers en groepen met toepassingen die worden geïmplementeerd een [systeem voor meerdere domeinen Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) webservice en accepteren van bearer-tokens van OAuth voor verificatie. Toepassingen moeten voldoen aan deze vereisten binnen de SCIM 2.0-specificatie:

* Ondersteunt het maken van gebruikers en/of groepen, aan de hand van sectie 3.3 van het protocol SCIM.  
* Biedt ondersteuning voor gebruikers en/of groepen met een patch-aanvragen aan de hand van sectie 3.5.2 gebruikt van het protocol SCIM wijzigen.  
* Biedt ondersteuning voor het ophalen van een bekende bron aan de hand van sectie 3.4.1 van het protocol SCIM.  
* Ondersteunt het opvragen van gebruikers en/of groepen, aan de hand van sectie 3.4.2 van het protocol SCIM.  Standaard wordt gebruikers gevraagd door externalId en groepen worden opgevraagd door weergavenaam.  
* Ondersteunt het opvragen van gebruiker door de ID en -beheer aan de hand van sectie 3.4.2 van het protocol SCIM.  
* Ondersteunt het opvragen van groepen door-ID en lid aan de hand van sectie 3.4.2 van het protocol SCIM.  
* Bearer-tokens van OAuth voor verificatie aan de hand van sectie 2.1 van het protocol SCIM accepteert.

Neem contact op met de provider van uw toepassing of de documentatie van de toepassingsprovider van uw voor overzichten van compatibiliteit met deze vereisten.

### <a name="getting-started"></a>Aan de slag
Toepassingen die ondersteuning bieden voor het SCIM-profiel dat wordt beschreven in dit artikel kunnen worden verbonden met Azure Active Directory met de functie 'buiten de galerie application' van de Azure AD-toepassingsgalerie. Eenmaal verbinding hebben, voert Azure AD een synchronisatieproces voor elke 40 minuten waar deze query's van de toepassing SCIM eindpunt voor toegewezen gebruikers en groepen, en maakt of wijzigt u ze op basis van de details van de toewijzing.

**Verbinding met het maken van een toepassing die ondersteuning biedt voor SCIM:**

1. Aanmelden bij [de Azure-portal](https://portal.azure.com). 
2. Blader naar **Azure Active Directory > bedrijfstoepassingen**, en selecteer **nieuwe toepassing > alle > niet in de galerij toepassing**.
3. Voer een naam voor uw toepassing en klikt u op **toevoegen** pictogram om een app-object te maken.
    
   ![][1]
   *Afbeelding 2: Azure AD-toepassingsgalerie*
    
4. Selecteer in het scherm, de **Provisioning** tabblad in de linkerkolom staat.
5. In de **inrichting modus** in het menu **automatische**.
    
   ![][2]
   *Afbeelding 3: Configureren van het inrichten in Azure portal*
    
6. In de **Tenant-URL** en voer de URL van SCIM-eindpunt van de toepassing. Voorbeeld: https://api.contoso.com/scim/v2/
7. Als het eindpunt SCIM een OAuth-bearer-token van een uitgever dan Azure AD vereist, kopieert u de vereiste OAuth bearer-token naar de optionele **geheim Token** veld. Als dit veld leeg laat, opgenomen Azure AD een OAuth-bearer-token van Azure AD met elke aanvraag is uitgegeven. Apps die gebruikmaken van Azure AD als een id-provider kunt controleren of deze Azure AD-token dat is uitgegeven.
8. Klik op de **testverbinding** knop met Azure Active Directory wilt verbinden met het eindpunt SCIM. Als de pogingen mislukken, wordt informatie over de fout wordt weergegeven.  

    >[!NOTE]
    >**Verbinding testen** query naar het eindpunt SCIM voor een gebruiker die niet bestaat, met behulp van een willekeurige GUID als de overeenkomende eigenschap in de Azure AD-configuratie is geselecteerd. De verwachte reactie van de juiste is HTTP 200 OK met een leeg SCIM ListResponse-bericht. 

9. Als de pogingen tot verbinding maken met de toepassing te voltooien, klikt u op **opslaan** om op te slaan de beheerdersreferenties.
10. In de **toewijzingen** sectie, zijn er twee sets selecteerbare Kenmerktoewijzingen: één voor gebruikersobjecten en één voor de groepsobjecten worden weergegeven. Selecteren om te controleren van de kenmerken die worden gesynchroniseerd vanuit Active Directory van Azure aan uw app. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikers en groepen in uw app voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

    >[!NOTE]
    >U kunt desgewenst uitschakelen door het uitschakelen van de toewijzing van 'groepen'-synchronisatie van de groepsobjecten worden weergegeven. 

11. Onder **instellingen**, wordt de **bereik** veld wordt gedefinieerd welke gebruikers en groepen worden gesynchroniseerd. Selecteren 'Sync alleen toegewezen gebruikers en groepen' (aanbevolen) synchroniseert alleen gebruikers en groepen die zijn toegewezen de **gebruikers en groepen** tabblad.
12. Nadat de configuratie voltooid is, wijzigt de **Inrichtingsstatus** naar **op**.
13. Klik op **opslaan** starten van de Azure AD-inrichtingsservice. 
14. Als het synchroniseren van alleen toegewezen gebruikers en groepen (aanbevolen), moet u selecteren de **gebruikers en groepen** tabblad en wijs de gebruikers en/of groepen die u wilt synchroniseren.

Nadat de initiële synchronisatie is gestart, kunt u de **auditlogboeken** tab om de voortgang bewaken, waarin alle acties die worden uitgevoerd door de provisioning-service op uw app. Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](check-status-user-account-provisioning.md).

> [!NOTE]
> De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>Het bouwen van uw eigen oplossing voor elke toepassing
Als u een SCIM-webservice die is gekoppeld met Azure Active Directory maakt, kunt u één gebruiker voor aanmelding en automatische inrichting voor nagenoeg elke toepassing die een REST- of SOAP gebruiker Inrichtings-API biedt.

Dit is hoe het werkt:

1. Azure AD biedt een gemeenschappelijke infrastructuur-bibliotheek voor taal is met de naam [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Systeemintegrators en ontwikkelaars kunnen deze bibliotheek maken en implementeren van een SCIM gebaseerde web service-eindpunt van het verbinden van Azure AD voor de identiteit van een App store kunnen gebruiken.
2. Toewijzingen worden geïmplementeerd in de webservice om het schema voor de gestandaardiseerde toewijzen aan het gebruikersschema en het protocol vereist voor de toepassing.
3. De eindpunt-URL is geregistreerd in Azure AD als onderdeel van een aangepaste toepassing in de toepassingengalerie.
4. Gebruikers en groepen worden toegewezen aan deze toepassing in Azure AD. Bij toewijzing, worden ze in een wachtrij om te worden gesynchroniseerd met de doeltoepassing geplaatst. Het proces van synchronisatie verwerken van de wachtrij wordt elke 40 minuten uitgevoerd.

### <a name="code-samples"></a>Codevoorbeelden
Om dit proces eenvoudiger [codevoorbeelden](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) zijn opgegeven die een SCIM web service-eindpunt maken en demonstreren van automatische inrichting. Een voorbeeld is van een provider die wordt onderhouden door een bestand met door komma's gescheiden waarden voor gebruikers en groepen rijen.  Het andere type is voor een provider die van invloed op de Amazon Web Services Identity and Access Management-service.  

**Vereisten**

* Visual Studio 2013 of hoger
* [Azure-SDK voor .NET](https://azure.microsoft.com/downloads/)
* Windows machine die ondersteuning biedt voor het framework ASP.NET 4.5 moet worden gebruikt als het eindpunt SCIM. Deze machine moet toegankelijk zijn vanuit de cloud
* [Een Azure-abonnement met een proef- of gelicentieerde versie van Azure AD Premium](https://azure.microsoft.com/services/active-directory/)

### <a name="getting-started"></a>Aan de slag
De eenvoudigste manier voor het implementeren van een eindpunt SCIM inrichting aanvragen van Azure AD kan accepteren, is te bouwen en implementeren van de voorbeeldcode die uitvoer van de ingerichte gebruikers naar een bestand met door komma's gescheiden waarden (CSV).

**Een voorbeeld SCIM-eindpunt maken:**

1. Download het pakket voor het voorbeeld van code op [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2. Pak het pakket en plaats deze op uw Windows-machine op een locatie zoals C:\AzureAD-BYOA-Provisioning-Samples\.
3. In deze map, start u het FileProvisioning\Host\FileProvisioningService.csproj-project in Visual Studio.
4. Selecteer **Tools > NuGet Package Manager > Package Manager Console**, en voer de volgende opdrachten voor het project FileProvisioningService om op te lossen de verwijzingen van de oplossing:

   ```
    Update-Package -Reinstall
   ```

5. Bouw het project FileProvisioningService.
6. Start de opdrachtprompt-toepassing in Windows (als een beheerder) en gebruik de **cd** opdracht om de map op uw **\AzureAD-BYOA-Provisioning-Samples\FileProvisioning\Host\bin\Debug**map.
7. Voer de volgende opdracht uit, < ip-adres > vervangen door de naam van de IP-adres of de domeinnaam van de Windows-machine:

   ```
    FileSvc.exe http://<ip-address>:9000 TargetFile.csv
   ```

8. In Windows onder **Windows-instellingen > netwerk & instellingen voor Internet**, selecteer de **Windows Firewall > Geavanceerde instellingen**, en maak een **regel voor binnenkomende verbindingen** die biedt binnenkomende toegang tot poort 9000.
9. Als de Windows-machine achter een router is, moet de router worden geconfigureerd voor het uitvoeren van Network Access Translation tussen de poort 9000 die wordt blootgesteld aan internet en 9000-poort op de Windows-machine. Deze configuratie is vereist voor Azure AD kunnen toegang hebben tot dit eindpunt in de cloud.

**Het voorbeeld SCIM-eindpunt registreren in Azure AD:**

1. Aanmelden bij [de Azure-portal](https://portal.azure.com). 
2. Blader naar **Azure Active Directory > bedrijfstoepassingen**, en selecteer **nieuwe toepassing > alle > niet in de galerij toepassing**.
3. Voer een naam voor uw toepassing en klikt u op **toevoegen** pictogram om een app-object te maken. De toepassingsobject gemaakt is bedoeld om weer te geven van de doel-app u zou worden ingericht voor en uitvoering van eenmalige aanmelding voor en niet alleen het eindpunt SCIM.
4. Selecteer in het scherm, de **Provisioning** tabblad in de linkerkolom staat.
5. In de **inrichting modus** in het menu **automatische**.
    
   ![][2]
   *Afbeelding 4: Configureren van het inrichten in Azure portal*
    
6. In de **Tenant-URL** en voer de beschikbaar gesteld op internet-URL en poort van uw eindpunt SCIM. De vermelding is iets als http://testmachine.contoso.com:9000 of http://<ip-address>:9000/, waarbij < ip-adres > de internet is-IP-adres weergegeven.  
7. Als het eindpunt SCIM een OAuth-bearer-token van een uitgever dan Azure AD vereist, kopieert u de vereiste OAuth bearer-token naar de optionele **geheim Token** veld. Als dit veld leeg laat, wordt Azure AD een OAuth-bearer-token dat is uitgegeven van Azure AD met elke aanvraag bevatten. Apps die gebruikmaken van Azure AD als een id-provider kunt controleren of deze Azure AD-token dat is uitgegeven.
8. Klik op de **testverbinding** knop met Azure Active Directory wilt verbinden met het eindpunt SCIM. Als de pogingen mislukken, wordt informatie over de fout wordt weergegeven.  

    >[!NOTE]
    >**Verbinding testen** query naar het eindpunt SCIM voor een gebruiker die niet bestaat, met behulp van een willekeurige GUID als de overeenkomende eigenschap in de Azure AD-configuratie is geselecteerd. De verwachte reactie van de juiste is HTTP 200 OK met een leeg SCIM ListResponse-bericht

9. Als de pogingen tot verbinding maken met de toepassing te voltooien, klikt u op **opslaan** om op te slaan de beheerdersreferenties.
10. In de **toewijzingen** sectie, zijn er twee sets selecteerbare Kenmerktoewijzingen: één voor gebruikersobjecten en één voor de groepsobjecten worden weergegeven. Selecteren om te controleren van de kenmerken die worden gesynchroniseerd vanuit Active Directory van Azure aan uw app. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikers en groepen in uw app voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.
11. Onder **instellingen**, wordt de **bereik** veld wordt gedefinieerd welke gebruikers en groepen worden gesynchroniseerd. Selecteren 'Sync alleen toegewezen gebruikers en groepen' (aanbevolen) synchroniseert alleen gebruikers en groepen die zijn toegewezen de **gebruikers en groepen** tabblad.
12. Nadat de configuratie voltooid is, wijzigt de **Inrichtingsstatus** naar **op**.
13. Klik op **opslaan** starten van de Azure AD-inrichtingsservice. 
14. Als het synchroniseren van alleen toegewezen gebruikers en groepen (aanbevolen), moet u selecteren de **gebruikers en groepen** tabblad en wijs de gebruikers en/of groepen die u wilt synchroniseren.

Nadat de initiële synchronisatie is gestart, kunt u de **auditlogboeken** tab om de voortgang bewaken, waarin alle acties die worden uitgevoerd door de provisioning-service op uw app. Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](check-status-user-account-provisioning.md).

De laatste stap bij het controleren van het voorbeeld is de TargetFile.csv-bestand te openen in de map \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug op uw Windows-computer. Nadat het inrichtingsproces is uitgevoerd, wordt dit bestand bevat de details van alle toegewezen en ingericht gebruikers en groepen.

### <a name="development-libraries"></a>Ontwikkelingsbibliotheken
Voor het ontwikkelen van uw eigen webservice die aan de specificatie SCIM voldoet eerst raken met de volgende bibliotheken geleverd door Microsoft om te het ontwikkelingsproces versnellen: 

1. Common Language Infrastructure (CLI)-bibliotheken worden aangeboden voor gebruik met de talen die op basis van die infrastructuur, zoals C#. Een van deze bibliotheken [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), declareert een interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, dat is weergegeven in de volgende afbeelding:  Een ontwikkelaar met behulp van de bibliotheken wilt implementeren die interface met een klasse die kan worden verwezen naar algemeen, als een provider. De clientbibliotheken bieden de ontwikkelaar een webservice die voldoet aan de specificatie SCIM implementeren. De webservice kan ofwel worden gehost in Internet Information Services of een uitvoerbaar bestand Common Language Infrastructure-assembly. Aanvraag wordt omgezet in aanroepen van methoden van de provider, die door de ontwikkelaar zou worden geprogrammeerd om te worden uitgevoerd op bepaalde identiteitsarchief.
  
   ![][3]
  
2. [Express route-handlers](https://expressjs.com/guide/routing.html) zijn beschikbaar voor het parseren van de aanvraag-objecten van een node.js-aanroepen (zoals gedefinieerd door de specificatie SCIM), die aangebracht aan een node.js-web-service.   

### <a name="building-a-custom-scim-endpoint"></a>Het bouwen van een aangepaste SCIM-eindpunt
Met behulp van de CLI-bibliotheken, kunnen ontwikkelaars die gebruikmaken van deze bibliotheken hun services in een uitvoerbaar bestand Common Language Infrastructure-assembly of in Internet Information Services hosten. Hier volgt de voorbeeldcode voor het hosten van een service in een uitvoerbaar bestand assembly, op het adres http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Deze service moet beschikken over een HTTP-adres en de server certificaat voor clientverificatie waarvan de basiscertificeringsinstantie een van de volgende namen is: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Een certificaat voor serververificatie kan worden gebonden aan een poort op een Windows-host met behulp van het hulpprogramma network shell: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

De opgegeven waarde voor het argument certhash is hier de vingerafdruk van het certificaat, terwijl de waarde die is opgegeven voor het argument appid een wereldwijd unieke identifier is.  

Voor het hosten van de service in Internet Information Services, zou een ontwikkelaar een CLA code bibliotheek assembly bouwen met een klasse met de naam opstarten in de standaard-naamruimte van de assembly.  Hier volgt een voorbeeld van een klasse bestaat: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>Verwerking-eindpuntverificatie
Aanvragen van Azure Active Directory bevatten een OAuth 2.0-bearer-token.   Elke service die is ontvangen van de aanvraag moet de verlener namens de verwachte Azure Active Directory-tenant, voor toegang tot de webservice Azure Active Directory Graph als Azure Active Directory worden geverifieerd.  In het token wordt de verlener wordt geïdentificeerd door een iss-claim, zoals "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/'.  In dit voorbeeld wordt het basisadres van de claimwaarde https://sts.windows.net, identificeert Azure Active Directory als de uitgever, hoewel de relatieve segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is de unieke id van de Azure Active Directory-tenant op andere gebruikers die het token is uitgegeven.  Als het token is uitgegeven voor toegang tot de Azure Active Directory Graph-webservice, moet de id van die service, 00000002-0000-0000-c000-000000000000, zich in de waarde van van het token aud claim.  

Ontwikkelaars die gebruikmaken van de CLA-bibliotheken die is geleverd door Microsoft voor het bouwen van een service SCIM kunnen verifiëren van aanvragen van Azure Active Directory met behulp van het pakket Microsoft.Owin.Security.ActiveDirectory door de volgende stappen: 

1. In een provider, implementeert u de eigenschap Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior doordat het retourneren van een methode om te worden aangeroepen wanneer de service wordt gestart: 

   ```
     public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
     {
       get
       {
         return this.OnServiceStartup;
       }
     }

     private void OnServiceStartup(
       Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
       System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
     {
     }
   ```

2. Voeg de volgende code toe die methode dat elke aanvraag aan een van de service-eindpunten die zijn geverifieerd als voorzien van een token dat is uitgegeven door Azure Active Directory namens een opgegeven tenant, voor toegang tot de Azure AD Graph-webservice: 

   ```
     private void OnServiceStartup(
       Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
       System.Web.Http.HttpConfiguration HttpConfiguration configuration)
     {
       // IFilter is defined in System.Web.Http.dll.  
       System.Web.Http.Filters.IFilter authorizationFilter = 
         new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

       // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
       // System.IdentityModel.Token.Jwt.dll.
       SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
         new TokenValidationParameters()
         {
           ValidAudience = "00000002-0000-0000-c000-000000000000"
         };

       // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
       // Microsoft.Owin.Security.ActiveDirectory.dll
       Microsoft.Owin.Security.ActiveDirectory.
       WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
         new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
         TokenValidationParameters = tokenValidationParameters,
         Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                       // identifier for this one.  
       };

       applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
     }
   ```


## <a name="user-and-group-schema"></a>Schema voor gebruikers en groepen
Azure Active Directory kunt twee typen resources naar SCIM webservices inrichten.  Deze typen resources worden gebruikers en groepen.  

Gebruikersbronnen worden geïdentificeerd door de schema-id, "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User', die is opgenomen in dit protocol specification: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  De standaardtoewijzing van de kenmerken van gebruikers in Azure Active Directory met de kenmerken van "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User" resources hieronder vindt u in de tabel 1.  

Groep resources worden aangeduid met de schema-id en http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabel 2 hieronder toont de standaardtoewijzing van de kenmerken van groepen in Azure Active Directory met de kenmerken van http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resources.  

### <a name="table-1-default-user-attribute-mapping"></a>Tabel 1: Standaard gebruiker kenmerktoewijzing

| Azure Active Directory-gebruiker | "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User" |
| --- | --- |
| IsSoftDeleted |actief |
| displayName |displayName |
| Facsimile-TelephoneNumber |phoneNumbers [type eq "fax"] .value |
| givenName |name.givenName |
| Functie |titel |
| mail |e-mailberichten [type eq 'werk'] .value |
| mailNickname |externalId |
| beheerder |beheerder |
| mobiele |phoneNumbers [type eq 'mobiel'] .value |
| object-id |Id |
| Postcode |adressen type eq 'werk'.postalCode |
| proxy-adressen |e-mailberichten [Typ eq 'ander']. Waarde |
| physical-Delivery-OfficeName |adressen [Typ eq 'ander']. Indeling |
| streetAddress |adressen type eq 'werk'.streetAddress |
| Achternaam |name.familyName |
| Telefoonnummer |phoneNumbers [type eq 'werk'] .value |
| user-PrincipalName |Gebruikersnaam |

### <a name="table-2-default-group-attribute-mapping"></a>Tabel 2: Standaard groep kenmerktoewijzing

| Azure Active Directory-groep | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| displayName |externalId |
| mail |e-mailberichten [type eq 'werk'] .value |
| mailNickname |displayName |
| leden |leden |
| object-id |Id |
| proxyAddresses |e-mailberichten [Typ eq 'ander']. Waarde |

## <a name="user-provisioning-and-de-provisioning"></a>Inrichten van gebruikers en het ongedaan maken inrichting
De volgende afbeelding ziet u de berichten die Azure Active Directory voor het beheren van de levenscyclus van een gebruiker in een andere identiteit store verzendt naar een SCIM-service. Het diagram toont ook hoe een service die SCIM wordt geïmplementeerd met behulp van de CLI-bibliotheken geleverd door Microsoft voor het bouwen dat dergelijke services deze aanvragen vertalen in aanroepen naar de methoden van een provider.  

![][4]
*Afbeelding 5: Inrichten van gebruikers en het ongedaan maken inrichting reeks*

1. Azure Active Directory vraagt de service voor een gebruiker met een externalId kenmerkwaarde die overeenkomt met de waarde van het kenmerk mailNickname van een gebruiker in Azure AD. De query wordt uitgedrukt als een aanvraag Hypertext Transfer Protocol (HTTP) zoals in dit voorbeeld, waarin jyoung een voorbeeld van een mailNickname van een gebruiker in Azure Active Directory is: 

   ```
     GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
     Authorization: Bearer ...
   ```

   Als de service is gebouwd met behulp van de Common Language Infrastructure-bibliotheken geleverd door Microsoft voor het implementeren van services SCIM, wordt klikt u vervolgens de aanvraag omgezet in een aanroep van de Query-methode van de provider van de service.  Dit is de handtekening van deze methode: 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  
 
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]>  Query(
       Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
       string correlationIdentifier);
   ````

   Hier volgt de definitie van de interface Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters: 

   ```
     public interface IQueryParameters: 
       Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
     {
         System.Collections.Generic.IReadOnlyCollection  <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
         { get; }
     }

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
     {
       system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
       { get; }
       System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
       { get; }
       string SchemaIdentifier 
       { get; }
     }

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
     {
         Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
           { get; set; }
         string AttributePath 
           { get; } 
         Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
           { get; }
         string ComparisonValue 
           { get; }
     }

     public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
     {
         Equals
     }
   ```

   In het volgende voorbeeld van een query voor een gebruiker met een opgegeven waarde voor het kenmerk externalId zijn de waarden van de argumenten doorgegeven aan de Query-methode: 
   * parameters.AlternateFilters.Count: 1
   * de parameters. AlternateFilters.ElementAt(0). AttributePath: "externalId"
   * parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals
   * de parameters. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
   * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"] 

2. Als het antwoord op een query met de webservice voor een gebruiker met een externalId kenmerk-waarde die overeenkomt met de waarde van het kenmerk mailNickname van een gebruiker geen gebruikers worden geretourneerd, klikt u vervolgens Azure Active Directory-aanvragen dat de service inrichten van een gebruiker die overeenkomt met de in Azure Active Directory.  Hier volgt een voorbeeld van een aanvraag voor: 

   ```
     POST https://.../scim/Users HTTP/1.1
     Authorization: Bearer ...
     Content-type: application/scim+json
     {
       "schemas":
       [
         "urn:ietf:params:scim:schemas:core:2.0:User",
         "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
       "externalId":"jyoung",
       "userName":"jyoung",
       "active":true,
       "addresses":null,
       "displayName":"Joy Young",
       "emails": [
         {
           "type":"work",
           "value":"jyoung@Contoso.com",
           "primary":true}],
       "meta": {
         "resourceType":"User"},
        "name":{
         "familyName":"Young",
         "givenName":"Joy"},
       "phoneNumbers":null,
       "preferredLanguage":null,
       "title":null,
       "department":null,
       "manager":null}
   ```

   De Common Language Infrastructure-bibliotheken geleverd door Microsoft voor het implementeren van services SCIM zou dit verzoek vertalen naar een aanroep van de methode voor maken van de service-provider.  De methode voor maken, heeft deze handtekening: 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
 
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create (
       Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
       string correlationIdentifier);
   ```

   In een aanvraag voor het inrichten van een gebruiker, is de waarde van de resource-argument een exemplaar van de Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser klasse is gedefinieerd in de bibliotheek Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  Als de aanvraag voor het inrichten van de gebruiker is geslaagd, wordt de implementatie van de methode verwacht om terug te keren een exemplaar van de Microsoft.SystemForCrossDomainIdentityManagement. Klasse van Core2EnterpriseUser, met de waarde van de id-eigenschap ingesteld op de unieke id van de nieuw ingerichte gebruiker.  

3. Voor het bijwerken van een gebruiker bekend is dat in een archief voor de klantidentiteit fronted door een SCIM bestaan, wordt Azure Active Directory uitgevoerd door het aanvragen van de huidige status van die gebruiker van de service met een aanvraag, zoals: 

   ```
     GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
   ```

   De aanvraag wordt in een service die is gebouwd met behulp van de Common Language Infrastructure-bibliotheken geleverd door Microsoft voor het implementeren van services SCIM, omgezet in een aanroep van de methode ophalen van de provider van de service.  Dit is de handtekening van de methode ophalen: 

    ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
     // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
        Retrieve(
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
            parameters, 
            string correlationIdentifier);
 
     public interface 
       Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
         IRetrievalParameters
         {
           Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
             ResourceIdentifier 
               { get; }
     }
     public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
     {
         string Identifier 
           { get; set; }
         string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
           { get; set; }
     }
   ```

   In het voorbeeld van een aanvraag voor het ophalen van de huidige status van een gebruiker, zijn de waarden van de eigenschappen van het object dat is opgegeven als de waarde van het argument parameters als volgt uit: 
  
   * -ID: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. Als een verwijzingskenmerk worden bijgewerkt, wordt Azure Active Directory query naar de service om te bepalen of de huidige waarde van het verwijzingskenmerk in het identiteitsarchief op de al fronted door de service komt overeen met de waarde van dit kenmerk in Azure Active De map. Voor gebruikers is het enige kenmerk waarvan de huidige waarde op deze manier wordt gevraagd het kenmerk manager. Hier volgt een voorbeeld van een verzoek om te bepalen of het kenmerk manager van een bepaalde gebruiker-object op dat moment een bepaalde waarde heeft: 

   ```
     GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq  2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
     Authorization: Bearer ...
   ```

   De waarde van de queryparameter van kenmerken, 'ID' betekent dat als een gebruikersobject bestaat dat voldoet aan de expressie die is opgegeven als de waarde van de queryparameter filter, en vervolgens wordt verwacht dat de service reageren met een ' urn: ietf:params:scim:schemas:core:2.0: Gebruiker' of "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User" resource, met inbegrip van alleen de waarde van 'ID'-kenmerk van die resource.  De waarde van de **ID** kenmerk bekend is dat de aanvrager. Het is opgenomen in de waarde van de queryparameter filter; het doel van dit wordt gevraagd is daadwerkelijk om aan te vragen van een minimale weergave van een resource die voldoen aan de filterexpressie aan te geven of deze objecten bestaat.   

   Als de service is gebouwd met behulp van de Common Language Infrastructure-bibliotheken geleverd door Microsoft voor het implementeren van services SCIM, wordt klikt u vervolgens de aanvraag omgezet in een aanroep van de Query-methode van de provider van de service. De waarde van de eigenschappen van het object dat is opgegeven als de waarde van het argument parameters zijn als volgt: 
  
   * parameters.AlternateFilters.Count: 2
   * parameters.AlternateFilters.ElementAt(x).AttributePath: 'Id'
   * parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals
   * de parameters. AlternateFilter.ElementAt(x). ComparisonValue:  "54D382A4-2050-4C03-94D1-E769F1D15682"
   * de parameters. AlternateFilters.ElementAt(y). AttributePath: "manager"
   * de parameters. AlternateFilters.ElementAt(y). De vergelijkingsoperator: ComparisonOperator.Equals
   * de parameters. AlternateFilter.ElementAt(y). ComparisonValue:  "2819c223-7f76-453a-919d-413861904646"
   * parameters.RequestedAttributePaths.ElementAt(0): 'Id'
   * de parameters. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

   Hier de waarde van de x-index kan zijn ingesteld op 0 en 1, kan worden door de waarde van de index van de y of de waarde van x mogelijk 1 en de waarde van y kan zijn ingesteld op 0, afhankelijk van de volgorde van de expressies van het filter-queryparameter.   

5. Hier volgt een voorbeeld van een aanvraag van Azure Active Directory aan een service SCIM om bij te werken van een gebruiker: 

   ```
     PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
     Content-type: application/scim+json
     {
       "schemas": 
       [
         "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
       "Operations":
       [
         {
           "op":"Add",
           "path":"manager",
           "value":
             [
               {
                 "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                 "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
   ```

   De Microsoft Common Language Infrastructure-bibliotheken voor het implementeren van services SCIM vertaalt de aanvraag in een aanroep van de Update-methode van de provider van de service. Dit is de handtekening van de Update-methode: 

   ```
     // System.Threading.Tasks.Tasks and 
     // System.Collections.Generic.IReadOnlyCollection<T>
     // are defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
     // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
     // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
     // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
     // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
     // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
     // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

     System.Threading.Tasks.Task Update(
       Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
       string correlationIdentifier);

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
     {
     Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
       PatchRequest 
         { get; set; }
     Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
       ResourceIdentifier 
         { get; set; }        
     }

     public class PatchRequest2: 
       Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
     {
     public System.Collections.Generic.IReadOnlyCollection
       <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
         Operations
         { get;}

     public void AddOperation(
       Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
     }

     public class PatchOperation
     {
     public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
       Name
       { get; set; }

     public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
       Path
       { get; set; }

     public System.Collections.Generic.IReadOnlyCollection
       <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
       { get; }

     public void AddValue(
       Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
     }

     public enum OperationName
     {
       Add,
       Remove,
       Replace
     }

     public interface IPath
     {
       string AttributePath { get; }
       System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
       Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
     }

     public class OperationValue
     {
       public string Reference
       { get; set; }

       public string Value
       { get; set; }
     }
   ```

    In het voorbeeld van een aanvraag voor het bijwerken van een gebruiker, is het object dat is opgegeven als de waarde van de patch-argument waarden van deze eigenschappen: 
  
   * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
   * (PatchRequest als PatchRequest2). Operations.Count: 1
   * (PatchRequest als PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
   * (PatchRequest als PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "manager"
   * (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.Count: 1
   * (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Verwijzing: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
   * (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Waarde: 2819c223-7f76-453A-919d-413861904646

6. Voor het inrichten ongedaan maken van een gebruiker vanuit een archief voor de klantidentiteit fronted door een SCIM-service, Azure AD een aanvraag verzendt, zoals: 

   ```
     DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
   ```

   Als de service is gebouwd met behulp van de Common Language Infrastructure-bibliotheken geleverd door Microsoft voor het implementeren van services SCIM, wordt klikt u vervolgens de aanvraag omgezet in een aanroep naar de Delete-methode van de provider van de service.   Deze methode heeft deze handtekening: 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
     // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
     System.Threading.Tasks.Task Delete(
       Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
         resourceIdentifier, 
       string correlationIdentifier);
   ```

   Het object dat is opgegeven als de waarde van het argument resourceIdentifier heeft deze eigenschapswaarden in het voorbeeld van een aanvraag voor de inrichting ongedaan maken van een gebruiker: 

   * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="group-provisioning-and-de-provisioning"></a>Groepsinrichting en ongedaan maken inrichting
De volgende afbeelding ziet u de berichten die Azure AcD naar een SCIM-service verzendt voor het beheren van de levenscyclus van een groep in een andere identiteit store.  Deze berichten verschillen van de berichten die betrekking hebben op gebruikers op drie manieren: 

* Het schema van een bron van gebruikersgroep wordt geïdentificeerd als `http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group`.  
* Aanvragen voor het ophalen van groepen kunt u bepalen dat het kenmerk leden moeten worden uitgesloten van een resource die is opgegeven in antwoord op de aanvraag is.  
* Verzoeken om te bepalen of een verwijzingskenmerk een bepaalde waarde heeft zijn aanvragen over het kenmerk leden.  

![][5]
*Afbeelding 6: Groepsinrichting en ongedaan maken inrichting reeks*

## <a name="related-articles"></a>Verwante artikelen:
* [Gebruiker inrichting/ongedaan maken van inrichting voor SaaS-toepassingen automatiseren](user-provisioning.md)
* [Kenmerktoewijzingen voor het inrichten van gebruikers aan te passen](customize-application-attributes.md)
* [Expressies schrijven voor kenmerktoewijzingen](functions-for-customizing-application-data.md)
* [Bereikfilters toevoegen voor het inrichten van gebruikers](define-conditional-rules-for-provisioning-user-accounts.md)
* [Meldingen over accountinrichting](user-provisioning.md)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](../saas-apps/tutorial-list.md)

<!--Image references-->
[0]: ./media/use-scim-to-provision-users-and-groups/scim-figure-1.png
[1]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2a.png
[2]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2b.png
[3]: ./media/use-scim-to-provision-users-and-groups/scim-figure-3.png
[4]: ./media/use-scim-to-provision-users-and-groups/scim-figure-4.png
[5]: ./media/use-scim-to-provision-users-and-groups/scim-figure-5.png
