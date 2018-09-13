---
title: Een Docker-installatiekopie maken voor Modelbeheer in Azure Machine Learning | Microsoft Docs
description: In dit artikel beschrijft de stappen voor het maken van een Docker-installatiekopie voor uw webservice.
services: machine-learning
author: chhavib
ms.author: chhavib
manager: hjerez
editor: jasonwhowell
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: c0f51e47038737d6aa743be718ad6b28c161c766
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35645765"
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>Azure Machine Learning-Model Management Account API-naslaginformatie

Zie voor meer informatie over het instellen van de implementatieomgeving [Modelbeheer-account instellen](deployment-setup-configuration.md).

Azure Machine Learning-Model Management Account API implementeert de volgende bewerkingen:

- Registratie van model
- Het maken van het manifest
- Docker-installatiekopie maken
- Het maken van Web Services

U kunt deze installatiekopie gebruiken om te maken van een webservice lokaal of op een externe Azure Container Service-cluster of een andere Docker-ondersteunde omgeving van uw keuze.

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u hebt doorlopen tot de installatiestappen uit in de [installeren en maken van de snelstartgids](../service/quickstart-installation.md) document.

De onderstaande onderdelen vereist voordat u doorgaat:
1. Model voor het inrichten van beheer
2. Maken van de omgeving voor het implementeren en beheren van modellen
3. Een Machine Learning-model

### <a name="azure-ad-token"></a>Azure AD-token
Als u Azure CLI, aanmelden met behulp van `az login`. De CLI maakt gebruik van uw Azure Active Directory (Azure AD)-token van het .azure-bestand. Als u de API's gebruiken wilt, hebt u de volgende opties.

##### <a name="acquire-the-azure-ad-token-manually"></a>Handmatig de Azure AD-token ophalen
U kunt `az login` en de meest recente toegangstoken ophalen uit het .azure-bestand op de basismap.

##### <a name="acquire-the-azure-ad-token-programmatically"></a>De Azure AD-token via een programma te verkrijgen
```
az ad sp create-for-rbac --scopes /subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroupName> --role Contributor --years <length of time> --name <MyservicePrincipalContributor>
```
Nadat u de service-principal gemaakt, kunt u de uitvoer opslaan. U kunt dat nu gebruiken om op te halen van een token uit Azure AD:

```cs
 private static async Task<string> AcquireTokenAsync(string clientId, string password, string authority, string resource)
{
        var creds = new ClientCredential(clientId, password);
        var context = new AuthenticationContext(authority);
        var token = await context.AcquireTokenAsync(resource, creds).ConfigureAwait(false);
        return token.AccessToken;
}
```
Het token wordt geplaatst in een autorisatie-header voor API-aanroepen.


## <a name="register-a-model"></a>Een model registreren

De registratiestap model registreert uw Machine Learning-model met de Modelbeheer van Azure-account dat u hebt gemaakt. Deze registratie kunnen bijhouden van de modellen en hun versies die zijn toegewezen op het moment van inschrijving. De gebruiker geeft de naam van het model. Volgende registratie van modellen onder dezelfde naam genereert een nieuwe versie en -ID.

### <a name="request"></a>Aanvraag

| Methode | Aanvraag-URI |
|------------|------------|
| POST |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / modellen 
 |
### <a name="description"></a>Beschrijving
Een model registreert.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| model | hoofdtekst | De nettolading die wordt gebruikt voor het registreren van een model. | Ja | [Model](#model) |


### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | OK. De model-registratie is voltooid. | [Model](#model) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>Query uitvoeren op de lijst met modellen in een account
### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / modellen 
 |
### <a name="description"></a>Beschrijving
De lijst met modellen in een account een query uitgevoerd. U kunt de lijst met resultaten door-tag en de naam filteren. Als er is geen filter is doorgegeven, bevat de query alle modellen in het account. De geretourneerde lijst is gepagineerd en het aantal items in elke pagina is een optionele parameter.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| naam | query | Objectnaam. | Nee | tekenreeks |
| tag | query | Label van het model. | Nee | tekenreeks |
| count | query | Aantal items om op te halen in een pagina. | Nee | tekenreeks |
| $skipToken | query | Het vervolgtoken om op te halen van de volgende pagina. | Nee | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [PaginatedModelList](#paginatedmodellist) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Modeldetails van ophalen
### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /models/ {id}  
 |

### <a name="description"></a>Beschrijving
Een model met id opgehaald

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Object-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [Model](#model) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Een manifest registreren met het geregistreerde model en alle afhankelijkheden

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| POST |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / manifesten | 

### <a name="description"></a>Beschrijving
Registreert een manifest bij het geregistreerde model en alle bijbehorende afhankelijkheden.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| manifestRequest | hoofdtekst | De nettolading die wordt gebruikt voor het registreren van een manifest. | Ja | [Manifest](#manifest) |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Manifest van de registratie is gelukt. | [Manifest](#manifest) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>Query uitvoeren op de lijst met manifesten in een account

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / manifesten | 

### <a name="description"></a>Beschrijving
De lijst met manifesten in een account een query uitgevoerd. U kunt filteren van de lijst met resultaten op model-ID en naam van het manifest. Als er is geen filter is doorgegeven, bevat de query de manifesten in het account. De geretourneerde lijst is gepagineerd en het aantal items in elke pagina is een optionele parameter.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| modelId | query | Model-ID. | Nee | tekenreeks |
| manifestName | query | Naam van het manifest. | Nee | tekenreeks |
| count | query | Aantal items om op te halen in een pagina. | Nee | tekenreeks |
| $skipToken | query | Het vervolgtoken om op te halen van de volgende pagina. | Nee | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [PaginatedManifestList](#paginatedmanifestlist) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Details van het manifest ophalen

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /manifests/ {id} | 

### <a name="description"></a>Beschrijving
Haalt het manifest van-ID.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Object-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [Manifest](#manifest) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="create-an-image"></a>Een installatiekopie maken

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| POST |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / images | 

### <a name="description"></a>Beschrijving
Hiermee maakt u een afbeelding als een Docker-installatiekopie in Azure Container Registry.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| imageRequest | hoofdtekst | De nettolading die wordt gebruikt om een installatiekopie te maken. | Ja | [ImageRequest](#imagerequest) |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Headers | Schema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Asynchrone bewerking locatie-URL. Een GET-aanroep ziet u de status van de installatiekopie van het maken van taak. | Bewerking-locatie |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>Query uitvoeren op de lijst met installatiekopieën in een account

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / images | 

### <a name="description"></a>Beschrijving
De lijst met installatiekopieën in een account van een query uitgevoerd. U kunt de lijst met resultaten op manifest-ID en naam filteren. Als er is geen filter is doorgegeven, bevat de query alle afbeeldingen in het account. De geretourneerde lijst is gepagineerd en het aantal items in elke pagina is een optionele parameter.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| Manifest-id | query | Manifest-ID. | Nee | tekenreeks |
| manifestName | query | Naam van het manifest. | Nee | tekenreeks |
| count | query | Aantal items om op te halen in een pagina. | Nee | tekenreeks |
| $skipToken | query | Het vervolgtoken om op te halen van de volgende pagina. | Nee | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [PaginatedImageList](#paginatedimagelist) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Details van de afbeelding ophalen

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /images/ {id} | 

### <a name="description"></a>Beschrijving
Een installatiekopie met id opgehaald

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Installatiekopie-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [Installatiekopie](#image) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |


## <a name="create-a-service"></a>Een service maken

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| POST |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / services | 

### <a name="description"></a>Beschrijving
Hiermee maakt u een service van een installatiekopie.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| Serviceaanvraag | hoofdtekst | De nettolading die wordt gebruikt om een service te maken. | Ja | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Headers | Schema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Asynchrone bewerking locatie-URL. Een GET-aanroep ziet u de status van de taak service maken. | Bewerking-locatie |
| 409 | Er bestaat al een service met de opgegeven naam. |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>Query uitvoeren op de lijst met services in een account

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / services | 

### <a name="description"></a>Beschrijving
De lijst met services in een account een query uitgevoerd. U kunt filteren, de lijst met resultaten door de naam van model /-ID, manifest naam-ID, afbeeldings-ID, servicenaam of Machine Learning compute resource-ID. Als er is geen filter is doorgegeven, is de query een lijst met alle services in het account. De geretourneerde lijst is gepagineerd en het aantal items in elke pagina is een optionele parameter.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| serviceName | query | Naam van de service. | Nee | tekenreeks |
| modelId | query | Naam van het model. | Nee | tekenreeks |
| modelName | query | Model-ID. | Nee | tekenreeks |
| Manifest-id | query | Manifest-ID. | Nee | tekenreeks |
| manifestName | query | Naam van het manifest. | Nee | tekenreeks |
| imageId | query | Installatiekopie-ID. | Nee | tekenreeks |
| computeResourceId | query | Machine Learning compute resource-ID. | Nee | tekenreeks |
| count | query | Aantal items om op te halen in een pagina. | Nee | tekenreeks |
| $skipToken | query | Het vervolgtoken om op te halen van de volgende pagina. | Nee | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [PaginatedServiceList](#paginatedservicelist) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Servicedetails van de ophalen

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} | 

### <a name="description"></a>Beschrijving
Een service met id opgehaald

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Object-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [ServiceResponse](#serviceresponse) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse)

## <a name="update-a-service"></a>Een service bijwerken

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| PUT |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} | 

### <a name="description"></a>Beschrijving
Een bestaande service-updates.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Object-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| serviceUpdateRequest | hoofdtekst | De nettolading die wordt gebruikt voor het bijwerken van een bestaande service. | Ja |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Headers | Schema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Asynchrone bewerking locatie-URL. Een GET-aanroep ziet u de status van de taak van de service-update. | Bewerking-locatie |
| 404 | De service met de opgegeven ID bestaat niet. |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse)

## <a name="delete-a-service"></a>Een service verwijderen

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| DELETE |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} | 

### <a name="description"></a>Beschrijving
Hiermee verwijdert u een service.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Object-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. |  |
| 204 | De service met de opgegeven ID bestaat niet. |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Servicesleutels ophalen

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} / sleutels | 

### <a name="description"></a>Beschrijving
Servicesleutels opgehaald.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Service-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [AuthKeys](#authkeys)
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse)

## <a name="regenerate-service-keys"></a>Servicesleutels opnieuw genereren

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| POST |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} / regenerateKeys | 

### <a name="description"></a>Beschrijving
Servicesleutels opnieuw genereren en retourneert deze.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Service-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| regenerateKeyRequest | hoofdtekst | De nettolading die wordt gebruikt voor het bijwerken van een bestaande service. | Ja | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [AuthKeys](#authkeys)
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>Query uitvoeren op de lijst met implementaties in een account

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / implementaties | 

### <a name="description"></a>Beschrijving
De lijst met implementaties in een account een query uitgevoerd. U kunt de lijst met resultaten met service-ID, die alleen de implementaties die zijn gemaakt voor de service wordt geretourneerd filteren. Als er is geen filter is doorgegeven, is de query een lijst met alle implementaties in het account.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |
| serviceId | query | Service-ID. | Nee | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [DeploymentList](#deploymentlist) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse)

## <a name="get-deployment-details"></a>Details van de implementatie ophalen

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /deployments/ {id} | 

### <a name="description"></a>Beschrijving
Met deze eigenschap wordt de implementatie van-ID.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Implementatie-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [Implementatie](#deployment) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse)

## <a name="get-operation-details"></a>Informatie over de bewerking ophalen

### <a name="request"></a>Aanvraag
| Methode | Aanvraag-URI |
|------------|------------|
| GET |  /-API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /operations/ {id} | 

### <a name="description"></a>Beschrijving
Met deze eigenschap wordt de status van de asynchrone bewerking door de bewerking-ID.

### <a name="parameters"></a>Parameters
| Naam | In | Beschrijving | Vereist | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | pad | Azure-abonnement-ID. | Ja | tekenreeks |
| resourceGroupName | pad | De naam van de resourcegroep waarin het Modelbeheer-account zich bevindt. | Ja | tekenreeks |
| accountName | pad | De naam van het Modelbeheer-account. | Ja | tekenreeks |
| id | pad | Bewerking-ID. | Ja | tekenreeks |
| API-versie | query | De versie van de Microsoft.Machine.Learning resourceprovider API beschikbaar te maken. | Ja | tekenreeks |
| Autorisatie | koptekst | Verificatietoken. Het moet er ongeveer als "Bearer XXXXXX." | Ja | tekenreeks |

### <a name="responses"></a>Antwoorden
| Code | Beschrijving | Schema |
|--------------------|--------------------|--------------------|
| 200 | Geslaagd. | [OperationStatus](#asyncoperationstatus) |
| standaardinstelling | Foutbericht waarin wordt beschreven waarom de bewerking is mislukt. | [ErrorResponse](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Definities

<a name="asset"></a>
### <a name="asset"></a>Asset
De asset-object dat nodig tijdens het maken van de Docker-installatiekopie is.


|Naam|Beschrijving|Schema|
|---|---|---|
|**id**  <br>*Optioneel*|Asset-ID.|tekenreeks|
|**mimeType**  <br>*Optioneel*|MIME-type van de inhoud van gegevensmodel. Zie voor meer informatie over het MIME-type, de [lijst met typen IANA media](https://www.iana.org/assignments/media-types/media-types.xhtml).|tekenreeks|
|**uitpakken**  <br>*Optioneel*|Geeft aan waar moeten we de inhoud uitpakken tijdens het maken van de Docker-installatiekopie.|booleaans|
|**URL**  <br>*Optioneel*|Asset-locatie-URL.|tekenreeks|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
De status van de asynchrone bewerking.

*Type*: enum (NotStarted, die wordt uitgevoerd, geannuleerd, geslaagd, mislukt)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
De status van de bewerking.


|Naam|Beschrijving|Schema|
|---|---|---|
|**Aanmaaktijd**  <br>*Optioneel*  <br>*read-only*|Asynchrone bewerking-Aanmaaktijd (UTC).|tekenreeks (datum / tijd)|
|**endTime**  <br>*Optioneel*  <br>*read-only*|Asynchrone bewerking-eindtijd (UTC).|tekenreeks (datum / tijd)|
|**Fout**  <br>*Optioneel*||[ErrorResponse](#errorresponse)|
|**id**  <br>*Optioneel*|Asynchrone bewerking-ID.|tekenreeks|
|**OperationType**  <br>*Optioneel*|Asynchrone bewerkingstype.|Enum (afbeelding, Service)|
|**resourceLocation**  <br>*Optioneel*|Resource gemaakt of bijgewerkt door de asynchrone bewerking.|tekenreeks|
|**status**  <br>*Optioneel*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
De verificatiesleutels voor een service.


|Naam|Beschrijving|Schema|
|---|---|---|
|**primaryKey**  <br>*Optioneel*|Primaire sleutel.|tekenreeks|
|**secundaire sleutel**  <br>*Optioneel*|Secundaire sleutel.|tekenreeks|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>Automatisch schalen
Instellingen voor automatisch schalen.


|Naam|Beschrijving|Schema|
|---|---|---|
|**autoscaleEnabled**  <br>*Optioneel*|In- of uitschakelen van het automatisch schalen.|booleaans|
|**maxReplicas**  <br>*Optioneel*|Maximum aantal schilreplica's tot schalen.  <br>**Minimale waarde**: `1`|geheel getal|
|**minReplicas**  <br>*Optioneel*|Minimum aantal schilreplica's omlaag te schalen.  <br>**Minimale waarde**: `0`|geheel getal|
|**refreshPeriodInSeconds**  <br>*Optioneel*|Tijd voor automatisch schalen trigger vernieuwen.  <br>**Minimale waarde**: `1`|geheel getal|
|**targetUtilization**  <br>*Optioneel*|Percentage Buffergebruik die automatisch schalen wordt geactiveerd.  <br>**Minimale waarde**: `0`  <br>**Maximale waarde**: `100`|geheel getal|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
De Machine Learning-compute-resource.


|Naam|Beschrijving|Schema|
|---|---|---|
|**id**  <br>*Optioneel*|Resource-ID.|tekenreeks|
|**type**  <br>*Optioneel*|Het type resource.|Enum (Cluster)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Configuratie van resources voor een container in het cluster te reserveren.


|Naam|Beschrijving|Schema|
|---|---|---|
|**cpu**  <br>*Optioneel*|Hiermee geeft u CPU-reservering. Indeling voor Kubernetes: Zie [betekenis van de CPU](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|tekenreeks|
|**Geheugen**  <br>*Optioneel*|Hiermee geeft u de geheugenreservering. Indeling voor Kubernetes: Zie [betekenis van het geheugen](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|tekenreeks|


<a name="deployment"></a>
### <a name="deployment"></a>Implementatie
Een instantie van een Azure Machine Learning-implementatie.


|Naam|Beschrijving|Schema|
|---|---|---|
|**createdAt**  <br>*Optioneel*  <br>*read-only*|Implementatie Aanmaaktijd (UTC).|tekenreeks (datum / tijd)|
|**expiredAt**  <br>*Optioneel*  <br>*read-only*|Implementatie verlopen tijd (UTC).|tekenreeks (datum / tijd)|
|**id**  <br>*Optioneel*|Implementatie-ID.|tekenreeks|
|**imageId**  <br>*Optioneel*|Afbeeldings-ID die is gekoppeld aan deze implementatie.|tekenreeks|
|**serviceName**  <br>*Optioneel*|Naam van de service.|tekenreeks|
|**status**  <br>*Optioneel*|De huidige Implementatiestatus.|tekenreeks|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Een matrix van implementatie-objecten.

*Type*: <[implementatie](#deployment)> matrix


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Model Management-service-foutdetails.


|Naam|Beschrijving|Schema|
|---|---|---|
|**code**  <br>*Vereist*|Foutcode.|tekenreeks|
|**Bericht**  <br>*Vereist*|Foutbericht.|tekenreeks|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>ErrorResponse
Een object van de fout in de Modelbeheer-service.


|Naam|Beschrijving|Schema|
|---|---|---|
|**code**  <br>*Vereist*|Foutcode.|tekenreeks|
|**details**  <br>*Optioneel*|Matrix van aanvraagdoelen fout.|<[ErrorDetail](#errordetail)> matrix|
|**Bericht**  <br>*Vereist*|Foutbericht.|tekenreeks|
|**statusCode**  <br>*Optioneel*|HTTP-statuscode.|geheel getal|


<a name="image"></a>
### <a name="image"></a>Installatiekopie
De installatiekopie van het Azure Machine Learning.


|Naam|Beschrijving|Schema|
|---|---|---|
|**computeResourceId**  <br>*Optioneel*|ID van de omgeving hebt gemaakt in de Machine Learning-compute-resource.|tekenreeks|
|**Aanmaaktijd**  <br>*Optioneel*|Afbeelding Aanmaaktijd (UTC).|tekenreeks (datum / tijd)|
|**creationState**  <br>*Optioneel*||[AsyncOperationState](#asyncoperationstate)|
|**Beschrijving**  <br>*Optioneel*|Beschrijving van de installatiekopie.|tekenreeks|
|**Fout**  <br>*Optioneel*||[ErrorResponse](#errorresponse)|
|**id**  <br>*Optioneel*|Installatiekopie-ID.|tekenreeks|
|**imageBuildLogUri**  <br>*Optioneel*|De URI van de geüploade logboeken van de build van de installatiekopie.|tekenreeks|
|**imageLocation**  <br>*Optioneel*|Azure Container Registry locatietekenreeks voor de gemaakte installatiekopie.|tekenreeks|
|**imageType**  <br>*Optioneel*||[ImageType](#imagetype)|
|**manifest**  <br>*Optioneel*||[Manifest](#manifest)|
|**De naam**  <br>*Optioneel*|Naam van de installatiekopie.|tekenreeks|
|**Versie**  <br>*Optioneel*|De versie van de installatiekopie is ingesteld door de Model-Management-service.|geheel getal|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>imageRequest
Een aanvraag voor het maken van een installatiekopie van een Azure Machine Learning.


|Naam|Beschrijving|Schema|
|---|---|---|
|**computeResourceId**  <br>*Vereist*|ID van de omgeving hebt gemaakt in de Machine Learning-compute-resource.|tekenreeks|
|**Beschrijving**  <br>*Optioneel*|Beschrijving van de installatiekopie.|tekenreeks|
|**imageType**  <br>*Vereist*||[ImageType](#imagetype)|
|**manifestId**  <br>*Vereist*|ID van het manifest van waaruit de installatiekopie wordt gemaakt.|tekenreeks|
|**De naam**  <br>*Vereist*|Naam van de installatiekopie.|tekenreeks|


<a name="imagetype"></a>
### <a name="imagetype"></a>ImageType
Hiermee geeft u het type van de installatiekopie.

*Type*: enum (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Manifest
Het manifest van de Azure Machine Learning.


|Naam|Beschrijving|Schema|
|---|---|---|
|**Activa**  <br>*Vereist*|Lijst van activa.|<[Asset](#asset)> matrix|
|**Aanmaaktijd**  <br>*Optioneel*  <br>*read-only*|Manifest van de aanmaaktijd (UTC).|tekenreeks (datum / tijd)|
|**Beschrijving**  <br>*Optioneel*|Manifest van de beschrijvende tekst.|tekenreeks|
|**driverProgram**  <br>*Vereist*|Stuurprogramma-programma van het manifest.|tekenreeks|
|**id**  <br>*Optioneel*|Manifest-ID.|tekenreeks|
|**modelIds**  <br>*Optioneel*|Lijst van model-id's van de geregistreerde modellen. De aanvraag mislukt als een van de opgenomen modellen niet zijn geregistreerd.|<string> Matrix|
|**modelType**  <br>*Optioneel*|Hiermee geeft u op dat de modellen al zijn geregistreerd met de Modelbeheer-service.|Enum (geregistreerd)|
|**De naam**  <br>*Vereist*|Naam van het manifest.|tekenreeks|
|**targetRuntime**  <br>*Vereist*||[TargetRuntime](#targetruntime)|
|**Versie**  <br>*Optioneel*  <br>*read-only*|Manifest van de versie die door de Model-Management-service is toegewezen.|geheel getal|
|**webserviceType**  <br>*Optioneel*|Hiermee geeft u het gewenste type webservice die wordt gemaakt van het manifest.|Enum (Realtime)|


<a name="model"></a>
### <a name="model"></a>Model
Een exemplaar van een Azure Machine Learning-model.


|Naam|Beschrijving|Schema|
|---|---|---|
|**createdAt**  <br>*Optioneel*  <br>*read-only*|Model Aanmaaktijd (UTC).|tekenreeks (datum / tijd)|
|**Beschrijving**  <br>*Optioneel*|Beschrijving van het model.|tekenreeks|
|**id**  <br>*Optioneel*  <br>*read-only*|Model-ID.|tekenreeks|
|**mimeType**  <br>*Vereist*|MIME-type van de inhoud van het gegevensmodel. Zie voor meer informatie over het MIME-type, de [lijst met typen IANA media](https://www.iana.org/assignments/media-types/media-types.xhtml).|tekenreeks|
|**De naam**  <br>*Vereist*|Naam van het model.|tekenreeks|
|**tags**  <br>*Optioneel*|Lijst met modellen-tag.|<string> Matrix|
|**uitpakken**  <br>*Optioneel*|Geeft aan of we wilt uitpakken van het model tijdens het maken van de Docker-installatiekopie.|booleaans|
|**URL**  <br>*Vereist*|URL van het model. Meestal plaats we een shared access signature-URL hier.|tekenreeks|
|**Versie**  <br>*Optioneel*  <br>*read-only*|Versie van het model door de Model-Management-service is toegewezen.|geheel getal|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
Het model gegevens Verzamelingsgegevens.


|Naam|Beschrijving|Schema|
|---|---|---|
|**eventHubEnabled**  <br>*Optioneel*|Schakel een event hub voor een service.|booleaans|
|**storageEnabled**  <br>*Optioneel*|Schakel de opslag voor een service.|booleaans|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Een gepagineerd lijst met installatiekopieën.


|Naam|Beschrijving|Schema|
|---|---|---|
|**nextLink**  <br>*Optioneel*|Voortzetting van koppeling (absolute URI) naar de volgende pagina van de resultaten in de lijst.|tekenreeks|
|**value**  <br>*Optioneel*|Matrix met objecten.|<[Afbeelding](#image)> matrix|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Een gepagineerd lijst met manifesten.


|Naam|Beschrijving|Schema|
|---|---|---|
|**nextLink**  <br>*Optioneel*|Voortzetting van koppeling (absolute URI) naar de volgende pagina van de resultaten in de lijst.|tekenreeks|
|**value**  <br>*Optioneel*|Matrix van de manifest-objecten.|<[Manifest](#manifest)> matrix|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Een gepagineerd lijst met modellen.


|Naam|Beschrijving|Schema|
|---|---|---|
|**nextLink**  <br>*Optioneel*|Voortzetting van koppeling (absolute URI) naar de volgende pagina van de resultaten in de lijst.|tekenreeks|
|**value**  <br>*Optioneel*|Matrix met objecten.|<[Model](#model)> matrix|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Een gepagineerd lijst met services.


|Naam|Beschrijving|Schema|
|---|---|---|
|**nextLink**  <br>*Optioneel*|Voortzetting van koppeling (absolute URI) naar de volgende pagina van de resultaten in de lijst.|tekenreeks|
|**value**  <br>*Optioneel*|Matrix van service-objecten.|<[ServiceResponse](#serviceresponse)> matrix|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Een aanvraag voor het maken van een service.


|Naam|Beschrijving|Schema|
|---|---|---|
|**appInsightsEnabled**  <br>*Optioneel*|Application insights voor een service inschakelen.|booleaans|
|**autoScaler**  <br>*Optioneel*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Vereist*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*Optioneel*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*Optioneel*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*Vereist*|De afbeelding om de service te maken.|tekenreeks|
|**maxConcurrentRequestsPerContainer**  <br>*Optioneel*|Maximum aantal gelijktijdige aanvragen.  <br>**Minimale waarde**: `1`|geheel getal|
|**De naam**  <br>*Vereist*|Naam van de service.|tekenreeks|
|**numReplicas**  <br>*Optioneel*|Het aantal schilreplica's die op elk gewenst moment worden uitgevoerd. Kan niet worden opgegeven als automatisch schalen is ingeschakeld.  <br>**Minimale waarde**: `0`|geheel getal|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Een aanvraag voor het genereren van een sleutel voor een service.


|Naam|Beschrijving|Schema|
|---|---|---|
|**keyType**  <br>*Optioneel*|Hiermee geeft u op welke sleutel opnieuw genereren.|Enum (primair, secundair)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
De gedetailleerde status van de service.


|Naam|Beschrijving|Schema|
|---|---|---|
|**createdAt**  <br>*Optioneel*|Service Aanmaaktijd (UTC).|tekenreeks (datum / tijd)|
|**ID**  <br>*Optioneel*|Service-ID.|tekenreeks|
|**image**  <br>*Optioneel*||[Installatiekopie](#image)|
|**manifest**  <br>*Optioneel*||[Manifest](#manifest)|
|**models**  <br>*Optioneel*|Lijst met modellen.|<[Model](#model)> matrix|
|**De naam**  <br>*Optioneel*|Naam van de service.|tekenreeks|
|**scoringUri**  <br>*Optioneel*|De URI voor het scoren van de service.|tekenreeks|
|**status**  <br>*Optioneel*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*Optioneel*|Laatste bijwerking (UTC).|tekenreeks (datum / tijd)|
|**appInsightsEnabled**  <br>*Optioneel*|Application insights voor een service inschakelen.|booleaans|
|**autoScaler**  <br>*Optioneel*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Vereist*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*Optioneel*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*Optioneel*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*Optioneel*|Maximum aantal gelijktijdige aanvragen.  <br>**Minimale waarde**: `1`|geheel getal|
|**numReplicas**  <br>*Optioneel*|Het aantal schilreplica's die op elk gewenst moment worden uitgevoerd. Kan niet worden opgegeven als automatisch schalen is ingeschakeld.  <br>**Minimale waarde**: `0`|geheel getal|
|**Fout**  <br>*Optioneel*||[ErrorResponse](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>serviceUpdateRequest
Een aanvraag voor het bijwerken van een service.


|Naam|Beschrijving|Schema|
|---|---|---|
|**appInsightsEnabled**  <br>*Optioneel*|Application insights voor een service inschakelen.|booleaans|
|**autoScaler**  <br>*Optioneel*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*Optioneel*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*Optioneel*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*Optioneel*|De afbeelding om de service te maken.|tekenreeks|
|**maxConcurrentRequestsPerContainer**  <br>*Optioneel*|Maximum aantal gelijktijdige aanvragen.  <br>**Minimale waarde**: `1`|geheel getal|
|**numReplicas**  <br>*Optioneel*|Het aantal schilreplica's die op elk gewenst moment worden uitgevoerd. Kan niet worden opgegeven als automatisch schalen is ingeschakeld.  <br>**Minimale waarde**: `0`|geheel getal|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
Het type van de doelruntime.


|Naam|Beschrijving|Schema|
|---|---|---|
|**Eigenschappen**  <br>*Vereist*||< string, string > kaart|
|**runtimeType**  <br>*Vereist*|Hiermee geeft u de runtime.|Enum (SparkPython, Python)|

