---
title: Azure AD-kenmerktoewijzingen aanpassen | Microsoft Docs
description: Meer informatie over welke kenmerktoewijzingen voor SaaS-apps in Azure Active Directory zijn hoe u kunt deze aanpassen om de behoeften van uw bedrijf op te lossen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2018
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7dad9f3e688c43de3eabd430bf5618ad4632ca3d
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035930"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Gebruikers inrichten kenmerktoewijzingen voor SaaS-toepassingen in Azure Active Directory aanpassen
Microsoft Azure AD biedt ondersteuning voor gebruikersinrichting voor SaaS-toepassingen van derden zoals Salesforce en Google Apps. Als u gebruikers inrichten voor een SaaS-toepassing van derden is ingeschakeld hebt, bepaalt de Azure-portal de kenmerkwaarden weer in de vorm van kenmerk toewijzingen.

Er is een vooraf geconfigureerde set kenmerken en kenmerktoewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten van elke SaaS-app. Sommige apps beheren andere typen objecten naast gebruikers, zoals groepen. <br> 
 U kunt de standaard-kenmerktoewijzingen aanpassen volgens uw bedrijfsbehoeften. Dit houdt in dat u kunt wijzigen of verwijderen van bestaande kenmerktoewijzingen, of nieuwe kenmerktoewijzingen maken.
 
## <a name="editing-user-attribute-mappings"></a>Kenmerk Gebruikerstoewijzingen bewerken

In de Azure AD-portal, opent u deze functie door te klikken op een **toewijzingen** configuratie onder **inrichten** in de **beheren** gedeelte van een  **Bedrijfstoepassing**.


![SalesForce][5] 

Te klikken op een **toewijzingen** configuratie, de verwante geopend **kenmerk toewijzing** scherm. Er zijn kenmerktoewijzingen die door een SaaS-toepassing vereist zijn te laten functioneren. Voor de vereiste kenmerken de **verwijderen** functie is niet beschikbaar.


![SalesForce][6]  

In het bovenstaande voorbeeld kunt u zien dat de **gebruikersnaam** kenmerk van een beheerd object in Salesforce is gevuld met de **userPrincipalName** waarde van de gekoppelde Azure Active Directory-Object.

U kunt bestaande **kenmerktoewijzingen** door te klikken op een toewijzing. Hiermee opent u de **kenmerk bewerken** scherm.

![SalesForce][7]  


### <a name="understanding-attribute-mapping-types"></a>Typen van de toewijzing van kenmerken
Met kenmerktoewijzingen, kunt u bepalen hoe kenmerken worden ingevuld in een SaaS-toepassing van derden. Er zijn vier verschillende toewijzingstypen ondersteund:

* **Directe** : het kenmerk target is gevuld met de waarde van een kenmerk van het gekoppelde object in Azure AD.
* **Constante** : het kenmerk target is gevuld met een specifieke tekenreeks die u hebt opgegeven.
* **Expressie** -het kenmerk target is ingevuld op basis van het resultaat van een script-achtige-expressie. 
  Zie voor meer informatie [schrijven expressies voor kenmerktoewijzingen in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Geen** -het kenmerk target blijft ongewijzigd. Echter, als het kenmerk target ooit leeg is, wordt dit ingevuld met de standaardwaarde die u opgeeft.

Naast deze vier eenvoudige typen aangepaste kenmerktoewijzingen ondersteuning voor het concept van een optionele **standaard** waarde toewijzing. De toewijzing van de standaard waarde zorgt ervoor dat een target-kenmerk is gevuld met een waarde als er geen waarde in Azure AD en ook op het doelobject. De meest voorkomende configuratie is leeg laten.


### <a name="understanding-attribute-mapping-properties"></a>Inzicht in de eigenschappen van de toewijzing van kenmerken

In de vorige sectie hebt u al is geïntroduceerd voor de eigenschap type van de toewijzing van kenmerken.
Naast deze eigenschap ondersteunen kenmerktoewijzingen ook de volgende kenmerken:

- **Bronkenmerk** -gebruikerskenmerk uit het bronsysteem (voorbeeld: Azure Active Directory).
- **Doelkenmerk** – het gebruikerskenmerk in het doelsysteem (voorbeeld: ServiceNow).
- **Overeen met objecten met behulp van dit kenmerk** : of deze toewijzing moet worden gebruikt als unieke identificatie van gebruikers tussen de bron en doel-systemen of niet. Dit is standaard ingesteld op het kenmerk userPrincipalName of e-mail in Azure AD, die meestal naar een veld username in een doeltoepassing is toegewezen.
- **Overeenkomende voorrang** – meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere, moeten ze worden geëvalueerd in de volgorde gedefinieerd door dit veld. Als een overeenkomst is gevonden, er is geen verdere overeenkomende kenmerken worden geëvalueerd.
- **Deze toewijzing is van toepassing**
    - **Altijd** : deze toewijzing is van toepassing op beide maken van een gebruikersaccount en acties bijwerken
    - **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op gebruikersacties maken


## <a name="editing-group-attribute-mappings"></a>Kenmerktoewijzingen groep bewerken

Aantal toepassingen, zoals ServiceNow, vak en Google Apps, ondersteunen de mogelijkheid om in te richten groepsobjecten naast gebruikersobjecten. Groepsobjecten kunnen bevatten groepseigenschappen zoals weergavenamen en e-aliassen naast groepsleden.

![ServiceNow][8]  

Inrichting van de groep kan worden eventueel ingeschakeld of uitgeschakeld door het selecteren van de toewijzing van de groep onder **toewijzingen**, en de instelling **ingeschakeld** met de gewenste optie in de **kenmerk-toewijzing** scherm.

De kenmerken die zijn ingericht als onderdeel van een groepsobjecten kunnen worden aangepast op dezelfde manier als gebruikersobjecten, zoals eerder besproken. 

>[!TIP]
>Het inrichten van een groepsobjecten (eigenschappen en leden) is een afzonderlijke concept van [groepen toewijzen](manage-apps/assign-user-or-group-access-portal.md) tot een toepassing. Het is mogelijk een groep worden toegewezen aan een toepassing, maar alleen de gebruikersobjecten in de groep inrichten. Het inrichten van een volledige groepsobjecten is niet vereist voor het gebruik van groepen in de toewijzingen.


## <a name="editing-the-list-of-supported-attributes"></a>De lijst met ondersteunde kenmerken bewerken

De kenmerken van de gebruiker ondersteund voor een bepaalde toepassing zijn vooraf geconfigureerd. De meeste toepassing Gebruikersbeheer API's bieden geen ondersteuning voor detectie van schema, de Azure AD-service inricht is daarom niet dynamisch genereren van de lijst met ondersteunde kenmerken door het aanroepen van de toepassing. 

Sommige toepassingen ondersteunen echter aangepaste kenmerken. In de volgorde voor de Azure AD-service inricht kunnen lezen en schrijven naar aangepaste kenmerken, de definities moeten worden ingevoerd in de Azure portal met behulp van de **geavanceerde opties weergeven** selectievakje aan de onderkant van de  **Toewijzing van kenmerken** scherm.

Toepassingen en systemen die ondersteuning voor aanpassing van de lijst met kenmerken zijn onder andere:

* SalesForce
* ServiceNow
* Werkdag
* Azure Active Directory ([Azure AD Graph API standaardkenmerken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity) en aangepaste directory-uitbreidingen worden ondersteund)
* Apps die ondersteuning bieden voor [SCIM 2.0](https://tools.ietf.org/html/rfc7643), waarbij de kenmerken die zijn gedefinieerd in de [core schema](https://tools.ietf.org/html/rfc7643) moeten worden toegevoegd

>[!NOTE]
>De lijst met ondersteunde kenmerken bewerken wordt alleen aanbevolen voor beheerders die het schema van de toepassingen en systemen hebt aangepast en eerste hand kennis hebben van hoe de aangepaste kenmerken zijn gedefinieerd. Hiervoor moet soms bekend bent met de API's en ontwikkelaars-hulpprogramma's van een toepassing of het systeem. 

![Editor][9]  

Wanneer u de lijst met ondersteunde kenmerken bewerkt, vindt u de volgende eigenschappen:

* **Naam** -de systeemnaam van het kenmerk, zoals gedefinieerd in het schema van het doelobject. 
* **Type** -het type gegevens dat het kenmerk is opgeslagen, zoals gedefinieerd in het schema van het doelobject. Dit kan een van de volgende zijn:
   * *Binaire* -kenmerk bevat de binaire gegevens.
   * *Booleaanse* -kenmerk bevat een waarde True of False.
   * *Datum-/* -kenmerk bevat een datumtekenreeks.
   * *Geheel getal* -kenmerk bevat een geheel getal.
   * *Verwijzing* -kenmerk bevat een ID die verwijst naar een waarde die is opgeslagen in een andere tabel in de doeltoepassing.
   * *Tekenreeks* -kenmerk bevat een tekenreeks. 
* **Primaire sleutel?** -Of het kenmerk is gedefinieerd als een veld van de primaire sleutel in het schema van het doelobject.
* **Vereist?** -Het kenmerk is al dan niet vereist om te worden gevuld in de doeltoepassing of het systeem.
* **Met meerdere waarden?** -Controleren of het kenmerk ondersteunt meerdere waarden.
* **Hoofdletters?** -De waarden van kenmerken worden al dan niet in een hoofdlettergevoelige manier geëvalueerd.
* **API-expressie** -niet gebruikt, tenzij om dit te doen door de documentatie voor een specifieke inrichting connector (zoals Workday).
* **Het kenmerk Object waarnaar wordt verwezen** : als dit een kenmerk van het type verwijzing is, wordt dit menu kunt u de tabel en het kenmerk in de doeltoepassing waarin de waarde die is gekoppeld aan het kenmerk selecteren. Als u een kenmerk genaamd 'Afdeling' waarvan opgeslagen waarde verwijst naar een object in een aparte 'Afdelingen' tabel hebt, selecteert u bijvoorbeeld 'Departments.Name'. Denk eraan dat de verwijzing naar tabellen en de primaire ID velden die worden ondersteund voor een bepaalde toepassing vooraf is geconfigureerd en momenteel kunnen niet worden bewerkt met de Azure portal, maar kunnen worden bewerkt met behulp van de [Graph API](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes).

Als u wilt een nieuw kenmerk toevoegen, Ga naar het einde van de lijst met ondersteunde kenmerken, vul de velden dan het gebruik van de opgegeven invoer en selecteer **kenmerk toevoegen**. Selecteer **opslaan** wanneer u klaar bent met het toevoegen van kenmerken. Vervolgens moet u opnieuw laden van de **inrichten** tabblad voor de nieuwe kenmerken beschikbaar in de editor voor de toewijzing van kenmerken.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Herstellen van de standaardkenmerken en kenmerktoewijzingen

U moet beginnen en opnieuw instellen van uw bestaande toewijzingen terug naar de standaardstatus, kunt u de **herstellen standaardtoewijzingen** selectievakje en de configuratie op te slaan. Hiermee stelt u alle toewijzingen als de toepassing heeft zojuist toegevoegd aan uw Azure AD-tenant uit de galerie met toepassingen. 

Deze optie wordt een nieuwe synchronisatie van alle gebruikers effectief geforceerd terwijl de inrichting-service wordt uitgevoerd. 

>[!IMPORTANT]
>Het is raadzaam dat **Inrichtingsstatus** worden ingesteld op **uit** voordat deze optie wordt aangeroepen.


## <a name="what-you-should-know"></a>Wat u moet weten

* Microsoft Azure AD biedt een efficiënte implementatie van een synchronisatieproces. In een omgeving met geïnitialiseerde worden alleen objecten die moet worden bijgewerkt verwerkt tijdens een synchronisatiecyclus. 

* Kenmerktoewijzingen bijwerken, heeft een invloed op de prestaties van een synchronisatiecyclus. Een update voor de configuratie van de toewijzing van kenmerken is vereist voor alle beheerde objecten die moeten worden opnieuw worden geëvalueerd. 

* Het is aanbevolen om het aantal opeenvolgende wijzigingen tot uw kenmerktoewijzingen minimaal te beperken.


## <a name="next-steps"></a>Volgende stappen

* [Automatisch gebruikers inrichten/opheffen van inrichting tot SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Expressies voor kenmerktoewijzingen schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereikfilters voor gebruikers inrichten](active-directory-saas-scoping-filters.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](manage-apps/use-scim-to-provision-users-and-groups.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](saas-apps/tutorial-list.md)

<!--Image references-->
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png
[8]: ./media/active-directory-saas-customizing-attribute-mappings/24.png
[9]: ./media/active-directory-saas-customizing-attribute-mappings/25.PNG

