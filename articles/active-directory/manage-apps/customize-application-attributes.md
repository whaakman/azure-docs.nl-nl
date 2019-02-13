---
title: Azure AD-kenmerktoewijzingen aanpassen | Microsoft Docs
description: Meer informatie over welke kenmerktoewijzingen voor SaaS-apps in Azure Active Directory zijn hoe u ze om te voldoen aan uw bedrijfsbehoeften kunt aanpassen.
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
ms.date: 09/09/2018
ms.author: celested
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1926849c8ec63b4240d951e46b1341f31f7c5bd
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56170343"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Kenmerktoewijzingen voor Gebruikersinrichting voor SaaS-toepassingen in Azure Active Directory aanpassen
Microsoft Azure AD biedt ondersteuning voor het inrichten van gebruikers voor SaaS-toepassingen van derden zoals Salesforce, Google Apps en anderen. Als u hebt met het inrichten van gebruikers voor een SaaS-toepassing van derden ingeschakeld, bepaalt de Azure-portal de kenmerkwaarden in de vorm van kenmerk-toewijzingen.

Er is een vooraf geconfigureerde set kenmerken en kenmerktoewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten elke SaaS-app. Sommige apps beheren andere typen objecten naast de gebruikers, zoals groepen. <br> 
 U kunt de standaard-kenmerktoewijzingen aanpassen op basis van de behoeften van uw bedrijf. Dit houdt in dat u kunt wijzigen of verwijderen van bestaande kenmerktoewijzingen, of nieuwe kenmerktoewijzingen maken.
 
## <a name="editing-user-attribute-mappings"></a>Gebruiker-kenmerktoewijzingen bewerken

In de Azure AD-portal, kunt u deze functie openen door te klikken op een **toewijzingen** configuratie onder **Provisioning** in de **beheren** gedeelte van een  **Bedrijfstoepassing**.


![SalesForce](./media/customize-application-attributes/21.png) 

Te klikken op een **toewijzingen** -configuratie, is het gerelateerde geopend **kenmerktoewijzing** scherm. Er zijn kenmerktoewijzingen die voor een SaaS-toepassing vereist zijn te laten functioneren. Voor de vereiste kenmerken, de **verwijderen** functie is niet beschikbaar.


![SalesForce](./media/customize-application-attributes/22.png)

In het bovenstaande voorbeeld ziet u dat de **gebruikersnaam** kenmerk van een beheerd object in Salesforce wordt gevuld met de **userPrincipalName** waarde van de gekoppelde Azure Active Directory-Object.

U kunt bestaande **kenmerktoewijzingen** door te klikken op een-toewijzing. Hiermee opent u de **kenmerk bewerken** scherm.

![SalesForce](./media/customize-application-attributes/23.png)


### <a name="understanding-attribute-mapping-types"></a>Kenmerktoewijzing typen
Met kenmerktoewijzingen, kunt u bepalen hoe kenmerken worden ingevuld in een SaaS-toepassing van derden. Er zijn vier verschillende toewijzen die worden ondersteund:

* **Directe** : het kenmerk target is gevuld met de waarde van een kenmerk van het gekoppelde object in Azure AD.
* **Constante** – het doelkenmerk gevuld met een specifieke tekenreeks die u hebt opgegeven.
* **Expressie** -het kenmerk target is ingevuld op basis van het resultaat van een script-achtige-expressie. 
  Zie voor meer informatie, [expressies schrijven voor kenmerktoewijzingen in Azure Active Directory](functions-for-customizing-application-data.md).
* **Geen** -het kenmerk target blijft ongewijzigd. Echter, als het doelkenmerk ooit leeg is, wordt dit ingevuld met de standaard-waarde die u opgeeft.

Naast deze vier eenvoudige typen aangepaste kenmerktoewijzingen bieden ondersteuning voor het concept van een optionele **standaard** toewijzing-waarde. De toewijzing van de standaard-waarde zorgt ervoor dat een target-kenmerk is gevuld met een waarde als er geen waarde in Azure AD, noch van het doelobject. De meest voorkomende configuratie is te laat dit veld leeg.


### <a name="understanding-attribute-mapping-properties"></a>Understanding kenmerktoewijzing eigenschappen

In de vorige sectie, hebt u al kennisgemaakt met de eigenschap type van kenmerk-toewijzing.
Naast deze eigenschap ondersteunen kenmerktoewijzingen ook de volgende kenmerken:

- **Bronkenmerk** -het gebruikerskenmerk van de van het bronsysteem (voorbeeld: Azure Active Directory).
- **Doelkenmerk** – het gebruikerskenmerk in het doelsysteem (voorbeeld: ServiceNow).
- **Overeenkomen met de objecten met behulp van dit kenmerk** – ongeacht of deze toewijzing moet worden gebruikt voor het aanduiden van gebruikers tussen de bron en doel-systemen of niet. Dit is standaard ingesteld op het kenmerk userPrincipalName of e-mail in Azure AD, die meestal is toegewezen aan een veld username in een doeltoepassing.
- **Prioriteit bij** : meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere, worden deze geëvalueerd in de volgorde die is gedefinieerd in dit veld. Zodra een overeenkomst wordt gevonden, geen verdere overeenkomende kenmerken worden geëvalueerd.
- **Deze toewijzing toepassen**
    - **Altijd** : deze toewijzing toepassen op beide gebruiker maken en bijwerken van acties
    - **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op acties van het maken van de gebruiker


## <a name="editing-group-attribute-mappings"></a>Kenmerktoewijzingen groep bewerken

Aantal toepassingen, zoals ServiceNow, Box en Google Apps, ondersteunen de mogelijkheid om in te richten groepsobjecten worden weergegeven naast gebruikersobjecten. Groepsobjecten kunnen bevatten groepseigenschappen zoals weergavenamen en e-aliassen, naast de groepsleden.

![ServiceNow](./media/customize-application-attributes/24.png)

Inrichten van de groep kan worden eventueel ingeschakeld of uitgeschakeld door het selecteren van de toewijzing van apparaatgroepen onder **toewijzingen**, en de instelling **ingeschakeld** aan de gewenste optie in de **kenmerktoewijzing** scherm.

De kenmerken die zijn ingericht als onderdeel van de groepsobjecten kunnen worden aangepast op dezelfde manier als de objecten gebruiker, die eerder zijn beschreven. 

>[!TIP]
>Het inrichten van een groepsobjecten (eigenschappen en leden) is een afzonderlijke concept van [groepen toewijzen](assign-user-or-group-access-portal.md) tot een toepassing. Het is mogelijk een groep toewijzen aan een toepassing, maar alleen de gebruikersobjecten in de groep inrichten. Inrichting van de volledige groepsobjecten is niet vereist voor het gebruik van groepen in toewijzingen.


## <a name="editing-the-list-of-supported-attributes"></a>De lijst met ondersteunde kenmerken bewerken

De kenmerken van de gebruiker ondersteund voor een bepaalde toepassing zijn vooraf geconfigureerd. De meeste toepassingen Gebruikersbeheer-API's bieden geen ondersteuning voor detectie van schema, de Azure AD-inrichtingsservice is daarom niet dynamisch genereren van de lijst met ondersteunde kenmerken door het aanroepen van de toepassing. 

Sommige toepassingen ondersteunen echter aangepaste kenmerken. In de volgorde voor de Azure AD-inrichtingsservice kunnen lezen en schrijven naar aangepaste kenmerken, de definities moeten worden ingevoerd in de Azure portal met de **geavanceerde opties weergeven** selectievakje aan de onderkant van de  **Kenmerktoewijzing** scherm.

Toepassingen en systemen die ondersteuning bieden voor aanpassing van de lijst met kenmerken zijn onder andere:

* SalesForce
* ServiceNow
* WorkDay
* Azure Active Directory ([Azure AD Graph API standaardkenmerken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity) en aangepaste mapextensies worden ondersteund)
* Apps die ondersteuning bieden voor [SCIM 2.0](https://tools.ietf.org/html/rfc7643), waarbij de kenmerken die zijn gedefinieerd in de [core schema](https://tools.ietf.org/html/rfc7643) moeten worden toegevoegd

>[!NOTE]
>De lijst met ondersteunde kenmerken bewerken wordt alleen aanbevolen voor beheerders die het schema van hun toepassingen en systemen hebt aangepast en eerste hand kennis hebben van hoe de aangepaste kenmerken zijn gedefinieerd. Hiervoor moet soms vertrouwd zijn met de API's en ontwikkelhulpprogramma's geleverd door een toepassing of het systeem. 

![Editor](./media/customize-application-attributes/25.png) 

Tijdens het bewerken van de lijst met ondersteunde kenmerken, vindt u de volgende eigenschappen:

* **Naam** -de systeemnaam van het kenmerk, zoals gedefinieerd in het schema van het doelobject. 
* **Type** -het type gegevens dat het kenmerk opslaat, zoals gedefinieerd in het schema van het doelobject. Dit kan een van de volgende zijn:
   * *Binaire* -kenmerk bevat binaire gegevens.
   * *Booleaanse* -kenmerk bevat een waarde waar of ONWAAR.
   * *Datum-/* -kenmerk bevat een tekenreeks met datum.
   * *Geheel getal* -kenmerk bevat een geheel getal zijn.
   * *Naslaginformatie over* -kenmerk bevat een ID die verwijst naar een waarde die is opgeslagen in een andere tabel in de doeltoepassing.
   * *Tekenreeks* -kenmerk bevat een tekenreeks met tekst. 
* **Primaire sleutel?** -Of het kenmerk wordt gedefinieerd als een veld van de primaire sleutel in het schema van het doelobject.
* **Vereist?** -Het kenmerk is wel of niet moet worden ingevuld in de doeltoepassing of het systeem.
* **Meerdere waarden?** -Controleren of het kenmerk ondersteunt meerdere waarden.
* **Hoofdlettergevoelig?** -Al dan niet de waarden voor kenmerken worden geëvalueerd in een hoofdlettergevoelige manier.
* **API-expressie** -niet gebruikt, tenzij dit niet doet, in de documentatie voor een specifieke inrichting connector (zoals Workday).
* **Objectkenmerk waarnaar wordt verwezen** : als dit een kenmerk van het type referentie, is wordt dit menu kunt u de tabel en het kenmerk in de doeltoepassing waarin de waarde die is gekoppeld aan het kenmerk selecteren. Als u een kenmerk met de naam 'Afdeling' waarvan u de opgeslagen waarde verwijst naar een object in een afzonderlijke "Diensten"-tabel hebt, zou u bijvoorbeeld 'Departments.Name' selecteren. Houd er rekening mee dat de referentietabellen en de primaire id-velden die worden ondersteund voor een bepaalde toepassing vooraf zijn geconfigureerd en op dit moment kunnen niet worden bewerkt met behulp van de Azure portal, maar kunnen worden bewerkt met behulp van de [Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes).

Als u wilt een nieuw kenmerk toevoegen, Ga naar het einde van de lijst met ondersteunde kenmerken, vul de velden dan het gebruik van de opgegeven invoer en selecteer **kenmerk toevoegen**. Selecteer **opslaan** wanneer u klaar bent kenmerken toe te voegen. Vervolgens moet u laden van de **Provisioning** tabblad voor de nieuwe kenmerken beschikbaar in de editor kenmerk-toewijzing.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Herstellen van de standaardkenmerken en kenmerktoewijzingen

U moet beginnen en opnieuw instellen van uw bestaande toewijzingen terug naar de standaardstatus heeft, kunt u de **standaardtoewijzingen herstellen** selectievakje en de configuratie op te slaan. Hiermee wordt alle toewijzingen ingesteld als de toepassing hadden NET is toegevoegd aan uw Azure AD-tenant uit de galerie. 

Deze optie selecteert, wordt een hernieuwde synchronisatie van alle gebruikers effectief gedwongen terwijl de provisioning-service wordt uitgevoerd. 

>[!IMPORTANT]
>Het wordt sterk aanbevolen die **Inrichtingsstatus** worden ingesteld op **uit** voordat het aanroepen van deze optie.


## <a name="what-you-should-know"></a>Wat u moet weten

* Microsoft Azure AD biedt een efficiënte implementatie van een proces van synchronisatie. In een omgeving geïnitialiseerd, worden alleen de objecten waarvoor updates zijn vereist tijdens een synchronisatiecyclus verwerkt. 

* Kenmerk-toewijzingen bijwerken, heeft een invloed op de prestaties van een synchronisatiecyclus. Een update voor de configuratie van de kenmerk-toewijzing is vereist voor alle beheerde objecten om te worden opnieuw geëvalueerd. 

* Het is een aanbevolen procedure om te blijven het aantal opeenvolgende wijzigingen in uw kenmerktoewijzingen ten minste.


## <a name="next-steps"></a>Volgende stappen

* [Gebruiker inrichting/ongedaan maken van inrichting voor SaaS-toepassingen automatiseren](user-provisioning.md)
* [Expressies schrijven voor kenmerktoewijzingen](functions-for-customizing-application-data.md)
* [Bereikfilters toevoegen voor het inrichten van gebruikers](define-conditional-rules-for-provisioning-user-accounts.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](use-scim-to-provision-users-and-groups.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](../saas-apps/tutorial-list.md)


