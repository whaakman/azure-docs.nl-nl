---
title: Rollover van ondertekeningssleutel in Azure AD
description: In dit artikel vindt u de ondertekening sleutelrollover aanbevolen procedures voor Azure Active Directory
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2018
ms.author: celested
ms.reviewer: paulgarn, hirsin
ms.custom: aaddev
ms.openlocfilehash: 6174dd210cf85bfa6a3daf38965d1da65801321c
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076329"
---
# <a name="signing-key-rollover-in-azure-active-directory"></a>Rollover van ondertekeningssleutel gebruiken in Azure Active Directory
In dit artikel wordt beschreven wat u moet weten over de openbare sleutels die worden gebruikt in Azure Active Directory (Azure AD) om beveiligingstokens te ondertekenen. Het is belangrijk te weten dat de rollover van deze sleutels op periodieke basis en, in geval van nood, direct kan worden meegenomen. Alle toepassingen die gebruikmaken van Azure AD moet kunnen via de programmacode verwerken van het proces sleutelrollover of tot stand brengen van een periodieke handmatige rollover-proces. Lees verder voor u te begrijpen hoe de sleutels werken, het beoordelen wat de impact van de overschakeling van uw toepassing en het bijwerken van uw toepassing of een rollover van periodieke handmatige proces voor het afhandelen van sleutelrollover indien nodig tot stand brengen.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Overzicht van het ondertekenen van sleutels in Azure AD
Azure AD maakt gebruik van cryptografie met openbare sleutels die is gebouwd op industrienormen vertrouwensrelatie tussen zelf en de toepassingen die worden gebruikt. Dit werkt in de praktijk in de volgende manier: Azure AD maakt gebruik van een ondertekeningssleutel die uit een openbare en persoonlijke sleutelpaar bestaat. Wanneer een gebruiker zich aanmeldt bij een toepassing die gebruikmaakt van Azure AD voor verificatie, maakt Azure AD een beveiligingstoken dat informatie over de gebruiker bevat. Dit token is ondertekend door Azure AD met behulp van de persoonlijke sleutel voordat deze wordt verzonden naar de toepassing. Om te controleren dat het token geldig en oorsprong van Azure AD is, de toepassing van het token handtekening met behulp van de openbare sleutel beschikbaar is gemaakt door Azure AD die is opgenomen in de tenant moet worden gevalideerd [OpenID Connect discovery-document](https://openid.net/specs/openid-connect-discovery-1_0.html) of SAML / WS-Federation [document met federatieve metagegevens](azure-ad-federation-metadata.md).

Uit veiligheidsoverwegingen kan belangrijke rollen op periodieke basis en, in het geval van een noodsituatie voor ondertekening van Azure AD worden meegenomen onmiddellijk. Elke toepassing die kan worden geïntegreerd met Azure AD moet worden voorbereid voor het afhandelen van een gebeurtenis sleutelrollover, ongeacht hoe vaak deze optreden. Als dat niet het geval, en uw toepassing probeert een verlopen sleutel gebruiken om te controleren of de handtekening van een token, mislukken de aanmeldingsaanvraag.

Er is altijd meer dan één geldige sleutel beschikbaar in de OpenID Connect discovery-document en het document met federatieve metagegevens. Uw toepassing moet worden voorbereid om te gebruiken een van de sleutels die zijn opgegeven in het document, omdat een sleutel, wordt mogelijk teruggedraaid binnenkort, een andere kan worden de vervanging ervan, enzovoort.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Hoe kunnen beoordelen als uw toepassing worden beïnvloed en wat te doen over het
Hoe uw toepassing omgaat met sleutelrollover, is afhankelijk van variabelen, zoals het type toepassing of welke identiteit protocol en de bibliotheek is gebruikt. De onderstaande secties te beoordelen of de meest voorkomende typen toepassingen worden beïnvloed door de rollover van ondertekeningssleutel en advies over het bijwerken van de toepassing geven voor de ondersteuning van automatische rollover of handmatig bijwerken van de sleutel.

* [Native clienttoepassingen toegang tot bronnen](#nativeclient)
* [Webtoepassingen / API's toegang tot bronnen](#webclient)
* [Webtoepassingen / API's beveiligen van bronnen en gebouwd met behulp van Azure App Services](#appservices)
* [Webtoepassingen / API's beveiligen van resources met behulp van .NET OWIN OpenID Connect, WS-Federation of WindowsAzureActiveDirectoryBearerAuthentication middleware](#owin)
* [Webtoepassingen / API's beveiligen van resources met behulp van .NET Core OpenID Connect of JwtBearerAuthentication middleware](#owincore)
* [Webtoepassingen / API's beveiligen van resources met behulp van Node.js passport-azure-ad-module](#passport)
* [Webtoepassingen / API's beveiligen van bronnen en die zijn gemaakt met Visual Studio 2015 of Visual Studio 2017](#vs2015)
* [Beveiligen van bronnen en die zijn gemaakt met Visual Studio 2013](#vs2013)
* [Web-API's beveiligen van bronnen en die zijn gemaakt met Visual Studio 2013](#vs2013_webapi)
* [Beveiligen van bronnen en die zijn gemaakt met Visual Studio 2012](#vs2012)
* [Beveiligen van bronnen en die zijn gemaakt met Visual Studio 2010, 2008 o met behulp van Windows Identity Foundation](#vs2010)
* [Webtoepassingen / API's beveiligen van resources met behulp van een andere bibliotheken of handmatig implementeren van een van de ondersteunde protocollen](#other)

Dit artikel gaat in **niet** van toepassing op:

* Toepassingen die zijn toegevoegd vanuit de Azure AD-Toepassingsgalerie (met inbegrip van aangepaste) hebben afzonderlijke richtlijnen met betrekking tot het ondertekenen van sleutels. [Meer informatie.](../manage-apps/manage-certificates-for-federated-single-sign-on.md)
* On-premises toepassingen die zijn gepubliceerd via toepassingsproxy geen zorgen te hoeven maken over het ondertekenen van sleutels.

### <a name="nativeclient"></a>Native clienttoepassingen toegang tot bronnen
Toepassingen die alleen toegang tot bronnen (zoals zijn) Microsoft Graph, KeyVault, Outlook-API en andere Microsoft-APIs) in het algemeen alleen een token verkrijgen en deze doorgeven aan aan de resource-eigenaar. Gezien het feit dat ze alle resources die niet beveiligt, worden ze doen het token niet controleren en hoeft dus niet om te controleren of dat deze correct is ondertekend.

Systeemeigen client-toepassingen, of bureaublad of mobiel, kunnen worden onderverdeeld in deze categorie en worden dus niet beïnvloed door de rollover.

### <a name="webclient"></a>Webtoepassingen / API's toegang tot bronnen
Toepassingen die alleen toegang tot bronnen (zoals zijn) Microsoft Graph, KeyVault, Outlook-API en andere Microsoft-APIs) in het algemeen alleen een token verkrijgen en deze doorgeven aan aan de resource-eigenaar. Gezien het feit dat ze alle resources die niet beveiligt, worden ze doen het token niet controleren en hoeft dus niet om te controleren of dat deze correct is ondertekend.

Webtoepassingen en web-API's die van de app alleen-lezen stroom gebruikmaken (clientreferenties / clientcertificaat), kunnen worden onderverdeeld in deze categorie en worden dus niet beïnvloed door de rollover.

### <a name="appservices"></a>Webtoepassingen / API's beveiligen van bronnen en gebouwd met behulp van Azure App Services
Azure App Services verificatie / autorisatie (EasyAuth)-functionaliteit heeft al de benodigde logica voor het automatisch rollover van ondertekeningssleutel afhandelen.

### <a name="owin"></a>Webtoepassingen / API's beveiligen van resources met behulp van .NET OWIN OpenID Connect, WS-Federation of WindowsAzureActiveDirectoryBearerAuthentication middleware
Als uw toepassing van de .NET OWIN OpenID Connect, WS-Federation of WindowsAzureActiveDirectoryBearerAuthentication middleware gebruikmaakt, is deze al heeft van de benodigde logica voor het automatisch rollover van ondertekeningssleutel afhandelen.

U kunt bevestigen dat uw toepassing gebruik maakt van een van deze door te zoeken naar een van de volgende codefragmenten in de Startup.cs of Startup.Auth.cs van uw toepassing

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
     });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Webtoepassingen / API's beveiligen van resources met behulp van .NET Core OpenID Connect of JwtBearerAuthentication middleware
Als uw toepassing van de .NET Core OWIN OpenID Connect of JwtBearerAuthentication middleware gebruikmaakt, is deze al heeft van de benodigde logica voor het automatisch rollover van ondertekeningssleutel afhandelen.

U kunt bevestigen dat uw toepassing gebruik maakt van een van deze door te zoeken naar een van de volgende codefragmenten in de Startup.cs of Startup.Auth.cs van uw toepassing

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
     });
```

### <a name="passport"></a>Webtoepassingen / API's beveiligen van resources met behulp van Node.js passport-azure-ad-module
Als uw toepassing van de passport-ad-module voor Node.js gebruikmaakt, is deze al heeft van de benodigde logica voor het automatisch rollover van ondertekeningssleutel afhandelen.

U kunt bevestigen dat uw toepassing passport-ad door te zoeken naar het volgende codefragment in van uw toepassing app.js

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Webtoepassingen / API's beveiligen van bronnen en die zijn gemaakt met Visual Studio 2015 of Visual Studio 2017
Als uw toepassing is gemaakt met behulp van een sjabloon in Visual Studio 2015 of Visual Studio 2017 en u hebt geselecteerd **werk en Schoolaccounts** uit de **Change Authentication** in het menu deze al heeft de benodigde logica voor het automatisch rollover van ondertekeningssleutel afhandelen. Deze logica, ingesloten in de OpenID Connect OWIN-middleware, opgehaald en de sleutels van het OpenID Connect discovery-document in de cache opslaat en ze worden regelmatig vernieuwd.

Als u verificatie handmatig toegevoegd aan uw oplossing, is uw toepassing mogelijk niet de logica die nodig zijn sleutelrollover. U moet zelf schrijven of volg de stappen in [webtoepassingen / API's met behulp van een andere bibliotheken of handmatig implementeren van een van de ondersteunde protocollen](#other).

### <a name="vs2013"></a>Beveiligen van bronnen en die zijn gemaakt met Visual Studio 2013
Als uw toepassing is gemaakt met behulp van een sjabloon in Visual Studio 2013 en u hebt geselecteerd **Organisatieaccounts** uit de **Change Authentication** in het menu heeft al de benodigde logica voor het afhandelen van automatisch rollover van ondertekeningssleutel. Deze logica slaat de unieke id van uw organisatie en de sleutel ondertekeningsinformatie in twee databasetabellen die zijn gekoppeld aan het project. U kunt de verbindingsreeks voor de database vinden in Web.config-bestand van het project.

Als u verificatie handmatig toegevoegd aan uw oplossing, is uw toepassing mogelijk niet de logica die nodig zijn sleutelrollover. U moet zelf schrijven of volg de stappen in [webtoepassingen / API's met behulp van een andere bibliotheken of handmatig implementeren van een van de ondersteunde protocollen.](#other).

De volgende stappen kunt u controleren of de logica goed in uw toepassing werkt.

1. Open de oplossing in Visual Studio 2013, en klik vervolgens op de **Server Explorer** tabblad in het rechter venster.
2. Vouw **gegevensverbindingen**, **DefaultConnection**, en vervolgens **tabellen**. Zoek de **IssuingAuthorityKeys** tabel, met de rechtermuisknop op en klik vervolgens op **tabelgegevens weergeven**.
3. In de **IssuingAuthorityKeys** tabel, is er ten minste een rij die overeenkomt met de vingerafdrukwaarde voor de sleutel. Alle rijen in de tabel verwijderen.
4. Met de rechtermuisknop op de **Tenants** tabel en klik vervolgens op **tabelgegevens weergeven**.
5. In de **Tenants** tabel, is er ten minste een rij die overeenkomt met een unieke directory-tenant-id. Alle rijen in de tabel verwijderen. Als u de rijen in beide niet verwijdert de **Tenants** tabel en **IssuingAuthorityKeys** tabel, krijgt u een fout opgetreden tijdens runtime.
6. Ontwikkel en voer de toepassing. Nadat u bent aangemeld bij uw account, kunt u de toepassing kunt stoppen.
7. Ga terug naar de **Server Explorer** en bekijk de waarden in de **IssuingAuthorityKeys** en **Tenants** tabel. U zult zien dat ze hebben is automatisch opnieuw gevuld met de juiste gegevens uit het document met federatieve metagegevens.

### <a name="vs2013"></a>Web-API's beveiligen van bronnen en die zijn gemaakt met Visual Studio 2013
Als u een web-API-toepassing in Visual Studio 2013 met behulp van de Web-API-sjabloon hebt gemaakt en vervolgens geselecteerd **Organisatieaccounts** uit de **Change Authentication** in het menu u al hebt u de benodigde de logica in uw toepassing.

Als u verificatie handmatig hebt geconfigureerd, volgt u de instructies hieronder voor meer informatie over het configureren van uw Web-API voor het automatisch bijwerken van de belangrijkste informatie.

Het volgende codefragment laat zien hoe u de meest recente sleutels ophalen uit het document met federatieve metagegevens en gebruik vervolgens de [JWT-Token Handler](https://msdn.microsoft.com/library/dn205065.aspx) voor het valideren van het token. Het volgende codefragment wordt ervan uitgegaan dat die u uw eigen caching-mechanisme gebruikt voor het opslaan van de sleutel voor het valideren van toekomstige tokens van Azure AD, ongeacht of dit in een database, -configuratiebestand of ergens anders.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Beveiligen van bronnen en die zijn gemaakt met Visual Studio 2012
Als uw toepassing in Visual Studio 2012 is gemaakt, u waarschijnlijk gebruikt de identiteit en toegang tot hulpprogramma om uw toepassing te configureren. Ook is het waarschijnlijk dat u gebruikmaakt van de [valideren van certificaatverlener naam register (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). De VINR is verantwoordelijk voor het onderhouden van informatie over de vertrouwde id-providers (Azure AD) en de sleutels die worden gebruikt voor het valideren van tokens die zijn uitgegeven door hen. De VINR ook kunt u eenvoudig automatisch bijwerken van de gegevens van de sleutel die zijn opgeslagen in een Web.config-bestand door te downloaden van de meest recente document met federatieve metagegevens die zijn gekoppeld aan uw directory, controleren of de configuratie bijgewerkt met de meest recente document is, en bijwerken van de toepassing voor het gebruik van de nieuwe sleutel zo nodig.

Als u uw toepassing met behulp van de voorbeelden van code of scenario documentatie die wordt geleverd door Microsoft gemaakt, wordt de logica rollover van ondertekeningssleutel is al opgenomen in uw project. U ziet dat de onderstaande code in uw project al bestaat. Als uw toepassing heeft nog geen deze logica, de volgende stappen toe te voegen en te controleren of deze correct werkt.

1. In **Solution Explorer**, Voeg een verwijzing naar de **System.IdentityModel** assembly voor het juiste project.
2. Open de **Global.asax.cs** -bestand en voeg de volgende using-instructies:
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. Toevoegen van de volgende methode naar de **Global.asax.cs** bestand:
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. Aanroepen van de **RefreshValidationSettings()** methode in de **Application_Start()** methode in **Global.asax.cs** zoals wordt weergegeven:
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

Nadat u deze stappen hebt gevolgd, wordt met de meest recente gegevens uit het document met federatieve metagegevens, met inbegrip van de meest recente sleutels van uw toepassing Web.config bijgewerkt. Deze update wordt uitgevoerd telkens als uw groep van toepassingen wordt gerecycled in IIS. IIS is standaard ingesteld op toepassingen recyclen om 29 uur.

Volg de stappen hieronder om te controleren of de logica sleutelrollover werkt.

1. Nadat u hebt gecontroleerd dat uw toepassing met behulp van de bovenstaande code, opent u de **Web.config** bestands- en navigeer naar de **<issuerNameRegistry>** blok, specifiek op zoek naar de volgende paar regels:
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. In de **<add thumbprint="">** instelt, wijzigt u de vingerafdrukwaarde door een willekeurig teken vervangen door een andere schijf. Sla de **Web.config** bestand.
3. Maken van de toepassing en voer het vervolgens uit. Als u kunt het aanmeldingsproces hebt voltooid, wordt uw toepassing de sleutel is bijgewerkt door het downloaden van de vereiste gegevens van het document met federatieve metagegevens van uw directory. Als u problemen met aanmelden ondervindt, controleert u of de wijzigingen in uw toepassing juist zijn door het lezen van de [toe te voegen aanmelding in voor uw Web-toepassing met Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) artikel of downloaden op te halen en het volgende codevoorbeeld: [Multitenant-Cloud-toepassing voor Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).

### <a name="vs2010"></a>Beveiligen van bronnen en die zijn gemaakt met Visual Studio 2008 of 2010 en Windows Identity Foundation (WIF) v1.0 voor .NET 3.5
Als u een toepassing gebouwd op WIF v1.0, is er geen opgegeven mechanisme voor automatisch vernieuwen van uw toepassing configureren voor het gebruik van een nieuwe sleutel.

* *Eenvoudigste manier* de FedUtil programma's die is opgenomen in de SDK WIF, die het meest recente metadata-document ophalen en bijwerken van uw configuratie gebruiken.
* Uw toepassing bijwerken voor .NET 4.5, waaronder de nieuwste versie van WIF zich in de naamruimte System. Vervolgens kunt u de [valideren van certificaatverlener naam register (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) automatische updates van de configuratie van de toepassing uitvoert.
* Voer een handmatige rollover volgens de instructies aan het einde van dit document met richtlijnen.

Instructies voor het gebruik van de FedUtil uw configuratie bij te werken:

1. Controleer of u de WIF v1.0 SDK geïnstalleerd op uw ontwikkelcomputer voor Visual Studio 2008 of 2010. U kunt [downloaden vanaf hier](https://www.microsoft.com/en-us/download/details.aspx?id=4451) als u nog niet hebt geïnstalleerd het.
2. Open de oplossing in Visual Studio, klik met de rechtermuisknop op het desbetreffende project en selecteer **bijwerken van de federatiemetagegevens**. Als deze optie niet beschikbaar is, is FedUtil en/of de WIF v1.0 SDK niet geïnstalleerd.
3. Selecteer in de prompt **Update** om te beginnen met het bijwerken van uw federatiemetagegevens. Hebt u toegang tot de server-omgeving waar de toepassing wordt gehost, kunt u eventueel de FedUtil gebruiken [metagegevens van automatische update scheduler](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klik op **voltooien** om de updateproces te voltooien.

### <a name="other"></a>Webtoepassingen / API's beveiligen van resources met behulp van een andere bibliotheken of handmatig implementeren van een van de ondersteunde protocollen
Als u met behulp van sommige andere bibliotheek of een van de ondersteunde protocollen handmatig zijn geïmplementeerd, moet u de bibliotheek of uw implementatie om ervoor te zorgen dat de sleutel wordt opgehaald uit het OpenID Connect discovery-document of de federatiemetagegevens bekijken het document. Een manier om dit te controleren is te doen van een zoekopdracht in uw code of code van de bibliotheek voor de aanroepen uit voor de OpenID-discovery-document of het document met federatieve metagegevens.

Als de sleutel ergens worden opgeslagen of vastgelegd in uw toepassing, u handmatig kunt de sleutel ophalen en werk deze dienovereenkomstig door een handmatige rollover volgens de instructies aan het einde van dit document met richtlijnen uitvoeren. **Het is sterk aangeraden dat u uw toepassing voor de ondersteuning van automatische rollover verbeteren** via een van de omtrek van de methoden in dit artikel om te voorkomen dat toekomstige onderbrekingen en overhead als Azure AD verhoogt het proces of een noodsituatie out-of-band rollover.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Het testen van uw toepassing om te bepalen als deze wordt beïnvloed
U kunt controleren of uw toepassing automatische sleutelrollover ondersteunt door de scripts downloaden en de instructies in [deze GitHub-opslagplaats.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-your-application-does-not-support-automatic-rollover"></a>Het uitvoeren van een rollover van handmatige als automatische rollover biedt geen ondersteuning voor uw toepassing
Als uw toepassing **niet** automatische rollover ondersteunen, moet u een proces dat periodiek monitors Azure AD het ondertekenen van sleutels en voert een handmatige rollover dienovereenkomstig vast te stellen. [Deze GitHub-opslagplaats](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) bevat scripts en instructies over hoe u dit doet.

