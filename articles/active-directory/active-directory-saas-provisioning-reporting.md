---
title: Rapportage over Azure Active Directory automatisch inrichten van gebruikersaccounts voor SaaS-toepassingen | Microsoft Docs
description: Informatie over de status van automatische gebruikersaccount inrichten controleren en problemen oplossen met het inrichten van afzonderlijke gebruikers.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: e3be74fbb571a806fc03a92d0b1b373e35d196be
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363611"
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Zelfstudie: Rapportage over het inrichten van automatische gebruikersaccounts


Azure Active Directory bevat een [gebruikersaccount inrichtingsservice](active-directory-saas-app-provisioning.md) die helpt om de inrichting ongedaan maken inrichting van gebruikersaccounts in SaaS-apps en andere systemen, ten behoeve van de identiteit van de end-to-end levenscyclus automatiseren beheer. Azure AD biedt ondersteuning voor vooraf geïntegreerde gebruikersinrichting connectors voor alle toepassingen en systemen in de sectie 'Aanbevolen' van de [Azure AD-toepassingsgalerie](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured).

Dit artikel wordt beschreven hoe u controleert de status van het inrichten, nadat ze zijn ingesteld en het oplossen van het inrichten van gebruikers en groepen.

## <a name="overview"></a>Overzicht

Inrichting connectors zijn ingesteld en geconfigureerd met behulp van de [Azure-portal](https://portal.azure.com), door de [documentatie opgegeven](saas-apps/tutorial-list.md) voor de ondersteunde toepassing. Wanneer geconfigureerd en actief, worden inrichten gerapporteerd over het gebruik van een van twee methoden:

* **Azure-beheerportal** -in dit artikel wordt voornamelijk beschreven bij het ophalen van rapportgegevens uit de [Azure-portal](https://portal.azure.com), waarmee u zowel een overzichtsrapport inrichten, evenals gedetailleerde inrichting auditlogboeken voor een bepaalde toepassing.

* **Controle uitvoeren op API** -Azure Active Directory biedt ook een Audit-API die programmatische voor het ophalen van de inrichting gedetailleerde auditlogboeken. Zie [Azure Active Directory audit API-verwijzing](active-directory-reporting-api-audit-reference.md) voor documentatie die specifiek zijn voor het gebruik van deze API. Hoewel dit artikel omvat niet specifiek over het gebruik van de API, deze de typen gebeurtenissen die zijn vastgelegd in het auditlogboek inrichten gedetailleerd beschreven.

### <a name="definitions"></a>Definities

In dit artikel wordt gebruikgemaakt van de volgende voorwaarden is hieronder gedefinieerd:

* **Bronsysteem** -de opslagplaats van de gebruikers die de Azure AD-inrichtingsservice synchroniseert uit. Azure Active Directory is het bronsysteem voor het merendeel van vooraf geïntegreerde inrichting connectors, maar er zijn enkele uitzonderingen (voorbeeld: synchronisatie van Workday Inbound).

* **Doelsysteem** -de opslagplaats van de gebruikers die de Azure AD-inrichtingsservice synchroniseert op. Dit is meestal een SaaS-toepassing (voorbeelden: Salesforce, ServiceNow, Google Apps, Dropbox voor bedrijven), maar in sommige gevallen kan een on-premises systeem, zoals Active Directory (voorbeeld: Workday inkomende synchronisatie naar Active Directory).


## <a name="getting-provisioning-reports-from-the-azure-management-portal"></a>Ophalen van de inrichting van rapporten in de Azure management portal

Start als u wilt voor de inrichtingsservice ophalen rapportgegevens voor een bepaalde toepassing, door het starten van de [Azure-beheerportal](https://portal.azure.com) en te bladeren naar de Enterprise-toepassing waarvoor inrichting is geconfigureerd. Bijvoorbeeld, als u gebruikers met verhoogde bevoegdheden uitvoeren LinkedIn inricht, is de navigatiepad naar de details van de toepassing:

**Azure Active Directory > bedrijfstoepassingen > alle toepassingen > LinkedIn met verhoogde bevoegdheden uitvoeren**

Hier kunt u toegang tot het overzichtsrapport van de inrichting en de inrichting controlelogboeken, zowel die hieronder worden beschreven.


## <a name="provisioning-summary-report"></a>Overzichtsrapport voor inrichting

Het overzichtsrapport van de inrichting is zichtbaar in de **Provisioning** tabblad voor de opgegeven toepassing. Bevindt het zich in de **synchronisatiedetails** sectie onder **instellingen**, en biedt de volgende informatie:

* Het totale aantal gebruikers en groepen op die zijn gesynchroniseerd en zijn momenteel in het bereik voor het inrichten van tussen de bron en het doelsysteem

* De laatste keer dat de synchronisatie is uitgevoerd. Synchronisaties zich meestal elke 20-40 minuten, na een [voor de eerste synchronisatie](active-directory-saas-app-provisioning.md#what-happens-during-provisioning) is voltooid.

* Wel of niet een [voor de eerste synchronisatie](active-directory-saas-app-provisioning.md#what-happens-during-provisioning) is voltooid

* Of het inrichtingsproces in quarantaine zijn geplaatst, en wat de reden voor de status van de quarantaine is (bijvoorbeeld fout om te communiceren met doelsysteem vanwege ongeldige Administrator-referenties)

Het overzichtsrapport van de inrichting, moet de eerste plaats beheerders bekijken om te controleren op de operationele status van de taak.

 ![Samenvattend rapport](./media/active-directory-saas-provisioning-reporting/summary_report.PNG)

## <a name="provisioning-audit-logs"></a>Controlelogboeken voor inrichting
Alle activiteiten die worden uitgevoerd door de inrichtingsservice worden vastgelegd in de auditlogboeken van Azure AD, dat kunnen worden weergegeven in de **auditlogboeken** tabblad onder de **Accountinrichting** categorie. Geregistreerde activiteit gebeurtenistypen zijn onder andere:

* **Gebeurtenissen importeren** -telkens wanneer de Azure AD-inrichtingsservice wordt informatie opgehaald over een afzonderlijke gebruiker of groep van bron-systemen of doelsysteem wordt een 'import' gebeurtenis vastgelegd. Tijdens de synchronisatie worden gebruikers opgehaald uit het bronsysteem eerst met de resultaten die zijn vastgelegd als "importeren" gebeurtenissen. De overeenkomende id's van de opgehaalde gebruikers worden vervolgens een query uitgevoerd op het doelsysteem om te controleren of ze bestaan, met de resultaten ook als 'importeren' gebeurtenissen vastgelegd. Deze gebeurtenissen opnemen alle toegewezen gebruikerskenmerken en hun waarden die zichtbaar zijn voor de Azure AD-inrichtingsservice uit te voeren op het moment van de gebeurtenis. 

* **Regel synchronisatiegebeurtenissen** : deze gebeurtenissen rapporteren over de resultaten van de kenmerk-toewijzingsregels en een bereikfilters, geconfigureerd wanneer gebruikersgegevens is geïmporteerd en geëvalueerd vanuit de bron en doel-systemen. Bijvoorbeeld, als een gebruiker in een bronsysteem wordt geacht binnen het bereik voor het inrichten en beschouwd als niet in het doelsysteem bestaan en vervolgens deze gebeurtenis die registreert wordt de gebruiker ingericht in het doelsysteem. 

* **Gebeurtenissen exporteren** -een "export"-gebeurtenis wordt geregistreerd telkens wanneer de Azure AD-inrichtingsservice een gebruikersobject-account of groep voor het doelsysteem schrijft. Deze gebeurtenissen opnemen alle gebruikerskenmerken en hun waarden die zijn geschreven door de Azure AD-service inricht op het moment van de gebeurtenis. Als er een fout opgetreden is tijdens het schrijven van het gebruikersobject-account of groep op het doelsysteem, wordt deze hier weergegeven.

* **Escrow-gebeurtenissen verwerken** -proces borgen optreden wanneer de provisioning-service een fout wordt aangetroffen tijdens een poging een bewerking en probeer het opnieuw met een uitstelinterval periode wordt gestart. Een gebeurtenis 'escrow' wordt geregistreerd telkens wanneer die een bewerking voor inrichting is buiten gebruik gesteld.

Wanneer het inrichten van gebeurtenissen voor een afzonderlijke gebruiker bekijkt, worden de gebeurtenissen normaal plaats in de aangegeven volgorde:

1. Importeren van gebeurtenis: gebruiker wordt opgehaald uit het bronsysteem.

2. Importeren van gebeurtenis: doelsysteem wordt gevraagd om te controleren of er sprake van de gebruiker opgehaald.

3. Synchronisatie regel gebeurtenis: gebruikersgegevens van de bron en doel-systemen worden geëvalueerd op basis van de geconfigureerde kenmerktoewijzing regels en bereikfilters toevoegen om te bepalen welke actie, indien van toepassing, moet worden uitgevoerd.

4. Exporteren van gebeurtenis: als de gebeurtenis van de regel voor synchronisatie bepaald dat een actie moet worden uitgevoerd (toevoegen, bijwerken, verwijderen), en vervolgens de resultaten van de actie worden vastgelegd in een uitvoer-gebeurtenis.

![Het maken van een Azure AD-testgebruiker](./media/active-directory-saas-provisioning-reporting/audit_logs.PNG)


### <a name="looking-up-provisioning-events-for-a-specific-user"></a>Opzoeken van de inrichting van gebeurtenissen voor een specifieke gebruiker

De meest voorkomende use-case voor de inrichting auditlogboeken is om te controleren of de status van de inrichting van een individueel gebruikersaccount. Om te controleren of de laatste inrichting gebeurtenissen voor een specifieke gebruiker:

1. Ga naar de **auditlogboeken** sectie.

2. Uit de **categorie** in het menu **Accountinrichting**.

3. In de **datumbereik** in het menu selecteert u het datumbereik dat u zoeken wilt,

4. In de **zoeken** balk, geef de gebruikers-ID van de gebruiker die u wilt zoeken. De indeling van de id-waarde moet overeenkomen met wat u hebt geselecteerd als de primaire overeenkomende ID in de configuratie van de toewijzing van kenmerken (bijvoorbeeld, userPrincipalName of werknemer-ID-nummer). De waarde van de ID die vereist zijn zichtbaar in de kolom doelen.

5. Druk op Enter om te zoeken. De meest recente inrichting gebeurtenissen eerst geretourneerd.

6. Als gebeurtenissen geretourneerd, noteert u de activiteitstypen en of ze geslaagd of mislukt. Als er geen resultaten worden geretourneerd, vervolgens wordt de gebruiker niet bestaat, of is niet nog gedetecteerd door het inrichtingsproces als een volledige synchronisatie is nog niet voltooid.

7. Klik op de afzonderlijke gebeurtenissen om uitgebreide informatie, zoals de eigenschappen van alle gebruikers die zijn opgehaald, geëvalueerd of geschreven als onderdeel van de gebeurtenis weer te geven.

Zie de onderstaande video voor een demonstratie over het gebruik van de auditlogboeken. De auditlogboeken worden weergegeven rond de 5:30 markeren:

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

### <a name="tips-for-viewing-the-provisioning-audit-logs"></a>Tips voor het weergeven van de inrichting auditlogboeken

Voor de beste leesbaarheid in Azure portal, selecteert u de **kolommen** knop en kiest u deze kolommen:

* **Datum** -geeft de datum waarop de gebeurtenis heeft plaatsgevonden.
* **Doel(en)** -ziet u de app en gebruikers-ID die de onderwerpen van de gebeurtenis zijn.
* **Activiteit** -het activiteitstype, zoals eerder beschreven.
* **Status** - of de gebeurtenis is voltooid of niet.
* **Reden van de status** -een samenvatting van wat is er gebeurd in de inrichting gebeurtenis.


## <a name="troubleshooting"></a>Problemen oplossen

De inrichting logboeken voor het rapport en controle van samenvatting spelen een belangrijke rol helpt beheerders verschillende gebruikersaccount inrichten problemen oplossen.

Zie voor instructies over het automatisch inrichten van gebruikers oplossen scenario's gebaseerde [problemen bij het configureren en inrichten van gebruikers voor een toepassing](active-directory-application-provisioning-content-map.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
