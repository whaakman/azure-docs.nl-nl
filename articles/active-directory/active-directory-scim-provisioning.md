---
title: Inrichting van apps in Azure Active Directory met behulp van SCIM automatiseren | Microsoft Docs
description: Azure Active Directory kunnen automatisch worden ingericht, gebruikers en groepen naar een toepassing of identiteit store die door een webservice is fronted met de interface die is gedefinieerd in de specificatie SCIM protocol
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: mtillman
editor: 
ms.assetid: 4d86f3dc-e2d3-4bde-81a3-4a0e092551c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;seohack1
ms.openlocfilehash: 17732ae616339020f11bc8973dc57b6d0fff4884
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="using-system-for-cross-domain-identity-management-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a>Systeem voor het identiteitsbeheer van verschillende domeinen gebruiken voor het automatisch inrichten van gebruikers en groepen van Azure Active Directory voor toepassingen

## <a name="overview"></a>Overzicht
Azure Active Directory (Azure AD) kunnen automatisch worden ingericht, gebruikers en groepen naar een toepassing of identiteit store die door een webservice met de interface is fronted gedefinieerd in de [systeem voor verschillende domeinen Identity Management (SCIM) 2.0-protocol specificatie](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory kunt verzenden aanvragen voor het maken, wijzigen of verwijderen toegewezen gebruikers en groepen met de webservice. De webservice kan vervolgens deze aanvragen worden omgezet in bewerkingen op de doel-id-store. 

![][0]
*Afbeelding 1: Inrichting van Azure Active Directory naar een winkel identiteit via een webservice*

Deze mogelijkheid kan worden gebruikt in combinatie met de functie 'bring your own app' in Azure AD voor eenmalige aanmelding en automatisch gebruikers inrichten voor toepassingen die op of door een webservice SCIM zijn fronted.

Er zijn twee gebruiksvoorbeelden voor using SCIM in Azure Active Directory:

* **Inrichting van gebruikers en groepen van toepassingen die ondersteuning bieden voor SCIM** toepassingen die ondersteuning bieden voor SCIM 2.0 en bearer-tokens van OAuth gebruiken voor verificatie met Azure AD zonder configuratie werkt.
* **Maak uw eigen inrichting oplossing voor toepassingen die ondersteuning bieden voor andere inrichting op basis van een API** voor niet-SCIM toepassingen, kunt u een eindpunt SCIM voor de omzetting tussen het eindpunt van de Azure AD SCIM en API biedt ondersteuning voor de gebruiker voor de toepassing maken inrichting. Als u een eindpunt SCIM ontwikkelen, bieden we Common Language Infrastructure (CLI)-bibliotheken en codevoorbeelden die wordt beschreven hoe u met een eindpunt SCIM bieden en vertalen SCIM berichten.  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Gebruikers en groepen van toepassingen die ondersteuning bieden voor SCIM inrichten
Azure AD kan worden geconfigureerd voor het automatisch inrichten toegewezen gebruikers en groepen van toepassingen die worden geïmplementeerd een [systeem voor verschillende domeinen Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) webservice en OAuth bearer-tokens voor authenticatie geaccepteerd. Binnen de SCIM 2.0-specificatie toepassingen moeten voldoen aan deze vereisten voldoet:

* Ondersteunt het maken van gebruikers en/of groepen aan de hand van sectie 3.3 van het protocol SCIM.  
* Biedt ondersteuning voor gebruikers en/of groepen met patch-aanvragen aan de hand van sectie 3.5.2 van het protocol SCIM wijzigen.  
* Ondersteunt het ophalen van een bekende bron volgens punt 3.4.1 van het protocol SCIM.  
* Biedt ondersteuning voor gebruikers en/of groepen aan de hand van sectie 3.4.2 van het protocol SCIM opvragen.  Standaard door externalId aan gebruikers gevraagd en groepen worden opgevraagd met de weergavenaam.  
* Ondersteunt het opvragen van de gebruiker door-ID en -beheer volgens sectie 3.4.2 van het protocol SCIM.  
* Ondersteunt het uitvoeren van query's groepen door-ID en door een lid aan de hand van sectie 3.4.2 van het protocol SCIM.  
* OAuth-bearer-tokens voor autorisatie volgens punt 2.1 van het protocol SCIM accepteert.

Neem contact op met uw toepassingsprovider of de documentatie van uw toepassingsprovider voor overzichten van compatibiliteit met deze vereisten.

### <a name="getting-started"></a>Aan de slag
Toepassingen die ondersteuning bieden voor het profiel SCIM is beschreven in dit artikel kunnen worden gekoppeld aan Azure Active Directory met de functie 'niet galerie application' in de galerie van Azure AD-toepassing. Eenmaal zijn verbonden, voert Azure AD een synchronisatieproces om de 20 minuten waar deze query's van de toepassing SCIM eindpunt voor toegewezen gebruikers en groepen uit en maakt of wijzigt ze volgens de details van de toewijzing.

**Verbinding met het maken van een toepassing die SCIM ondersteunt:**

1. Aanmelden bij [de Azure-portal](https://portal.azure.com). 
2. Blader naar ** Azure Active Directory > zakelijke toepassingen en selecteer **nieuwe toepassing > alle > niet-galerie toepassing**.
3. Voer een naam voor uw toepassing en klik op **toevoegen** pictogram van een app-object te maken.
    
  ![][1]
  *Afbeelding 2: Azure AD-toepassingsgalerie*
    
4. Selecteer in het scherm voor het resulterende de **inrichten** tabblad in de linkerkolom staat.
5. In de **modus inrichting** selecteert u **automatische**.
    
  ![][2]
  *Afbeelding 3: Configureren in de Azure portal-inrichting*
    
6. In de **Tenant-URL** en voer de URL van de toepassing SCIM eindpunt. Voorbeeld: https://api.contoso.com/scim/v2/
7. Als het eindpunt SCIM een OAuth bearer-token van een verlener dan Azure AD vereist, kopieert u de vereiste OAuth bearer-token naar de optionele **geheim Token** veld. Als dit veld leeg blijft, opgenomen een OAuth-bearer-token uitgegeven vanuit Azure AD bij elke aanvraag met Azure AD. Apps die gebruikmaken van Azure AD als een id-provider kunt controleren of deze Azure AD-verleend token.
8. Klik op de **testverbinding** knop Azure Active Directory proberen te verbinden met het eindpunt SCIM hebben. Als de pogingen mislukken, wordt informatie over de fout wordt weergegeven.  
9. Als de pogingen tot verbinding maken met de toepassing te voltooien, klikt u op **opslaan** om op te slaan de beheerdersreferenties.
10. In de **toewijzingen** sectie, zijn er twee sets van selecteerbare Kenmerktoewijzingen: één voor gebruikersobjecten en één voor groepsobjecten. Selecteer elke om te controleren van de kenmerken die aan uw app van Azure Active Directory zijn gesynchroniseerd. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikers en groepen in uw app voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

    >[!NOTE]
    >U kunt desgewenst uitschakelen door het uitschakelen van de toewijzing 'groepen' van een groepsobjecten worden gesynchroniseerd. 

11. Onder **instellingen**, wordt de **bereik** veld wordt gedefinieerd welke gebruikers en groepen worden gesynchroniseerd. Selecteren 'Sync alleen toegewezen gebruikers en groepen' (aanbevolen) synchroniseert alleen gebruikers en groepen die zijn toegewezen de **gebruikers en groepen** tabblad.
12. Zodra de configuratie voltooid is, verandert de **inrichting Status** naar **op**.
13. Klik op **opslaan** starten van de Azure AD-service inricht. 
14. Als gebruikers en groepen (aanbevolen) synchronisatie alleen worden toegewezen, moet u selecteren de **gebruikers en groepen** tabblad en toewijzen van gebruikers en/of groepen die u wilt synchroniseren.

Nadat de initiële synchronisatie is gestart, kunt u de **controlelogboeken** tabblad aan de vooruitgang van de monitor, waarin alle acties die worden uitgevoerd door de inrichting van uw app-service. Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).

>[!NOTE]
>De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>Uw eigen oplossing voor elke toepassing bouwen
Als u een SCIM-webservice die is gekoppeld met Azure Active Directory maakt, kunt u één gebruiker voor eenmalige aanmelding en automatische inrichting voor vrijwel elke toepassing die voorziet in een API-inrichting REST of SOAP-gebruiker.

Hier ziet u hoe het werkt:

1. Azure AD levert een algemene language infrastructure-bibliotheek met de naam [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Systeemintegrators en ontwikkelaars kunnen gebruikmaken van deze bibliotheek maken en implementeren van een SCIM gebaseerde webservice-eindpunt kan verbinding maken met Azure AD voor een toepassingsarchief identiteit.
2. Toewijzingen worden geïmplementeerd in de webservice schema voor de gestandaardiseerde toewijzen aan het gebruikersschema en het protocol is vereist voor de toepassing.
3. De eindpunt-URL is geregistreerd in Azure AD als onderdeel van een aangepaste toepassing in de galerie met toepassingen.
4. Gebruikers en groepen zijn toegewezen aan deze toepassing in Azure AD. Bij toewijzing, worden ze in een wachtrij moeten worden gesynchroniseerd naar de doeltoepassing geplaatst. Het synchronisatieproces afhandeling van de wachtrij wordt uitgevoerd om de 20 minuten.

### <a name="code-samples"></a>Codevoorbeelden
Om dit proces eenvoudiger, een reeks [codevoorbeelden](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) zijn opgegeven die een SCIM webservice-eindpunt maken en Demonstreer automatische inrichting. Een voorbeeld is van een provider die wordt onderhouden door een bestand met de rijen met door komma's gescheiden waarden die gebruikers en groepen vertegenwoordigt.  Het andere type is van een provider die wordt toegepast op de Amazon Web Services Identity and Access Management-service.  

**Vereisten**

* Visual Studio 2013 of hoger
* [Azure-SDK voor .NET](https://azure.microsoft.com/downloads/)
* Windows-computer die ondersteuning biedt voor de ASP.NET-framework 4.5 moet worden gebruikt als het SCIM-eindpunt. Deze machine moet toegankelijk zijn vanuit de cloud
* [Een Azure-abonnement met een proefabonnement of een gelicentieerde versie van Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* Het voorbeeld Amazon AWS vereist bibliotheken van de [AWS-Toolkit voor Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Zie het Leesmij-bestand met het voorbeeld voor meer informatie.

### <a name="getting-started"></a>Aan de slag
De eenvoudigste manier voor het implementeren van een SCIM-eindpunt dat inrichting verzoeken van Azure AD kan accepteren is het bouwen en implementeren van de voorbeeldcode die de ingerichte gebruikers naar een bestand met door komma's gescheiden waarden (CSV levert).

**Een voorbeeld SCIM-eindpunt maken:**

1. De code voorbeeld downloaden op [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2. Pak het pakket en plaats het op uw Windows-machine op een locatie zoals C:\AzureAD-BYOA-Provisioning-Samples\.
3. In deze map, start u de FileProvisioningAgent oplossing in Visual Studio.
4. Selecteer **Extra > Library Package Manager > Package Manager Console**, en voer de volgende opdrachten voor het project FileProvisioningAgent omzetten van de oplossing verwijzingen:
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. Bouw het project FileProvisioningAgent.
6. Start de toepassing van de opdrachtprompt in Windows (als Administrator) en gebruiken de **cd** opdracht om te wijzigen van de directory voor uw **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** de map.
7. Voer de volgende opdracht, waarbij < ip-adres > vervangt door de IP-adres of domeinnaam de naam van de Windows-computer:
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. In Windows onder **Windows-instellingen > netwerk- en instellingen voor Internet**, selecteer de **Windows Firewall > instellingen voor geavanceerde**, en maak een **regel voor binnenkomende verbindingen** die kan inkomende toegang tot poort 9000.
9. Als de Windows-computer zich achter een router, moet de router worden geconfigureerd voor het uitvoeren van Network Access Translation tussen de poort 9000 die wordt blootgesteld aan internet en poort 9000 op de Windows-computer. Dit is vereist voor Azure AD kunnen toegang krijgen tot dit eindpunt in de cloud.

**Het voorbeeld SCIM eindpunt in Azure AD registreren:**

1. Aanmelden bij [de Azure-portal](https://portal.azure.com). 
2. Blader naar ** Azure Active Directory > zakelijke toepassingen en selecteer **nieuwe toepassing > alle > niet-galerie toepassing**.
3. Voer een naam voor uw toepassing en klik op **toevoegen** pictogram van een app-object te maken. Het application-object gemaakt is bedoeld om de doel-app u zou inrichten en implementeren van eenmalige aanmelding voor, en niet alleen het eindpunt SCIM vertegenwoordigen.
4. Selecteer in het scherm voor het resulterende de **inrichten** tabblad in de linkerkolom staat.
5. In de **modus inrichting** selecteert u **automatische**.
    
  ![][2]
  *Afbeelding 4: Configureren in de Azure portal-inrichting*
    
6. In de **Tenant-URL** en voer de beschikbaar gesteld op internet-URL en poort van uw eindpunt SCIM. Zou dit iets zoals http://testmachine.contoso.com:9000 of http://<ip-address>:9000/, waarbij < ip-adres > internet is blootgesteld IP adres.  
7. Als het eindpunt SCIM een OAuth bearer-token van een verlener dan Azure AD vereist, kopieert u de vereiste OAuth bearer-token naar de optionele **geheim Token** veld. Als dit veld leeg laat, wordt Azure AD een OAuth-bearer-token van Azure AD bij elke aanvraag uitgegeven opnemen. Apps die gebruikmaken van Azure AD als een id-provider kunt controleren of deze Azure AD-verleend token.
8. Klik op de **testverbinding** knop Azure Active Directory proberen te verbinden met het eindpunt SCIM hebben. Als de pogingen mislukken, wordt informatie over de fout wordt weergegeven.  
9. Als de pogingen tot verbinding maken met de toepassing te voltooien, klikt u op **opslaan** om op te slaan de beheerdersreferenties.
10. In de **toewijzingen** sectie, zijn er twee sets van selecteerbare Kenmerktoewijzingen: één voor gebruikersobjecten en één voor groepsobjecten. Selecteer elke om te controleren van de kenmerken die aan uw app van Azure Active Directory zijn gesynchroniseerd. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikers en groepen in uw app voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.
11. Onder **instellingen**, wordt de **bereik** veld wordt gedefinieerd welke gebruikers en groepen worden gesynchroniseerd. Selecteren 'Sync alleen toegewezen gebruikers en groepen' (aanbevolen) synchroniseert alleen gebruikers en groepen die zijn toegewezen de **gebruikers en groepen** tabblad.
12. Zodra de configuratie voltooid is, verandert de **inrichting Status** naar **op**.
13. Klik op **opslaan** starten van de Azure AD-service inricht. 
14. Als gebruikers en groepen (aanbevolen) synchronisatie alleen worden toegewezen, moet u selecteren de **gebruikers en groepen** tabblad en toewijzen van gebruikers en/of groepen die u wilt synchroniseren.

Nadat de initiële synchronisatie is gestart, kunt u de **controlelogboeken** tabblad aan de vooruitgang van de monitor, waarin alle acties die worden uitgevoerd door de inrichting van uw app-service. Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).

De laatste stap bij het controleren van het voorbeeld is de TargetFile.csv-bestand te openen in de map \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug op uw Windows-computer. Zodra het inrichtingsproces wordt uitgevoerd, wordt dit bestand bevat de details van alle toegewezen en ingericht gebruikers en groepen.

### <a name="development-libraries"></a>-Ontwikkelbibliotheken
Voor het ontwikkelen van uw eigen webservice die aan de specificatie SCIM voldoet eerst raken met de volgende bibliotheken geleverd door Microsoft te helpen het ontwikkelingsproces versnellen: 

1. Algemene Language Infrastructure (CLI) bibliotheken zijn beschikbaar voor gebruik met talen die op basis van die infrastructuur, zoals C#. Een van deze bibliotheken [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), declareert een interface Microsoft.SystemForCrossDomainIdentityManagement.IProvider, weergegeven in de volgende afbeelding: A ontwikkelaar met behulp van de bibliotheken wilt implementeren die interface met een klasse die algemeen als een provider, kan worden verwezen. De tapewisselaars inschakelen de ontwikkelaar voor het implementeren van een webservice die voldoet aan de SCIM-specificatie. De webservice kan ofwel worden gehost in Internet Information Services of een uitvoerbaar Common Language Infrastructure-assembly. Aanvraag wordt omgezet in aanroepen van methoden van de provider, die door de ontwikkelaar zou worden geprogrammeerd bewerkingen uitvoeren op bepaalde identiteit store.
  
  ![][3]
  
2. [Express route-handlers](http://expressjs.com/guide/routing.html) zijn beschikbaar voor het parseren van de aanvraag-objecten van een node.js-aanroepen (zoals gedefinieerd door de specificatie SCIM), die aangebracht aan een node.js-web-service.   

### <a name="building-a-custom-scim-endpoint"></a>Het bouwen van een aangepaste SCIM-eindpunt
Met behulp van de CLI-bibliotheken, kunnen ontwikkelaars die bibliotheken met hun services in uitvoerbaar Common Language Infrastructure assembly of in Internet Information Services host. Hier volgt een voorbeeld van code voor het hosten van een service binnen een uitvoerbare assembly, het adres http://localhost:9000: 

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

Deze service moet een HTTP-adres en de server certificaat voor serververificatie hebben waarvan de basiscertificeringsinstantie het volgende is: 

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

De opgegeven waarde voor het argument certhash is hier de vingerafdruk van het certificaat, terwijl de opgegeven waarde voor het argument appid een globally unique identifier is.  

Voor het hosten van de Internet Information Services-service, zou een ontwikkelaar een CLA code bibliotheek-assembly bouwen met een klasse met de naam opstarten in de standaardnaamruimte van de assembly.  Hier volgt een voorbeeld van deze klasse: 

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
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>Afhandeling van endpoint-verificatie
Aanvragen van Azure Active Directory bevatten een OAuth 2.0-bearer-token.   Alle services die de aanvraag ontvangt, moet de verlener als Azure Active Directory namens de verwachte Azure Active Directory-tenant voor toegang tot de webservice Azure Active Directory Graph verifiëren.  In het token de certificaatverlener die wordt geïdentificeerd door een claim iss, zoals 'iss': 'https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/'.  In dit voorbeeld wordt het basisadres van de claimwaarde https://sts.windows.net, identificeert Azure Active Directory als de verlener terwijl het segment relatief adres, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is de unieke id van de Azure Active Directory tenant namens die het token is uitgegeven.  Als het token is uitgegeven voor toegang tot de Azure Active Directory Graph-webservice, klikt u vervolgens moet de id van die service, 00000002-0000-0000-c000-000000000000, de waarde van het token aud claim.  

Ontwikkelaars met behulp van de bibliotheken CLA is geleverd door Microsoft voor het bouwen van een service SCIM kunnen verifiëren aanvragen van Azure Active Directory met het pakket Microsoft.Owin.Security.ActiveDirectory met de volgende stappen: 

1. In een provider, door de eigenschap Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior te implementeren door het retourneren van een methode om te worden aangeroepen wanneer de service wordt gestart: 

  ````
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
  ````

2. De volgende code toevoegen aan deze methode om een aanvraag aan een van de service-eindpunten die zijn geverifieerd als een token dat is uitgegeven door Azure Active Directory namens een tenant opgegeven voor toegang tot de webservice Azure AD Graph voorzien: 

  ````
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
  ````


## <a name="user-and-group-schema"></a>Schema voor gebruikers en groepen
Azure Active Directory kunnen twee soorten resources met webservices SCIM inrichten.  Deze typen resources zijn gebruikers en groepen.  

User-bronnen worden geïdentificeerd door de schema-id en urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, die is opgenomen in deze specificatie van het protocol: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  De standaardtoewijzing van de kenmerken van gebruikers in Azure Active Directory in de kenmerken van resources urn: ietf:params:scim:schemas:extension:enterprise:2.0:User hieronder vindt u in de tabel 1.  

Groep resources worden geïdentificeerd door de schema-id en http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabel 2 hieronder bevat de standaardtoewijzing van de kenmerken van groepen in Azure Active Directory op de kenmerken van http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resources.  

### <a name="table-1-default-user-attribute-mapping"></a>Tabel 1: Standaard Gebruikerskoppeling kenmerk
| Azure Active Directory user | urn:ietf:params:scim:schemas:extension:enterprise:2.0:User |
| --- | --- |
| IsSoftDeleted |actief |
| displayName |displayName |
| Facsimile-TelephoneNumber |phoneNumbers type eq 'fax'.value |
| givenName |name.givenName |
| jobTitle |titel |
| E-mail |e-mailberichten type eq 'werk'.value |
| mailNickname |externalId |
| manager |manager |
| mobiele |phoneNumbers[type eq "mobile"].value |
| objectId |id |
| postalCode |addresses[type eq "work"].postalCode |
| proxy-adressen |e-mailberichten [Geef eq 'andere']. Waarde |
| physical-Delivery-OfficeName |adressen [Geef eq 'andere']. Indeling |
| StreetAddress |adressen type eq 'werk'.streetAddress |
| Achternaam |name.familyName |
| Telefoonnummer |phoneNumbers type eq 'werk'.value |
| user-PrincipalName |userName |

### <a name="table-2-default-group-attribute-mapping"></a>Tabel 2: Standaard kenmerk apparaatgroeptoewijzing
| Azure Active Directory-groep | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| displayName |externalId |
| E-mail |e-mailberichten type eq 'werk'.value |
| mailNickname |displayName |
| leden |leden |
| objectId |id |
| proxyAddresses |e-mailberichten [Geef eq 'andere']. Waarde |

## <a name="user-provisioning-and-de-provisioning"></a>Gebruikers inrichten en de inrichting
De volgende afbeelding ziet u de Azure Active Directory voor het beheren van de levenscyclus van een gebruiker in een andere identiteit store verzendt naar een service SCIM berichten. Het diagram toont ook hoe een SCIM service geïmplementeerd met behulp van de CLI-bibliotheken geleverd door Microsoft voor het bouwen dat dergelijke services deze aanvragen vertalen in aanroepen van de methoden van een provider.  

![][4]
*Afbeelding 5: Gebruikers inrichten en de inrichting reeks*

1. Azure Active Directory query op de service voor een gebruiker met een externalId-kenmerkwaarde die overeenkomt met de waarde van het mailNickname-kenmerk van een gebruiker in Azure AD. De query wordt uitgedrukt als een aanvraag Hypertext Transfer Protocol (HTTP) zoals dit voorbeeld waarin jyoung een voorbeeld van een mailNickname van een gebruiker in Azure Active Directory is: 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  Als de service is gebouwd met behulp van de Common Language Infrastructure-bibliotheken voor het implementeren van services SCIM door Microsoft geleverd, wordt de aanvraag omgezet in een aanroep van de Query-methode van de service provider.  Dit is de handtekening van deze methode: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
  ````
  Hier volgt de definitie van de interface Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters: 
  ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
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
  ````
  In het volgende voorbeeld van een query voor een gebruiker met een opgegeven waarde voor het kenmerk externalId zijn de waarden van de argumenten doorgegeven aan de Query-methode: 
  * parameters.AlternateFilters.Count: 1
  * parameters. AlternateFilters.ElementAt(0). AttributePath: 'externalId'
  * parameters. AlternateFilters.ElementAt(0). Vergelijkingsoperator: ComparisonOperator.Equals
  * parameters. AlternateFilter.ElementAt(0). ComparisonValue: 'jyoung'
  * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"] 

2. Als het antwoord op een query met de webservice voor een gebruiker met een kenmerkwaarde externalId die overeenkomt met de waarde van het mailNickname-kenmerk van een gebruiker niet alle gebruikers, klikt u vervolgens Azure Active Directory-aanvragen dat de service een gebruiker die overeenkomt met het inrichten in Azure Active Directory.  Hier volgt een voorbeeld van een aanvraag: 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
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
  ````
  De Common Language Infrastructure-bibliotheken geleverd door Microsoft voor het implementeren van services SCIM zou die aanvraag vertalen naar een aanroep van de methode voor maken van de service provider.  De methode Create heeft deze handtekening: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  De waarde van de resource-argument is in een aanvraag voor het inrichten van een gebruiker een exemplaar van de Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser klasse is gedefinieerd in de bibliotheek Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  Als de aanvraag voor het inrichten van de gebruiker is gelukt, wordt de implementatie van de methode verwacht een exemplaar van de Microsoft.SystemForCrossDomainIdentityManagement retourneren. Core2EnterpriseUser klasse met de waarde van de id-eigenschap ingesteld op de unieke id van de nieuw ingerichte gebruiker.  

3. Voor het bijwerken van een gebruiker bekend in een archief fronted door een SCIM is Azure Active Directory wordt uitgevoerd door de huidige status van die gebruiker vraagt van de service met een aanvraag, zoals: 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  De aanvraag wordt in een service gemaakt met de Common Language Infrastructure-bibliotheken geleverd door Microsoft voor het implementeren van services SCIM wordt omgezet in een aanroep van de methode ophalen van de service provider.  Dit is de handtekening van de methode ophalen: 
  ````
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
  ````
  In het voorbeeld van een aanvraag voor het ophalen van de huidige status van een gebruiker zijn de waarden van de eigenschappen van het object dat is opgegeven als de waarde van het argument parameters als volgt uit: 
  
  * Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * SchemaIdentifier: 'urn: ietf:params:scim:schemas:extension:enterprise:2.0:User'

4. Als een verwijzingskenmerk is worden bijgewerkt, wordt de service om te bepalen of de huidige waarde van het verwijzingskenmerk in de store identiteit op de al fronted door de service Azure Active Directory een query overeenkomt met de waarde van dit kenmerk in Azure Active De map. Voor gebruikers is het enige kenmerk waarvan de huidige waarde op deze manier wordt opgevraagd het kenmerk manager. Hier volgt een voorbeeld van een aanvraag om te bepalen of het kenmerk manager van een bepaalde gebruiker-object een bepaalde waarde heeft: 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  De waarde van de queryparameter kenmerken id, betekent dat als een gebruikersobject aanwezig is die voldoet aan de expressie die is opgegeven als de waarde van de queryparameter filter vervolgens de service moet reageren met een urn: ietf:params:scim:schemas:core:2.0:User of urn: ietf:params:scim:schemas:extension:enterprise:2.0:User bron, met inbegrip van alleen de waarde van kenmerk van de bron-id.  De waarde van de **id** kenmerk bekend is dat de aanvrager. Het is opgenomen in de waarde van de queryparameter filter; het doel van deze vragen is daadwerkelijk om aan te vragen een minimale representatie van een resource die voldoen aan de filterexpressie een indicatie van het al dan niet een dergelijk object bestaat.   

  Als de service is gebouwd met behulp van de Common Language Infrastructure-bibliotheken voor het implementeren van services SCIM door Microsoft geleverd, wordt de aanvraag omgezet in een aanroep van de Query-methode van de service provider. De waarde van de eigenschappen van het object dat is opgegeven als de waarde van het argument parameters zijn als volgt: 
  
  * parameters.AlternateFilters.Count: 2
  * parameters. AlternateFilters.ElementAt(x). AttributePath: 'id'
  * parameters. AlternateFilters.ElementAt(x). Vergelijkingsoperator: ComparisonOperator.Equals
  * parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * parameters. AlternateFilters.ElementAt(y). AttributePath: 'manager'
  * parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals
  * parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
  * parameters. RequestedAttributePaths.ElementAt(0): 'id'
  * parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

  Hier de waarde van de x-index kan niet 0 en 1, is de waarde van de index y mogelijk of de waarde van x mogelijk 1 en de waarde van y kan zijn ingesteld op 0, afhankelijk van de volgorde van de expressies van de queryparameter filter.   

5. Hier volgt een voorbeeld van een aanvraag van Azure Active Directory een SCIM-service voor het bijwerken van een gebruiker: 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
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
  ````
  De Microsoft Common Language Infrastructure-bibliotheken voor het implementeren van services SCIM wordt de aanvraag worden omgezet in een aanroep van de methode Update van de service provider. Dit is de handtekening van de methode Update: 
  ````
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
  ````
    In het voorbeeld van een aanvraag voor het bijwerken van een gebruiker heeft het object dat is opgegeven als de waarde van de patch-argument waarden van deze eigenschappen: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: 'urn: ietf:params:scim:schemas:extension:enterprise:2.0:User'
  * (PatchRequest als PatchRequest2). Operations.Count: 1
  * (PatchRequest als PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
  * (PatchRequest als PatchRequest2). Operations.ElementAt(0). Path.AttributePath: 'manager'
  * (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.Count: 1
  * (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Verwijzing: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
  * (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Waarde: 2819c223-7f76-453a-919d-413861904646

6. Voor het inrichten van een gebruiker uit een identiteit store fronted door een service SCIM ongedaan, verzendt Azure AD een aanvraag op, zoals: 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Als de service is gebouwd met behulp van de Common Language Infrastructure-bibliotheken voor het implementeren van services SCIM door Microsoft geleverd, wordt de aanvraag omgezet in een aanroep van de Delete-methode van de service provider.   Deze methode heeft deze handtekening: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  Het object dat is opgegeven als de waarde van het argument resourceIdentifier heeft waarden van deze eigenschappen in het voorbeeld van een aanvraag voor het inrichten van een gebruiker ongedaan: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: 'urn: ietf:params:scim:schemas:extension:enterprise:2.0:User'

## <a name="group-provisioning-and-de-provisioning"></a>Groep inrichting en ongedaan inrichten
De volgende afbeelding ziet u de berichten dat Azure AcD naar een service SCIM verzendt voor het beheren van de levenscyclus van een groep in een andere identiteit store.  Deze berichten verschillen van de berichten die betrekking hebben op gebruikers op drie manieren: 

* Het schema van een bron wordt geïdentificeerd als http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Aanvragen voor het ophalen van groepen kunt u bepalen dat het kenmerk leden moeten worden uitgesloten van een resource in het antwoord op de aanvraag is.  
* Aanvragen om te bepalen of een verwijzingskenmerk een bepaalde waarde zijn aanvragen over het kenmerk leden.  

![][5]
*Afbeelding 6: Inrichting en ongedaan inrichting sequence groeperen*

## <a name="related-articles"></a>Verwante artikelen:
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Automatisch gebruikers inrichten/opheffen van inrichting tot SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Kenmerktoewijzingen voor gebruikers inrichten aanpassen](active-directory-saas-customizing-attribute-mappings.md)
* [Expressies voor kenmerktoewijzingen schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereikfilters voor gebruikers inrichten](active-directory-saas-scoping-filters.md)
* [Meldingen inrichten van een account](active-directory-saas-account-provisioning-notifications.md)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
