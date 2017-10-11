---
title: Ondertekening sleutelrollover in Azure AD | Microsoft Docs
description: Dit artikel wordt de ondertekening sleutelrollover best practices voor Azure Active Directory
services: active-directory
documentationcenter: .net
author: dstrockis
manager: krassk
editor: 
ms.assetid: ed964056-0723-42fe-bb69-e57323b9407f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 228bb9058537af1e4eb38207c376c2eb86aee68c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a>Ondertekening sleutelrollover in Azure Active Directory
In dit onderwerp wordt beschreven wat u moet weten over de openbare sleutels die worden gebruikt in Azure Active Directory (Azure AD) voor het ondertekenen van beveiligingstokens. Het is belangrijk te weten dat deze rollover voor sleutels op periodieke basis en, in een noodsituatie onmiddellijk kan worden overgeschakeld. Alle toepassingen die gebruikmaken van Azure AD moeten kunnen programmatisch verwerken van het proces sleutelrollover of een overschakeling van de periodieke handmatige proces tot stand brengen. Blijven lezen om te begrijpen hoe de sleutels werken, het beoordelen van de impact van de overschakeling van uw toepassing en het bijwerken van uw toepassing of tot stand brengen van een overschakeling van de periodieke handmatige proces voor het afhandelen van sleutelrollover indien nodig.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Overzicht van ondertekeningssleutels in Azure AD
Azure AD maakt gebruik van cryptografie met openbare sleutels gebaseerd op industriestandaarden vertrouwensrelatie tussen zichzelf en de toepassingen die worden gebruikt. In de praktijk dit werkt in de volgende manier: Azure AD maakt gebruik van een ondertekeningssleutel die uit een openbare en persoonlijke sleutelpaar bestaat. Wanneer een gebruiker zich bij een toepassing die gebruikmaakt van Azure AD voor verificatie aanmeldt, maakt Azure AD een beveiligingstoken dat informatie over de gebruiker bevat. Dit token is ondertekend door Azure AD met behulp van de persoonlijke sleutel voordat deze terug naar de toepassing wordt verzonden. Om te verifiëren dat het token geldig en daadwerkelijk oorsprong van Azure AD is, moet de toepassing de handtekening van het token met de openbare sleutel die worden weergegeven door Azure AD dat is opgenomen in de tenant valideren [OpenID Connect discovery-document](http://openid.net/specs/openid-connect-discovery-1_0.html) of de WS-SAML-Fed [document met federatieve metagegevens](active-directory-federation-metadata.md).

Om veiligheidsredenen kan sleutel rollen op periodieke basis en, in het geval van een noodgeval voor ondertekening van Azure AD worden vernieuwd onmiddellijk. Alle toepassingen die kan worden geïntegreerd met Azure AD moet worden voorbereid voor het afhandelen van een gebeurtenis sleutelrollover ongeacht hoe vaak deze optreden. Als dit niet het geval, en uw toepassing probeert een verlopen sleutel gebruiken om te verifiëren van de handtekening van een token, mislukt de aanvraag aanmelden.

Er is altijd meer dan een geldige sleutel beschikbaar is in het discovery-document met OpenID Connect en het document met federatieve metagegevens. Uw toepassing moet een van de sleutels die zijn opgegeven in het document, omdat één sleutel kan snel worden teruggedraaid, een ander kan worden de vervanging ervan, enzovoort.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Hoe vast te stellen als uw toepassing worden beïnvloed en wat u moet doen
Hoe uw toepassing sleutelrollover verwerkt, is afhankelijk van variabelen zoals het type van de toepassing of welke identiteit protocol en de bibliotheek is gebruikt. De volgende secties te beoordelen of de meest voorkomende typen toepassingen worden beïnvloed door de sleutelrollover en advies over het bijwerken van de toepassing geven voor de ondersteuning van automatische rollover of handmatig bijwerken van de sleutel.

* [Systeemeigen clienttoepassingen toegang krijgen tot bronnen](#nativeclient)
* [Webtoepassingen / API's toegang tot bronnen](#webclient)
* [Webtoepassingen / API's voor het beveiligen van resources en gebouwd met behulp van Azure App Services](#appservices)
* [Webtoepassingen / API's voor het beveiligen van resources met behulp van .NET OWIN OpenID Connect, WS-Fed of WindowsAzureActiveDirectoryBearerAuthentication middleware](#owin)
* [Webtoepassingen / API's voor het beveiligen van resources met behulp van .NET Core OpenID Connect of JwtBearerAuthentication middleware](#owincore)
* [Webtoepassingen / API's voor het beveiligen van resources met behulp van Node.js passport-azure-ad-module](#passport)
* [Webtoepassingen / API's voor het beveiligen van resources en gemaakt met Visual Studio 2015 of Visual Studio 2017](#vs2015)
* [Webtoepassingen bescherming van bronnen en met Visual Studio 2013 gemaakt](#vs2013)
* [Web-API's voor het beveiligen van resources en gemaakt met Visual Studio 2013](#vs2013_webapi)
* [Webtoepassingen bescherming van bronnen en met Visual Studio 2012 gemaakt](#vs2012)
* [Webtoepassingen bescherming van bronnen en met Visual Studio 2010, 2008 o met behulp van Windows Identity Foundation gemaakt](#vs2010)
* [Webtoepassingen / API's voor het beveiligen van resources met behulp van een andere bibliotheken of handmatig implementeren van de ondersteunde protocollen](#other)

Dit advies is **niet** van toepassing op:

* Toepassingen die zijn toegevoegd vanuit Azure AD-Toepassingsgalerie (met inbegrip van aangepaste) hebben afzonderlijke richtlijnen met betrekking tot het ondertekenen van sleutels. [Meer informatie.](../active-directory-sso-certs.md)
* Een on-premises toepassingen die zijn gepubliceerd via toepassingsproxy hoeft over het ondertekenen van sleutels.

### <a name="nativeclient"></a>Systeemeigen clienttoepassingen toegang krijgen tot bronnen
Toepassingen die alleen toegang bronnen (eenledige tot zijn Microsoft Graph, KeyVault, API Outlook en andere Microsoft-APIs) in het algemeen alleen een token verkrijgen en deze doorgeven aan de resource-eigenaar. Gezien het feit dat ze niet alle resources beveiligt, worden ze doen het token niet controleren en hoeft dus niet om te controleren of dat deze correct is ondertekend.

Native client-toepassingen, of desktop of mobile, vallen in deze categorie en worden dus niet beïnvloed door de overschakeling van de.

### <a name="webclient"></a>Webtoepassingen / API's toegang tot bronnen
Toepassingen die alleen toegang bronnen (eenledige tot zijn Microsoft Graph, KeyVault, API Outlook en andere Microsoft-APIs) in het algemeen alleen een token verkrijgen en deze doorgeven aan de resource-eigenaar. Gezien het feit dat ze niet alle resources beveiligt, worden ze doen het token niet controleren en hoeft dus niet om te controleren of dat deze correct is ondertekend.

Webtoepassingen en -API's die van de app alleen-lezen stroom gebruikmaken (clientreferenties / clientcertificaat), vallen in deze categorie en worden dus niet beïnvloed door de overschakeling van de.

### <a name="appservices"></a>Webtoepassingen / API's voor het beveiligen van resources en gebouwd met behulp van Azure App Services
Azure App Services verificatie / autorisatie (EasyAuth)-functionaliteit is al de benodigde logica sleutelrollover automatisch afhandelen.

### <a name="owin"></a>Webtoepassingen / API's voor het beveiligen van resources met behulp van .NET OWIN OpenID Connect, WS-Fed of WindowsAzureActiveDirectoryBearerAuthentication middleware
Als uw toepassing van de .NET OWIN OpenID Connect, WS-Fed of WindowsAzureActiveDirectoryBearerAuthentication middleware gebruikmaakt, is deze al de benodigde logica sleutelrollover automatisch afhandelen.

U kunt bevestigen dat uw toepassing van een van deze gebruikmaakt door te zoeken naar een van de volgende codefragmenten in uw toepassing Startup.cs of Startup.Auth.cs

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

### <a name="owincore"></a>Webtoepassingen / API's voor het beveiligen van resources met behulp van .NET Core OpenID Connect of JwtBearerAuthentication middleware
Als uw toepassing van de .NET Core OWIN OpenID Connect of JwtBearerAuthentication middleware gebruikmaakt, is deze al de benodigde logica sleutelrollover automatisch afhandelen.

U kunt bevestigen dat uw toepassing van een van deze gebruikmaakt door te zoeken naar een van de volgende codefragmenten in uw toepassing Startup.cs of Startup.Auth.cs

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

### <a name="passport"></a>Webtoepassingen / API's voor het beveiligen van resources met behulp van Node.js passport-azure-ad-module
Als uw toepassing van de passport-ad-module voor Node.js gebruikmaakt, is deze al de benodigde logica sleutelrollover automatisch afhandelen.

U kunt bevestigen dat uw toepassing passport-ad door te zoeken naar het volgende fragment in uw toepassing app.js

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Webtoepassingen / API's voor het beveiligen van resources en gemaakt met Visual Studio 2015 of Visual Studio 2017
Als uw toepassing is gemaakt met een sjabloon in Visual Studio 2015 of Visual Studio 2017 en u hebt geselecteerd **werk-en Schoolaccounts** van de **verificatie wijzigen** menu er al de benodigde logica sleutelrollover automatisch afhandelen. Deze logica, ingesloten in de middleware OWIN OpenID Connect opgehaald en de sleutels van het OpenID Connect discovery-document opslaat in de cache en ze worden regelmatig vernieuwd.

Als u verificatie handmatig toegevoegd aan uw oplossing, hebben de toepassing mogelijk niet de benodigde sleutelrollover logica. U moet zelf schrijven of de stappen in [webtoepassingen / API's met behulp van een andere bibliotheken of handmatig implementeren van de ondersteunde protocollen.](#other).

### <a name="vs2013"></a>Webtoepassingen bescherming van bronnen en met Visual Studio 2013 gemaakt
Als uw toepassing is gemaakt met behulp van een sjabloon in Visual Studio 2013 en u hebt geselecteerd **Organisatieaccounts** van de **verificatie wijzigen** menu heeft al de benodigde logica sleutelrollover automatisch afhandelen. Deze logica slaat de unieke id van uw organisatie en de gegevens van de ondertekening sleutel in twee databasetabellen die zijn gekoppeld aan het project. U vindt de verbindingsreeks voor de database in het Web.config-bestand van het project.

Als u verificatie handmatig toegevoegd aan uw oplossing, hebben de toepassing mogelijk niet de benodigde sleutelrollover logica. U moet zelf schrijven of de stappen in [webtoepassingen / API's met behulp van een andere bibliotheken of handmatig implementeren van de ondersteunde protocollen.](#other).

De volgende stappen kunt u controleren of de logica goed in uw toepassing werkt.

1. Open de oplossing in Visual Studio 2013, en klik vervolgens op de **Server Explorer** tabblad op het rechtervenster.
2. Vouw **gegevensverbindingen**, **DefaultConnection**, en vervolgens **tabellen**. Zoek de **IssuingAuthorityKeys** tabel, met de rechtermuisknop en klik vervolgens op **tabelgegevens weergeven**.
3. In de **IssuingAuthorityKeys** tabel, gaan er ten minste een rij die overeenkomt met de vingerafdrukwaarde voor de sleutel. Alle rijen in de tabel verwijderen.
4. Met de rechtermuisknop op de **Tenants** tabel en klik vervolgens op **tabelgegevens weergeven**.
5. In de **Tenants** tabel, gaan er ten minste een rij die overeenkomt met een unieke map tenant-id. Alle rijen in de tabel verwijderen. Als u de rijen in beide niet verwijdert de **Tenants** tabel en **IssuingAuthorityKeys** tabel, ontvangt u een fout opgetreden tijdens runtime.
6. Ontwikkel en voer de toepassing. Nadat u zich hebt aangemeld bij uw account, kunt u de toepassing stoppen.
7. Ga terug naar de **Server Explorer** en bekijk de waarden in de **IssuingAuthorityKeys** en **Tenants** tabel. U zult zien dat ze hebben is automatisch opnieuw gevuld met de bijbehorende gegevens uit het document met federatieve metagegevens.

### <a name="vs2013"></a>Web-API's voor het beveiligen van resources en gemaakt met Visual Studio 2013
Als u een web API-toepassing in Visual Studio 2013 met de Web-API-sjabloon hebt gemaakt en vervolgens geselecteerd **Organisatieaccounts** van de **verificatie wijzigen** menu u al is de benodigde de logica in uw toepassing.

Als u verificatie handmatig hebt geconfigureerd, volgt u de onderstaande instructies voor informatie over het configureren van uw Web-API voor het automatisch bijwerken van de belangrijke informatie.

Het volgende codefragment laat zien hoe u de meest recente sleutels ophalen uit het document met federatieve metagegevens en gebruik vervolgens de [JWT-Token Handler](https://msdn.microsoft.com/library/dn205065.aspx) valideren van het token. Het codefragment gaat ervan uit dat u uw eigen cachemechanisme voor persistent maken van de sleutel om toekomstige tokens van Azure AD te valideren ongeacht of deze in een database, configuratiebestand of elders.

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
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
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

### <a name="vs2012"></a>Webtoepassingen bescherming van bronnen en met Visual Studio 2012 gemaakt
Als uw toepassing is gemaakt in Visual Studio 2012, u waarschijnlijk gebruikt de identiteit en toegang tot hulpprogramma voor het configureren van uw toepassing. Het is ook mogelijk dat u gebruikmaakt van de [valideren van certificaatverlener naam register (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). De VINR is verantwoordelijk voor het onderhouden van informatie over vertrouwde id-providers (Azure AD) en de sleutels die worden gebruikt om tokens die zijn uitgegeven door ze te valideren. De VINR ook kunt u gemakkelijk automatisch bijwerken van de belangrijkste informatie opgeslagen in een Web.config-bestand downloaden van de meest recente document met federatieve metagegevens die zijn gekoppeld aan uw directory, controleren of de configuratie bijgewerkt met de meest recente document is en bijwerken van de toepassing naar de nieuwe sleutel gebruiken als nodig.

Als u uw toepassing met behulp van de codevoorbeelden of walkthrough documentatie van Microsoft hebt gemaakt, wordt de logica voor de overschakeling van de gegevensversleutelingssleutel is al opgenomen in uw project. U ziet dat de onderstaande code in uw project al bestaat. Als uw toepassing geen al deze logica heeft, de volgende stappen toe te voegen en te controleren of deze correct werkt.

1. In **Solution Explorer**, Voeg een verwijzing naar de **System.IdentityModel** assembly voor het juiste project.
2. Open de **Global.asax.cs** -bestand en voeg het volgende toe met behulp van instructies:
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. Voeg de volgende methode voor de **Global.asax.cs** bestand:
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. Aanroepen de **RefreshValidationSettings()** methode in de **Application_Start()** methode in **Global.asax.cs** zoals wordt weergegeven:
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

Zodra u deze stappen hebt uitgevoerd, wordt Web.config voor uw toepassing met de meest recente gegevens uit het document met federatieve metagegevens, met inbegrip van de meest recente sleutels worden bijgewerkt. Deze update wordt uitgevoerd telkens wanneer de groep van toepassingen wordt gerecycled in IIS. IIS is standaard ingesteld op het recyclen van toepassingen om 29 uur.

Volg de onderstaande stappen om te controleren of de logica sleutelrollover werkt.

1. Nadat u hebt gecontroleerd dat uw toepassing van de bovenstaande code gebruikmaakt, opent u de **Web.config** bestands- en navigeer naar de  **<issuerNameRegistry>**  blok, speciaal op zoek naar de volgende paar regels:
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. In de  **<add thumbprint=””>**  instelt, de vingerafdrukwaarde wijzigen door te willekeurig teken vervangen door een andere naam. Sla de **Web.config** bestand.
3. De toepassing bouwen en uitvoeren. Als u het proces aanmelden voltooien kunt, wordt uw toepassing de sleutel is bijgewerkt door de vereiste gegevens van uw directory document met federatieve metagegevens wordt gedownload. Als u problemen met aanmelden ondervindt, controleert u de wijzigingen in uw toepassing correct zijn door te lezen de [toevoegen van aanmelding bij uw Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) onderwerp of downloaden en bekijken in het volgende voorbeeld: [ Multitenant Cloudtoepassing voor Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).

### <a name="vs2010"></a>Webtoepassingen bescherming van bronnen en zijn gemaakt met Visual Studio 2008 of 2010 en Windows Identity Foundation (WIF) v1.0 voor .NET 3.5
Als u een toepassing op WIF v1.0 gebouwd, is er geen opgegeven mechanisme voor automatisch vernieuwen van de configuratie van uw toepassing om een nieuwe sleutel te gebruiken.

* *Eenvoudigst* gebruiken de FedUtil tooling opgenomen in de SDK WIF, die het meest recente metagegevensdocument ophalen en bijwerken van uw configuratie.
* Uw toepassing bijwerken voor .NET 4.5, waaronder de nieuwste versie van WIF zich in de naamruimte System. U kunt de [valideren van certificaatverlener naam register (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) automatische updates van de configuratie van de toepassing uitvoeren.
* Voer een handmatige rollover volgens de instructies aan het einde van dit document richtlijnen.

Instructies voor het gebruik van de FedUtil uw configuratie bij te werken:

1. Controleer of u de WIF v1.0 SDK is geïnstalleerd op uw ontwikkelcomputer voor Visual Studio 2008 of 2010. U kunt [downloaden vanaf hier](https://www.microsoft.com/en-us/download/details.aspx?id=4451) als u nog niet hebt geïnstalleerd het.
2. Open de oplossing in Visual Studio, en met de rechtermuisknop op het betreffende project en selecteer **federatiemetagegevens bijwerken**. Als deze optie is niet beschikbaar, is FedUtil en/of de WIF v1.0 SDK niet geïnstalleerd.
3. Selecteer in de prompt **Update** om te beginnen met het bijwerken van uw federatiemetagegevens. Als u toegang tot de server-omgeving waar de toepassing wordt gehost hebt, kunt u eventueel de FedUtil gebruiken [metagegevens van de automatische update scheduler](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klik op **voltooien** om het bijwerkproces te voltooien.

### <a name="other"></a>Webtoepassingen / API's voor het beveiligen van resources met behulp van een andere bibliotheken of handmatig implementeren van de ondersteunde protocollen
Als u van een aantal andere bibliotheek gebruikmaakt of geïmplementeerd handmatig van de ondersteunde protocollen, hebt u nodig om te controleren van de bibliotheek of uw implementatie om ervoor te zorgen dat de sleutel van het OpenID Connect discovery-document of de federatiemetagegevens worden opgehaald document. Een manier om dit te controleren of is een zoekopdracht in uw code of code van de bibliotheek voor alle aanroepen van het OpenID discovery-document of het document met federatieve metagegevens doen.

Als deze sleutel is ergens wordt opgeslagen of vastgelegd in uw toepassing, u handmatig kunt de sleutel en het dienovereenkomstig door uitvoeren van een handmatige rollover volgens de instructies aan het einde van dit document richtlijnen update ophalen. **Het wordt ten zeerste aangemoedigd verbeteren van uw toepassing ter ondersteuning van automatische rollover** met behulp van een van de omtrek benaderingen in dit artikel om te voorkomen dat toekomstige onderbrekingen en overhead als Azure AD de rollover uitgebracht verhoogt of een noodsituatie heeft overschakeling van de out-of-band.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Het testen van uw toepassing om te bepalen of deze wordt beïnvloed
U kunt nagaan of uw toepassing automatische sleutelrollover ondersteunt door te downloaden van de scripts en volg de instructies in [deze GitHub-opslagplaats.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Het uitvoeren van een handmatige rollover als u een toepassing biedt geen ondersteuning voor automatische rollover
Als uw toepassing wel **niet** automatische rollover ondersteunen, moet u een proces dat periodiek monitors Azure AD het ondertekenen van sleutels en voert een handmatige rollover dienovereenkomstig vast te stellen. [Deze GitHub-opslagplaats](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) bevat scripts en instructies over hoe u dit doet.

