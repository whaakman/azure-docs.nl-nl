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
ms.date: 04/03/2019
ms.author: celested
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: b63118a7c5fae49edebe4ae4976a1362781ae8cf
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63759771"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Kenmerktoewijzingen voor Gebruikersinrichting voor SaaS-toepassingen in Azure Active Directory aanpassen
Microsoft Azure AD biedt ondersteuning voor het inrichten van gebruikers voor SaaS-toepassingen van derden zoals Salesforce, G Suite en anderen. Als u het inrichten van gebruikers voor een SaaS-toepassing van derden hebt ingeschakeld, bepaalt de Azure-portal de kenmerkwaarden via kenmerktoewijzingen.

Er is een vooraf geconfigureerde set kenmerken en kenmerktoewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten elke SaaS-app. Sommige apps beheren andere soorten objecten, samen met gebruikers, zoals groepen.

U kunt de standaard-kenmerktoewijzingen aanpassen op basis van de behoeften van uw bedrijf. U kunt dus, wijzigen of verwijderen van bestaande kenmerktoewijzingen of nieuwe kenmerktoewijzingen maken.
 
## <a name="editing-user-attribute-mappings"></a>Gebruiker-kenmerktoewijzingen bewerken

Volg deze stappen voor toegang tot de **toewijzingen** functie van het inrichten van gebruikers:

1. Aanmelden bij de [Azure Active Directory-portal](https://aad.portal.azure.com).

1. Selecteer **bedrijfstoepassingen** in het linkerdeelvenster. Een lijst met alle geconfigureerde apps wordt weergegeven, met inbegrip van apps die zijn toegevoegd vanuit de galerie.

1. Selecteer de gewenste app te laden van de app management in het deelvenster waar u kunt rapporten weergeven en beheren van app-instellingen.

1. Selecteer **Provisioning** voor het beheren van instellingen voor de geselecteerde app voor het inrichten gebruikersaccount.

1. Vouw **toewijzingen** bekijken en bewerken van de kenmerken van de gebruiker die tussen Azure AD stromen en de doeltoepassing. Als de doeltoepassing wordt ondersteund, wordt in deze sectie kunt u eventueel configureren van groepen en accounts voor gebruikers wordt ingericht.

   ![SalesForce](./media/customize-application-attributes/21.png) 

1. Selecteer een **toewijzingen** configuratie van de gerelateerde open **kenmerk toewijzing** scherm. Sommige kenmerktoewijzingen zijn vereist voor een SaaS-toepassing te laten functioneren. Voor de vereiste kenmerken, de **verwijderen** functie is niet beschikbaar.

   ![SalesForce](./media/customize-application-attributes/22.png)

   In deze schermafbeelding ziet u dat de **gebruikersnaam** kenmerk van een beheerd object in Salesforce wordt gevuld met de **userPrincipalName** waarde van de gekoppelde Azure Active Directory-Object.

1. Selecteer een bestaande **kenmerk toewijzing** openen de **kenmerk bewerken** scherm. Hier kunt u de kenmerken van de gebruiker die tussen Azure AD stromen en de doeltoepassing.

   ![SalesForce](./media/customize-application-attributes/23.png)


### <a name="understanding-attribute-mapping-types"></a>Kenmerktoewijzing typen
Met kenmerktoewijzingen, kunt u bepalen hoe kenmerken worden ingevuld in een SaaS-toepassing van derden. Er zijn vier verschillende toewijzen die worden ondersteund:

* **Directe** : het kenmerk target is gevuld met de waarde van een kenmerk van het gekoppelde object in Azure AD.
* **Constante** – het doelkenmerk gevuld met een specifieke tekenreeks die u hebt opgegeven.
* **Expressie** -het kenmerk target is ingevuld op basis van het resultaat van een script-achtige-expressie. 
  Zie voor meer informatie, [expressies schrijven voor kenmerktoewijzingen in Azure Active Directory](functions-for-customizing-application-data.md).
* **Geen** -het kenmerk target blijft ongewijzigd. Echter, als het doelkenmerk ooit leeg is, wordt dit ingevuld met de standaard-waarde die u opgeeft.

Samen met deze vier eenvoudige typen ondersteuning voor aangepaste kenmerktoewijzingen het concept van een optionele **standaard** toewijzing-waarde. De toewijzing van de standaard-waarde zorgt ervoor dat een target-kenmerk is gevuld met een waarde als er geen waarde in Azure AD of van het doelobject is. De meest voorkomende configuratie is te laat dit veld leeg.


### <a name="understanding-attribute-mapping-properties"></a>Understanding kenmerktoewijzing eigenschappen

In de vorige sectie zijn u al toegevoegd aan de eigenschap type van kenmerk-toewijzing.
Samen met deze eigenschap ondersteuning kenmerktoewijzingen ook voor de volgende kenmerken:

- **Bronkenmerk** -het gebruikerskenmerk van de van het bronsysteem (voorbeeld: Azure Active Directory).
- **Doelkenmerk** – het gebruikerskenmerk in het doelsysteem (voorbeeld: ServiceNow).
- **Overeenkomen met de objecten met behulp van dit kenmerk** : bepaalt of deze toewijzing moet worden gebruikt voor het aanduiden van gebruikers tussen de bron en doel-systemen. Dit standaard ingesteld op het kenmerk userPrincipalName of e-mail in Azure AD, die meestal is toegewezen aan een veld username in een doeltoepassing.
- **Prioriteit bij** : meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere zijn, zijn ze geëvalueerd in de volgorde die is gedefinieerd in dit veld. Zodra een overeenkomst wordt gevonden, geen verdere overeenkomende kenmerken worden geëvalueerd.
- **Deze toewijzing toepassen**
    - **Altijd** : deze toewijzing toepassen op beide gebruiker maken en bijwerken van acties.
    - **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op acties van het maken van de gebruiker.


## <a name="editing-group-attribute-mappings"></a>Kenmerktoewijzingen groep bewerken

Aantal toepassingen, zoals ServiceNow, Box en G Suite, ondersteunen de mogelijkheid voor het inrichten van de groep en gebruikersobjecten. Groepsobjecten kunnen bevatten groepseigenschappen zoals weergavenamen en aliassen, samen met leden van een e-mail.

![ServiceNow](./media/customize-application-attributes/24.png)

Inrichten van de groep kan worden eventueel ingeschakeld of uitgeschakeld door het selecteren van de toewijzing van apparaatgroepen onder **toewijzingen**, en de instelling **ingeschakeld** met de optie die u wilt dat in de **kenmerktoewijzing** scherm.

De kenmerken die zijn ingericht als onderdeel van de groepsobjecten kunnen worden aangepast op dezelfde manier als de objecten gebruiker, die eerder zijn beschreven. 

>[!TIP]
>Het inrichten van een groepsobjecten (eigenschappen en leden) is een afzonderlijke concept van [groepen toewijzen](assign-user-or-group-access-portal.md) tot een toepassing. Het is mogelijk een groep toewijzen aan een toepassing, maar alleen de gebruikersobjecten in de groep inrichten. Inrichting van de volledige groepsobjecten is niet vereist voor het gebruik van groepen in toewijzingen.


## <a name="editing-the-list-of-supported-attributes"></a>De lijst met ondersteunde kenmerken bewerken

De kenmerken van de gebruiker ondersteund voor een bepaalde toepassing zijn vooraf geconfigureerd. De meeste toepassingen Gebruikersbeheer-API's bieden geen ondersteuning voor schema-detectie. De Azure AD-inrichtingsservice is dus niet dynamisch genereren van de lijst met ondersteunde kenmerken door het aanroepen van de toepassing. 

Echter bepaalde toepassingen ondersteuning bieden voor aangepaste kenmerken en de Azure AD-inrichtingsservice kunt lezen en schrijven naar aangepaste kenmerken. Om in te voeren hun definities in de Azure-portal, selecteert u de **geavanceerde opties weergeven** selectievakje aan de onderkant van de **kenmerk toewijzing** scherm en selecteer vervolgens **kenmerkenlijst bewerken voor** uw app.

Toepassingen en systemen die ondersteuning bieden voor aanpassing van de lijst met kenmerken zijn onder andere:

* SalesForce
* ServiceNow
* WorkDay
* Azure Active Directory ([Azure AD Graph API standaardkenmerken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity) en aangepaste mapextensies worden ondersteund)
* Apps die ondersteuning bieden voor [SCIM 2.0](https://tools.ietf.org/html/rfc7643), waarbij de kenmerken die zijn gedefinieerd in de [core schema](https://tools.ietf.org/html/rfc7643) moeten worden toegevoegd

>[!NOTE]
>De lijst met ondersteunde kenmerken bewerken wordt alleen aanbevolen voor beheerders die het schema van hun toepassingen en systemen hebt aangepast en eerste hand kennis hebben van hoe de aangepaste kenmerken zijn gedefinieerd. Hiervoor moet soms vertrouwd zijn met de API's en ontwikkelhulpprogramma's geleverd door een toepassing of het systeem. 

Tijdens het bewerken van de lijst met ondersteunde kenmerken, vindt u de volgende eigenschappen:

* **Naam** -de systeemnaam van het kenmerk, zoals gedefinieerd in het schema van het doelobject. 
* **Type** -het type gegevens van het kenmerk opslaat, zoals gedefinieerd in het schema van het doelobject, dit kan een van de volgende typen:
   * *Binaire* -kenmerk bevat binaire gegevens.
   * *Booleaanse* -kenmerk bevat een waarde waar of ONWAAR.
   * *Datum-/* -kenmerk bevat een tekenreeks met datum.
   * *Geheel getal* -kenmerk bevat een geheel getal zijn.
   * *Naslaginformatie over* -kenmerk bevat een ID die verwijst naar een waarde die is opgeslagen in een andere tabel in de doeltoepassing.
   * *Tekenreeks* -kenmerk bevat een tekenreeks met tekst. 
* **Primaire sleutel?** -Of het kenmerk wordt gedefinieerd als een veld van de primaire sleutel in het schema van het doelobject.
* **Vereist?** -Of het kenmerk is vereist om te worden ingevuld in de doeltoepassing of het systeem.
* **Meerdere waarden?** -Of het kenmerk meerdere waarden ondersteunt.
* **Hoofdlettergevoelig?** -Of de waarden voor kenmerken worden geëvalueerd in een hoofdlettergevoelige manier.
* **API-expressie** -niet gebruikt, tenzij dit niet doet, in de documentatie voor een specifieke inrichting connector (zoals Workday).
* **Objectkenmerk waarnaar wordt verwezen** : als het is een kenmerk van het type verwijzing vervolgens dit menu kunt u de tabel en het kenmerk in de doeltoepassing waarin de waarde die is gekoppeld aan het kenmerk selecteren. Als u een kenmerk met de naam 'Afdeling' waarvan u de opgeslagen waarde verwijst naar een object in een afzonderlijke "Diensten"-tabel hebt, zou u bijvoorbeeld 'Departments.Name' selecteren. De referentietabellen en de primaire id-velden die worden ondersteund voor een bepaalde toepassing vooraf zijn geconfigureerd en op dit moment kan niet worden bewerkt met behulp van de Azure portal, maar kan worden bewerkt met behulp van de [Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes).

Als u wilt een nieuw kenmerk toevoegen, Ga naar het einde van de lijst met ondersteunde kenmerken, vul de velden dan het gebruik van de opgegeven invoer en selecteer **kenmerk toevoegen**. Selecteer **opslaan** wanneer u klaar bent kenmerken toe te voegen. Moet u vervolgens opnieuw te laden de **Provisioning** tabblad voor de nieuwe kenmerken beschikbaar in de editor kenmerk-toewijzing.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Herstellen van de standaardkenmerken en kenmerktoewijzingen

U moet beginnen en opnieuw instellen van uw bestaande toewijzingen terug naar de standaardstatus heeft, kunt u de **standaardtoewijzingen herstellen** selectievakje en de configuratie op te slaan. Alle toewijzingen in dat geval worden ingesteld als de toepassing zojuist is toegevoegd aan uw Azure AD-tenant uit de galerie. 

Deze optie selecteert, wordt een hersynchronisatie van alle gebruikers effectief gedwongen terwijl de provisioning-service wordt uitgevoerd. 

>[!IMPORTANT]
>Wordt sterk aangeraden **Inrichtingsstatus** worden ingesteld op **uit** voordat het aanroepen van deze optie.


## <a name="what-you-should-know"></a>Wat u moet weten

* Microsoft Azure AD biedt een efficiënte implementatie van een proces van synchronisatie. In een omgeving geïnitialiseerd, worden alleen de objecten waarvoor updates zijn vereist tijdens een synchronisatiecyclus verwerkt. 

* Kenmerk-toewijzingen bijwerken, heeft een invloed op de prestaties van een synchronisatiecyclus. Een update voor de configuratie van de kenmerk-toewijzing is vereist voor alle beheerde objecten om te worden opnieuw geëvalueerd. 

* Een aanbevolen procedure is het aantal opeenvolgende wijzigingen aan uw kenmerktoewijzingen ten minste houden.


## <a name="next-steps"></a>Volgende stappen

* [Gebruiker inrichting/ongedaan maken van inrichting voor SaaS-toepassingen automatiseren](user-provisioning.md)
* [Expressies schrijven voor kenmerktoewijzingen](functions-for-customizing-application-data.md)
* [Bereikfilters toevoegen voor het inrichten van gebruikers](define-conditional-rules-for-provisioning-user-accounts.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](use-scim-to-provision-users-and-groups.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](../saas-apps/tutorial-list.md)


