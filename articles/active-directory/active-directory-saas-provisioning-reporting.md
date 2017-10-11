---
title: Rapportage van Azure Active Directory automatisch account gebruikersinrichting voor SaaS-toepassingen | Microsoft Docs
description: Informatie over het controleren van de status van automatische gebruikersaccount inrichten en het oplossen van de inrichting van afzonderlijke gebruikers.
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: asmalser-msft
ms.openlocfilehash: 86b9a3d93745045904c6038583b9bc6ebac5667e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Zelfstudie: Rapportage over automatische gebruikers account inrichten


Azure Active Directory bevat een [gebruikersaccount-service inricht](active-directory-saas-app-provisioning.md) die helpt om de inrichting ongedaan inrichting van gebruikersaccounts in de SaaS-apps en andere systemen, omwille van de levenscyclus van end-to-end identiteitsbeheer automatiseren. Azure AD biedt ondersteuning voor vooraf geïntegreerde gebruikers inrichten van connectors voor alle toepassingen en systemen in de sectie 'Aanbevolen' van de [Azure AD-toepassingsgalerie](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured).

Dit artikel wordt beschreven hoe u controleert de status van het inrichten nadat ze zijn ingesteld en het oplossen van de inrichting van afzonderlijke gebruikers en groepen.

## <a name="overview"></a>Overzicht

Inrichting connectors zijn voornamelijk ingesteld en geconfigureerd met behulp van de [Azure-beheerportal](https://portal.azure.com), door de [documentatie opgegeven](active-directory-saas-tutorial-list.md) voor de toepassing waarin het inrichten van het account vereist is. Zodra de geconfigureerde en actieve worden inrichten voor een toepassing gerapporteerd over het gebruik van een van twee methoden:

* **Azure-beheerportal** -in dit artikel beschrijft voornamelijk bij het ophalen van rapportgegevens uit de [Azure-beheerportal](https://portal.azure.com), waarmee u zowel een overzichtsrapport inrichten als gedetailleerde inrichting controlelogboeken voor een bepaalde toepassing.

* **Audit API** -Azure Active Directory biedt ook een Audit-API die basisfunctionaliteit voor programmatische voor het ophalen van de gedetailleerde inrichting controlelogboeken. Zie [Azure Active Directory-audit API-referentiemateriaal](active-directory-reporting-api-audit-reference.md) voor documentatie over het gebruik van deze API. Terwijl dit artikel het gebruik van de API niet specifiek omvat, dit de typen gebeurtenissen die zijn vastgelegd in het controlelogboek inrichting toegelicht.

### <a name="definitions"></a>Definities

In dit artikel maakt gebruik van de volgende termen, zoals hieronder gedefinieerd:

* **Bronsysteem** -de opslagplaats van gebruikers die de Azure AD-service inricht synchroniseert uit. Azure Active Directory is het bronsysteem voor het merendeel van de vooraf geïntegreerde connectors inrichten, zijn echter enkele uitzonderingen (voorbeeld: Workday inkomende synchronisatie).

* **Doelsysteem** -de opslagplaats van gebruikers die de Azure AD-service inricht voor synchroniseert. Dit is doorgaans een SaaS-toepassing (voorbeelden: Salesforce, ServiceNow, Google Apps, Dropbox voor bedrijven), maar in sommige gevallen kan een on-premises systeem, zoals Active Directory (voorbeeld: Workday inkomende synchronisatie naar Active Directory).


## <a name="getting-provisioning-reports-from-the-azure-management-portal"></a>Inrichting van rapporten van de Azure-beheerportal ophalen

Voor het ophalen van rapportgegevens voor een bepaalde toepassing inrichten, starten door het starten van de [Azure-beheerportal](https://portal.azure.com) en bladeren naar de Enterprise-toepassing waarvoor de inrichting is geconfigureerd. Bijvoorbeeld, als u gebruikers wilt uitbreiden LinkedIn inricht, is het pad navigatie gedetailleerde informatie over de toepassing:

**Azure Active Directory > bedrijfstoepassingen > alle toepassingen > LinkedIn uitbreiden**

Hier kunt u toegang hebt tot het overzichtsrapport voor het inrichten en de inrichting controlelogboeken, beide die hieronder worden beschreven.


### <a name="provisioning-summary-report"></a>Het overzichtsrapport voor inrichting

Het overzichtsrapport voor inrichting is zichtbaar in de **inrichten** tabblad voor de opgegeven toepassing. Bevindt het zich in de sectie synchronisatiedetails onder **instellingen**, en biedt de volgende informatie:

* Het totale aantal gebruikers en groepen op die zijn gesynchroniseerd en zijn momenteel in het bereik voor het inrichten van tussen de bron en het doelsysteem.

* De laatste keer dat de synchronisatie is uitgevoerd. Synchronisaties optreden doorgaans elke 20-40 minuten nadat een volledige synchronisatie is voltooid.

* Of er een initiële volledige synchronisatie is voltooid.

* Wel of niet tijdens het inrichtingsproces in quarantaine zijn geplaatst en wat de oorzaak van de quarantaine-status is bijvoorbeeld (kan niet communiceren met het doelsysteem vanwege ongeldige Administrator-referenties)

Het overzichtsrapport voor de inrichting, moet de eerste plaats admins uiterlijk bij het controleren van de operationele status van de taak.

 ![Het overzichtsrapport voor](./media/active-directory-saas-provisioning-reporting/summary_report.PNG)

### <a name="provisioning-audit-logs"></a>Inrichting controlelogboeken
Alle activiteiten die worden uitgevoerd door de inrichting-service worden geregistreerd in de Azure AD-auditlogboeken kunnen worden weergegeven in de **controlelogboeken** tabblad onder de **Account inrichten** categorie. Geregistreerde activiteit gebeurtenistypen zijn onder andere:

* **Gebeurtenissen importeren** -een 'import' gebeurtenis vastgelegd elke keer dat de Azure AD-service inricht informatie over een afzonderlijke gebruiker of groep van een bronsysteem of het doelsysteem haalt. Tijdens de synchronisatie worden gebruikers opgehaald uit het bronsysteem eerst met de resultaten die zijn vastgelegd als 'importeren' gebeurtenissen. De overeenkomende id's van de opgehaalde gebruikers worden vervolgens een query uitgevoerd op het doelsysteem om te controleren of ze bestaan, met de resultaten ook als 'importeren' gebeurtenissen vastgelegd. Deze gebeurtenissen opnemen alle toegewezen gebruikerskenmerken en hun waarden die zichtbaar zijn voor de Azure AD-service op het moment van de gebeurtenis inricht. 

* **Synchronisatie regel gebeurtenissen** - deze gebeurtenissen rapporteren over de resultaten van de toewijzingsregels kenmerk en een bereik filters geconfigureerd nadat gebruikersgegevens is geïmporteerd en geëvalueerd op basis van de bron en doel-systemen. Bijvoorbeeld, als een gebruiker in een bronsysteem wordt geacht binnen het bereik van de inrichting en dat niet bestaat in het doelsysteem aangenomen en vervolgens deze gebeurtenis die registreert wordt de gebruiker ingericht in het doelsysteem. 

* **Gebeurtenissen exporteren** -een "export" gebeurtenis vastgelegd elke keer dat de Azure AD-service inricht schrijft een account of groep van een gebruikersobject voor het doelsysteem. Deze gebeurtenissen opnemen alle gebruikerskenmerken en de waarden die zijn geschreven door de Azure AD-service op het moment van de gebeurtenis inricht. Als er een fout opgetreden is tijdens het schrijven van het gebruikersobject-account of groep naar het doelsysteem, wordt deze hier weergegeven.

* **Escrow gebeurtenissen verwerken** -proces borgen optreden wanneer de inrichting service een fout aangetroffen tijdens een poging een bewerking en probeer het opnieuw met een interval van back-off tijd wordt gestart. Telkens wanneer die een inrichtingsbewerking buiten gebruik werd gesteld, wordt een 'escrow' gebeurtenis vastgelegd.

Bij het onderzoeken van de inrichting van gebeurtenissen voor een afzonderlijke gebruiker, de normaal gebeurtenissen in deze volgorde:

1. Importeren van gebeurtenis: gebruiker wordt opgehaald uit het bronsysteem.

2. Importeren van gebeurtenis: doelsysteem om te controleren op de aanwezigheid van de opgehaalde gebruiker wordt gevraagd.

3. Regel synchronisatiegebeurtenis: gebruikersgegevens van de bron en doel-systemen worden geëvalueerd op basis van het geconfigureerde kenmerk mapping regels en bereikfilters om te bepalen welke actie, indien van toepassing, moet worden uitgevoerd.

4. Gebeurtenis exporteren: als de regel synchronisatiegebeurtenis bepaald dat een actie moet worden uitgevoerd (bijvoorbeeld toevoegen, bijwerken, verwijderen), en vervolgens de resultaten van de actie in een Export-gebeurtenis worden vastgelegd.

![Een Azure AD-testgebruiker maken](./media/active-directory-saas-provisioning-reporting/audit_logs.PNG)


### <a name="looking-up-provisioning-events-for-a-specific-user"></a>Opzoeken van de inrichting van gebeurtenissen voor een specifieke gebruiker

De meest voorkomende gebruiksvoorbeeld voor de inrichting controlelogboeken is om te controleren van de status van de inrichting van een afzonderlijk gebruikersaccount. De laatste inrichting gebeurtenissen voor een specifieke gebruiker opzoeken:

1. Ga naar de **controlelogboeken** sectie.

2. Van de **categorie** selecteert u **Account inrichten**.

3. In de **datumbereik** menu, selecteer het datumbereik dat u zoeken wilt,

4. In de **Search** balk, voert u de gebruikers-ID van de gebruiker die u wilt zoeken. De indeling van de id-waarde moet overeenkomen met wat u hebt geselecteerd als de primaire overeenkomende ID in de configuratie van de toewijzing (bijvoorbeeld userPrincipalName of werknemer-ID-nummer). De waarde van de ID die vereist zijn zichtbaar in de kolom doel(en).

5. Druk op Enter om te zoeken. De meest recente gebeurtenissen in de inrichting eerst geretourneerd.

6. Als er gebeurtenissen worden geretourneerd, noteert u de activiteitstypen en of ze is gelukt of mislukt. Als er geen resultaten worden geretourneerd, dan betekent dit dat de gebruiker bestaat niet, of is niet nog gedetecteerd bij het inrichtingsproces als een volledige synchronisatie is nog niet voltooid.

7. Klik op afzonderlijke gebeurtenissen om uitgebreide details, met inbegrip van alle eigenschappen van de gebruiker die zijn opgehaald, geëvalueerd of geschreven als onderdeel van de gebeurtenis te bekijken.


### <a name="tips-for-viewing-the-provisioning-audit-logs"></a>Tips voor het weergeven van de controlelogboeken van de inrichting

Voor de beste leesbaarheid in de Azure portal, selecteer de **kolommen** knop en kies deze kolommen:

* **Datum** -geeft de datum van de gebeurtenis heeft plaatsgevonden.
* **Doel(en)** -ziet u de app en gebruikers-ID die de onderdelen van de gebeurtenis zijn.
* **Activiteit** -het activiteitstype, zoals eerder beschreven.
* **Status** - of de gebeurtenis is voltooid of niet.
* **Statusreden** -een overzicht van wat er gebeurd in de inrichting gebeurtenis is.


## <a name="troubleshooting"></a>Problemen oplossen

De inrichting samenvatting rapport en audit logboeken spelen een belangrijke rol helpen beheerders verschillende gebruikersaccount inrichten problemen oplossen.

Zie voor instructies over het oplossen van problemen met Automatische gebruikersaanvragen scenario's gebaseerde [problemen bij het configureren en inrichten van gebruikers van een toepassing](active-directory-application-provisioning-content-map.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
