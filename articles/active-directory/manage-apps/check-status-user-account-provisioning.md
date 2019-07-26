---
title: Automatische gebruikers accounts inrichten voor SaaS-toepassingen | Microsoft Docs
description: Informatie over het controleren van de status van automatische toewijzing van gebruikers accounts en het oplossen van problemen met het inrichten van afzonderlijke gebruikers.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.date: 09/09/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: fda7654ca2d825ae4112dd06021c7e83ed6867cd
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68381256"
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Zelfstudie: Rapportage over het automatisch inrichten van gebruikers accounts

Azure Active Directory (Azure AD) bevat een [service](user-provisioning.md) voor het inrichten van gebruikers accounts die helpt bij het automatiseren van het inrichten van gebruikers account in Saas-apps en andere systemen, met het oog op end-to-end identiteits levenscyclus beheer. Azure AD biedt ondersteuning voor vooraf geïntegreerde User Provisioning connectors voor alle toepassingen en systemen in de sectie ' Aanbevolen ' van de [Azure AD-toepassings galerie](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured).

In dit artikel wordt beschreven hoe u de status van inrichtings taken kunt controleren nadat deze zijn ingesteld en hoe u problemen met het inrichten van afzonderlijke gebruikers en groepen kunt oplossen.

## <a name="overview"></a>Overzicht

De inrichtings connectors worden ingesteld en geconfigureerd met behulp van de [Azure Portal](https://portal.azure.com), door de [meegeleverde documentatie](../saas-apps/tutorial-list.md) voor de ondersteunde toepassing te volgen. Zodra de configuratie is geconfigureerd en wordt uitgevoerd, kunnen er twee methoden worden gebruikt voor het inrichten van taken:

* **Azure Portal** : in dit artikel wordt voornamelijk beschreven hoe u rapport gegevens ophaalt op basis van de [Azure Portal](https://portal.azure.com), die zowel een samenvattings rapport voor de inrichting als gedetailleerde audit logboeken voor een bepaalde toepassing bevat.
* **Audit-API** -Azure Active Directory biedt ook een controle-API waarmee de gedetailleerde inrichtings audit logboeken kunnen worden opgehaald. Zie [Azure Active Directory audit API-naslag informatie](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) voor documentatie die specifiek is voor het gebruik van deze API. Hoewel dit artikel niet specifiek betrekking heeft op het gebruik van de API, worden de typen inrichtings gebeurtenissen die in het audit logboek zijn vastgelegd, gedetailleerd beschreven.

### <a name="definitions"></a>Definities

In dit artikel worden de volgende voor waarden gebruikt, zoals hieronder gedefinieerd:

* **Bron systeem** : de opslag plaats van gebruikers die de Azure AD Provisioning-service synchroniseert. Azure Active Directory is het bron systeem voor het meren deel van vooraf geïntegreerde inrichtings connectors. er zijn echter enkele uitzonde ringen (bijvoorbeeld: Werkdag inkomende synchronisatie).
* **Doel systeem** : de opslag plaats van gebruikers aan wie de Azure AD-inrichtings service synchroniseert. Dit is doorgaans een SaaS-toepassing (bijvoorbeeld: Sales Force, ServiceNow, G suite, Dropbox voor bedrijven, maar in sommige gevallen kan een on-premises systeem zijn, zoals Active Directory (bijvoorbeeld: Werkdag inkomende synchronisatie naar Active Directory).

## <a name="getting-provisioning-reports-from-the-azure-portal"></a>Inrichtings rapporten ophalen van de Azure Portal

Als u informatie over het inrichtings rapport voor een bepaalde toepassing wilt ophalen, start u de [Azure Portal](https://portal.azure.com) en bladert u naar de bedrijfs toepassing waarvoor het inrichten is geconfigureerd. Als u bijvoorbeeld gebruikers inricht voor LinkedIn, is het pad naar de details van de toepassing:

**Azure Active Directory > bedrijfs toepassingen > alle toepassingen > LinkedIn-uitbrei ding**

Hier kunt u toegang krijgen tot het samenvattings rapport van de inrichting en de audit logboeken voor inrichtingen, zoals hieronder beschreven.

## <a name="provisioning-summary-report"></a>Samenvattings rapport voor inrichting

Het samenvattings rapport voor inrichtingen wordt weer gegeven op het tabblad **inrichten** voor de opgegeven toepassing. Het bevindt zich in de sectie **synchronisatie Details** onder **instellingen**en bevat de volgende informatie:

* Het totale aantal gebruikers en/of groepen dat is gesynchroniseerd en zich momenteel in het bereik bevindt voor de inrichting van het bron systeem en het doel systeem.
* De laatste keer dat de synchronisatie is uitgevoerd. Synchronisaties worden meestal om de 20-40 minuten uitgevoerd nadat een [initiële synchronisatie](user-provisioning.md#what-happens-during-provisioning) is voltooid.
* Hiermee wordt aangegeven of er al dan niet een [initiële synchronisatie](user-provisioning.md#what-happens-during-provisioning) is voltooid.
* Of het inrichtings proces al dan niet in quarantaine is geplaatst en wat de reden voor de quarantaine status is (bijvoorbeeld fout bij het communiceren met het doel systeem als gevolg van ongeldige beheerders referenties).

Het samenvattings rapport van de inrichting moet de eerste locatie beheerders zijn om de operationele status van de inrichtings taak te controleren.

 ![Samenvattend rapport](./media/check-status-user-account-provisioning/summary_report.PNG)

## <a name="provisioning-audit-logs"></a>Audit logboeken inrichten

Alle activiteiten die worden uitgevoerd door de inrichtings service worden vastgelegd in de Azure AD-controle logboeken, die kunnen worden weer gegeven op het tabblad **controle logboeken** in de categorie **account inrichting** . Gebeurtenis typen voor geregistreerde activiteiten zijn onder andere:

* **Gebeurtenissen importeren** : een import gebeurtenis wordt geregistreerd telkens wanneer de Azure AD-inrichtings service informatie ophaalt over een afzonderlijke gebruiker of groep van een bron systeem of doel systeem. Tijdens de synchronisatie worden gebruikers eerst uit het bron systeem opgehaald, met de resultaten die zijn vastgelegd als gebeurtenissen importeren. De overeenkomende Id's van de opgehaalde gebruikers worden vervolgens opgevraagd bij het doel systeem om te controleren of ze bestaan, met de resultaten die ook zijn vastgelegd als gebeurtenissen importeren. Deze gebeurtenissen registreren alle toegewezen gebruikers kenmerken en hun waarden die zijn gedetecteerd door de Azure AD-inrichtings service op het moment van de gebeurtenis.
* **Synchronisatie regel gebeurtenissen** : deze gebeurtenissen rapporteren over de resultaten van de regels voor kenmerk toewijzing en alle geconfigureerde filters voor bereik, nadat gebruikers gegevens zijn geïmporteerd en geëvalueerd op basis van de bron-en doel systemen. Als een gebruiker in een bron systeem bijvoorbeeld wordt geacht binnen het bereik van de inrichting te zijn, en als niet bestaat in het doel systeem, wordt deze gebeurtenis vastgelegd in het doel systeem om de gebruiker in te richten.
* **Gebeurtenissen exporteren** : een gebeurtenis ' exporteren ' wordt geregistreerd telkens wanneer de Azure AD-inrichtings service een gebruikers account of groeps object naar een doel systeem schrijft. Deze gebeurtenissen registreren alle gebruikers kenmerken en hun waarden die zijn geschreven door de Azure AD-inrichtings service op het moment van de gebeurtenis. Als er een fout is opgetreden tijdens het schrijven van het gebruikers account of groeps object naar het doel systeem, wordt dit hier weer gegeven.
* **Borg gebeurtenissen verwerken** : het verwerken van borgingen treedt op wanneer de inrichtings service een fout tegen komt tijdens het uitvoeren van een bewerking, en begint de bewerking opnieuw uit te voeren op een tijds interval van tijd. Elke keer dat een inrichtings bewerking opnieuw is uitgevoerd, wordt de gebeurtenis ' borg ' geregistreerd.

Bij het inrichtings gebeurtenissen voor een afzonderlijke gebruiker worden de gebeurtenissen normaal gesp roken in deze volg orde uitgevoerd:

1. Gebeurtenis importeren: De gebruiker wordt opgehaald uit het bron systeem.
1. Gebeurtenis importeren: Er wordt een query uitgevoerd op het doel systeem om te controleren of de opgehaalde gebruiker bestaat.
1. Synchronisatie regel gebeurtenis: Gebruikers gegevens van bron-en doel systemen worden geëvalueerd op basis van de geconfigureerde regels voor kenmerk toewijzing en bereik filters om te bepalen welke actie, indien van toepassing, moet worden uitgevoerd.
1. Gebeurtenis exporteren: Als de synchronisatie regel gebeurtenis bepaalt dat een actie moet worden uitgevoerd (toevoegen, bijwerken, verwijderen), worden de resultaten van de actie vastgelegd in een export gebeurtenis.

   ![Voorbeeld: Controle logboek pagina waarop de activiteiten en status worden weer gegeven](./media/check-status-user-account-provisioning/audit_logs.PNG)

### <a name="looking-up-provisioning-events-for-a-specific-user"></a>Inrichtings gebeurtenissen zoeken voor een specifieke gebruiker

De meest voorkomende use-case voor de inrichtings controle Logboeken is om de inrichtings status van een afzonderlijke gebruikers account te controleren. De laatste inrichtings gebeurtenissen voor een specifieke gebruiker opzoeken:

1. Ga naar de sectie **audit logs** .
1. Selecteer in het menu **categorie** de optie **account inrichten**.
1. Selecteer in het menu **datum bereik** het datum bereik dat u wilt zoeken.
1. Voer in de **Zoek** balk de gebruikers-id in van de gebruiker die u wilt zoeken. De indeling van ID-waarde moet overeenkomen met wat u hebt geselecteerd als de primaire overeenkomende ID in de configuratie voor kenmerk toewijzing (bijvoorbeeld userPrincipalName of werk nemer-ID-nummer). De vereiste ID-waarde wordt weer gegeven in de kolom doel (en).
1. Druk op ENTER om te zoeken. De meest recente inrichtings gebeurtenissen worden eerst geretourneerd.
1. Als er gebeurtenissen worden geretourneerd, noteert u de activiteitstypen en of deze zijn geslaagd of mislukt. Als er geen resultaten worden geretourneerd, betekent dit dat de gebruiker niet bestaat of nog niet is gedetecteerd door het inrichtings proces als een volledige synchronisatie nog niet is voltooid.
1. Klik op afzonderlijke gebeurtenissen om uitgebreide details weer te geven, inclusief alle gebruikers eigenschappen die zijn opgehaald, geëvalueerd of geschreven als onderdeel van de gebeurtenis.

Zie de onderstaande video voor een demonstratie van het gebruik van de audit Logboeken. De audit logboeken worden weer gegeven rond de 5:30-markering:

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

### <a name="tips-for-viewing-the-provisioning-audit-logs"></a>Tips voor het weer geven van de audit logboeken voor inrichting

Voor de beste Lees baarheid in de Azure Portal selecteert u de knop **kolommen** en kiest u de volgende kolommen:

* **Datum** : toont de datum waarop de gebeurtenis heeft plaatsgevonden.
* **Doel (en)** : hier worden de app-naam en gebruikers-id weer gegeven die de onderwerpen van de gebeurtenis zijn.
* **Activiteit** : het type activiteit, zoals eerder beschreven.
* **Status** : Hiermee wordt aangegeven of de gebeurtenis is geslaagd of niet.
* **Status reden** : een samen vatting van wat er is gebeurd in de inrichtings gebeurtenis.

## <a name="troubleshooting"></a>Problemen oplossen

Het samenvattings rapport voor inrichtings rapporten en controle logboeken spelen een belang rijke rol om beheerders te helpen bij het oplossen van problemen met het inrichten van verschillende gebruikers accounts.

Zie problemen met het [configureren en inrichten van gebruikers voor een toepassing](application-provisioning-config-problem.md)voor op scenario's gebaseerde richt lijnen voor het oplossen van problemen met automatische gebruikers inrichting.

## <a name="additional-resources"></a>Aanvullende resources

* [Inrichten van gebruikers accounts voor zakelijke apps beheren](configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
