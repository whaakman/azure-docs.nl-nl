---
title: Automatiseer de inrichting van apps die SCIM gebruiken in Azure Active Directory | Microsoft Docs
description: Azure Active Directory kan automatisch inrichten van gebruikers en groepen voor elke toepassing of identiteit store die door een webservice is fronted met de interface die is gedefinieerd in de specificatie van het protocol SCIM
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 2/22/2018
ms.author: celested
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 785c02a0187cf02df3bb3cf5d248f97a3edf74c6
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961751"
---
# <a name="using-system-for-cross-domain-identity-management-scim-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a>Met behulp van systeem voor meerdere domeinen Identity Management (SCIM) voor het automatisch inrichten van gebruikers en groepen uit Azure Active Directory voor toepassingen

## <a name="overview"></a>Overzicht

SCIM is gestandaardiseerde protocol en het schema dat is erop op de schijf groter consistentie gericht in hoe identiteiten verschillende systemen worden beheerd. Wanneer een toepassing een SCIM-eindpunt voor beheer van gebruikers ondersteunt, kan de Azure AD-gebruiker inrichtingsservice aanvragen maken, wijzigen of verwijderen van de toegewezen gebruikers en groepen naar dit eindpunt verzenden. 

Veel van de toepassingen voor welke Azure AD ondersteunt [vooraf geïntegreerde automatisch gebruikers inrichten](../saas-apps/tutorial-list.md) SCIM implementeren, zoals de mogelijkheid om gebruikers ontvangen meldingen wijzigen.  Naast deze klanten verbinding kunnen maken met toepassingen die ondersteuning bieden voor een specifiek profiel van de [SCIM 2.0-protocol specification](https://tools.ietf.org/html/rfc7644) met de optie Algemeen "buiten de galerie"-integratie in Azure portal. 

De primaire focus van dit document is op het profiel van SCIM 2.0 dat Azure AD als onderdeel van de algemene SCIM-connector voor buiten de galerie-apps implementeert. Ondersteunt echter wel geslaagde testen van een toepassing die SCIM met de algemene Azure AD-connector is een stap voor het ophalen van een app die worden vermeld in de galerie van Azure AD ondersteunt het inrichten van gebruikers. Zie voor meer informatie over het ophalen van uw toepassing te laten opnemen in de Azure AD-toepassingsgalerie de [Microsoft Application Network](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx).
 

>[!IMPORTANT]
>Het gedrag van de implementatie van Azure AD SCIM het laatst is bijgewerkt op 18 December 2018. Zie voor meer informatie over wat er nieuw [SCIM 2.0-protocol naleving van de Azure AD-gebruiker Provisioning-service](application-provisioning-config-problem-scim-compatibility.md).

![][0]
*Afbeelding 1: Inrichten van Azure Active Directory naar een toepassing of identiteit store die SCIM implementeert*

In dit artikel is opgesplitst in vier secties:

* **[Inrichten van gebruikers en groepen met toepassingen van derden die ondersteuning bieden voor SCIM 2.0](#provisioning-users-and-groups-to-applications-that-support-scim)**  : als uw organisatie van een toepassing van derden gebruikmaakt dat implementeert het profiel van SCIM 2.0 dat door Azure AD ondersteunt, kun u beide automatiseren inrichting en ongedaan maken inrichting van gebruikers en groepen vandaag nog.

* **[Informatie over de implementatie van Azure AD SCIM](#understanding-the-azure-ad-scim-implementation)**  -als u een toepassing die ondersteuning biedt voor een SCIM 2.0 API voor gebruikersbeheer bouwt, deze sectie in detail wordt beschreven hoe de Azure AD SCIM-client is geïmplementeerd, en hoe u moet een model het protocol SCIM aanvragen verwerken en antwoorden.
  
* **[Het bouwen van een SCIM-eindpunt met behulp van Microsoft CLI bibliotheken](#building-a-scim-endpoint-using-microsoft-cli-libraries)**  : als u wilt helpen ontwikkelen van een eindpunt SCIM, er zijn bibliotheken en codevoorbeelden die laten zien hoe u een eindpunt SCIM bieden u Common Language Infrastructure (CLI) en vertalen SCIM berichten.  

* **[Gebruikers en groepen-schemaverwijzing](#user-and-group-schema-reference)**  -beschrijving van de gebruiker en groep-schema wordt ondersteund door de implementatie van de Azure AD SCIM voor buiten de galerie-apps. 

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Inrichten van gebruikers en groepen met toepassingen die SCIM ondersteunen
Azure AD kan worden geconfigureerd om automatisch inrichten toegewezen gebruikers en groepen met toepassingen die voor het implementeren van een specifiek profiel van de [SCIM 2.0-protocol](https://tools.ietf.org/html/rfc7644). De details van het profiel zijn gedocumenteerd in [inzicht in de implementatie van Azure AD SCIM](#implementing-a-scim-endpoint-that-works-with-azure-ad-user-provisioning).

Neem contact op met de provider van uw toepassing of de documentatie van de toepassingsprovider van uw voor overzichten van compatibiliteit met deze vereisten.

>[!IMPORTANT]
>De Azure AD SCIM-uitvoering is gebaseerd op de Azure AD-gebruiker provisioning-service, die is ontworpen om gebruikers altijd tussen Azure AD gesynchroniseerd houden en de doeltoepassing en een zeer specifieke set standaardbewerkingen implementeert. het is belangrijk om te begrijpen van deze problemen om te begrijpen van het gedrag van de Azure AD SCIM-client. Zie voor meer informatie, [wat er gebeurt tijdens het inrichten van gebruikers?](user-provisioning.md#what-happens-during-provisioning).

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
7. Als het eindpunt SCIM een OAuth-bearer-token van een uitgever dan Azure AD vereist, kopieert u de vereiste OAuth bearer-token naar de optionele **geheim Token** veld. Als dit veld leeg is, bevat Azure AD een OAuth-bearer-token van Azure AD met elke aanvraag is uitgegeven. Apps die gebruikmaken van Azure AD als id-provider kunnen deze Azure AD uitgegeven tokens te valideren.
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


## <a name="understanding-the-azure-ad-scim-implementation"></a>Informatie over de implementatie van Azure AD SCIM

Als u een toepassing die ondersteuning biedt voor een SCIM 2.0 API voor gebruikersbeheer bouwt, deze sectie in detail wordt beschreven hoe de Azure AD SCIM-client wordt geïmplementeerd, en hoe u moet een model van uw SCIM protocol aanvragen verwerken en antwoorden. Nadat u uw eindpunt SCIM hebt geïmplementeerd, kunt u deze testen door de volgende procedure zoals beschreven in de vorige sectie.

Binnen de [SCIM 2.0-protocol specification](http://www.simplecloud.info/#Specification), uw toepassing moet aan deze vereisten voldoen:

* Ondersteunt het maken van gebruikers, en u kunt desgewenst ook groepen, aan de hand van sectie [3.3 van het protocol SCIM](https://tools.ietf.org/html/rfc7644#section-3.3).  
* Ondersteunt het wijzigen van gebruikers en/of groepen met een PATCH-aanvragen als per [sectie 3.5.2 gebruikt van het protocol SCIM](https://tools.ietf.org/html/rfc7644#section-3.5.2).  
* Ondersteunt het ophalen van een bekende bron voor een gebruiker of groep eerder hebt gemaakt, als per [sectie 3.4.1 van het protocol SCIM](https://tools.ietf.org/html/rfc7644#section-3.4.1).  
* Ondersteunt het opvragen van gebruikers en/of groepen, aan de hand van sectie [3.4.2 van het protocol SCIM](https://tools.ietf.org/html/rfc7644#section-3.4.2).  Standaard gebruikers worden opgehaald door hun `id` en opgevraagd door hun `username` en `externalid`, en groepen worden opgevraagd door `displayName`.  
* Ondersteunt het opvragen van gebruiker door de ID en -beheer aan de hand van sectie 3.4.2 van het protocol SCIM.  
* Ondersteunt het opvragen van groepen door-ID en lid aan de hand van sectie 3.4.2 van het protocol SCIM.  
* Een enkele bearer-token voor verificatie en autorisatie van Azure AD aan uw toepassing accepteert.

Volg ook deze algemene richtlijnen bij het implementeren van een eindpunt SCIM compatibiliteit met Azure AD:

* `id` is een vereiste eigenschap voor de resources. elke reactie die wordt geretourneerd met een resource moet elke resource is deze eigenschap, met uitzondering van `ListResponse` met nul leden.
* Reactie op een queryfilter /-aanvraag moet altijd een `ListResponse`.
* Groepen zijn optioneel, maar wordt alleen ondersteund als de implementatie SCIM PATCH-aanvragen ondersteunt.
* Het is niet nodig zijn voor de gehele resourcegroep in het PATCH-antwoord.
* Microsoft Azure AD maakt alleen gebruik van de volgende operatoren  
     - `eq`
     - `and`
* Hoeven niet een hoofdlettergevoelige overeenkomst op structurele elementen in SCIM in bepaalde PATCH `op` bewerking waarden, zoals gedefinieerd in https://tools.ietf.org/html/rfc7644#section-3.5.2. Azure AD de waarden van 'op' verzendt als `Add`, `Replace`, en `Remove`.
* Microsoft Azure AD kunt u aanvragen voor het ophalen van een willekeurige gebruiker en groep om ervoor te zorgen dat het eindpunt en de referenties geldig zijn. Dit gebeurt ook als onderdeel van **testverbinding** over flow in de [Azure-portal](https://portal.azure.com). 
* Het kenmerk dat de bronnen kunnen worden opgevraagd op als een overeenkomende kenmerk moet worden ingesteld op de toepassing in de [Azure-portal](https://portal.azure.com). Zie voor meer informatie, [gebruiker inrichting kenmerktoewijzingen aanpassen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

### <a name="user-provisioning-and-de-provisioning"></a>Inrichten van gebruikers en het ongedaan maken inrichting
De volgende afbeelding ziet u de berichten die Azure Active Directory voor het beheren van de levenscyclus van een gebruiker in het archief van uw toepassing verzendt naar een SCIM-service.  

![][4]
*Afbeelding 4: Inrichten van gebruikers en het ongedaan maken inrichting reeks*

### <a name="group-provisioning-and-de-provisioning"></a>Groepsinrichting en ongedaan maken inrichting
Groep inrichting en ongedaan maken inrichting zijn optioneel. Wanneer geïmplementeerd en ingeschakeld, de volgende afbeelding toont de berichten die Azure AD verzendt naar een SCIM-service voor het beheren van de levenscyclus van een groep in het archief van uw toepassing.  Deze berichten verschillen van de berichten die betrekking hebben op gebruikers op twee manieren: 

* Aanvragen voor het ophalen van groepen kunt u bepalen dat het kenmerk leden moeten worden uitgesloten van een resource die is opgegeven in antwoord op de aanvraag is.  
* Verzoeken om te bepalen of een verwijzingskenmerk een bepaalde waarde heeft zijn aanvragen over het kenmerk leden.  

![][5]
*Afbeelding 5: Groepsinrichting en ongedaan maken inrichting reeks*

### <a name="scim-protocol-requests-and-responses"></a>SCIM protocol aanvragen en antwoorden
Deze sectie vindt voorbeeld SCIM aanvragen verzonden door de Azure AD SCIM client, evenals de verwachte voorbeeldantwoorden. Voor optimale resultaten, moet u de code van uw app voor het verwerken van deze aanvragen in deze indeling en de verwachte antwoorden verzenden.

>[!IMPORTANT]
>Zie voor meer informatie over hoe en wanneer de Azure AD-gebruiker inrichtingsservice de bewerkingen die verzendt hieronder worden beschreven, [wat er gebeurt tijdens het inrichten van gebruikers?](user-provisioning.md#what-happens-during-provisioning).

- [Bewerkingen van gebruikers](#user-operations)
    - [Gebruiker maken](#create-user) - [aanvragen](#request) - [antwoord](#response)
    - [Gebruiker ophalen](#get-user) - [aanvragen](#request-1) - [antwoord](#response-1)
    - [Gebruiker ophalen door query](#get-user-by-query) - [aanvragen](#request-2) - [antwoord](#response-2)
    - [Gebruiker ophalen door query - nul resultaten](#get-user-by-query---zero-results) - [aanvragen](#request-3) - [antwoord](#response-3)
    - [Gebruiker [Eigenschappen van meerdere waarden] bijwerken](#update-user-multi-valued-properties) - [aanvragen](#request-4) - [antwoord](#response-4)
    - [Gebruiker [Eigenschappen van één waarde] bijwerken](#update-user-single-valued-properties) - [aanvragen](#request-5) - [antwoord](#response-5)
    - [Gebruiker verwijderen](#delete-user) - [aanvragen](#request-6) - [antwoord](#response-6)
- [Bewerkingen van de groep](#group-operations)
    - [Groep maken](#create-group) - [aanvragen](#request-7) - [antwoord](#response-7)
    - [Get-groep](#get-group) - [aanvragen](#request-8) - [antwoord](#response-8)
    - [Get-groeperen op displayName](#get-group-by-displayname) - [aanvragen](#request-9) - [antwoord](#response-9)
    - [Groep bijwerken [lid zijn van niet-kenmerken]](#update-group-non-member-attributes) - [aanvragen](#request-10) - [antwoord](#response-10)
    - [Groep bijwerken [leden toevoegen]](#update-group-add-members) - [aanvragen](#request-11) - [antwoord](#response-11)
    - [Groep bijwerken [verwijderen leden]](#update-group-remove-members) - [aanvragen](#request-12) - [antwoord](#response-12)
    - [Groep verwijderen](#delete-group) - [aanvragen](#request-13) - [antwoord](#response-13)

### <a name="user-operations"></a>Bewerkingen van gebruikers

* Gebruikers kunnen worden opgevraagd met `userName` of `email[type eq "work"]` kenmerken.  

#### <a name="create-user"></a>Gebruiker maken

###### <a name="request"></a>Aanvraag
*POST/gebruikers*
```json
{
    "schemas": [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"],
    "externalId": "0a21f0f2-8d2a-4f8e-bf98-7363c4aed4ef",
    "userName": "Test_User_ab6490ee-1e48-479e-a20b-2d77186b5dd1",
    "active": true,
    "emails": [{
        "primary": true,
        "type": "work",
        "value": "Test_User_fd0ea19b-0777-472c-9f96-4f70d2226f2e@testuser.com"
    }],
    "meta": {
        "resourceType": "User"
    },
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName"
    },
    "roles": []
}
```

##### <a name="response"></a>Antwoord
*HTTP/1.1 201-gemaakt*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "48af03ac28ad4fb88478",
    "externalId": "0a21f0f2-8d2a-4f8e-bf98-7363c4aed4ef",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_ab6490ee-1e48-479e-a20b-2d77186b5dd1",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_fd0ea19b-0777-472c-9f96-4f70d2226f2e@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```


#### <a name="get-user"></a>Gebruiker ophalen

###### <a name="request"></a>Aanvraag
*GET /Users/5d48a0a8e9f04aa38008* 

###### <a name="response"></a>Antwoord
*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "5d48a0a8e9f04aa38008",
    "externalId": "58342554-38d6-4ec8-948c-50044d0a33fd",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_feed3ace-693c-4e5a-82e2-694be1b39934",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_22370c1a-9012-42b2-bf64-86099c2a1c22@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```
#### <a name="get-user-by-query"></a>Gebruiker ophalen door query

##### <a name="request"></a>Aanvraag
*GET /Users?filter=userName eq "Test_User_dfeef4c5-5681-4387-b016-bdf221e82081"*

##### <a name="response"></a>Antwoord
*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 1,
    "Resources": [{
        "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
        "id": "2441309d85324e7793ae",
        "externalId": "7fce0092-d52e-4f76-b727-3955bd72c939",
        "meta": {
            "resourceType": "User",
            "created": "2018-03-27T19:59:26.000Z",
            "lastModified": "2018-03-27T19:59:26.000Z"
            
        },
        "userName": "Test_User_dfeef4c5-5681-4387-b016-bdf221e82081",
        "name": {
            "familyName": "familyName",
            "givenName": "givenName"
        },
        "active": true,
        "emails": [{
            "value": "Test_User_91b67701-697b-46de-b864-bd0bbe4f99c1@testuser.com",
            "type": "work",
            "primary": true
        }]
    }],
    "startIndex": 1,
    "itemsPerPage": 20
}

```

#### <a name="get-user-by-query---zero-results"></a>Gebruiker ophalen door query - nul resultaten

##### <a name="request"></a>Aanvraag
*GET/gebruikers? filter = gebruikersnaam eq 'niet-bestaande gebruiker'*

##### <a name="response"></a>Antwoord
*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 0,
    "Resources": [],
    "startIndex": 1,
    "itemsPerPage": 20
}

```

#### <a name="update-user-multi-valued-properties"></a>Gebruiker [Eigenschappen van meerdere waarden] bijwerken

##### <a name="request"></a>Aanvraag
*PATCH /Users/6764549bef60420686bc HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [
            {
            "op": "Replace",
            "path": "emails[type eq \"work\"].value",
            "value": "updatedEmail@microsoft.com"
            },
            {
            "op": "Replace",
            "path": "name.familyName",
            "value": "updatedFamilyName"
            }
    ]
}
```

##### <a name="response"></a>Antwoord
*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "6764549bef60420686bc",
    "externalId": "6c75de36-30fa-4d2d-a196-6bdcdb6b6539",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_fbb9dda4-fcde-4f98-a68b-6c5599e17c27",
    "name": {
        "formatted": "givenName updatedFamilyName",
        "familyName": "updatedFamilyName",
        "givenName": "givenName"
    },
    "active": true,
    "emails": [{
        "value": "updatedEmail@microsoft.com",
        "type": "work",
        "primary": true
    }]
}
```

#### <a name="update-user-single-valued-properties"></a>Gebruiker [Eigenschappen van één waarde] bijwerken

##### <a name="request"></a>Aanvraag
*PATCH/gebruikers/5171a35d82074e068ce2 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Replace",
        "path": "userName",
        "value": "5b50642d-79fc-4410-9e90-4c077cdd1a59@testuser.com"
    }]
}
```

##### <a name="response"></a>Antwoord
*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "5171a35d82074e068ce2",
    "externalId": "aa1eca08-7179-4eeb-a0be-a519f7e5cd1a",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
        
    },
    "userName": "5b50642d-79fc-4410-9e90-4c077cdd1a59@testuser.com",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_49dc1090-aada-4657-8434-4995c25a00f7@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```

#### <a name="delete-user"></a>Gebruiker verwijderen

##### <a name="request"></a>Aanvraag
*/Users/5171a35d82074e068ce2 HTTP/1.1 verwijderen*

##### <a name="response"></a>Antwoord
*204 HTTP/1.1 geen inhoud*

### <a name="group-operations"></a>Bewerkingen van de groep

* Groepen moeten altijd worden gemaakt met een lege ledenlijst.
* Groepen kunnen worden opgevraagd met de `displayName` kenmerk.
* Update van de groep PATCH-aanvraag moet tot een *HTTP 204 geen inhoud* in het antwoord. Retourneren van een instantie met een lijst van alle leden wordt niet aangeraden.
* Het is niet nodig voor het retourneren van alle leden van de groep ondersteunen.

#### <a name="create-group"></a>Groep maken

##### <a name="request"></a>Aanvraag
*POST/Groups HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group", "http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/2.0/Group"],
    "externalId": "8aa1a0c0-c4c3-4bc0-b4a5-2ef676900159",
    "id": "c4d56c3c-bf3b-4e96-9b64-837018d6060e",
    "displayName": "displayName",
    "members": [],
    "meta": {
        "resourceType": "Group"
    }
}
```

##### <a name="response"></a>Antwoord
*HTTP/1.1 201-gemaakt*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
    "id": "927fa2c08dcb4a7fae9e",
    "externalId": "8aa1a0c0-c4c3-4bc0-b4a5-2ef676900159",
    "meta": {
        "resourceType": "Group",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
        
    },
    "displayName": "displayName",
    "members": []
}
```

#### <a name="get-group"></a>Groep ophalen

##### <a name="request"></a>Aanvraag
*GET /Groups/40734ae655284ad3abcc?excludedAttributes=members HTTP/1.1*

##### <a name="response"></a>Antwoord
*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
    "id": "40734ae655284ad3abcc",
    "externalId": "60f1bb27-2e1e-402d-bcc4-ec999564a194",
    "meta": {
        "resourceType": "Group",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "displayName": "displayName",
}
```

#### <a name="get-group-by-displayname"></a>Get-groeperen op displayName

##### <a name="request"></a>Aanvraag
*/ Groups GET? excludedAttributes = leden & filter = displayName-eq "displayName" HTTP/1.1*

##### <a name="response"></a>Antwoord
*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 1,
    "Resources": [{
        "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
        "id": "8c601452cc934a9ebef9",
        "externalId": "0db508eb-91e2-46e4-809c-30dcbda0c685",
        "meta": {
            "resourceType": "Group",
            "created": "2018-03-27T22:02:32.000Z",
            "lastModified": "2018-03-27T22:02:32.000Z",
            
        },
        "displayName": "displayName",
    }],
    "startIndex": 1,
    "itemsPerPage": 20
}
```
#### <a name="update-group-non-member-attributes"></a>Groep bijwerken [lid zijn van niet-kenmerken]

##### <a name="request"></a>Aanvraag
*PATCH/groepen/fa2ce26709934589afc5 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Replace",
        "path": "displayName",
        "value": "1879db59-3bdf-4490-ad68-ab880a269474updatedDisplayName"
    }]
}
```

##### <a name="response"></a>Antwoord
*204 HTTP/1.1 geen inhoud*

### <a name="update-group-add-members"></a>Groep bijwerken [leden toevoegen]

##### <a name="request"></a>Aanvraag
*PATCH/groepen/a99962b9f99d4c4fac67 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Add",
        "path": "members",
        "value": [{
            "$ref": null,
            "value": "f648f8d5ea4e4cd38e9c"
        }]
    }]
}
```

##### <a name="response"></a>Antwoord
*204 HTTP/1.1 geen inhoud*

#### <a name="update-group-remove-members"></a>Groep bijwerken [leden verwijderen]

##### <a name="request"></a>Aanvraag
*PATCH/groepen/a99962b9f99d4c4fac67 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Remove",
        "path": "members",
        "value": [{
            "$ref": null,
            "value": "f648f8d5ea4e4cd38e9c"
        }]
    }]
}
```

##### <a name="response"></a>Antwoord
*204 HTTP/1.1 geen inhoud*

#### <a name="delete-group"></a>Groep verwijderen

##### <a name="request"></a>Aanvraag
*/Groups/cdb1ce18f65944079d37 HTTP/1.1 verwijderen*

##### <a name="response"></a>Antwoord
*204 HTTP/1.1 geen inhoud*


## <a name="building-a-scim-endpoint-using-microsoft-cli-libraries"></a>Het bouwen van een SCIM-eindpunt met behulp van Microsoft-CLI-bibliotheken
Als u een SCIM-webservice die is gekoppeld met Azure Active Directory maakt, kunt u automatisch gebruikers inrichten voor vrijwel elke toepassing of identity-store.

Dit is hoe het werkt:

1. Azure AD biedt een gemeenschappelijke infrastructuur (CLI)-bibliotheek voor taal is met de naam Microsoft.SystemForCrossDomainIdentityManagement, opgenomen in de code voorbeelden hieronder wordt beschreven. Systeemintegrators en ontwikkelaars kunnen deze bibliotheek maken en implementeren van een SCIM gebaseerde web service-eindpunt van het verbinden van Azure AD voor de identiteit van een App store kunnen gebruiken.
2. Toewijzingen worden geïmplementeerd in de webservice om het schema voor de gestandaardiseerde toewijzen aan het gebruikersschema en het protocol vereist voor de toepassing. 
3. De eindpunt-URL is geregistreerd in Azure AD als onderdeel van een aangepaste toepassing in de toepassingengalerie.
4. Gebruikers en groepen worden toegewezen aan deze toepassing in Azure AD. Bij toewijzing, worden ze in een wachtrij om te worden gesynchroniseerd met de doeltoepassing geplaatst. Het proces van synchronisatie verwerken van de wachtrij wordt elke 40 minuten uitgevoerd.

### <a name="code-samples"></a>Codevoorbeelden
Om dit proces eenvoudiger [codevoorbeelden](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) zijn opgegeven die een SCIM web service-eindpunt maken en demonstreren van automatische inrichting. Het voorbeeld is van een provider die wordt onderhouden door een bestand met door komma's gescheiden waarden voor gebruikers en groepen rijen.    

**Vereisten**

* Visual Studio 2013 of hoger
* [Azure-SDK voor .NET](https://azure.microsoft.com/downloads/)
* Windows machine die ondersteuning biedt voor het framework ASP.NET 4.5 moet worden gebruikt als het eindpunt SCIM. Deze machine moet toegankelijk zijn vanuit de cloud
* [Een Azure-abonnement met een proef- of gelicentieerde versie van Azure AD Premium](https://azure.microsoft.com/services/active-directory/)

### <a name="getting-started"></a>Aan de slag
De eenvoudigste manier voor het implementeren van een eindpunt SCIM inrichting aanvragen van Azure AD kan accepteren, is te bouwen en implementeren van de voorbeeldcode die uitvoer van de ingerichte gebruikers naar een bestand met door komma's gescheiden waarden (CSV).

#### <a name="to-create-a-sample-scim-endpoint"></a>Een voorbeeld SCIM-eindpunt maken

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

#### <a name="to-register-the-sample-scim-endpoint-in-azure-ad"></a>Het voorbeeld SCIM-eindpunt in Azure AD registreren

1. Aanmelden bij [de Azure-portal](https://portal.azure.com). 
2. Blader naar **Azure Active Directory > bedrijfstoepassingen**, en selecteer **nieuwe toepassing > alle > niet in de galerij toepassing**.
3. Voer een naam voor uw toepassing en klikt u op **toevoegen** pictogram om een app-object te maken. De toepassingsobject gemaakt is bedoeld om weer te geven van de doel-app u zou worden ingericht voor en uitvoering van eenmalige aanmelding voor en niet alleen het eindpunt SCIM.
4. Selecteer in het scherm, de **Provisioning** tabblad in de linkerkolom staat.
5. In de **inrichting modus** in het menu **automatische**.
    
  ![][2]
  *Afbeelding 6: Configureren van het inrichten in Azure portal*
    
6. In de **Tenant-URL** en voer de beschikbaar gesteld op internet-URL en poort van uw eindpunt SCIM. De vermelding is iets als http://testmachine.contoso.com:9000 of http://\<ip-adres >: 9000 / waar \<ip-adres > is internet blootgesteld IP adres.  
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

1. Common Language Infrastructure (CLI)-bibliotheken worden aangeboden voor gebruik met de talen die op basis van die infrastructuur, zoals C#. Een van deze bibliotheken, Microsoft.SystemForCrossDomainIdentityManagement.Service, declareert een interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, dat is weergegeven in de volgende afbeelding. Een ontwikkelaar met behulp van de bibliotheken wilt implementeren die interface met een klasse die kan worden verwezen naar algemeen, als een provider. De clientbibliotheken bieden de ontwikkelaar een webservice die voldoet aan de specificatie SCIM implementeren. De webservice kan ofwel worden gehost in Internet Information Services of een uitvoerbaar bestand CLI-assembly. Aanvraag wordt omgezet in aanroepen van methoden van de provider, die door de ontwikkelaar zou worden geprogrammeerd om te worden uitgevoerd op bepaalde identiteitsarchief.
  
   ![][3]
  
2. [Express route-handlers](https://expressjs.com/guide/routing.html) zijn beschikbaar voor het parseren van de aanvraag-objecten van een node.js-aanroepen (zoals gedefinieerd door de specificatie SCIM), die aangebracht aan een node.js-web-service.   

### <a name="building-a-custom-scim-endpoint"></a>Het bouwen van een aangepaste SCIM-eindpunt
Met behulp van de CLI-bibliotheken, kunnen ontwikkelaars die gebruikmaken van deze bibliotheken hun services in een uitvoerbaar bestand CLI-assembly of in Internet Information Services hosten. Hier volgt de voorbeeldcode voor het hosten van een service in een uitvoerbaar bestand assembly, op het adres http://localhost:9000: 

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

Voor het hosten van de service in Internet Information Services, zou een ontwikkelaar een assembly CLI code bibliotheek bouwen met een klasse met de naam opstarten in de standaard-naamruimte van de assembly.  Hier volgt een voorbeeld van een klasse bestaat: 

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
Aanvragen van Azure Active Directory bevatten een OAuth 2.0-bearer-token.   Elke service die is ontvangen van de aanvraag moet de verlener namens de verwachte Azure Active Directory-tenant, voor toegang tot de webservice Azure Active Directory Graph als Azure Active Directory worden geverifieerd.  In het token wordt de verlener wordt geïdentificeerd door een iss-claim, zoals "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/'.  In dit voorbeeld wordt het basisadres van de claimwaarde https://sts.windows.net, identificeert Azure Active Directory als de uitgever, hoewel de relatieve segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is de unieke id van de Azure Active Directory-tenant op andere gebruikers die het token is uitgegeven.  Als het token is uitgegeven voor toegang tot de Azure Active Directory Graph-webservice, moet de id van die service, 00000002-0000-0000-c000-000000000000, zich in de waarde van van het token aud claim.  Houd er rekening mee dat elk van de toepassingen die zijn geregistreerd in één tenant van dezelfde ontvangen kan `iss` claim met SCIM aanvragen.

Ontwikkelaars die gebruikmaken van de CLI-bibliotheken die is geleverd door Microsoft voor het bouwen van een service SCIM kunnen verifiëren van aanvragen van Azure Active Directory met behulp van het pakket Microsoft.Owin.Security.ActiveDirectory door de volgende stappen: 

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

### <a name="handling-provisioning-and-deprovisioning-of-users"></a>Afhandeling van inrichting en ongedaan maken van inrichting van gebruikers

1. Azure Active Directory vraagt de service voor een gebruiker met een externalId kenmerkwaarde die overeenkomt met de waarde van het kenmerk mailNickname van een gebruiker in Azure AD. De query wordt uitgedrukt als een aanvraag Hypertext Transfer Protocol (HTTP) zoals in dit voorbeeld, waarin jyoung een voorbeeld van een mailNickname van een gebruiker in Azure Active Directory is.

    >[!NOTE]
    > Dit is slechts een voorbeeld. Niet alle gebruikers hebben een kenmerk mailNickname en de waarde die een gebruiker heeft misschien niet uniek in de map. Bovendien kan het kenmerk dat wordt gebruikt voor het afstemmen van (dit is in dit geval externalId) worden geconfigureerd in de [Azure AD-kenmerktoewijzingen](customize-application-attributes.md).

  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  Als de service is gebouwd met behulp van de CLI-bibliotheken geleverd door Microsoft voor het implementeren van services SCIM, wordt klikt u vervolgens de aanvraag omgezet in een aanroep van de Query-methode van de provider van de service.  Dit is de handtekening van deze methode: 
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

   ```
     GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
     Authorization: Bearer ...
   ```

   If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider.  Here is the signature of that method: 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  
 
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]>  Query(
       Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
       string correlationIdentifier);
   ```

   Here is the definition of the Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters interface: 

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

   In the following sample of a query for a user with a given value for the externalId attribute, values of the arguments passed to the Query method are: 
   * parameters.AlternateFilters.Count: 1
   * parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"
   * parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals
   * parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"
   * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"] 

2. If the response to a query to the web service for a user with an externalId attribute value that matches the mailNickname attribute value of a user does not return any users, then Azure Active Directory requests that the service provision a user corresponding to the one in Azure Active Directory.  Here is an example of such a request: 

  ````
    POST- https://.../scim/Users HTTP/1.1-autorisatie: Bearer...  Inhoudstype: application/scim + json {'schema': ["urn: ietf:params:scim:schemas:core:2.0:User", "urn: ietf:params:scim:schemas:extension:enterprise:2.0User"], "externalId": "jyoung', 'gebruikersnaam': 'jyoung', 'active': true,"adres": null    "displayName": "Joy Young',"e-mailberichten": [{"type":"werkt","waarde":"jyoung@Contoso.com', 'primaire': waar}], 'meta': {'resourceType': 'Gebruiker'}, 'name': {'familyName': 'Jonge","givenName': 'Zelf'}, "phoneNumbers": null is, "preferredLa nguage': null, "title": null is, 'afdeling': null, een 'manager': null}
  ````
  The CLI libraries provided by Microsoft for implementing SCIM services would translate that request into a call to the Create method of the service’s provider.  The Create method has this signature: 
  ````
    System.Threading.Tasks.Tasks is gedefinieerd in mscorlib.dll.  
    Microsoft.SystemForCrossDomainIdentityManagement.Resource is gedefinieerd in / / Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task < Microsoft.SystemForCrossDomainIdentityManagement.Resource > maken (Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, tekenreeks correlationIdentifier);
  ````
  In a request to provision a user, the value of the resource argument is an instance of the Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser class, defined in the Microsoft.SystemForCrossDomainIdentityManagement.Schemas library.  If the request to provision the user succeeds, then the implementation of the method is expected to return an instance of the Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser class, with the value of the Identifier property set to the unique identifier of the newly provisioned user.  

3. To update a user known to exist in an identity store fronted by an SCIM, Azure Active Directory proceeds by requesting the current state of that user from the service with a request such as: 
  ````
    HAAL ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1-autorisatie: Bearer...
  ````
  In a service built using the CLI libraries provided by Microsoft for implementing SCIM services, the request is translated into a call to the Retrieve method of the service’s provider.  Here is the signature of the Retrieve method: 
  ````
    System.Threading.Tasks.Tasks is gedefinieerd in mscorlib.dll.  
    Microsoft.SystemForCrossDomainIdentityManagement.Resource en / / Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters / / zijn gedefinieerd in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task < Microsoft.SystemForCrossDomainIdentityManagement.Resource > ophalen (Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters parameters, tekenreeks correlationIdentifier);

    openbare interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters {Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier ResourceIdentifier {get;}} public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier {tekenreeks id {get; set;} tekenreeks Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier {get; set;}}
  ````
  In the example of a request to retrieve the current state of a user, the values of the properties of the object provided as the value of the parameters argument are as follows: 
  
   * Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. If a reference attribute is to be updated, then Azure Active Directory queries the service to determine whether or not the current value of the reference attribute in the identity store fronted by the service already matches the value of that attribute in Azure Active Directory. For users, the only attribute of which the current value is queried in this way is the manager attribute. Here is an example of a request to determine whether the manager attribute of a particular user object currently has a certain value: 

  If the service was built using the CLI libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider. The value of the properties of the object provided as the value of the parameters argument are as follows: 
  
  * parameters.AlternateFilters.Count: 2
  * parameters.AlternateFilters.ElementAt(x).AttributePath: "ID"
  * parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals
  * parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"
  * parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals
  * parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
  * parameters.RequestedAttributePaths.ElementAt(0): "ID"
  * parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

  Here, the value of the index x may be 0 and the value of the index y may be 1, or the value of x may be 1 and the value of y may be 0, depending on the order of the expressions of the filter query parameter.   

5. Here is an example of a request from Azure Active Directory to an SCIM service to update a user: 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1-autorisatie: Bearer...  Inhoudstype: application/scim + json {'schema': ["urn: ietf:params:scim:api:messages:2.0:PatchOp"], "Activiteiten": [{'op': 'Toevoegen', "pad": "manager", "waarde": [{"$ref": "http://.../scim/Users/2819c223-7f76-453a-919d-413861904646", "waarde": "2819c223-7f76-453a-919d-413861904646"}]}]}
  ````
  The Microsoft CLI libraries for implementing SCIM services would translate the request into a call to the Update method of the service’s provider. Here is the signature of the Update method: 
  ````
    System.Threading.Tasks.Tasks en / / System.Collections.Generic.IReadOnlyCollection<T> / / zijn gedefinieerd in mscorlib.dll.  
    Microsoft.SystemForCrossDomainIdentityManagement.IPatch, / / Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, / / Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, / / Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, / / Microsoft.SystemForCrossDomainIdentityManagement.OperationName, / / Microsoft.SystemForCrossDomainIdentityManagement.IPath en / / Microsoft.SystemForCrossDomainIdentityManagement.OperationValue / / alle gedefinieerd in Microsoft.SystemForCrossDomainIdentityManagement.Protocol zijn. 

    System.Threading.Tasks.Task bijwerken (Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, tekenreeks correlationIdentifier);

    openbare interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch {Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase PatchRequest {get; set;} Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier ResourceIdentifier {get; set;}        
    }

    openbare klasse PatchRequest2:    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase {openbare System.Collections.Generic.IReadOnlyCollection < Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation > Operations {get;}


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

6. Voor het inrichten ongedaan maken van een gebruiker vanuit een archief voor de klantidentiteit fronted door een SCIM-service, Azure AD een aanvraag verzendt, zoals: 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Als de service is gebouwd met behulp van de CLI-bibliotheken geleverd door Microsoft voor het implementeren van services SCIM, wordt klikt u vervolgens de aanvraag omgezet in een aanroep naar de Delete-methode van de provider van de service.   Deze methode heeft deze handtekening: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  Het object dat is opgegeven als de waarde van het argument resourceIdentifier heeft deze eigenschapswaarden in het voorbeeld van een aanvraag voor de inrichting ongedaan maken van een gebruiker: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="user-and-group-schema-reference"></a>Gebruikers en groepen-schemaverwijzing
Azure Active Directory kunt twee typen resources naar SCIM webservices inrichten.  Deze typen resources worden gebruikers en groepen.  

Gebruikersbronnen worden aangeduid met de schema-id en `urn:ietf:params:scim:schemas:extension:enterprise:2.0:User`, die is opgenomen in deze protocolspecificatie: https://tools.ietf.org/html/rfc7643.  De standaardtoewijzing van de kenmerken van gebruikers in Azure Active Directory met de kenmerken van resources van gebruiker is opgegeven in de onderstaande tabel 1.  

Groep resources worden aangeduid met de schema-id en `urn:ietf:params:scim:schemas:core:2.0:Group`.  Tabel 2 hieronder ziet u de standaardtoewijzing van de kenmerken van groepen in Azure Active Directory met de kenmerken van een groep resources.  

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
| Azure Active Directory-groep | urn:ietf:params:scim:schemas:core:2.0:Group |
| --- | --- |
| displayName |externalId |
| mail |e-mailberichten [type eq 'werk'] .value |
| mailNickname |displayName |
| leden |leden |
| object-id |Id |
| proxyAddresses |e-mailberichten [Typ eq 'ander']. Waarde |


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
