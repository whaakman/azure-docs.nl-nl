---
title: Configureren van uw API Management-service met behulp van Git - Azure | Microsoft Docs
description: Informatie over het opslaan en het configureren van de configuratie van uw API Management-service met behulp van Git.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: mattfarm
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: apimpm
ms.openlocfilehash: e8d0522dc4cc218792569498c149b32b3de652cb
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52443573"
---
# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Het opslaan en het configureren van de configuratie van uw API Management-service met behulp van Git

Elk exemplaar van API Management-service onderhoudt een configuratiedatabase die informatie over de configuratie en de metagegevens voor de service-exemplaar bevat. Wijzigingen kunnen worden gemaakt naar de service-exemplaar door het wijzigen van een instelling in de Azure-portal, met behulp van een PowerShell-cmdlet of het maken van een REST-API-aanroep. U kunt ook uw serviceconfiguratie-exemplaar met behulp van Git, het inschakelen van service management-scenario's zoals beheren naast deze methoden:

* Versiebeheer van configuratie - downloaden en opslaan van verschillende versies van de configuratie van uw service
* Bulk-wijzigingen in de configuratie - wijzigingen aanbrengen in meerdere delen van de configuratie van uw service in uw lokale opslagplaats en de wijzigingen naar de server integreren in één bewerking
* Bekend Git-hulpprogrammaketen en workflow - gebruik van de Git-hulpprogramma's en werkstromen die u al bekend met bent

Het volgende diagram toont een overzicht van de verschillende manieren voor het configureren van uw API Management service-exemplaar.

![GIT configureren][api-management-git-configure]

Wanneer u wijzigingen aan uw service met behulp van de Azure portal, PowerShell-cmdlets of de REST-API aanbrengt, u beheert uw service configuration database met de `https://{name}.management.azure-api.net` eindpunt, zoals wordt weergegeven aan de rechterkant van het diagram. Aan de linkerkant van het diagram ziet u hoe u de configuratie van uw service met behulp van Git kunt beheren en Git-opslagplaats voor uw service zich bevindt in `https://{name}.scm.azure-api.net`.

De volgende stappen bevatten een overzicht van het beheer van uw API Management-service-exemplaar met behulp van Git.

1. Toegang tot Git-configuratie in uw service
2. Uw service-configuratiedatabase opslaan in de Git-opslagplaats
3. Kloon de Git-opslagplaats naar uw lokale computer
4. De meest recente opslagplaats op uw lokale computer en de wijzigingen doorvoeren en push terug naar uw opslagplaats halen
5. De wijzigingen in uw opslagplaats implementeren in uw service-configuratiedatabase

In dit artikel wordt beschreven hoe u om te schakelen en gebruik Git om de serviceconfiguratie van uw te beheren en bevat een referentie voor de bestanden en mappen in de Git-opslagplaats.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="access-git-configuration-in-your-service"></a>Toegang tot Git-configuratie in uw service

Als u wilt weergeven en configureren uw Git-configuratie-instellingen, klikt u op de **Security** menu en navigeer naar de **configuratieopslagplaats** tabblad.

![Inschakelen van GIT][api-management-enable-git]

> [!IMPORTANT]
> Geen geheimen die niet zijn gedefinieerd als eigenschappen worden opgeslagen in de opslagplaats en in de geschiedenis tot u blijven uitschakelen en weer inschakelen Git-toegang. Eigenschappen bieden een veilige plaats voor het beheren van constante tekenreekswaarden geheimen, waaronder in alle configuratie-API en het beleid, zodat u hoeft voor het opslaan van deze rechtstreeks in de beleidsinstructies van uw. Zie voor meer informatie, [Dubbeleigenschappen gebruiken in Azure API Management-beleidsregels](api-management-howto-properties.md).
> 
> 

Zie voor meer informatie over het inschakelen of uitschakelen van Git-toegang met behulp van de REST-API [in- of uitschakelen van de Git-toegang met behulp van de REST-API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Configuratie van de service om op te slaan de Git-opslagplaats

De eerste stap voorafgaand aan het kopiëren van de opslagplaats is om op te slaan van de huidige status van de configuratie van de service naar de opslagplaats. Klik op **opslaan in opslagplaats**.

Breng de gewenste wijzigingen in het bevestigingsscherm en klikt u op **Ok** om op te slaan.

Na enkele ogenblikken de configuratie wordt opgeslagen en de status van de configuratie van de opslagplaats wordt weergegeven, met inbegrip van de datum en tijd van de laatste wijziging in de configuratie en de laatste synchronisatie tussen de serviceconfiguratie en de opslagplaats.

Wanneer de configuratie is opgeslagen in de opslagplaats, kunnen worden gekloond.

Zie voor informatie over het uitvoeren van deze bewerking via de REST API, [doorvoeren configuratie momentopname over met de REST-API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Kloon de opslagplaats naar uw lokale computer

Als u wilt een opslagplaats klonen, moet u de URL naar uw opslagplaats en de naam van een gebruiker een wachtwoord. Als u de gebruikersnaam van de en andere referenties, klikt u op **toegangsreferenties** bovenaan de pagina.  
 
Voor het genereren van een wachtwoord, eerst voor zorgen dat de **verlopen** is ingesteld op de gewenste datum en tijd, en klik vervolgens op **genereren**.

> [!IMPORTANT]
> Noteer dit wachtwoord. Nadat u deze pagina verlaat wordt het wachtwoord niet opnieuw weergegeven.
> 

De volgende voorbeelden gebruiken de Git Bash-hulpprogramma van [Git voor Windows](http://www.git-scm.com/downloads) maar kunt u een Git-hulpprogramma dat u bekend met bent.

Open uw Git-hulpprogramma in de gewenste map en voer de volgende opdracht voor het klonen van de git-opslagplaats naar uw lokale computer, met de opdracht die is geleverd door de Azure-portal.

```
git clone https://{name}.scm.azure-api.net/
```

Geef de gebruikersnaam en wachtwoord wanneer hierom wordt gevraagd.

Als u eventuele fouten ontvangt, probeert u het wijzigen van uw `git clone` opdracht om op te nemen van de gebruikersnaam en wachtwoord, zoals wordt weergegeven in het volgende voorbeeld.

```
git clone https://username:password@{name}.scm.azure-api.net/
```

Als dit een fout opgetreden, probeer het URL-codering van de wachtwoordgedeelte van de opdracht. Een snelle manier om dit te doen is Visual Studio opent, en geef de volgende opdracht uit in de **venster direct**. Om te openen de **venster direct**, een oplossing of project openen in Visual Studio (of maak een nieuwe toepassing in de lege console), en kies **Windows**, **direct** vanuit de **Debug** menu.

```
?System.NetWebUtility.UrlEncode("password from the Azure portal")
```

Gebruik het gecodeerde wachtwoord samen met de locatie van de naam en de opslagplaats te maken van de git-opdracht.

```
git clone https://username:url encoded password@{name}.scm.azure-api.net/
```

Zodra de opslagplaats hebt gekloond, kunt u bekijken en ermee te werken in uw lokale bestandssysteem. Zie voor meer informatie, [bestands- en structuur van verwijzing van lokale Git-opslagplaats](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Uw lokale opslagplaats bijwerken met de configuratie van de meest recente service-exemplaar

Als u wijzigingen in uw API Management-service-exemplaar in de Azure-portal of via de REST-API aanbrengt, moet u deze wijzigingen opslaat in de opslagplaats, voordat u uw lokale opslagplaats met de meest recente wijzigingen bijwerken kunt. Om dit te doen, klikt u op **configuratie opslaan in opslagplaats** op de **configuratieopslagplaats** tabblad in de Azure-portal en geef vervolgens de volgende opdracht in uw lokale opslagplaats.

```
git pull
```

Voordat u `git pull` Zorg ervoor dat u in de map voor uw lokale opslagplaats. Als u hebt zojuist de `git clone` opdracht, en vervolgens u de map in uw opslagplaats wijzigen moet door het uitvoeren van een opdracht als volgt.

```
cd {name}.scm.azure-api.net/
```

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Wijzigingen uit uw lokale opslagplaats naar de opslagplaats van de server pushen
Als u wilt wijzigingen vanuit uw lokale opslagplaats pushen naar de opslagplaats van de server, moet u uw wijzigingen doorvoeren en push ze vervolgens naar de opslagplaats van de server. Als u wilt uw wijzigingen doorvoeren, open uw hulpprogramma voor Git-opdracht, Ga naar de map van uw lokale opslagplaats en uitgeven van de volgende opdrachten.

```
git add --all
git commit -m "Description of your changes"
```

Als u wilt alle van de doorvoeringen doorgeven naar de server pushen, moet u de volgende opdracht uitvoeren.

```
git push
```

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Eventuele wijzigingen in de serviceconfiguratie implementeren naar de API Management service-exemplaar

Zodra uw lokale wijzigingen zijn doorgevoerd en naar de opslagplaats van de server gepusht, kunt u deze implementeren naar uw API Management service-exemplaar.

Zie voor informatie over het uitvoeren van deze bewerking via de REST API, [implementeren Git wordt gewijzigd in de REST-API-configuratiedatabase](https://docs.microsoft.com/rest/api/apimanagement/tenantconfiguration).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Verwijzing voor bestands- en structuur van de lokale Git-opslagplaats

De bestanden en mappen in de lokale git-opslagplaats bevatten de configuratie-informatie over het service-exemplaar.

| Item | Beschrijving |
| --- | --- |
| hoofdmap van de api-beheer |Op het hoogste niveau van de configuratie voor de service-exemplaar bevat |
| map van de API 's |Bevat de configuratie voor de API's in het service-exemplaar |
| map groepen |Bevat de configuratie voor de groepen in het service-exemplaar |
| map beleid |Bevat de beleidsregels in het service-exemplaar |
| portalStyles map |Bevat de configuratie voor de developer portal aanpassingen in de service-exemplaar |
| producten map |Bevat de configuratie voor de producten in de service-exemplaar |
| map met sjablonen |Bevat de configuratie voor de e-mailsjablonen in het service-exemplaar |

Elke map kan een of meer bestanden bevatten, en in sommige gevallen een of meer mappen, bijvoorbeeld een map voor elke API, producten of -groep. De bestanden in elke map zijn specifiek voor het entiteitstype beschreven door de naam van de map.

| Bestandstype | Doel |
| --- | --- |
| json |Configuratie-informatie over de respectieve entiteit |
| HTML-code |Informatie over de entiteit, vaak weergegeven in de portal voor ontwikkelaars |
| xml |Beleidsinstructies |
| CSS |Opmaakmodellen voor ontwikkelaars van de portal aanpassen |

Deze bestanden kunnen worden gemaakt, verwijderd, bewerkt en beheerd op het lokale bestandssysteem en de wijzigingen terug naar uw API Management service-exemplaar geïmplementeerd.

> [!NOTE]
> De volgende entiteiten zijn niet opgenomen in de Git-opslagplaats en kunnen niet worden geconfigureerd met behulp van Git.
> 
> * Gebruikers
> * Abonnementen
> * Eigenschappen
> * Developer portal entiteiten dan stijlen
> 

### <a name="root-api-management-folder"></a>hoofdmap van de api-beheer
De hoofdmap `api-management` map bevat een `configuration.json` bestand die op het hoogste niveau informatie over de service-exemplaar in de volgende notatie bevat.

```json
{
  "settings": {
    "RegistrationEnabled": "True",
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": "False",
    "UserRegistrationTermsConsentRequired": "False",
    "DelegationEnabled": "False",
    "DelegationUrl": "",
    "DelegatedSubscriptionEnabled": "False",
    "DelegationValidationKey": "",
    "RequireUserSigninEnabled": "false"
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

De eerste vier instellingen (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, en `UserRegistrationTermsConsentRequired`) toewijzen aan de volgende instellingen op de **identiteiten** tabblad de **Security** sectie.

| Identiteit | Toegewezen aan |
| --- | --- |
| RegistrationEnabled |Aanwezigheid van **gebruikersnaam en wachtwoord** id-provider |
| UserRegistrationTerms |**Gebruiksvoorwaarden bij gebruikersregistratie** tekstvak |
| UserRegistrationTermsEnabled |**Gebruiksvoorwaarden weergeven op registratiepagina** selectievakje |
| UserRegistrationTermsConsentRequired |**Toestemming vereisen** selectievakje |
| RequireUserSigninEnabled |**Anonieme gebruikers omleiden naar de aanmeldingspagina** selectievakje |

De volgende vier instellingen (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, en `DelegationValidationKey`) toewijzen aan de volgende instellingen op de **delegering** tabblad de **Security** sectie.

| Delegatie-instelling | Toegewezen aan |
| --- | --- |
| DelegationEnabled |**Gemachtigde aanmelden en meld u aan** selectievakje |
| DelegationUrl |**URL van de delegatie-eindpunt** tekstvak |
| DelegatedSubscriptionEnabled |**Productabonnement delegeren** selectievakje |
| DelegationValidationKey |**Delegeren validatiesleutel** tekstvak |

De instelling van de laatste `$ref-policy`, toegewezen aan het globale beleid instructies-bestand voor het service-exemplaar.

### <a name="apis-folder"></a>map van de API 's
De `apis` een map bevat voor elke API in het service-exemplaar dat de volgende items bevat.

* `apis\<api name>\configuration.json` -Dit is de configuratie voor de API en bevat informatie over de URL van de back-end-service en de bewerkingen. Dit is dezelfde informatie die wordt geretourneerd als u aan te roepen [ophalen van een bepaalde API](https://docs.microsoft.com/rest/api/apimanagement/api/get) met `export=true` in `application/json` indeling.
* `apis\<api name>\api.description.html` -Dit is de beschrijving van de API en komt overeen met de `description` eigenschap van de [API-entiteit](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.table._entity_property).
* `apis\<api name>\operations\` -Deze map bevat `<operation name>.description.html` bestanden die zijn toegewezen aan de bewerkingen in de API. Elk bestand bevat de beschrijving van een enkele bewerking in de API, die wordt toegewezen aan de `description` eigenschap van de [bewerking entiteit](https://docs.microsoft.com/rest/api/visualstudio/operations/list#operationproperties) in de REST-API.

### <a name="groups-folder"></a>map groepen
De `groups` map bevat een map voor elke groep die is gedefinieerd in het service-exemplaar.

* `groups\<group name>\configuration.json` -Dit is de configuratie voor de groep. Dit is dezelfde informatie die wordt geretourneerd als u aan te roepen de [ophalen van een specifieke groep](https://docs.microsoft.com/rest/api/apimanagement/group/get) bewerking.
* `groups\<group name>\description.html` -Dit is de beschrijving van de groep en komt overeen met de `description` eigenschap van de [groep entiteit](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity).

### <a name="policies-folder"></a>map beleid
De `policies` map bevat de beleidsinstructies voor uw service-exemplaar.

* `policies\global.xml` -beleidsregels die zijn opgegeven in het globale bereik voor uw service-exemplaar bevat.
* `policies\apis\<api name>\` -Als u alle beleidsregels die zijn opgegeven in het bereik van de API hebt, ze zijn opgenomen in deze map.
* `policies\apis\<api name>\<operation name>\` map - hebt u een beleid dat is opgegeven in het bewerkingsbereik, ze zijn opgenomen in deze map in `<operation name>.xml` bestanden die zijn toegewezen aan het voor beleidsinstructies voor elke bewerking.
* `policies\products\` -Als u alle beleidsregels die zijn opgegeven in het productbereik hebt, ze zijn opgenomen in deze map bevat `<product name>.xml` bestanden die zijn toegewezen aan de beleidsinstructies voor elk product.

### <a name="portalstyles-folder"></a>portalStyles map
De `portalStyles` map bevat configuratie- en opmaakmodellen voor developer portal aanpassingen voor de service-exemplaar.

* `portalStyles\configuration.json` -bevat de namen van de opmaakmodellen die worden gebruikt door de portal voor ontwikkelaars
* `portalStyles\<style name>.css` -elke `<style name>.css` -bestand bevat stijlen voor de developer-portal (`Preview.css` en `Production.css` standaard).

### <a name="products-folder"></a>producten map
De `products` map bevat een map voor elk product dat is gedefinieerd in het service-exemplaar.

* `products\<product name>\configuration.json` -Dit is de configuratie voor het product. Dit is dezelfde informatie die wordt geretourneerd als u aan te roepen de [ophalen van een specifiek product](https://docs.microsoft.com/rest/api/apimanagement/product/get) bewerking.
* `products\<product name>\product.description.html` -Dit is de beschrijving van het product en komt overeen met de `description` eigenschap van de [productenentiteit](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-product-entity) in de REST-API.

### <a name="templates"></a>sjablonen
De `templates` map bevat de configuratie voor de [e-mailsjablonen](api-management-howto-configure-notifications.md) van het service-exemplaar.

* `<template name>\configuration.json` -Dit is de configuratie voor de e-mailsjabloon.
* `<template name>\body.html` -Dit is de hoofdtekst van het e-mailsjabloon.

## <a name="next-steps"></a>Volgende stappen
Zie voor informatie over andere manieren voor het beheren van uw service-exemplaar:

* Beheer van uw service-exemplaar met behulp van de volgende PowerShell-cmdlets
  * [Naslaginformatie over PowerShell-cmdlets voor service-implementatie](https://docs.microsoft.com/powershell/module/wds)
  * [Naslaginformatie over PowerShell-cmdlets voor servicebeheer](https://docs.microsoft.com/powershell/azure/servicemanagement/overview)
* Beheer van uw service-exemplaar met behulp van de REST-API
  * [API Management REST API-naslaginformatie](https://msdn.microsoft.com/library/azure/dn776326.aspx)


[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




