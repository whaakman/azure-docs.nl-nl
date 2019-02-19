---
title: 'Zelfstudie: Configureren van Workday voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Workday.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: daveba
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/19/2019
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e209fe0486b72c14912fd0af1b29c878e4b4545
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56340107"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning"></a>Zelfstudie: Workday voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is om weer te geven van de stappen dat u moet uitvoeren voor het importeren van werknemer profielen uit Workday in Active Directory en Azure Active Directory, met optionele write-back van Workday e-mailadres.

## <a name="overview"></a>Overzicht

De [Azure Active Directory-gebruiker inrichtingsservice](../manage-apps/user-provisioning.md) kan worden geïntegreerd met de [Workday Human Resources API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) om het inrichten van gebruikersaccounts. Azure AD maakt gebruik van deze verbinding om in te schakelen van de volgende gebruiker inrichten van werkstromen:

* **Inrichten van gebruikers voor Active Directory** -geselecteerd inrichten van gebruikers ingesteld van Workday naar een of meer Active Directory-domeinen.

* **Inrichten van gebruikers alleen in de cloud met Azure Active Directory** - In scenario's waarbij on-premises Active Directory niet gebruikt wordt, gebruikers rechtstreeks vanuit Workday kunnen worden ingericht op Azure Active Directory met de Azure AD-gebruiker-service inricht.

* **Schrijven achterzijde e-mailadressen naar Workday** -de Azure AD-gebruiker inrichtingsservice kunt terugschrijven van het e-mailadressen van Azure AD-gebruikers naar Workday.

### <a name="what-human-resources-scenarios-does-it-cover"></a>Welke human resources-scenario's onder deze wet?

De Workday gebruiker inrichting werkstromen die worden ondersteund door de inrichtingsservice voor Azure AD-gebruiker inschakelen van de volgende human resources en scenario's voor het beheer van identiteit lifecycle-automatisering:

* **Nieuwe werknemers aanstellen** : wanneer een nieuwe werknemer wordt toegevoegd aan de werkdag, een gebruikersaccount wordt automatisch gemaakt in Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen ondersteund door Azure AD](../manage-apps/user-provisioning.md), met terugschrijven van het e-mailadres Workday.

* **Updates voor het kenmerk en het profiel van werknemer** : wanneer een werknemerrecord wordt bijgewerkt in Workday (zoals de naam, titel of manager), hun gebruikersaccount, automatisch worden bijgewerkt in Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen die worden ondersteund door Azure AD](../manage-apps/user-provisioning.md).

* **Werknemer afsluitingen** : wanneer een werknemer is beëindigd in Workday, hun gebruikersaccount is automatisch uitgeschakeld in Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen die door Azure worden ondersteund AD](../manage-apps/user-provisioning.md).

* **Werknemer rehires** : wanneer een werknemer is rehired in Workday, hun oude account kan worden automatisch geactiveerd of opnieuw ingericht (afhankelijk van uw voorkeur) naar Active Directory, Azure Active Directory, en eventueel Office 365 en [andere SaaS-toepassingen die worden ondersteund door Azure AD](../manage-apps/user-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Wie is deze gebruiker inrichting oplossing het beste geschikt is voor?

Deze oplossing voor gebruikersinrichting van Workday bevindt zich momenteel in openbare preview en is ideaal voor:

* Organisaties die behoefte hebben aan een vooraf gemaakte, cloud-gebaseerde oplossing voor het inrichten van Workday-gebruikers

* Organisaties waarvoor directe gebruikersinrichting van Workday naar Active Directory of Azure Active Directory

* Organisaties die vereisen dat gebruikers worden ingericht met behulp van gegevens die zijn verkregen van de werkdag HCM-module (Zie [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organisaties die lid wordt, verplaatst, vereisen en waardoor gebruikers kunnen worden gesynchroniseerd met een of meer Active Directory-Forests, domeinen en OE's alleen op basis van de gegevens gedetecteerd in de module Workday HCM wijzigen (Zie [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organisaties met behulp van Office 365 voor e-mail

## <a name="solution-architecture"></a>Architectuur voor de oplossing

Deze sectie beschrijft de end-to-end gebruikersinrichting oplossingsarchitectuur voor algemene hybride omgevingen. Er zijn twee verwante stromen:

* **Gezaghebbende HR-gegevensstroom – van Workday naar on-premises Active Directory:** In deze stroom worker-gebeurtenissen (zoals New Hires overdrachten afsluitingen) eerst optreden in de cloud Workday HR-tenant en vervolgens de gebeurtenisgegevens worden overgebracht naar on-premises Active Directory via Azure AD en de Agent wordt ingericht. Afhankelijk van de gebeurtenis, kan dit leiden tot bewerkingen voor maken/bijwerken/in-of uitschakelen in AD.
* **E Write-back van Flow – van on-premises Active Directory naar Workday:** Zodra het account te maken voltooid in Active Directory is, het is gesynchroniseerd met Azure AD via Azure AD Connect en e-kenmerk is afkomstig uit Active Directory kan worden teruggeschreven naar Workday.

![Overzicht](./media/workday-inbound-tutorial/wd_overview.png)

### <a name="end-to-end-user-data-flow"></a>Gegevensstroom van end-to-end-gebruiker

1. De HR-team voert worker-transacties (verbindingselementen/Movers/Leavers of nieuwe overdracht/medewerkers/afsluitingen) in Workday HCM
2. De Azure AD Provisioning-Service wordt uitgevoerd geplande synchronisaties van identiteiten uit Workday HR en hierin wijzigingen beschreven die moeten worden verwerkt voor synchronisatie met on-premises Active Directory.
3. De Azure AD-inrichtingsservice roept de on-premises AAD Connect inrichting Agent met een verzoek nettolading met AD-account maken/bijwerken/inschakelen/uitschakelen bewerkingen.
4. De Azure AD Connect inrichting Agent gebruikt een serviceaccount voor gegevens van AD-account toevoegen/bijwerken.
5. De Azure AD Connect / AD-synchronisatie-engine wordt uitgevoerd Deltasynchronisatie om op te halen van updates in AD.
6. De Active Directory-updates zijn gesynchroniseerd met Azure Active Directory.
7. Als de Write-back van Workday-connector is geconfigureerd, deze-write-backs kenmerk e-mailbericht naar Workday, op basis van het overeenkomstige kenmerk wordt gebruikt.

## <a name="planning-your-deployment"></a>Uw implementatie plannen

Controleer de onderstaande vereisten voordat u begint met uw Workday-integratie, en lees de volgende richtlijnen over het afstemmen op uw huidige Active Directory-architectuur en gebruikers vereisten inrichten met de oplossing(en) geleverd door Azure Active Directory. Een uitgebreide [implementatieplan](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans) met planning werkbladen is ook beschikbaar om te helpen u bij het samenwerken met uw partner voor Workday-integratie en HR-belanghebbenden.

In deze sectie worden de volgende aspecten van de planning:

* [Vereisten](#prerequisites)
* [Inrichting connector apps die worden geïmplementeerd selecteren](#selecting-provisioning-connector-apps-to-deploy)
* [Implementatie van Azure AD Connect inrichting Agent plannen](#planning-deployment-of-azure-ad-connect-provisioning-agent)
* [Integreren met meerdere Active Directory-domeinen](#integrating-with-multiple-active-directory-domains)
* [Planning van Workday kenmerktoewijzing Active Directory-gebruiker en transformaties](#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)

### <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure AD Premium P1 of hoger abonnement met de globale beheerderstoegang
* Een tenant van de implementatie van Workday voor test-en integratie
* Beheerdersmachtigingen in Workday een system integration-gebruiker maken en hierin wijzigingen voor het testen van gegevens van werknemers voor testdoeleinden
* Voor het inrichten van gebruikers met Active Directory, een server met WindowsServer 2012 of hoger met .NET 4.7.1+ runtime is vereist op de host de [on-premises inrichtingsagent](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) voor het synchroniseren van gebruikers tussen Active Directory en Azure AD

### <a name="selecting-provisioning-connector-apps-to-deploy"></a>Inrichting connector apps die worden geïmplementeerd selecteren

Om te kunnen inrichten werkstromen tussen Workday en Active Directory, biedt Azure AD meerdere inrichting apps van de connector die u uit de galerie met Azure AD-app kunt toevoegen:

![AAD-App-galerie](./media/workday-inbound-tutorial/wd_gallery.png)

* **WorkDay naar Active Directory-gebruikers inrichten** -deze app vereenvoudigt het uitvoeren van het inrichten van gebruikersaccounts uit Workday op één Active Directory-domein. Als u meerdere domeinen hebt, kunt u één exemplaar van deze app uit de galerie met Azure AD-app voor elk Active Directory-domein dat u inrichten wilt op toevoegen.

* **Met Azure AD-gebruiker inrichten WorkDay** : hoewel AAD Connect het hulpprogramma dat moet worden gebruikt is voor het synchroniseren van Active Directory: gebruikers met Azure Active Directory, deze app kunnen worden gebruikt voor het inrichten van gebruikers in Workday naar een enkel Azure alleen in de cloud vereenvoudigen Active Directory-tenant.

* **Write-back van WorkDay** -write-back van e-mailadressen van de gebruiker uit Azure Active Directory naar Workday vereenvoudigt het uitvoeren van deze app.

> [!TIP]
> De reguliere 'Werkdag'-app wordt gebruikt voor het instellen van eenmalige aanmelding tussen Workday en Azure Active Directory.

Gebruik de stroomdiagram besluit om te identificeren welke inrichting Workday-apps zijn relevant voor uw scenario.
    ![Stroomdiagram besluit](./media/workday-inbound-tutorial/wday_app_flowchart.png "besluit stroomdiagram")

Gebruik de inhoudsopgave om te gaan naar de betreffende sectie van deze zelfstudie.

### <a name="planning-deployment-of-azure-ad-connect-provisioning-agent"></a>Implementatie van Azure AD Connect inrichting Agent plannen

> [!NOTE]
> In deze sectie is alleen relevant als u van plan bent de Workday implementeren in Active Directory: gebruikers inrichten App. U kunt deze stap overslaan als u de Write-back van Workday of Workday naar Azure AD-gebruiker Provisioning-App implementeert.

De Workday naar oplossing inrichten van de AD-gebruikers is vereist voor het implementeren van een of meer Agents voor inrichting op servers met Windows 2012 R2 of hoger met ten minste 4 GB RAM-geheugen en .NET 4.7.1+ runtime. De volgende punten moeten rekening worden gehouden voordat u de inrichting-Agent installeert:

* Zorg ervoor dat de host-server waarop de Agent inrichting wordt uitgevoerd toegang tot het netwerk naar het doeldomein AD
* De configuratiewizard voor inrichting Agent registreert de agent bij uw Azure AD-tenant en het registratieproces vereist toegang tot *. msappproxy.net via de SSL-poort 443. Zorg ervoor dat uitgaande firewallregels die deze communicatie mogelijk. Biedt ondersteuning voor de agent [uitgaande HTTPS-proxyconfiguratie](#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication).
* De Agent voor het inrichten van een service-account gebruikt om te communiceren met de on-premises AD-domeinen. Voorafgaand aan de installatie van de agent is het aanbevolen dat u een service-account maken met administratormachtigingen en een wachtwoord dat niet verloopt.  
* Tijdens de configuratie van de Agent wordt ingericht, kunt u domeincontrollers die inrichting aanvragen moeten verwerken. Als u meerdere geografisch verspreide domeincontrollers hebt, de installatie inrichten in dezelfde site als uw voorkeur domeincontrollers voor het verbeteren van de betrouwbaarheid en prestaties van de end-to-end-oplossing
* Voor hoge beschikbaarheid, kunt u meer dan één inrichten van Agent implementeren en registreren voor het afhandelen van dezelfde set van on-premises AD-domeinen.

> [!IMPORTANT]
> In een productieomgeving, wordt aangeraden dat u een minimum van 3 inrichting Agents zijn geconfigureerd met uw Azure AD-tenant voor hoge beschikbaarheid hebt.

### <a name="integrating-with-multiple-active-directory-domains"></a>Integreren met meerdere Active Directory-domeinen

> [!NOTE]
> In deze sectie is alleen relevant als u van plan bent de Workday implementeren in Active Directory: gebruikers inrichten App. U kunt deze stap overslaan als u de Write-back van Workday of Workday naar Azure AD-gebruiker Provisioning-App implementeert.

Afhankelijk van uw Active Directory-topologie moet u bepalen het aantal gebruikers wordt ingericht-Apps-Connector en het aantal Agents inrichten om te configureren. Hieronder volgen enkele van de algemene implementatie-patronen die u verwijzen kunt naar als u van plan uw implementatie bent.

#### <a name="deployment-scenario-1--single-workday-tenant---single-ad-domain"></a>Implementatiescenario #1: Single Workday Tenant -> Single AD domain

In dit scenario, hebt u één tenant voor Workday en u graag zou willen gebruikers inrichten voor één doel AD-domein. Hier volgt de aanbevolen productieconfiguratie voor deze implementatie.

|   |   |
| - | - |
| Nee. van het inrichten van agents implementeren on-premises | 3 (voor hoge beschikbaarheid en een failover wordt uitgevoerd) |
| Nee. van Workday naar AD gebruiker inrichten van Apps in Azure portal configureren | 1 |

  ![Scenario 1](./media/workday-inbound-tutorial/dep_scenario1.png)

#### <a name="deployment-scenario-2--single-workday-tenant---multiple-child-ad-domains"></a>Implementatiescenario #2: Single Workday Tenant -> Multiple child AD domains

In dit scenario omvat het inrichten van gebruikers uit Workday voor meerdere doel AD onderliggende domeinen in een forest. Hier volgt de aanbevolen productieconfiguratie voor deze implementatie.

|   |   |
| - | - |
| Nee. van het inrichten van agents implementeren on-premises | 3 (voor hoge beschikbaarheid en een failover wordt uitgevoerd) |
| Nee. van Workday naar AD gebruiker inrichten van Apps in Azure portal configureren | een app per onderliggend domein |

  ![Scenario 2](./media/workday-inbound-tutorial/dep_scenario2.png)

#### <a name="deployment-scenario-3--single-workday-tenant---disjoint-ad-forests"></a>Implementatiescenario #3: Één werkdag Tenant -> niet-aaneengesloten AD-forests

In dit scenario omvat het inrichten van gebruikers uit Workday voor domeinen in niet-aaneengesloten AD-forests. Hier volgt de aanbevolen productieconfiguratie voor deze implementatie.

|   |   |
| - | - |
| Nee. van het inrichten van agents implementeren on-premises | 3 per niet-aaneengesloten AD-forest |
| Nee. van Workday naar AD gebruiker inrichten van Apps in Azure portal configureren | een app per onderliggend domein |

  ![Scenario 3](./media/workday-inbound-tutorial/dep_scenario3.png)

### <a name="planning-workday-to-active-directory-user-attribute-mapping-and-transformations"></a>Planning van Workday kenmerktoewijzing Active Directory-gebruiker en transformaties

> [!NOTE]
> In deze sectie is alleen relevant als u van plan bent de Workday implementeren in Active Directory: gebruikers inrichten App. U kunt deze stap overslaan als u de Write-back van Workday of Workday naar Azure AD-gebruiker Provisioning-App implementeert.

Voordat u configureert het inrichten van gebruikers aan een Active Directory-domein, moet u de volgende vragen beantwoorden. De antwoorden op deze vragen kunnen bepalen hoe uw bereikfilters en kenmerktoewijzingen moeten worden ingesteld.

* **Wat gebruikers in Workday moeten worden ingericht op deze Active Directory-forest?**

  * *Voorbeeld: Gebruikers waar het kenmerk Workday 'Bedrijf' bevat de waarde 'Contoso' en het kenmerk "Worker_Type" "Normale"*

* **Hoe worden gebruikers gerouteerd naar de andere organisatie-eenheden (OU's)?**

  * *Voorbeeld: Gebruikers worden doorgestuurd naar de OE's die met een office-locatie overeenkomen, zoals gedefinieerd in de Workday "Gemeenteraad" en "Country_Region_Reference" kenmerken*

* **Hoe moeten de volgende kenmerken worden ingevuld in de Active Directory?**

  * Algemene naam (cn)
    * *Voorbeeld: Gebruik de waarde van Workday User_ID, zoals ingesteld door human resources*

  * Werknemer-ID (employeeId)
    * *Voorbeeld: Gebruik de waarde van Workday Worker_ID*

  * SAM-accountnaam (sAMAccountName)
    * *Voorbeeld: Gebruik de waarde van Workday User_ID, gefilterd via een Azure AD-expressie wordt ingericht om ongeldige tekens te verwijderen*

  * User Principal Name (userPrincipalName)
    * *Voorbeeld: De waarde Workday User_ID gebruiken met een Azure AD-inrichting expressie de domeinnaam van een toe te voegen*

* **Hoe gebruikers overeenkomt tussen Workday en Active Directory?**

  * *Voorbeeld: Gebruikers met een specifieke werkdag 'Worker_ID', waarde wordt voldaan met Active Directory-gebruikers waar "employeeID" dezelfde waarde heeft. Als de waarde Worker_ID niet in Active Directory gevonden is, maakt u een nieuwe gebruiker.*
  
* **Bevat het Active Directory-forest al de gebruikers-id voor de overeenkomende logica is vereist om te werken?**

  * *Voorbeeld: Als dit een nieuwe implementatie van de werkdag is, verdient het aanbeveling dat Active Directory ingevuld met de juiste Workday Worker_ID waarden (of de unieke id-waarde van keuze worden) de overeenkomende logica zo eenvoudig mogelijk houden.*

Over het instellen en configureren van deze speciale inrichting connector-apps is het onderwerp van de rest van deze zelfstudie. Welke apps u wilt configureren, is afhankelijk van de systemen die u inrichten, en het aantal Active Directory: domeinen en Azure AD wilt-tenants in uw omgeving zijn.

## <a name="configure-integration-system-user-in-workday"></a>Integratie-systeemgebruiker in Workday configureren

Een algemene vereiste voor alle connectors inrichting Workday is die ze nodig hebben de referenties van een gebruiker Workday-integratie systeem verbinding maken met de Workday Human Resources-API. In deze sectie wordt beschreven hoe u een integratie van system-gebruiker maken in Workday en heeft de volgende secties:

* [Het maken van een integratie systeemgebruiker](#creating-an-integration-system-user)
* [Het maken van een beveiligingsgroep integratie](#creating-an-integration-security-group)
* [Beveiligingsmachtigingen voor beleid domein configureren](#configuring-domain-security-policy-permissions)
* [Zakelijke proces beveiligingsmachtigingen beleid configureren](#configuring-business-process-security-policy-permissions)
* [Wijzigingen in het beveiligingsbeleid activeren](#activating-security-policy-changes)

> [!NOTE]
> Het is mogelijk om deze procedure overslaan en gebruiken in plaats daarvan een globale beheerdersaccount van Workday als het systeemaccount van de integratie. Dit werkt prima voor demo's, maar wordt niet aanbevolen voor productie-implementaties.

### <a name="creating-an-integration-system-user"></a>Het maken van een integratie systeemgebruiker

**Maakt een systeemgebruiker integratie:**

1. Meld u aan bij uw Workday-tenant met een administrator-account. In de **Workday toepassing**, voer gebruiker maken in het zoekvak en klik vervolgens op **maken integratie systeemgebruiker**.

    ![Gebruiker maken](./media/workday-inbound-tutorial/wd_isu_01.png "gebruiker maken")
2. Voltooi de **integratie systeemgebruiker maken** taak door het opgeven van een gebruikersnaam en wachtwoord voor een nieuwe gebruiker van de integratie-systeem.  
  
* Laat de **wachtwoord vereisen bij volgende aanmelding In** optie dit selectievakje uitschakelt, omdat deze gebruiker via een programma aanmelden.
* Laat de **minuten voor de time-out sessie** met de standaardwaarde 0 en waarmee wordt voorkomen dat van de gebruiker sessies voortijdig een time-out opgetreden.
* Selecteer de optie **doen geen gebruikersinterface sessies toestaan** als het biedt een extra beveiligingslaag, waarmee wordt voorkomen een gebruiker met het wachtwoord van het integratiesysteem logboekregistratie in Workday dat.

    ![Integratie-systeemgebruiker maken](./media/workday-inbound-tutorial/wd_isu_02.png "Integration System-gebruiker maken")

### <a name="creating-an-integration-security-group"></a>Het maken van een beveiligingsgroep integratie

In deze stap maakt u een integratie met onbeperkte of beperkte system security group maken in Workday en de integratie van systeemgebruiker gemaakt in de vorige stap aan deze groep toewijzen.

**Een beveiligingsgroep maken:**

1. Voer security group maken in het zoekvak en klik vervolgens op **beveiligingsgroep maken**.

    ![Groep CreateSecurity](./media/workday-inbound-tutorial/wd_isu_03.png "CreateSecurity groep")
2. Voltooi de **beveiligingsgroep maken** taak. 

  * Er zijn twee soorten beveiligingsgroepen in Workday:
    * **Een:** Alle leden van de beveiligingsgroep toegang tot alle gegevens beveiligd door de beveiligingsgroep.
    * **Beperkte:** Alle leden van de beveiliging groep hebben contextuele toegang tot een subset van data-exemplaren (rijen) die toegang hebben tot de beveiligingsgroep.
  * Neem contact op met uw Workday-integratie-partner om te selecteren van de juiste beveiligingsgroeptype voor de integratie.
  * Als u welk groepstype weet, selecteert u **integratie Systeembeveiligingsgroep (onbeperkt)** of **integratie Systeembeveiligingsgroep (beperkt)** uit de **Type van de Tenants beveiligingsgroep**  vervolgkeuzelijst.

    ![Groep CreateSecurity](./media/workday-inbound-tutorial/wd_isu_04.png "CreateSecurity groep")

3. Nadat de beveiligingsgroep gemaakt is, ziet u een pagina waar u leden aan de beveiligingsgroep toewijzen kunt. De nieuwe integratie systeemgebruiker gemaakt in de vorige stap aan deze beveiligingsgroep toevoegen. Als u *beperkte* beveiligingsgroep, ook moet u de juiste Organisatiebereik selecteren.

    ![Beveiligingsgroep bewerken](./media/workday-inbound-tutorial/wd_isu_05.png "beveiligingsgroep bewerken")

### <a name="configuring-domain-security-policy-permissions"></a>Beveiligingsmachtigingen voor beleid domein configureren

In deze stap maakt u moet hiervoor toestemming 'domeinbeveiliging' beleid voor de worker-gegevens aan de beveiligingsgroep.

**Beveiligingsmachtigingen voor beleid domein configureren:**

1. Voer **domein Beveiligingsconfiguratie** in het zoekvak en klik vervolgens op de koppeling **configuratierapport van het domein Security**.  

    ![Domain Security Policies](./media/workday-inbound-tutorial/wd_isu_06.png "Domain Security Policies")  
2. In de **domein** in het tekstvak Zoeken naar de volgende domeinen bevinden, en ze toevoegen aan het filter één voor één.  
   * *Extern Account inrichten*
   * *Werknemersgegevens: Openbare Worker-rapporten*
   * *Persoonsgegevens: Neem contact op met gegevens werken*
   * *Werknemersgegevens: Alle functies*
   * *Werknemersgegevens: Huidige bezetting informatie*
   * *Werknemersgegevens: Functie in werknemersprofiel*

    ![Domain Security Policies](./media/workday-inbound-tutorial/wd_isu_07.png "Domain Security Policies")  

    ![Domain Security Policies](./media/workday-inbound-tutorial/wd_isu_08.png "Domain Security Policies") 

    Klik op **OK**.

3. In het rapport dat weergegeven wordt, selecteer het weglatingsteken (...) die wordt weergegeven naast **extern Account inrichten** en klik op de menuoptie **domein-Beleidsmachtigingen Security bewerken >**

    ![Domain Security Policies](./media/workday-inbound-tutorial/wd_isu_09.png "Domain Security Policies")  

4. Op de **bewerken domein beleid beveiligingsmachtigingen** pagina, blader naar de sectie **integratie machtigingen**. Klik op het teken '+' de integratie-systeemgroep toevoegen aan de lijst met beveiligingsgroepen met **ophalen** en **plaatsen** integratie machtigingen.

    ![Machtiging bewerken](./media/workday-inbound-tutorial/wd_isu_10.png "machtiging bewerken")  

5. Klik op het teken '+' de integratie-systeemgroep toevoegen aan de lijst met beveiligingsgroepen met **ophalen** en **plaatsen** integratie machtigingen.

    ![Machtiging bewerken](./media/workday-inbound-tutorial/wd_isu_11.png "machtiging bewerken")  

6. Herhaal stap 3 tot 5 hierboven voor elk van deze resterende beveiligingsbeleid:

   | Bewerking | Beveiligingsbeleid voor domein |
   | ---------- | ---------- |
   | Get- en Put | Werknemersgegevens: Openbare Worker-rapporten |
   | Get- en Put | Persoonsgegevens: Neem contact op met gegevens werken |
   | Ophalen | Werknemersgegevens: Alle functies |
   | Ophalen | Werknemersgegevens: Huidige bezetting informatie |
   | Ophalen | Werknemersgegevens: Functie in werknemersprofiel |

### <a name="configuring-business-process-security-policy-permissions"></a>Zakelijke proces beveiligingsmachtigingen beleid configureren

In deze stap maakt u moet hiervoor toestemming 'business-processen' beleid voor de worker-gegevens aan de beveiligingsgroep. Deze stap is vereist voor het instellen van de app-connector voor write-back van Workday.

**Zakelijke proces beveiligingsmachtigingen beleid configureren:**

1. Voer **proces bedrijfsbeleid** in het zoekvak en klik vervolgens op de koppeling **beveiligingsbeleid van de Business-proces bewerken** taak.  

    ![Business Process Security Policies](./media/workday-inbound-tutorial/wd_isu_12.png "Business Process Security Policies")  

2. In de **Business procestype** tekstvak Zoeken naar *Neem contact op met* en selecteer **contact op met de wijziging** zakelijke processen en klikt u op **OK**.

    ![Business Process Security Policies](./media/workday-inbound-tutorial/wd_isu_13.png "Business Process Security Policies")  

3. Op de **beveiligingsbeleid van de Business-proces bewerken** pagina, bladert u naar de **contactgegevens onderhouden (webservice)** sectie.

    ![Business Process Security Policies](./media/workday-inbound-tutorial/wd_isu_14.png "Business Process Security Policies")  

4. Selecteren en de nieuwe beveiligingsgroep voor de integratie van systeem toevoegen aan de lijst met beveiligingsgroepen die de aanvraag van de web-services kunnen initiëren. Klik op **gedaan**. 

    ![Business Process Security Policies](./media/workday-inbound-tutorial/wd_isu_15.png "Business Process Security Policies")  

### <a name="activating-security-policy-changes"></a>Wijzigingen in het beveiligingsbeleid activeren

**Wijzigingen in het beveiligingsbeleid activeren:**

1. Voer activeren in het zoekvak in en klik vervolgens op de koppeling **activeren in behandeling zijnde wijzigingen voor het domeinbeveiligingsbeleid**.

    ![Activeren](./media/workday-inbound-tutorial/wd_isu_16.png "activeren")

1. De taak activeren in behandeling zijnde wijzigingen voor het domeinbeveiligingsbeleid u eerst een opmerking invoeren voor controledoeleinden en klik vervolgens op **OK**.
1. De taak op het volgende scherm voltooien door het controleren van het selectievakje **bevestigen**, en klik vervolgens op **OK**.

    ![In afwachting van beveiliging activeren](./media/workday-inbound-tutorial/wd_isu_18.png "activeren in afwachting van beveiliging")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Gebruikersinrichting van Workday naar Active Directory configureren

Deze sectie bevat stappen voor het inrichten van gebruikersaccounts uit Workday aan elke Active Directory-domein binnen het bereik van uw integratie.

* [Installeren en configureren van on-premises inrichting agent (s)](#part-1-install-and-configure-on-premises-provisioning-agents)
* [De inrichting connector-app toevoegen en het maken van de verbinding met Workday](#part-2-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday)
* [Kenmerktoewijzingen configureren](#part-3-configure-attribute-mappings)
* [Inschakelen en starten met het inrichten van gebruikers](#enable-and-launch-user-provisioning)

### <a name="part-1-install-and-configure-on-premises-provisioning-agents"></a>Deel 1: Installeren en configureren van on-premises inrichting agent (s)

Om in te richten voor on-premises Active Directory, moet een agent worden geïnstalleerd op een server waarop .NET 4.7.1+ Framework en het netwerk toegang tot de gewenste Active Directory-domeinen.

> [!TIP]
> U kunt de versie van .NET framework controleren op de server met behulp van de instructies [hier](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed).
> Als de server heeft geen .NET 4.7.1 of hoger is geïnstalleerd, u dit ook via downloaden kunt [hier](https://support.microsoft.com/help/4033342/the-net-framework-4-7-1-offline-installer-for-windows).  

Wanneer u .NET 4.7.1+ hebt geïmplementeerd, kunt u downloaden de **[on-premises hier inrichtingsagent](https://go.microsoft.com/fwlink/?linkid=847801)** en volg de stappen hieronder om de agentconfiguratie te voltooien.

1. Meld u aan bij de Windows-Server waar u de nieuwe agent te installeren.
2. Start het installatieprogramma van Agent ingericht, ga akkoord met de voorwaarden en klik op de **installeren** knop.

   ![Installatie van scherm](./media/workday-inbound-tutorial/pa_install_screen_1.png "scherm installeren")
3. Nadat de installatie voltooid is, de wizard wordt geopend en u ziet de **verbinding maken met Azure AD** scherm. Klik op de **verifiëren** knop verbinding maken met uw Azure AD-exemplaar.

   ![Verbinding maken met Azure AD](./media/workday-inbound-tutorial/pa_install_screen_2.png "verbinding maken met Azure AD")
1. Worden geverifieerd bij uw Azure AD-exemplaar met behulp van de referenties van een globale beheerder.

   ![Beheerder Auth](./media/workday-inbound-tutorial/pa_install_screen_3.png "Admin Auth")

> [!NOTE]
> De referenties van de Azure AD-beheerder wordt alleen gebruikt om verbinding maken met uw Azure AD-tenant. De agent heeft niet de referenties opgeslagen lokaal op de server.

1. Na een succesvolle verificatie met Azure AD ziet u de **verbinding maken met Active Directory** scherm. In deze stap, voer de naam van uw AD-domein en klik op de **map toevoegen** knop.

   ![Map toevoegen](./media/workday-inbound-tutorial/pa_install_screen_4.png "map toevoegen")
  
1. U wordt nu gevraagd om in te voeren van de vereiste referenties voor het verbinding maken met het AD-domein. Op hetzelfde scherm, kunt u de **domain controller prioriteit selecteren** om op te geven van domeincontrollers die voor de agent wordt gebruikt voor het verzenden van aanvragen voor inrichting.

   ![Domeinreferenties](./media/workday-inbound-tutorial/pa_install_screen_5.png)
1. Na het configureren van het domein, geeft het installatieprogramma een lijst van geconfigureerde domeinen. In dit scherm kunt u herhaalt u stap #5 en 6 # om toe te voegen meer domeinen of klik op **volgende** wilt doorgaan met registratie van de agent.

   ![Domeinen geconfigureerd](./media/workday-inbound-tutorial/pa_install_screen_6.png "domeinen geconfigureerd")

   > [!NOTE]
   > Als u meerdere AD-domeinen (bijvoorbeeld na.contoso.com, emea.contoso.com) en voeg elk domein afzonderlijk toe aan de lijst. Alleen het toevoegen van het bovenliggende domein (bijvoorbeeld contoso.com) is niet voldoende. U moet elk onderliggend domein registreren met de agent.
1. Bekijk de informatie over de configuratie en klikt u op **bevestigen** om de agent te registreren.
  
   ![Scherm bevestigen](./media/workday-inbound-tutorial/pa_install_screen_7.png "scherm bevestigen")
1. De configuratiewizard geeft de voortgang van de registratie van de agent.
  
   ![Agentregistratie](./media/workday-inbound-tutorial/pa_install_screen_8.png "Agentregistratie")
1. Wanneer de agentregistratie gelukt is, kunt u klikken op **afsluiten** om de Wizard af te sluiten.
  
   ![Scherm afsluit](./media/workday-inbound-tutorial/pa_install_screen_9.png "scherm sluiten")
1. Controleer of de installatie van de Agent en zorg ervoor dat deze wordt uitgevoerd door de "Services"-module te openen en zoek naar de Service met de naam 'Microsoft Azure AD Connect Provisioning Agent'
  
   ![Services](./media/workday-inbound-tutorial/services.png)

### <a name="part-2-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Deel 2: De inrichting connector-app toevoegen en het maken van de verbinding met Workday

**Workday configureren voor het inrichten van Active Directory:**

1. Ga naar <https://portal.azure.com>

2. Selecteer in de linker navigatiebalk **Azure Active Directory**

3. Selecteer **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.

4. Selecteer **toevoegen van een toepassing**, en selecteer de **alle** categorie.

5. Zoeken naar **Workday Provisioning naar Active Directory**, en het toevoegen van de app uit de galerie.

6. Nadat de app is toegevoegd en het detailscherm app weergegeven, selecteert wordt **inrichten**

7. Wijzig de **inrichten** **modus** te **automatische**

8. Voltooi de **beheerdersreferenties** sectie als volgt:

   * **Admin Username** – Geef de gebruikersnaam van het systeemaccount van Workday-integratie met de naam van de tenant-domein toegevoegd. Het resultaat ziet er ongeveer als: **username@tenant_name**

   * **Beheerderswachtwoord –** voert u het wachtwoord van het systeem-account van de Workday-integratie

   * **Tenant-URL:** Voer de URL naar het Workday web services-eindpunt voor uw tenant. Deze waarde moet er als volgt uitzien: https://wd3-impl-services1.workday.com/ccx/service/contoso4, waarbij *contoso4* wordt vervangen door de juiste tenantnaam en *wd3 impl* wordt vervangen door de juiste omgeving-tekenreeks.

   * **Active Directory-Forest -** de 'naam' van uw Active Directory-domein, zoals die is geregistreerd met de agent. Gebruik de vervolgkeuzelijst te selecteren van het doeldomein voor het inrichten. Deze waarde is doorgaans een tekenreeks, zoals: *contoso.com*

   * **Active Directory-Container -** Voer de DN-naam van de container waarin de agent gebruikersaccounts standaard te maken.
        Voorbeeld: *OU=Standard Users,OU=Users,DC=contoso,DC=test*
> [!NOTE]
> Deze instelling komt alleen aan de orde voor gebruiker-account maken als de *parentDistinguishedName* kenmerk is niet geconfigureerd in de kenmerktoewijzingen. Deze instelling niet wordt gebruikt voor de gebruiker zoeken of update bewerkingen. De volledige-sub-domeinstructuur valt binnen het bereik van de search-bewerking.

   * **E-mailmelding –** Voer uw e-mailadres in en schakel het selectievakje 'e-mail verzenden als een fout optreedt' in.

> [!NOTE]
> De Azure AD-inrichtingsservice e-mailmelding verzonden als de taak een [quarantaine](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#quarantine) staat.

   * Klik op de **testverbinding** knop. Als de verbindingstest is geslaagd, klikt u op de **opslaan** bovenaan op de knop. Als dit mislukt, moet u controleren of de Workday-referenties en de AD-referenties die is geconfigureerd op de agent-instellingen geldig zijn.

     ![Azure Portal](./media/workday-inbound-tutorial/wd_1.png)

   * Zodra de referenties zijn opgeslagen, de **toewijzingen** sectie wordt weergegeven de standaardtoewijzing **Workday werknemers synchroniseren op lokale Active Directory**

### <a name="part-3-configure-attribute-mappings"></a>Deel 3: Kenmerktoewijzingen configureren

In deze sectie configureert u hoe gegevens stromen van Workday naar Active Directory.

1. Op het tabblad inrichten onder **toewijzingen**, klikt u op **Workday werknemers synchroniseren op lokale Active Directory**.

2. In de **bereik van het bronobject** veld, kunt u selecteren welke groepen gebruikers in Workday moet binnen het bereik voor het inrichten naar AD, bevat een reeks van filters op basis van een kenmerk. Het bereik van standaard is 'alle gebruikers in Workday'. Voorbeeld van de filters:

   * Voorbeeld: Bereik voor gebruikers met werknemer-id's tussen 1000000 en 2000000

      * Kenmerk: WorkerID

      * Operator: REGEX-overeenkomst

      * Waarde: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Voorbeeld: Alleen werknemers en niet voorwaardelijke werknemers

      * Kenmerk: Werknemer-id

      * Operator: IS NIET NULL

> [!TIP]
> Wanneer u de provisioning-app voor het eerst configureert, moet u testen en controleer of uw kenmerktoewijzingen en expressies om ervoor te zorgen dat deze is zodat u het gewenste resultaat. Microsoft raadt u aan met behulp van het bereik filters onder **bereik van het bronobject** voor het testen van uw toewijzingen met een paar testgebruikers van Workday. Nadat u hebt gecontroleerd dat de toewijzingen werken en vervolgens kunt u het filter verwijderen of geleidelijk uitvouwen om meer gebruikers te nemen.

3. In de **acties voor doelobject** veld wereldwijd kunt u filteren welke acties worden uitgevoerd op Active Directory. **Maak** en **Update** komen het meest.

4. In de **kenmerktoewijzingen** sectie, kunt u definiëren hoe afzonderlijke Workday kenmerken toewijzen aan Active Directory-kenmerken.

5. Klik op een bestaande kenmerktoewijzing bij te werken, of klik op **nieuwe toewijzing toevoegen** aan de onderkant van het scherm om toe te voegen nieuwe toewijzingen. Een afzonderlijke kenmerktoewijzing biedt ondersteuning voor deze eigenschappen:

      * **Toewijzingstype**

         * **Directe** : de waarde van het kenmerk Workday schrijft naar de AD-kenmerk, zonder wijzigingen

         * **Constante** -schrijven van een statisch, constante string-waarde naar het AD-kenmerk

         * **Expressie** – kunt u een aangepaste waarde schrijven naar de AD-kenmerk op basis van een of meer kenmerken van Workday. [Zie voor meer informatie in dit artikel voor expressies](../manage-apps/functions-for-customizing-application-data.md).

      * **Bronkenmerk** -het gebruikerskenmerk van de van Workday. Als het kenmerk dat u zoekt niet aanwezig is, raadpleegt u [aanpassen van de lijst met gebruikerskenmerken Workday](#customizing-the-list-of-workday-user-attributes).

      * **Standaardwaarde** -optioneel. Als het kenmerk van de gegevensbron een lege waarde heeft, wordt de toewijzing van deze waarde in plaats daarvan schrijven.
            De configuratie van de meest voorkomende is te laat dit veld leeg.

      * **Doelkenmerk** – het gebruikerskenmerk in Active Directory.

      * **Overeenkomen met de objecten met behulp van dit kenmerk** – ongeacht of deze toewijzing moet worden gebruikt voor het aanduiden van gebruikers tussen Workday en Active Directory of niet. Deze waarde wordt meestal ingesteld in het veld werknemer-ID voor Workday, die meestal is toegewezen aan een van de werknemer-ID-kenmerken in Active Directory.

      * **Prioriteit bij** : meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere, worden deze geëvalueerd in de volgorde die is gedefinieerd in dit veld. Zodra een overeenkomst wordt gevonden, geen verdere overeenkomende kenmerken worden geëvalueerd.

      * **Deze toewijzing toepassen**

         * **Altijd** : deze toewijzing toepassen op beide gebruiker maken en bijwerken van acties

         * **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op acties van het maken van de gebruiker

6. Om uw toewijzingen hebt opgeslagen, klikt u op **opslaan** aan de bovenkant van de kenmerktoewijzing sectie.

   ![Azure Portal](./media/workday-inbound-tutorial/wd_2.png)

#### <a name="below-are-some-example-attribute-mappings-between-workday-and-active-directory-with-some-common-expressions"></a>Hieronder worden enkele voorbeeld kenmerktoewijzingen tussen Workday en Active Directory, met enkele algemene expressies

* De expressie die is toegewezen aan de *parentDistinguishedName* kenmerk wordt gebruikt voor het inrichten van een gebruiker aan verschillende organisatie-eenheden op basis van een of meer Workday bronkenmerken. Hier in dit voorbeeld wordt gebruikers in verschillende organisatie-eenheden die zijn gebaseerd op welke plaats ze zich in.

* De *userPrincipalName* kenmerk in Active Directory wordt gegenereerd op basis van de functie deduplicatie [SelectUniqueValue](../manage-apps/functions-for-customizing-application-data.md#selectuniquevalue) die controleert of de aanwezigheid van een gegenereerde waarde in het doeldomein AD en alleen Hiermee stelt u deze als deze uniek is.  

* [Er wordt documentatie over het schrijven van expressies hier](../manage-apps/functions-for-customizing-application-data.md). In deze sectie bevat voorbeelden over het verwijderen van speciale tekens.

| WORKDAY KENMERK | ACTIVE DIRECTORY-KENMERK |  OVEREENKOMENDE ID? | MAKEN / BIJWERKEN |
| ---------- | ---------- | ---------- | ---------- |
| **WorkerID**  |  Werknemer-id | **Ja** | Geschreven alleen bij het maken |
| **PreferredNameData**    |  algemene naam    |   |   Geschreven alleen bij het maken |
| **SelectUniqueValue (deelnemen aan ("@", deelnemen aan (".", \[FirstName\], \[LastName\]), 'contoso.com'), deelnemen aan ("@", deelnemen aan (".", Mid (\[FirstName\], 1, 1), \[LastName\]), 'contoso.com'), Join (' @ ', Join ('. ', Mid (\[FirstName\], 1, 2), \[LastName\]), 'contoso.com'))**   | userPrincipalName     |     | Geschreven alleen bij het maken 
| **Vervang(Mid(Vervang(\[UserID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**      |    sAMAccountName            |     |         Geschreven alleen bij het maken |
| **Switch (\[Active\],, "0", "True", "1", "Onwaar")** |  accountDisabled      |     | Maken en bijwerken |
| **Voornaam**   | givenName       |     |    Maken en bijwerken |
| **LastName**   |   SN   |     |  Maken en bijwerken |
| **PreferredNameData**  |  displayName |     |   Maken en bijwerken |
| **Bedrijf**         | Bedrijf   |     |  Maken en bijwerken |
| **SupervisoryOrganization**  | department  |     |  Maken en bijwerken |
| **ManagerReference**   | beheerder  |     |  Maken en bijwerken |
| **BusinessTitle**   |  titel     |     |  Maken en bijwerken | 
| **AddressLineData**    |  streetAddress  |     |   Maken en bijwerken |
| **Gemeenteraad**   |   l   |     | Maken en bijwerken |
| **CountryReferenceTwoLetter**      |   CO |     |   Maken en bijwerken |
| **CountryReferenceTwoLetter**    |  c  |     |         Maken en bijwerken |
| **CountryRegionReference** |  St     |     | Maken en bijwerken |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Maken en bijwerken |
| **PostalCode**  |   Postcode  |     | Maken en bijwerken |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Maken en bijwerken |
| **Fax**      | facsimileTelephoneNumber     |     |    Maken en bijwerken |
| **Mobile**  |    mobiele       |     |       Maken en bijwerken |
| **LocalReference** |  preferredLanguage  |     |  Maken en bijwerken |                                               
| **Switch (\[gemeenteraad\], "organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid = standaard, organisatie-eenheid locaties, DC = = contoso, DC = com", 'Rotterdam', ' organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid Dallas, OU = locaties, DC = = contoso, DC = com ","Austin"," organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid Austin, OU = locaties, DC = = contoso, DC = com ","Seattle"," organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid Seattle, OU = locaties, DC = = contoso, DC = com ","Londen"," organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid Londen, OU = locaties, DC = = contoso, DC = com ")**  | parentDistinguishedName     |     |  Maken en bijwerken |

Zodra de configuratie van het kenmerk toewijzing voltooid is, kunt u nu [inschakelen en starten van de gebruiker inrichtingsservice](#enable-and-launch-user-provisioning).

## <a name="configuring-user-provisioning-to-azure-ad"></a>Configuratie van gebruikers inrichten met Azure AD

De volgende secties worden de stappen beschreven voor het configureren van gebruikersinrichting van Workday naar Azure AD voor alleen-implementaties.

* [De Azure AD-app voor inrichting connector toevoegen en het maken van de verbinding met Workday](#part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday)
* [Kenmerktoewijzingen Workday en Azure AD configureren](#part-2-configure-workday-and-azure-ad-attribute-mappings)
* [Inschakelen en starten met het inrichten van gebruikers](#enable-and-launch-user-provisioning)

> [!IMPORTANT]
> Volg de onderstaande procedure alleen als u alleen in de cloud-gebruikers die moeten worden ingericht met Azure AD hebben en niet on-premises Active Directory.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Deel 1: De Azure AD-app voor inrichting connector toevoegen en het maken van de verbinding met Workday

**Workday configureren voor het inrichten van Azure Active Directory voor gebruikers alleen in de cloud:**

1. Ga naar <https://portal.azure.com>.

2. Selecteer in de linker navigatiebalk **Azure Active Directory**

3. Selecteer **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.

4. Selecteer **toevoegen van een toepassing**, en selecteer vervolgens de **alle** categorie.

5. Zoeken naar **Workday voor het inrichten van Azure AD**, en het toevoegen van de app uit de galerie.

6. Nadat de app is toegevoegd en het detailscherm app weergegeven, selecteert wordt **inrichten**

7. Wijzig de **inrichten** **modus** te **automatische**

8. Voltooi de **beheerdersreferenties** sectie als volgt:

   * **Admin Username** – Geef de gebruikersnaam van het systeemaccount van Workday-integratie met de naam van de tenant-domein toegevoegd. Ziet er ongeveer als volgt: username@contoso4

   * **Beheerderswachtwoord –** voert u het wachtwoord van het systeem-account van de Workday-integratie

   * **Tenant-URL:** Voer de URL naar het Workday web services-eindpunt voor uw tenant. Deze waarde moet er als volgt uitzien: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, waarbij *contoso4* wordt vervangen door de juiste tenantnaam en *wd3 impl* wordt vervangen door de juiste omgeving-tekenreeks. Als deze URL niet bekend is, neem contact op met uw Workday-integratie-partner of ondersteuning-vertegenwoordiger om te bepalen van de juiste URL te gebruiken.

   * **E-mailmelding –** Voer uw e-mailadres in en schakel het selectievakje 'e-mail verzenden als een fout optreedt' in.

   * Klik op de **testverbinding** knop.

   * Als de verbindingstest is geslaagd, klikt u op de **opslaan** bovenaan op de knop. Als dit mislukt, moet u controleren dat de URL voor Workday en referenties geldig in Workday zijn.

### <a name="part-2-configure-workday-and-azure-ad-attribute-mappings"></a>Deel 2: Kenmerktoewijzingen Workday en Azure AD configureren

In deze sectie configureert u hoe de gebruikersgegevens wordt stromen van Workday naar Azure Active Directory voor cloudgebruikers.

1. Op het tabblad inrichten onder **toewijzingen**, klikt u op **werknemers synchroniseren naar Azure AD**.

2. In de **bereik van het bronobject** veld, kunt u selecteren welke groepen gebruikers in Workday moet binnen het bereik voor levering aan Azure AD bevat een reeks van filters op basis van een kenmerk. Het bereik van standaard is 'alle gebruikers in Workday'. Voorbeeld van de filters:

   * Voorbeeld: Bereik voor gebruikers met werknemer-id's tussen 1000000 en 2000000

      * Kenmerk: WorkerID

      * Operator: REGEX-overeenkomst

      * Waarde: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Voorbeeld: Alleen werknemers voorwaardelijke en niet regelmatig werknemers

      * Kenmerk: ContingentID

      * Operator: IS NIET NULL

3. In de **acties voor doelobject** veld wereldwijd kunt u filteren welke acties worden uitgevoerd op Azure AD. **Maak** en **Update** komen het meest.

4. In de **kenmerktoewijzingen** sectie, kunt u definiëren hoe afzonderlijke Workday kenmerken toewijzen aan Active Directory-kenmerken.

5. Klik op een bestaande kenmerktoewijzing bij te werken, of klik op **nieuwe toewijzing toevoegen** aan de onderkant van het scherm om toe te voegen nieuwe toewijzingen. Een afzonderlijke kenmerktoewijzing biedt ondersteuning voor deze eigenschappen:

   * **Toewijzingstype**

      * **Directe** : de waarde van het kenmerk Workday schrijft naar de AD-kenmerk, zonder wijzigingen

      * **Constante** -schrijven van een statisch, constante string-waarde naar het AD-kenmerk

      * **Expressie** – kunt u een aangepaste waarde schrijven naar de AD-kenmerk op basis van een of meer kenmerken van Workday. [Zie voor meer informatie in dit artikel voor expressies](../manage-apps/functions-for-customizing-application-data.md).

   * **Bronkenmerk** -het gebruikerskenmerk van de van Workday. Als het kenmerk dat u zoekt niet aanwezig is, raadpleegt u [aanpassen van de lijst met gebruikerskenmerken Workday](#customizing-the-list-of-workday-user-attributes).

   * **Standaardwaarde** -optioneel. Als het kenmerk van de gegevensbron een lege waarde heeft, wordt de toewijzing van deze waarde in plaats daarvan schrijven.
            De configuratie van de meest voorkomende is te laat dit veld leeg.

   * **Doelkenmerk** – het gebruikerskenmerk in Azure AD.

   * **Overeenkomen met de objecten met behulp van dit kenmerk** – ongeacht of dit kenmerk moet worden gebruikt voor het aanduiden van gebruikers tussen Workday en Azure AD of niet. Deze waarde wordt meestal ingesteld in het veld werknemer-ID voor Workday, die meestal is toegewezen aan de werknemer-ID-kenmerk (nieuw) of een extensiekenmerk in Azure AD.

   * **Prioriteit bij** : meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere, worden deze geëvalueerd in de volgorde die is gedefinieerd in dit veld. Zodra een overeenkomst wordt gevonden, geen verdere overeenkomende kenmerken worden geëvalueerd.

   * **Deze toewijzing toepassen**

     * **Altijd** : deze toewijzing toepassen op beide gebruiker maken en bijwerken van acties

     * **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op acties van het maken van de gebruiker

6. Om uw toewijzingen hebt opgeslagen, klikt u op **opslaan** aan de bovenkant van de kenmerktoewijzing sectie.

Zodra de configuratie van het kenmerk toewijzing voltooid is, kunt u nu [inschakelen en starten van de gebruiker inrichtingsservice](#enable-and-launch-user-provisioning).

## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Write-back van e-mailadressen naar Workday configureren

Volg deze instructies voor het configureren van Write-back van gebruiker e-mailadressen uit Azure Active Directory naar Workday.

* [De Write-back-connector-app toevoegen en het maken van de verbinding met Workday](#part-1-adding-the-writeback-connector-app-and-creating-the-connection-to-workday)
* [Write-back-kenmerktoewijzingen configureren](#part-2-configure-writeback-attribute-mappings)
* [Inschakelen en starten met het inrichten van gebruikers](#enable-and-launch-user-provisioning)

### <a name="part-1-adding-the-writeback-connector-app-and-creating-the-connection-to-workday"></a>Deel 1: De Write-back-connector-app toevoegen en het maken van de verbinding met Workday

**Het configureren van Write-back van Workday-connector:**

1. Ga naar <https://portal.azure.com>

2. Selecteer in de linker navigatiebalk **Azure Active Directory**

3. Selecteer **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.

4. Selecteer **toevoegen van een toepassing**en selecteer vervolgens de **alle** categorie.

5. Zoeken naar **Write-back van Workday**, en het toevoegen van de app uit de galerie.

6. Nadat de app is toegevoegd en het detailscherm app weergegeven, selecteert wordt **inrichten**

7. Wijzig de **inrichten** **modus** te **automatische**

8. Voltooi de **beheerdersreferenties** sectie als volgt:

   * **Admin Username** – Geef de gebruikersnaam van het systeemaccount van Workday-integratie met de naam van de tenant-domein toegevoegd. Ziet er ongeveer als volgt: *username@contoso4*

   * **Beheerderswachtwoord –** voert u het wachtwoord van het systeem-account van de Workday-integratie

   * **Tenant-URL:** Voer de URL naar het Workday web services-eindpunt voor uw tenant. Deze waarde moet er als volgt uitzien: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, waarbij *contoso4* wordt vervangen door de juiste tenantnaam en *wd3 impl* wordt vervangen door de juiste omgeving-tekenreeks (indien nodig).

   * **E-mailmelding –** Voer uw e-mailadres in en schakel het selectievakje 'e-mail verzenden als een fout optreedt' in.

   * Klik op de **testverbinding** knop. Als de verbindingstest is geslaagd, klikt u op de **opslaan** bovenaan op de knop. Als dit mislukt, moet u controleren dat de URL voor Workday en referenties geldig in Workday zijn.

### <a name="part-2-configure-writeback-attribute-mappings"></a>Deel 2: Write-back-kenmerktoewijzingen configureren

In deze sectie configureert u hoe de Write-back van kenmerken wordt overgebracht van Azure AD naar Workday.

1. Op het tabblad inrichten onder **toewijzingen**, klikt u op **synchroniseren Azure Active Directory: gebruikers naar Workday**.

2. In de **bereik van het bronobject** veld, kunt u desgewenst filteren, welke groepen gebruikers in Azure Active Directory moeten hebben hun e-mailadressen teruggeschreven naar Workday. Het bereik van standaard is 'alle gebruikers in Azure AD'.

3. In de **kenmerktoewijzingen** sectie, bij te werken van de overeenkomende ID om aan te geven het kenmerk in Azure Active Directory waar het Workday werknemer-ID of de werknemer-ID wordt opgeslagen. Een populaire overeenkomende methode is om de Workday werknemer-ID of de werknemer-ID aan extensionAttribute1 15 in Azure AD synchroniseren en vervolgens dit kenmerk in Azure AD gebruiken om gebruikers te vergelijken vorige in Workday.

4. Om uw toewijzingen hebt opgeslagen, klikt u op **opslaan** aan de bovenkant van de kenmerktoewijzing sectie.

Zodra de configuratie van het kenmerk toewijzing voltooid is, kunt u nu [inschakelen en starten van de gebruiker inrichtingsservice](#enable-and-launch-user-provisioning). 

## <a name="enable-and-launch-user-provisioning"></a>Inschakelen en starten met het inrichten van gebruikers

Zodra de inrichting app-configuraties van Workday zijn voltooid, kunt u de inrichtingsservice in Azure portal kunt inschakelen.

> [!TIP]
> Standaard als u de provisioning-service inschakelt, zal het starten inrichting bewerkingen voor alle gebruikers in het bereik. Als er fouten in de problemen met de gegevens koppelen of Workday, kan de inrichtingstaak van de mislukken en gaat u naar de status van de in quarantaine plaatsen. Om dit te voorkomen, als een best practice, wordt aangeraden configureren **bereik van het bronobject** filteren en testen van uw kenmerktoewijzingen met een paar testgebruikers voordat het starten van de volledige synchronisatie uit voor alle gebruikers. Nadat u hebt gecontroleerd dat de toewijzingen werken en geeft u de gewenste resultaten oplevert, kunt u het filter verwijderen of geleidelijk uitvouwen om meer gebruikers te nemen.

1. In de **Provisioning** tabblad, stelt u de **Inrichtingsstatus** naar **op**.

2. Klik op **Opslaan**.

3. Met deze bewerking wordt de eerste synchronisatie, wat een variabele aantal uren, afhankelijk van hoeveel gebruikers in de tenant Workday zijn kan duren gestart. 

4. Controleer op elk gewenst moment de **auditlogboeken** tabblad in de Azure portal om te zien welke acties de provisioning-service heeft uitgevoerd. De auditlogboeken worden alle afzonderlijke synchronisatie gebeurtenissen die worden uitgevoerd door de provisioning-service, zoals welke gebruikers worden gelezen uit Workday en vervolgens later toegevoegd of bijgewerkt naar Active Directory. Raadpleeg de sectie over probleemoplossing voor instructies over het bekijken van de auditlogboeken en inrichting fouten te corrigeren.

5. Zodra de initiële synchronisatie is voltooid, wordt er een overzichtsrapport van de audit schrijven in de **Provisioning** tabblad, zoals hieronder wordt weergegeven.

   ![Azure Portal](./media/workday-inbound-tutorial/wd_3.png)

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

* **Oplossing mogelijkheid vragen**
  * [Bij het verwerken van een nieuwe medewerkers van Workday, hoe de oplossing stelt het wachtwoord voor het nieuwe gebruikersaccount in Active Directory?](#when-processing-a-new-hire-from-workday-how-does-the-solution-set-the-password-for-the-new-user-account-in-active-directory)
  * [Ondersteunt de oplossing verzenden e-mailmeldingen na het inrichten van bewerkingen voltooid?](#does-the-solution-support-sending-email-notifications-after-provisioning-operations-complete)
  * [Hoe kan ik levering van wachtwoorden voor nieuwe medewerkers beheren en veilig bieden een mechanisme om hun wachtwoord opnieuw instellen?](#how-do-i-manage-delivery-of-passwords-for-new-hires-and-securely-provide-a-mechanism-to-reset-their-password)
  * [De oplossing in cache, Workday gebruikersprofielen in de Azure AD-cloud of op het niveau van de inrichting agent?](#does-the-solution-cache-workday-user-profiles-in-the-azure-ad-cloud-or-at-the-provisioning-agent-layer)
  * [De oplossing ondersteuning toe te wijzen op lokale AD-groepen aan de gebruiker?](#does-the-solution-support-assigning-on-premises-ad-groups-to-the-user)
  * [Welke Workday-API's wordt de oplossing gebruikt voor query- en Workday worker profielen?](#which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles)
  * [Kan ik mijn tenant Workday HCM configureren met twee Azure AD-tenants?](#can-i-configure-my-workday-hcm-tenant-with-two-azure-ad-tenants)
  * [Waarom 'Workday naar Azure AD' gebruikersinrichting app wordt niet ondersteund als we Azure AD Connect hebt geïmplementeerd?](#why-workday-to-azure-ad-user-provisioning-app-is-not-supported-if-we-have-deployed-azure-ad-connect)
  * [Hoe ik voorstellen verbeteringen of nieuwe functies met betrekking tot Workday en Azure AD-integratie aanvragen?](#how-do-i-suggest-improvements-or-request-new-features-related-to-workday-and-azure-ad-integration)

* **Inrichting Agent vragen**
  * [Wat is de GA-versie van de Agent inrichten?](#what-is-the-ga-version-of-the-provisioning-agent)
  * [Hoe weet ik de versie van mijn Agent inrichten?](#how-do-i-know-the-version-of-my-provisioning-agent)
  * [Microsoft automatisch inrichten agentupdates pushen?](#does-microsoft-automatically-push-provisioning-agent-updates)
  * [Kan ik de Provisioning-Agent installeren op dezelfde server met AAD Connect?](#can-i-install-the-provisioning-agent-on-the-same-server-running-aad-connect)
  * [Hoe kan ik de Agent wordt ingericht met een proxy-server voor uitgaande HTTP-communicatie configureren?](#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication)
  * [Hoe kan ik ervoor zorgen dat de Agent ingericht kan communiceren met de Azure AD-tenant en geen firewalls zijn door de agent vereiste poorten blokkeren?](#how-do-i-ensure-that-the-provisioning-agent-is-able-to-communicate-with-the-azure-ad-tenant-and-no-firewalls-are-blocking-ports-required-by-the-agent)
  * [Hoe kan ik het domein dat is gekoppeld aan mijn Agent inrichting ongedaan maken registreren?](#how-do-i-de-register-the-domain-associated-with-my-provisioning-agent)
  * [Hoe kan ik de Provisioning-Agent verwijderen?](#how-do-i-uninstall-the-provisioning-agent)
  
* **WorkDay op AD-kenmerk toewijzing en configuratie-vragen**
  * [Hoe kan ik een back-up of een werkende kopie van mijn Workday inrichting kenmerk toewijzing en het Schema exporteren?](#how-do-i-back-up-or-export-a-working-copy-of-my-workday-provisioning-attribute-mapping-and-schema)
  * [Ik heb aangepaste kenmerken in Workday en Active Directory. Hoe kan ik de oplossing voor het werken met mijn aangepaste kenmerken configureren?](#i-have-custom-attributes-in-workday-and-active-directory-how-do-i-configure-the-solution-to-work-with-my-custom-attributes)
  * [Kan ik inrichten van de gebruiker foto's van Workday naar Active Directory?](#can-i-provision-users-photo-from-workday-to-active-directory)
  * [Hoe synchroniseer ik mobiele nummers van Workday op basis van de gebruikerstoestemming voor het gebruik van de openbare?](#how-do-i-sync-mobile-numbers-from-workday-based-on-user-consent-for-public-usage)
  * [Hoe maak ik weergavenamen in AD op basis van de kenmerken van de afdeling/land/plaats en regionale verschillen ingang van de gebruiker?](#how-do-i-format-display-names-in-ad-based-on-the-users-departmentcountrycity-attributes-and-handle-regional-variances)
  * [Hoe kan ik SelectUniqueValue gebruiken voor het genereren van unieke waarden voor het kenmerk samAccountName?](#how-can-i-use-selectuniquevalue-to-generate-unique-values-for-samaccountname-attribute)
  * [Hoe ik tekens met diakritische tekens te verwijderen en deze omzetten in normale Engelse letters?](#how-do-i-remove-characters-with-diacritics-and-convert-them-into-normal-english-alphabets)

### <a name="solution-capability-questions"></a>Oplossing mogelijkheid vragen

#### <a name="when-processing-a-new-hire-from-workday-how-does-the-solution-set-the-password-for-the-new-user-account-in-active-directory"></a>Bij het verwerken van een nieuwe medewerkers van Workday, hoe de oplossing stelt het wachtwoord voor het nieuwe gebruikersaccount in Active Directory?

De on-premises inrichtingsagent een aanvraag voor het wordt maken van een nieuwe AD-account wordt automatisch genereert een willekeurige complex wachtwoord in die is ontworpen om te voldoen aan de vereisten voor wachtwoordcomplexiteit gedefinieerd door de server van AD en stelt dit op het gebruikersobject. Dit wachtwoord wordt niet overal geregistreerd.

#### <a name="does-the-solution-support-sending-email-notifications-after-provisioning-operations-complete"></a>Ondersteunt de oplossing verzenden e-mailmeldingen na het inrichten van bewerkingen voltooid?

Nee, e-mailmeldingen verzenden nadat het inrichtingsproces bewerkingen voltooien wordt niet ondersteund in de huidige release.

#### <a name="how-do-i-manage-delivery-of-passwords-for-new-hires-and-securely-provide-a-mechanism-to-reset-their-password"></a>Hoe kan ik levering van wachtwoorden voor nieuwe medewerkers beheren en veilig bieden een mechanisme om hun wachtwoord opnieuw instellen?

Een van de laatste stappen voor het inrichten van nieuwe AD account is de levering van het tijdelijke wachtwoord toegewezen aan AD-account van de gebruiker. Veel bedrijven nog steeds gebruiken de traditionele aanpak van het tijdelijke wachtwoord bij de manager van de gebruiker, die vervolgens via het wachtwoord voor de nieuwe medewerkers/voorwaardelijke werknemer handen af te leveren. Dit proces is een beveiligingsfout in de inherente en er is een optie die beschikbaar zijn voor het implementeren van een betere benadering met behulp van Azure AD-mogelijkheden.

Als onderdeel van het aannemen van personeel proces, HR-teams meestal een achtergrondcontrole uitvoeren en het mobiele telefoonnummer van de nieuwe medewerkers, screenen. Met de werkdag tot AD-gebruiker Provisioning-integratie, kunt u boven op dit feit bouwen en implementatie een self-service voor wachtwoord opnieuw instellen van de voor de gebruiker op dag 1. Dit wordt bereikt door het kenmerk 'Mobiel nummer' van de nieuwe medewerkers van Workday naar AD doorgeven en vervolgens vanaf AD met Azure AD met behulp van AAD Connect. Nadat het getal' mobiel' in Azure AD aanwezig is, kunt u inschakelen de [selfservice wachtwoord opnieuw instellen (SSPR)](../authentication/howto-sspr-authenticationdata.md) voor het gebruikersaccount, dus die op dag 1, een nieuwe medewerkers kunt gebruiken het geregistreerde en geverifieerd mobiele telefoonnummer voor verificatie.

#### <a name="does-the-solution-cache-workday-user-profiles-in-the-azure-ad-cloud-or-at-the-provisioning-agent-layer"></a>De oplossing in cache, Workday gebruikersprofielen in de Azure AD-cloud of op het niveau van de inrichting agent?

De oplossing houdt Nee, niet een cache van gebruikersprofielen. De Azure AD-inrichtingsservice fungeert gewoon als een processor van gegevens, gegevens uit Workday lezen en schrijven naar het doel Active Directory of Azure AD. Zie de sectie [persoonlijke gegevens beheren](#managing-personal-data) voor meer informatie met betrekking tot privacy- en gebruikersretentie.

#### <a name="does-the-solution-support-assigning-on-premises-ad-groups-to-the-user"></a>De oplossing ondersteuning toe te wijzen op lokale AD-groepen aan de gebruiker?

Deze functionaliteit is momenteel niet ondersteund. Aanbevolen tijdelijke oplossing is het implementeren van een PowerShell-script die in de Azure AD Graph API-eindpunt voor gegevens van een audit zoekt en gebruiken die voor het activeren van scenario's zoals groepstoewijzing. Dit PowerShell-script kan worden aangesloten op een Taakplanner en geïmplementeerd op dezelfde box waarop de inrichting agent wordt uitgevoerd.  

#### <a name="which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles"></a>Welke Workday-API's wordt de oplossing gebruikt voor query- en Workday worker profielen?

De oplossing gebruikt momenteel de volgende werkdag-API's:

* Get_Workers (v21.1) voor het ophalen van gegevens over werknemer
* Maintain_Contact_Information (v26.1) voor de functie werk e Write-back

#### <a name="can-i-configure-my-workday-hcm-tenant-with-two-azure-ad-tenants"></a>Kan ik mijn tenant Workday HCM configureren met twee Azure AD-tenants?

Ja, deze configuratie wordt ondersteund. Hier volgen de stappen op hoog niveau voor het configureren van dit scenario:

* Inrichtingsagent #1 implementeren en registreren bij Azure AD-tenant #1.
* Inrichtingsagent #2 implementeren en registreren bij Azure AD-tenant #2.
* Op basis van de 'onderliggende domeinen' waarmee elke Agent ingericht zullen worden beheerd, elke agent configureren met de domeinen. Een agent kan meerdere domeinen worden verwerkt.
* Instellen van de werkdag in AD-gebruiker Provisioning-App in elke tenant en configureer dit met de domeinen in Azure-portal.

#### <a name="why-workday-to-azure-ad-user-provisioning-app-is-not-supported-if-we-have-deployed-azure-ad-connect"></a>Waarom 'Workday naar Azure AD' gebruikersinrichting app wordt niet ondersteund als we Azure AD Connect hebt geïmplementeerd?

Wanneer Azure AD wordt gebruikt in de hybride-modus (wanneer deze een combinatie van de cloud bevat + on-premises gebruikers), is het belangrijk dat u hebt een duidelijke definitie van 'autoriteitsbron'. Implementatie van Azure AD Connect is doorgaans hybride scenario's vereist. Wanneer Azure AD Connect wordt geïmplementeerd, on-premises AD is de bron van de instantie. Maak kennis met de Workday naar Azure AD-connector in de combinatie kan leiden tot een situatie waarbij Workday kenmerkwaarden mogelijk de waarden die door Azure AD Connect kunnen overschrijven. Gebruik van 'Workday naar Azure AD' provisioning app is daarom niet ondersteund wanneer Azure AD Connect is ingeschakeld. In dergelijke situaties wordt u aangeraden "Workday aan AD-gebruiker' inrichting van de app voor het ophalen van gebruikers in on-premises AD en ze vervolgens in Azure AD met Azure AD Connect synchroniseert.

#### <a name="how-do-i-suggest-improvements-or-request-new-features-related-to-workday-and-azure-ad-integration"></a>Hoe ik voorstellen verbeteringen of nieuwe functies met betrekking tot Workday en Azure AD-integratie aanvragen?

Uw feedback wordt zeer gewaardeerd als het helpt ons de richting instellen voor de toekomstige releases en verbeteringen. We stellen alle feedback en raden u aan het indienen van uw suggestie idee of verbetering in de [forum met feedback van Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory). Selecteer de categorie voor specifieke feedback die betrekking hebben op de Workday-integratie, *SaaS-toepassingen* en zoeken met trefwoorden *Workday* te vinden van bestaande feedback die betrekking hebben op de werkdag.

![UserVoice SaaS Apps](media/workday-inbound-tutorial/uservoice_saas_apps.png)

![UserVoice Workday](media/workday-inbound-tutorial/uservoice_workday_feedback.png)

Wanneer een nieuwe idee voorstellen, controleer dan om te zien als iemand anders een vergelijkbare functie al is voorgesteld. In dat geval kunt u de functie of uitbreiding aanvraag maximaal stemmen. U kunt ook een opmerking met betrekking tot uw specifieke situatie voor het weergeven van het ondersteuningsteam van het idee en laten zien hoe het onderdeel is niet nuttig voor u te laten.

### <a name="provisioning-agent-questions"></a>Inrichting Agent vragen

#### <a name="what-is-the-ga-version-of-the-provisioning-agent"></a>Wat is de GA-versie van de Agent inrichten?

* De GA-versie van de Agent ingericht 1.1.30 is en hoger.
* Als uw versie van de agent minder dan 1.1.30 is, de openbare preview-versie wordt uitgevoerd en deze automatisch naar de GA-versie bijgewerkt wordt als de server die als host fungeert voor de agent .NET 4.7.1 heeft runtime.
  * U kunt [controleren van de .NET-versie](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) op uw server geïnstalleerd. Als de server wordt niet uitgevoerd voor .NET 4.7.1, kunt u [downloadt en installeert u .NET 4.7.1](https://support.microsoft.com/help/4033342/the-net-framework-4-7-1-offline-installer-for-windows). Uw provisioning-agent wordt automatisch worden bijgewerkt naar de GA-versie nadat u .NET 4.7.1 hebt geïnstalleerd.

#### <a name="how-do-i-know-the-version-of-my-provisioning-agent"></a>Hoe weet ik de versie van mijn Agent inrichten?

* Meld u aan bij de Windows-server waarop de Provisioning-Agent is geïnstalleerd.
* Ga naar **Configuratiescherm** -> **verwijderen of wijzigen van een programma** menu
* Zoekt u de versie die overeenkomt met de vermelding **Microsoft Azure AD Connect inrichten van Agent**

  ![Azure Portal](./media/workday-inbound-tutorial/pa_version.png)

#### <a name="does-microsoft-automatically-push-provisioning-agent-updates"></a>Microsoft automatisch inrichten agentupdates pushen?

Ja, de Microsoft de inrichtingsagent automatisch bijgewerkt. U kunt automatische updates uitschakelen door de Windows-service te stoppen **Microsoft Azure AD Connect-Agent Updater**.

#### <a name="can-i-install-the-provisioning-agent-on-the-same-server-running-aad-connect"></a>Kan ik de Provisioning-Agent installeren op dezelfde server met AAD Connect?

Ja, kunt u de inrichting-Agent installeren op dezelfde server die wordt uitgevoerd met AAD Connect.

#### <a name="at-the-time-of-configuration-the-provisioning-agent-prompts-for-azure-ad-admin-credentials-does-the-agent-store-the-credentials-locally-on-the-server"></a>Op het moment van de configuratie van de Agent ingericht gevraagd om referenties voor Azure AD-beheerder. De Agent slaat de referenties lokaal op de server?

Tijdens het configureren van de Agent ingericht gevraagd om referenties voor Azure AD-beheerder alleen voor de verbinding met uw Azure AD-tenant. Deze worden de referenties niet lokaal opgeslagen op de server. Maar behoudt de referenties waarmee verbinding maken met de *on-premises Active Directory-domein* in een lokale Windows-wachtwoord kluis.

#### <a name="how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication"></a>Hoe kan ik de Agent wordt ingericht met een proxy-server voor uitgaande HTTP-communicatie configureren?

De Agent ingericht ondersteunt het gebruik van de uitgaande proxy. U kunt deze configureren met het bewerken van het configuratiebestand van de agent **C:\Program Files\Microsoft Azure AD Connect inrichting Agent\AADConnectProvisioningAgent.exe.config**. Voeg de volgende regels in deze aan het einde van het bestand net voordat de afsluiting `</configuration>` tag.
Vervang de variabelen [proxyserver] en [proxypoort] met de naam van uw proxy-server en de poort van de waarden.

```xml
    <system.net>
          <defaultProxy enabled="true" useDefaultCredentials="true">
             <proxy
                usesystemdefault="true"
                proxyaddress="http://[proxy-server]:[proxy-port]"
                bypassonlocal="true"
             />
         </defaultProxy>
    </system.net>
```

#### <a name="how-do-i-ensure-that-the-provisioning-agent-is-able-to-communicate-with-the-azure-ad-tenant-and-no-firewalls-are-blocking-ports-required-by-the-agent"></a>Hoe kan ik ervoor zorgen dat de Agent ingericht kan communiceren met de Azure AD-tenant en geen firewalls zijn door de agent vereiste poorten blokkeren?

U kunt ook controleren of u beschikt over alle vereiste poorten door het openen van openen de [verbindingslijn poorten Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) vanaf uw on-premises netwerk. Meer een groen vinkje betekent meer flexibiliteit.

Als u wilt controleren of dat het hulpprogramma biedt u de juiste resultaten, moet u:

* Open het hulpprogramma op een browser van de server waar u de inrichting-Agent hebt geïnstalleerd.
* Zorg ervoor dat alle proxy's of firewalls van toepassing zijn op uw Provisioning-Agent ook worden toegepast op deze pagina. Dit kan in Internet Explorer worden gedaan door te gaan naar **instellingen -> Internet-opties -> verbindingen Lan-instellingen ->**. Op deze pagina ziet u het veld 'Met een Proxy-Server voor uw LAN'. Schakel dit selectievakje in en plaats het proxyadres in het veld "Adres".

#### <a name="can-one-provisioning-agent-be-configured-to-provision-multiple-ad-domains"></a>Kan één inrichting Agent worden geconfigureerd voor het inrichten van meerdere AD-domeinen?

Ja, een door de inrichting Agent kan worden geconfigureerd voor het afhandelen van meerdere AD-domeinen, zolang de agent peeren naar de respectieve domeincontrollers heeft. Microsoft raadt aan het instellen van een groep van 3 inrichting van agents die dezelfde set AD-domeinen om te waarborgen hoge beschikbaarheid en ondersteuning bieden voor mislukken.

#### <a name="how-do-i-de-register-the-domain-associated-with-my-provisioning-agent"></a>Hoe kan ik het domein dat is gekoppeld aan mijn Agent inrichting ongedaan maken registreren?

* Ophalen van de Azure-portal de *tenant-ID* van uw Azure AD-tenant.
* Meld u aan bij de Windows-server waarop de Agent inrichting wordt uitgevoerd.
* Open powershell als beheerder van Windows.
* Ga naar de map met de registratie-scripts en voer de volgende opdrachten vervangen de \[tenant-ID\] parameter met de waarde van uw tenant-ID.

  ```powershell
  cd “C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\RegistrationPowershell\Modules\PSModulesFolder”
  Import-Module "C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\RegistrationPowershell\Modules\PSModulesFolder\AppProxyPSModule.psd1"
  Get-PublishedResources -TenantId "[tenant ID]"
  ```

* In de lijst met agents die worden weergegeven: Kopieer de waarde van de "id" veld van die resource waarvan *resourceName* gelijk aan de naam van uw AD-domein.
* De id in deze opdracht te plakken en uitvoeren in Powershell.

  ```powershell
  Remove-PublishedResource -ResourceId "[resource ID]" -TenantId "[tenant ID]"
  ```

* De Agent-configuratiewizard opnieuw uit.
* Alle andere agents die eerder zijn toegewezen aan dit domein moet opnieuw worden geconfigureerd.

#### <a name="how-do-i-uninstall-the-provisioning-agent"></a>Hoe kan ik de Provisioning-Agent verwijderen?

* Meld u aan bij de Windows-server waarop de Provisioning-Agent is geïnstalleerd.
* Ga naar **Configuratiescherm** -> **verwijderen of wijzigen van een programma** menu
* De volgende programma's verwijderen:
  * Microsoft Azure AD Connect-Inrichtingsagent
  * Microsoft Azure AD Connect Agent Updater
  * Microsoft Azure AD Connect-Agentpakket inrichten

### <a name="workday-to-ad-attribute-mapping-and-configuration-questions"></a>WorkDay op AD-kenmerk toewijzing en configuratie-vragen

#### <a name="how-do-i-back-up-or-export-a-working-copy-of-my-workday-provisioning-attribute-mapping-and-schema"></a>Hoe kan ik een back-up of een werkende kopie van mijn Workday inrichting kenmerk toewijzing en het Schema exporteren?

U kunt Microsoft Graph API gebruiken om uw configuratie Gebruikersinrichting van Workday te exporteren. Raadpleeg de stappen in de sectie [exporteren en importeren van de configuratie van uw werkdag inrichting kenmerk Gebruikerstoewijzing](#exporting-and-importing-your-configuration) voor meer informatie.

#### <a name="i-have-custom-attributes-in-workday-and-active-directory-how-do-i-configure-the-solution-to-work-with-my-custom-attributes"></a>Ik heb aangepaste kenmerken in Workday en Active Directory. Hoe kan ik de oplossing voor het werken met mijn aangepaste kenmerken configureren?

De oplossing biedt ondersteuning voor aangepaste Workday en Active Directory-kenmerken. Uw aangepaste kenmerken toevoegen aan het koppelingsschema, opent u de **kenmerktoewijzing** blade en blader omlaag naar de sectie uitvouwen **geavanceerde opties weergeven**. 

![Kenmerkenlijst bewerken](./media/workday-inbound-tutorial/wd_edit_attr_list.png)

Als u wilt uw aangepaste Workday-kenmerken toevoegen, selecteert u de optie *kenmerkenlijst bewerken voor Workday* en uw aangepaste AD-kenmerken toevoegen, selecteert u de optie *kenmerkenlijst bewerken voor op lokale Active Directory*.

Zie ook:

* [De lijst met gebruikerskenmerken Workday aanpassen](#customizing-the-list-of-workday-user-attributes)

#### <a name="how-do-i-configure-the-solution-to-only-update-attributes-in-ad-based-on-workday-changes-and-not-create-any-new-ad-accounts"></a>Hoe kan ik de oplossing voor alleen kenmerken bijwerken in AD op basis van wijzigingen van Workday en niet maken van nieuwe AD-accounts configureren?

Deze configuratie kan worden bereikt door in te stellen de **acties voor doelobject** in de **kenmerktoewijzingen** blade zoals hieronder wordt weergegeven:

![Update-actie](./media/workday-inbound-tutorial/wd_target_update_only.png)

Schakel het selectievakje 'Update' voor alleen update-bewerkingen op die moeten stromen van Workday naar AD. 

#### <a name="can-i-provision-users-photo-from-workday-to-active-directory"></a>Kan ik inrichten van de gebruiker foto's van Workday naar Active Directory?

De oplossing op dit moment biedt geen ondersteuning voor binaire kenmerken zoals instellen *thumbnailphoto wordt door* en *jpegPhoto* in Active Directory.

#### <a name="how-do-i-sync-mobile-numbers-from-workday-based-on-user-consent-for-public-usage"></a>Hoe synchroniseer ik mobiele nummers van Workday op basis van de gebruikerstoestemming voor het gebruik van de openbare?

* Ga op de blade 'Inrichten' van uw App voor het inrichten van Workday.
* Klik op de kenmerktoewijzingen 
* Onder **toewijzingen**, selecteer **op lokale Active Directory-Workday medewerkers synchroniseren** (of **Workday werknemers synchroniseren naar Azure AD**).
* Op de pagina kenmerktoewijzingen Schuif omlaag en schakel het selectievakje 'Geavanceerde opties weergeven'.  Klik op **kenmerkenlijst bewerken voor Workday**
* In de blade die wordt geopend, Ga naar het kenmerk 'Mobiel' en klik op de rij, zodat u kunt bewerken de **API-expressie** ![Mobile Avg](./media/workday-inbound-tutorial/mobile_gdpr.png)

* Vervang de **API-expressie** met de volgende nieuwe expressie die het mobiele telefoonnummer werk haalt alleen als het 'openbare gebruik '-vlag is ingesteld op 'True' in Workday.

    ```
     wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Contact_Data/wd:Phone_Data[translate(string(wd:Phone_Device_Type_Reference/@wd:Descriptor),'abcdefghijklmnopqrstuvwxyz','ABCDEFGHIJKLMNOPQRSTUVWXYZ')='MOBILE' and translate(string(wd:Usage_Data/wd:Type_Data/wd:Type_Reference/@wd:Descriptor),'abcdefghijklmnopqrstuvwxyz','ABCDEFGHIJKLMNOPQRSTUVWXYZ')='WORK' and string(wd:Usage_Data/@wd:Public)='1']/@wd:Formatted_Phone
    ```

* Sla de lijst met kenmerken.
* Sla de kenmerktoewijzing van het.
* De huidige status wissen en opnieuw starten van de volledige synchronisatie.

#### <a name="how-do-i-format-display-names-in-ad-based-on-the-users-departmentcountrycity-attributes-and-handle-regional-variances"></a>Hoe maak ik weergavenamen in AD op basis van de kenmerken van de afdeling/land/plaats en regionale verschillen ingang van de gebruiker?

Het is een algemene vereiste voor het configureren van de *displayName* kenmerk in AD, zodat het bevat ook informatie over de afdeling en het land van de gebruiker. Voor bijvoorbeeld als John Smith in de marketingafdeling in Verenigde Staten werkt, kunt u zijn *displayName* als *Smith, John (Marketing-US)*.

Hier volgt hoe kunt u deze vereisten voor het maken van verwerken *CN* of *displayName* om op te nemen van kenmerken, zoals het bedrijf, bedrijfseenheid, plaats of land/regio.

* Elk kenmerk Workday wordt opgehaald met behulp van een onderliggende API XPATH-expressie, die kan worden geconfigureerd in **kenmerk Mapping -> geavanceerde sectie kenmerkenlijst bewerken voor Workday ->**. Dit is de standaard-API XPATH-expressie voor Workday *PreferredFirstName*, *PreferredLastName*, *bedrijf* en *SupervisoryOrganization* kenmerken.

     [!div class="mx-tdCol2BreakAll"]
     | WorkDay kenmerk | API-XPATH-expressie |
     | ----------------- | -------------------- |
     | PreferredFirstName | wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Name_Data/wd:Preferred_Name_Data/wd:Name_Detail_Data/wd:First_Name/text() |
     | PreferredLastName | wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Name_Data/wd:Preferred_Name_Data/wd:Name_Detail_Data/wd:Last_Name/text() |
     | Bedrijf | wd:Worker/wd:Worker_Data/wd:Organization_Data/wd:Worker_Organization_Data[wd:Organization_Data/wd:Organization_Type_Reference/wd:ID[@wd:type='Organization_Type_ID']='Company']/wd:Organization_Reference/@wd:Descriptor |
     | SupervisoryOrganization | WD:worker / wd:Worker_Data / wd:Organization_Data / wd:Worker_Organization_Data / wd:Organization_Data [wd:Organization_Type_Reference / wd:ID [@wd:type= "Organization_Type_ID"] = 'Toezichthoudende'] /wd:Organization_Name/text() |
  
   Controleer of met uw Workday-team dat de API-expressie hierboven geldig voor de configuratie van uw Workday-tenant is. Indien nodig, u deze bewerken kunt zoals beschreven in de sectie [aanpassen van de lijst met gebruikerskenmerken Workday](#customizing-the-list-of-workday-user-attributes).

* Op dezelfde manier de land-gegevens die aanwezig zijn in Workday wordt opgehaald met behulp van de volgende XPATH: *wd:Worker / wd:Worker_Data / wd:Employment_Data / wd:Position_Data / wd:Business_Site_Summary_Data / wd:Address_Data / wd:Country_Reference*

     Er zijn 5 land-gerelateerde kenmerken die beschikbaar in de sectie Workday kenmerk lijst zijn.

     | WorkDay kenmerk | API-XPATH-expressie |
     | ----------------- | -------------------- |
     | CountryReference | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference/wd:ID[@wd:type='ISO_3166-1_Alpha-3_Code']/text() |
     | CountryReferenceFriendly | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference/@wd:Descriptor |
     | CountryReferenceNumeric | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference/wd:ID[@wd:type='ISO_3166-1_Numeric-3_Code']/text() |
     | CountryReferenceTwoLetter | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference/wd:ID[@wd:type='ISO_3166-1_Alpha-2_Code']/text() |
     | CountryRegionReference | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Region_Reference/@wd:Descriptor |

  Controleer of met uw Workday-team dat de bovenstaande API-expressies geldig voor de configuratie van uw Workday-tenant zijn. Indien nodig, u deze bewerken kunt zoals beschreven in de sectie [aanpassen van de lijst met gebruikerskenmerken Workday](#customizing-the-list-of-workday-user-attributes).

* Voor het bouwen van de expressie van de toewijzing van juiste kenmerk identificeren welke Workday-kenmerk 'bindend' vertegenwoordigt van de gebruiker de naam van de eerste, laatste naam, land en afdeling. Stel dat de kenmerken zijn *PreferredFirstName*, *PreferredLastName*, *CountryReferenceTwoLetter* en *SupervisoryOrganization* respectievelijk. U kunt dit gebruiken om te maken van een expressie voor de AD *displayName* kenmerk als volgt om een weergavenaam, zoals *Smith, John (Marketing-US)*.

    ```
     Append(Join(", ",[PreferredLastName],[PreferredFirstName]), Join(""," (",[SupervisoryOrganization],"-",[CountryReferenceTwoLetter],")"))
    ```
    Zodra u de expressie voor rechts hebt, in de tabel kenmerktoewijzingen bewerken en wijzigen de *displayName* kenmerktoewijzing zoals hieronder wordt weergegeven:   ![DisplayName-toewijzing](./media/workday-inbound-tutorial/wd_displayname_map.png)

* Het bovenstaande voorbeeld is uitgebreid, laten we zeggen dat u wilt converteren die afkomstig zijn van Workday in verkorte versie van waarden plaatsnamen en vervolgens worden gebruikt om te bouwen weergavenamen zoals *Smith, John (CHI)* of *Doe, ANS (NYC)*, vervolgens dit resultaat kan worden bereikt met behulp van een Switch-expressie met de Workday *gemeenteraad* kenmerk, zoals de bepalende variabele.

     ```
    Switch
    (
      [Municipality],
      Join(", ", [PreferredLastName], [PreferredFirstName]),  
           "Chicago", Append(Join(", ",[PreferredLastName], [PreferredFirstName]), "(CHI)"),
           "New York", Append(Join(", ",[PreferredLastName], [PreferredFirstName]), "(NYC)"),
           "Phoenix", Append(Join(", ",[PreferredLastName], [PreferredFirstName]), "(PHX)")
    )
     ```
    Zie ook:
  * [De syntaxis van de switch-functie](../manage-apps/functions-for-customizing-application-data.md#switch)
  * [Syntaxis van de functie toevoegen](../manage-apps/functions-for-customizing-application-data.md#join)
  * [Syntaxis van de functie toevoegen](../manage-apps/functions-for-customizing-application-data.md#append)

#### <a name="how-can-i-use-selectuniquevalue-to-generate-unique-values-for-samaccountname-attribute"></a>Hoe kan ik SelectUniqueValue gebruiken voor het genereren van unieke waarden voor het kenmerk samAccountName?

Stel dat u wilt genereren van unieke waarden voor *samAccountName* met behulp van een combinatie van het kenmerk *FirstName* en *LastName* kenmerken van Workday. Hieronder ziet, is een expressie die u met beginnen kunt:

```
SelectUniqueValue(
    Replace(Mid(Replace(NormalizeDiacritics(StripSpaces(Join("",  Mid([FirstName],1,1), [LastName]))), , "([\\/\\\\\\[\\]\\:\\;\\|\\=\\,\\+\\*\\?\\<\\>])", , "", , ), 1, 20), , "(\\.)*$", , "", , ),
    Replace(Mid(Replace(NormalizeDiacritics(StripSpaces(Join("",  Mid([FirstName],1,2), [LastName]))), , "([\\/\\\\\\[\\]\\:\\;\\|\\=\\,\\+\\*\\?\\<\\>])", , "", , ), 1, 20), , "(\\.)*$", , "", , ),
    Replace(Mid(Replace(NormalizeDiacritics(StripSpaces(Join("",  Mid([FirstName],1,3), [LastName]))), , "([\\/\\\\\\[\\]\\:\\;\\|\\=\\,\\+\\*\\?\\<\\>])", , "", , ), 1, 20), , "(\\.)*$", , "", , ),
)
```

Hoe werkt de bovenstaande formule: Als de gebruiker John Smith is, wordt eerst geprobeerd voor het genereren van JSmith, als JSmith al bestaat, vervolgens het JoSmith, genereert als die bestaat, JohSmith wordt gegenereerd. De expressie zorgt er ook voor dat de gegenereerde waarde voldoet aan de lengtebeperking en speciale tekens beperking die is gekoppeld aan *samAccountName*.

Zie ook:

* [Mid syntaxis van de functie](../manage-apps/functions-for-customizing-application-data.md#mid)
* [Syntaxis van de functie vervangen](../manage-apps/functions-for-customizing-application-data.md#replace)
* [SelectUniqueValue Function Syntax](../manage-apps/functions-for-customizing-application-data.md#selectuniquevalue)

#### <a name="how-do-i-remove-characters-with-diacritics-and-convert-them-into-normal-english-alphabets"></a>Hoe ik tekens met diakritische tekens te verwijderen en deze omzetten in normale Engelse letters?

Gebruik de functie [NormalizeDiacritics](../manage-apps/functions-for-customizing-application-data.md#normalizediacritics) als u wilt verwijderen van speciale tekens in de voornaam en achternaam van de gebruiker, tijdens het samenstellen van de e-mailadres of de CN-waarde voor de gebruiker.

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

Deze sectie bevat specifieke hulp over het oplossen van de inrichting problemen met uw Workday-integratie met de Azure AD-auditlogboeken en logboeken van Windows Server-logboeken. Deze voortbouwt op de algemene stappen voor probleemoplossing en concepten die zijn vastgelegd in de [zelfstudie: Rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md)

In deze sectie worden de volgende aspecten van het oplossen van problemen:

* [Windows-Logboeken in te stellen voor het oplossen van agent](#setting-up-windows-event-viewer-for-agent-troubleshooting)
* [Instellen van Azure portal controlelogboeken voor het oplossen van service](#setting-up-azure-portal-audit-logs-for-service-troubleshooting)
* [Bewerkingen voor inzicht in Logboeken voor AD-gebruikersaccount maken](#understanding-logs-for-ad-user-account-create-operations)
* [Inzicht in Logboeken voor Manager updatebewerkingen](#understanding-logs-for-manager-update-operations)
* [Het omzetten van vaak fouten aangetroffen](#resolving-commonly-encountered-errors)

### <a name="setting-up-windows-event-viewer-for-agent-troubleshooting"></a>Windows-Logboeken in te stellen voor het oplossen van agent

* Meld u aan bij de Windows Server-machine waarop de Agent inrichting wordt geïmplementeerd
* Open **logboeken van Windows Server** desktop-app.
* Selecteer **Windows Logboeken > toepassing**.
* Gebruik de **Huidig logboek filteren...** optie voor het weergeven van alle gebeurtenissen vastgelegd onder de bron **AAD. Connect.ProvisioningAgent** en gebeurtenissen met gebeurtenis-ID '5', uitsluiten door het filter '-5' op te geven, zoals hieronder weergegeven.

  ![Windows-Logboeken](media/workday-inbound-tutorial/wd_event_viewer_01.png))

* Klik op **OK** en het resultaat weergeven per sorteren **datum en tijd** kolom.

### <a name="setting-up-azure-portal-audit-logs-for-service-troubleshooting"></a>Instellen van Azure portal controlelogboeken voor het oplossen van service

* Start de [Azure-portal](https://portal.azure.com), en navigeer naar de **auditlogboeken** sectie van uw toepassing wordt ingericht Workday.
* Gebruik de **kolommen** knop op de pagina controlelogboeken om weer te geven alleen de volgende kolommen in de weergave (datum, activiteit, Status, reden van de Status). Deze configuratie zorgt ervoor dat u zich op gegevens die relevant is richten voor het oplossen van problemen.

  ![Audit log kolommen](media/workday-inbound-tutorial/wd_audit_logs_00.png)

* Gebruik de **doel** en **datumbereik** queryparameters om de weergave te filteren. 
  * Stel de **doel** queryparameter naar het 'Werknemer-ID' of 'Werknemer-ID' van het object van de werknemer Workday.
  * Stel de **datumbereik** op een geschikte periode waarover u wilt onderzoeken op fouten of problemen met het inrichten.

  ![Audit log filters](media/workday-inbound-tutorial/wd_audit_logs_01.png)

### <a name="understanding-logs-for-ad-user-account-create-operations"></a>Bewerkingen voor inzicht in Logboeken voor AD-gebruikersaccount maken

Wanneer een nieuwe medewerkers in Workday wordt gedetecteerd (Stel met werknemer-ID *21023*), de service-aanvallen te maken van een nieuw AD-gebruikersaccount voor de werknemer en tijdens het inrichten op Azure AD maakt 4 records in auditlogboeken zoals hieronder wordt beschreven:

  [ ![Controlelogboek ops maken](media/workday-inbound-tutorial/wd_audit_logs_02.png) ](media/workday-inbound-tutorial/wd_audit_logs_02.png#lightbox)

Wanneer u een van de records in auditlogboeken, op de **activiteitendetails** pagina wordt geopend. Dit is wat de **activiteitendetails** pagina worden weergegeven voor elk type record.

* **WorkDay importeren** record: Deze logboekrecord weergegeven de worker-gegevens opgehaald uit Workday. Gebruik de informatie in de *extra Details* sectie van de logboekrecord op het oplossen van problemen met het ophalen van gegevens uit Workday. Hieronder ziet u een voorbeeldrecord voor een, samen met tips over het interpreteren van elk veld.

  ```JSON
  ErrorCode : None  // Use the error code captured here to troubleshoot Workday issues
  EventName : EntryImportAdd // For full sync, value is "EntryImportAdd" and for delta sync, value is "EntryImport"
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID (usually the Worker ID or Employee ID field)
  SourceAnchor : a071861412de4c2486eb10e5ae0834c3 // set to the WorkdayID (WID) associated with the record
  ```

* **AD-Import** record: Deze logboekrecord geeft informatie weer van het account dat is opgehaald uit Active Directory. Als tijdens het maken van de oorspronkelijke gebruiker geen AD-account is, de *reden van de activiteit Status* wordt aangegeven dat er is geen account met de waarde van het overeenkomende ID-kenmerk in Active Directory is gevonden. Gebruik de informatie in de *extra Details* sectie van de logboekrecord op het oplossen van problemen met het ophalen van gegevens uit Workday. Hieronder ziet u een voorbeeldrecord voor een, samen met tips over het interpreteren van elk veld.

  ```JSON
  ErrorCode : None // Use the error code captured here to troubleshoot Workday issues
  EventName : EntryImportObjectNotFound // Implies that object was not found in AD
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID
  ```

  Agent ingericht logboekrecords overeenkomt met deze importbewerking AD vindt de Windows-Logboeken openen en gebruiken de **zoeken...** menu-optie te vinden van vermeldingen in het logboek met de ID/toevoegen-eigenschap die overeenkomt met kenmerk-waarde (in dit geval *21023*).

  ![Find](media/workday-inbound-tutorial/wd_event_viewer_02.png)

  Zoek de vermelding met *gebeurtenis-ID = 9*, die wordt bieden u de LDAP filter gebruikt door de agent voor het ophalen van de AD-account zoeken. U kunt controleren als dit is de juiste zoekfilter om op te halen van unieke gebruikersvermeldingen.

  ![LDAP Search](media/workday-inbound-tutorial/wd_event_viewer_03.png)

  De record die onmiddellijk op met volgt *gebeurtenis-ID = 2* legt het resultaat van de zoekbewerking en als er geen resultaten geretourneerd.

  ![LDAP-resultaten](media/workday-inbound-tutorial/wd_event_viewer_04.png)

* **Synchronisatieregelactie** record: Deze logboekrecord geeft de resultaten van de toewijzingsregels kenmerk en samen met de provisioning actie die wordt uitgevoerd voor het verwerken van de inkomende gebeurtenis van Workday bereikfilters die zijn geconfigureerd. Gebruik de informatie in de *extra Details* sectie van de logboekrecord oplossen van problemen met de synchronisatieactie. Hieronder ziet u een voorbeeldrecord voor een, samen met tips over het interpreteren van elk veld.

  ```JSON
  ErrorCode : None // Use the error code captured here to troubleshoot sync issues
  EventName : EntrySynchronizationAdd // Implies that the object will be added
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID
  SourceAnchor : a071861412de4c2486eb10e5ae0834c3 // set to the WorkdayID (WID) associated with the profile in Workday
  ```

  Als er problemen met het kenmerk toewijzing expressies zijn of de binnenkomende gegevens van Workday problemen heeft (bijvoorbeeld: leeg of null-waarde voor de vereiste kenmerken), en vervolgens ziet u een fout in deze fase met de foutcode van de fout geven.

* **AD-Export** record: Deze logboekrecord toont het resultaat van bewerking van AD-account maken samen met de kenmerkwaarden weer die zijn ingesteld in het proces. Gebruik de informatie in de *extra Details* sectie van de logboekrecord om problemen met het account te maken. Hieronder ziet u een voorbeeldrecord voor een, samen met tips over het interpreteren van elk veld. In de sectie 'Extra Details' de 'EventName' is ingesteld op 'EntryExportAdd', de 'JoiningProperty' is ingesteld op de waarde van de kenmerk-ID die overeenkomt met het "SourceAnchor" is ingesteld op de WorkdayID (WID) die zijn gekoppeld aan de record en de 'TargetAnchor' is ingesteld op de waarde van de AD "ObjectGuid" kenmerk van de nieuwe gebruiker. 

  ```JSON
  ErrorCode : None // Use the error code captured here to troubleshoot AD account creation issues
  EventName : EntryExportAdd // Implies that object will be created
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID
  SourceAnchor : a071861412de4c2486eb10e5ae0834c3 // set to the WorkdayID (WID) associated with the profile in Workday
  TargetAnchor : 83f0156c-3222-407e-939c-56677831d525 // set to the value of the AD "objectGuid" attribute of the new user
  ```

  Agent ingericht logboekrecords overeenkomt met deze exportbewerking AD vindt de Windows-Logboeken openen en gebruiken de **zoeken...** menu-optie te vinden van vermeldingen in het logboek met de ID/toevoegen-eigenschap die overeenkomt met kenmerk-waarde (in dit geval *21023*).  

  Zoek naar een HTTP POST-record die overeenkomt met de tijdstempel van de exportbewerking met *gebeurtenis-ID = 2*. Deze record bevat de kenmerkwaarden weer die door de provisioning-service worden verzonden naar de inrichtingsagent.

  [![SCIM toevoegen](media/workday-inbound-tutorial/wd_event_viewer_05.png)](media/workday-inbound-tutorial/wd_event_viewer_05.png#lightbox)

  Onmiddellijk na de bovenstaande gebeurtenis, moet er een andere gebeurtenis waarmee het antwoord van de create AD accountbewerking wordt vastgelegd. Deze gebeurtenis wordt geretourneerd voor de nieuwe objectGuid gemaakt in AD en deze is ingesteld als het kenmerk TargetAnchor in de provisioning-service.

  [![SCIM toevoegen](media/workday-inbound-tutorial/wd_event_viewer_06.png)](media/workday-inbound-tutorial/wd_event_viewer_06.png#lightbox)

### <a name="understanding-logs-for-manager-update-operations"></a>Inzicht in Logboeken voor bewerkingen voor update

Het kenmerk manager is een verwijzingskenmerk in AD. De provisioning-service wordt niet ingesteld voor het kenmerk manager als onderdeel van een bewerking voor het maken van de gebruiker. In plaats van het kenmerk manager is ingesteld als onderdeel van een *bijwerken* bewerking nadat AD-account is gemaakt voor de gebruiker. Uitbreiden van het bovenstaande voorbeeld, stel een nieuwe medewerkers met werknemer-ID "21451" is geactiveerd in Workday en de nieuwe medewerkers manager (*21023*) heeft al een AD-account. In dit scenario weergegeven te zoeken naar de auditlogboeken voor gebruiker 21451 5 vermeldingen.

  [ ![Manager-Update](media/workday-inbound-tutorial/wd_audit_logs_03.png) ](media/workday-inbound-tutorial/wd_audit_logs_03.png#lightbox)

De eerste 4 records zijn, zoals we verkend als onderdeel van de gebruiker die de maakbewerking. De 5-record is de export manager kenmerk update is gekoppeld. De logboekrecord toont het resultaat van de AD-account manager updatebewerking, die wordt uitgevoerd met behulp van de manager *objectGuid* kenmerk.

  ```JSON
  // Modified Properties
  Name : manager
  New Value : "83f0156c-3222-407e-939c-56677831d525" // objectGuid of the user 21023

  // Additional Details
  ErrorCode : None // Use the error code captured here to troubleshoot AD account creation issues
  EventName : EntryExportUpdate // Implies that object will be created
  JoiningProperty : 21451 // Value of the Workday attribute that serves as the Matching ID
  SourceAnchor : 9603bf594b9901693f307815bf21870a // WorkdayID of the user
  TargetAnchor : 43b668e7-1d73-401c-a00a-fed14d31a1a8 // objectGuid of the user 21451

  ```

### <a name="resolving-commonly-encountered-errors"></a>Het omzetten van vaak fouten aangetroffen

In deze sectie bevat informatie over vaak waargenomen fouten met gebruikersinrichting van Workday en hoe u deze kunt oplossen. De fouten zijn als volgt onderverdeeld:

* [Agent-fouten bij het inrichten](#provisioning-agent-errors)
* [Connectiviteitsfouten](#connectivity-errors)
* [Fouten bij AD gebruiker het account maken](#ad-user-account-creation-errors)
* [Fouten tijdens account bijwerken en AD-gebruiker](#ad-user-account-update-errors)

#### <a name="provisioning-agent-errors"></a>Agent-fouten bij het inrichten

|#|Fout-Scenario |Waarschijnlijke oorzaken|Aanbevolen resolutie|
|--|---|---|---|
|1.| Fout bij het installeren van de inrichtingsagent met het volgende foutbericht:  *' Microsoft Azure AD Connect inrichting Agent-service (AADConnectProvisioningAgent) kan niet worden gestart. Controleer of u voldoende rechten om het systeem te starten.* | Deze fout wordt meestal weergegeven als u probeert de inrichting agent installeren op een domeincontroller en Groepsbeleid voorkomt de service niet kan worden gestart dat.  Dit wordt ook weergegeven als er een eerdere versie van de agent die wordt uitgevoerd en u deze niet hebt verwijderd voordat u een nieuwe installatie start.| De inrichting agent installeren op een niet-DC-server. Zorg ervoor dat de vorige versies van de agent worden verwijderd voordat u de nieuwe agent installeert.|
|2.| De Windows-Service ' Microsoft Azure AD Connect inrichting Agent' is in *vanaf* status en wordt niet overschakelen naar *met* staat. | Als onderdeel van de installatie van de wizard agent maakt u een lokaal account (**NT-Service\\AADConnectProvisioningAgent**) op de server en dit is de **aanmelden** account dat wordt gebruikt voor het starten van de de service. Als u een beveiligingsbeleid op uw Windows-server voorkomt dat lokale accounts met de services, wordt deze fout optreedt. | Open de *servicesconsole*. Klik met de rechtermuisknop op de Windows-Service ' Microsoft Azure AD Connect inrichting Agent' en geef het account van een domeinbeheerder de service uit te voeren in het tabblad aanmelden. Start de service. |
|3.| Bij het configureren van de inrichtingsagent met uw AD-domein in de stap *verbinding maken met Active Directory*, de wizard een lang duurt het laden van de AD-schema en uiteindelijk een time-out optreedt. | Deze fout meestal wordt weergegeven als de wizard geen contact opnemen met de AD domain controller-server vanwege problemen met een firewall is. | Op de *verbinding maken met Active Directory* scherm van de wizard, terwijl de referenties voor uw AD-domein, wordt de optie *Selecteer domain controller prioriteit*. Gebruik deze optie te selecteren van een domeincontroller die zich in dezelfde site als de agent-server en zorg ervoor dat er geen firewall-regels van de communicatie te blokkeren. |

#### <a name="connectivity-errors"></a>Fouten in de basisnetwerkverbinding

Als de provisioning-service kan geen verbinding maken met Workday of Active Directory is, kan dit leiden tot het inrichten om te gaan in de geïsoleerde modus. Gebruik de onderstaande tabel voor het oplossen van problemen met de netwerkverbinding.

|#|Fout-Scenario |Waarschijnlijke oorzaken|Aanbevolen resolutie|
|--|---|---|---|
|1.| Als u klikt op **testverbinding**, krijgt u het foutbericht weergegeven: *Er is een fout opgetreden bij het verbinden met Active Directory. Zorg ervoor dat de Agent voor het inrichten van on-premises wordt uitgevoerd en wordt deze geconfigureerd met de juiste Active Directory-domein.* | Deze fout meestal wordt weergegeven als de inrichting agent is niet actief of er is een firewall blokkeren van communicatie tussen Azure AD en de inrichtingsagent. U ziet deze fout kan ook als het domein is niet geconfigureerd in de Wizard Agent. | Open de *Services* -console op de Windows-server om te bevestigen dat de agent wordt uitgevoerd. Open de wizard inrichting agent en Bevestig dat het juiste domein met de agent is geregistreerd.  |
|2.| De inrichtingstaak krijgt de status van de quarantaine in het weekend (vr-za) en krijgen we een e-mailbericht dat er een fout opgetreden bij de synchronisatie is. | Een van de algemene oorzaken voor deze fout is de geplande uitvaltijd van Workday. Als u van een tenant van de implementatie van Workday gebruikmaakt, houd er rekening mee dat Workday heeft geplande uitvaltijd voor de implementatie-tenants via weekends (meestal van zaterdag van vrijdag op zaterdag ochtend) en tijdens die periode van de werkdag inrichting van apps overschakelen naar een kan staat in quarantaine plaatsen kan geen verbinding maken met Workday. Het wordt terug naar de normale status zodra de implementatie van Workday tenant weer online is. In zeldzame gevallen mogelijk ook ziet u deze fout als het wachtwoord van de systeemgebruiker integratie gewijzigd vanwege tenant vernieuwen of als het account is vergrendeld of verlopen staat. | Neem contact op met uw Workday-beheerder of integratie-partner om te zien wanneer Workday plant u downtime te negeren van waarschuwingen gedurende de periode van uitvaltijd en te bevestigen beschikbaarheid zodra Workday-exemplaar weer online is.  |


#### <a name="ad-user-account-creation-errors"></a>Fouten bij AD gebruiker het account maken

|#|Fout-Scenario |Waarschijnlijke oorzaken|Aanbevolen resolutie|
|--|---|---|---|
|1.| Exporteren van de mislukte bewerking in het auditlogboek met het bericht *fout: OperationsError-SvcErr: Er is een bewerkingsfout opgetreden. Er is geen bovenliggende verwijzing is geconfigureerd voor Active Directory. Active Directory is daarom niet probleem verwijzingen naar objecten buiten dit forest.* | Deze fout wordt meestal weergegeven als de *Active Directory-Container* organisatie-eenheid is niet juist ingesteld of als er problemen met zijn de toewijzing van de expressie wordt gebruikt voor *parentDistinguishedName*. | Controleer de *Active Directory-Container* parameter van de organisatie-eenheid voor typfouten. Als u *parentDistinguishedName* Zorg ervoor dat het altijd in een bekende container in het AD-domein resulteert in het kenmerk wordt toegewezen. Controleer de *exporteren* in de audit gebeurtenislogboeken om te zien van de gegenereerde waarde. |
|2.| Bewerkingsfouten in het auditlogboek met foutcode exporteren: *SystemForCrossDomainIdentityManagementBadResponse* en het bericht *fout: ConstraintViolation-AtrErr: Een waarde in de aanvraag is ongeldig. Een waarde voor het kenmerk is niet in het geaccepteerde bereik van waarden. details van \nError: CONSTRAINT_ATT_TYPE - bedrijf*. | Als deze fout is specifiek voor de *bedrijf* kenmerk, ziet u mogelijk deze fout voor andere kenmerken, zoals *CN* ook. Deze fout treedt op vanwege een beperking van afgedwongen AD-schema. Standaard de kenmerken, zoals *bedrijf* en *CN* in AD heeft een limiet van 64 tekens. Als de waarde die afkomstig zijn van Workday meer dan 64 tekens is, ziet u dit foutbericht wordt weergegeven. | Controleer de *exporteren* gebeurtenis in de controlelogboeken om de waarde voor het kenmerk te bekijken die zijn gerapporteerd in het foutbericht. Houd rekening met het afkappen van de waarde die afkomstig zijn van Workday met behulp van de [Mid](../manage-apps/functions-for-customizing-application-data.md#mid) functie of het wijzigen van de toewijzingen in een AD-kenmerk dat heeft geen soortgelijke lengtebeperkingen.  |

#### <a name="ad-user-account-update-errors"></a>Fouten tijdens account bijwerken en AD-gebruiker

Tijdens het updateproces AD gebruiker-account de provisioning-service leest gegevens uit zowel Workday en AD, wordt het kenmerk regels voor apparaatgroeptoewijzing wordt uitgevoerd en bepaalt u als een wijziging van kracht worden. Dienovereenkomstig wordt een update-gebeurtenis geactiveerd. Als een van deze stappen een fout wordt aangetroffen, wordt deze geregistreerd in de auditlogboeken. Gebruik de onderstaande tabel voor het oplossen van veelvoorkomende fouten tijdens het bijwerken.

|#|Fout-Scenario |Waarschijnlijke oorzaken|Aanbevolen resolutie|
|--|---|---|---|
|1.| Synchronisatie regel actie is mislukt in het auditlogboek met het bericht *EventName = EntrySynchronizationError en ErrorCode = EndpointUnavailable*. | Deze fout wordt weergegeven als de provisioning-service kan niet worden gebruikersprofielgegevens opgehaald uit Active Directory vanwege een fout in verwerking door de on-premises aangetroffen is inrichtingsagent. | Raadpleeg de logboeken van de Agent-logboeken inrichting gebeurtenissen op foutniveau die wijzen op problemen met de leesbewerking (filteren op gebeurtenis-ID #2). |
|2.| Het kenmerk manager in AD niet wordt bijgewerkt voor bepaalde gebruikers in AD. | De meest waarschijnlijke oorzaak van deze fout is als u van scoping regels gebruikmaakt en de manager van de gebruiker geen onderdeel van het bereik is. U kunt ook uitvoeren in dit probleem als de manager van de overeenkomende id-kenmerk (bijvoorbeeld EmployeeID) is niet in de doel-AD-domein gevonden of niet is ingesteld op de juiste waarde. | Bekijk de bereikfilter en toevoegen van de gebruiker manager binnen het bereik. Controleren van de manager-profiel in AD om ervoor te zorgen dat er een waarde voor de overeenkomende ID-kenmerk is. |

## <a name="managing-your-configuration"></a>Uw configuratie beheren

Deze sectie wordt beschreven hoe u kunt verder uitbreiden, aanpassen en beheren van uw configuratie voor gebruikersinrichting Workday-driven. Hierin worden de volgende onderwerpen:

* [De lijst met gebruikerskenmerken Workday aanpassen](#customizing-the-list-of-workday-user-attributes)  
* [De configuratie importeren en exporteren](#exporting-and-importing-your-configuration)

### <a name="customizing-the-list-of-workday-user-attributes"></a>De lijst met gebruikerskenmerken Workday aanpassen

De inrichting van apps voor Active Directory en Azure AD beide een lijst Workday gebruikerskenmerken bevatten Workday u kunt kiezen uit. Deze lijsten zijn echter niet volledig. WorkDay biedt ondersteuning voor vele honderden mogelijk gebruikerskenmerken, die standaard of uniek is voor uw Workday-tenant.

De Azure AD-inrichtingsservice ondersteunt de mogelijkheid om aan te passen van de lijst of Workday-kenmerk om op te nemen van de kenmerken die beschikbaar zijn in de [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) bewerking van het Human Resources-API.

Als u wilt deze wijziging doet, moet u [Workday Studio](https://community.workday.com/studio-download) om op te halen van de XPath-expressies die staan voor de kenmerken die u wilt gebruiken, en deze vervolgens toevoegen aan uw provisioning-configuratie met behulp van de geavanceerde kenmerkeditor in Azure portal .

**Een XPath-expressie voor een gebruikerskenmerk Workday ophalen:**

1. Download en installeer [Workday Studio](https://community.workday.com/studio-download). U moet een werkdag community-account voor toegang tot het installatieprogramma.

2. Download het Workday Human_Resources WSDL-bestand vanaf deze URL: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. Launch Workday Studio.

4. Selecteer in de opdrachtbalk de **Workday > Test Web Service in Tester** optie.

5. Selecteer **externe**, en selecteer het Human_Resources WSDL-bestand dat u hebt gedownload in stap 2.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio1.png)

6. Stel de **locatie** veld `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, maar "IMPL-CC" vervangen door uw werkelijke type instantie en 'TENANT' met de naam van uw bestaande tenant.

7. Stel **bewerking** naar **Get_Workers**

8.  Klik op de kleine **configureren** koppeling hieronder de deelvensters aanvraag/antwoord om in te stellen van uw Workday-referenties. Controleer **verificatie**, en voer de gebruikersnaam en het wachtwoord voor het systeem-account van uw Workday-integratie. Zorg ervoor dat u de naam van de gebruiker als indeling name@tenant, en laat de **WS-Security UsernameToken** optie is geselecteerd.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio2.png)

9. Selecteer **OK**.

10. In de **aanvragen** deelvenster plakken in het XML-bestand op de onderstaande en stel **werknemer** naar de werknemer-ID van een echte gebruiker in uw Workday-tenant. Selecteer een gebruiker die het kenmerk heeft ingevuld die u wilt extraheren.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="https://www.w3.org/2001/XMLSchema">
      <env:Body>
        <wd:Get_Workers_Request xmlns:wd="urn:com.workday/bsvc" wd:version="v21.1">
          <wd:Request_References wd:Skip_Non_Existing_Instances="true">
            <wd:Worker_Reference>
              <wd:ID wd:type="Employee_ID">21008</wd:ID>
            </wd:Worker_Reference>
          </wd:Request_References>
          <wd:Response_Group>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Personal_Information>true</wd:Include_Personal_Information>
            <wd:Include_Employment_Information>true</wd:Include_Employment_Information>
            <wd:Include_Management_Chain_Data>true</wd:Include_Management_Chain_Data>
            <wd:Include_Organizations>true</wd:Include_Organizations>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Transaction_Log_Data>true</wd:Include_Transaction_Log_Data>
            <wd:Include_Photo>true</wd:Include_Photo>
            <wd:Include_User_Account>true</wd:Include_User_Account>
          <wd:Include_Roles>true</wd:Include_Roles>
          </wd:Response_Group>
        </wd:Get_Workers_Request>
      </env:Body>
    </env:Envelope>
    ```

11. Klik op de **aanvraag verzenden** (groene pijl) de opdracht uit te voeren. Als geslaagd, het antwoord moet worden weergegeven in de **antwoord** deelvenster. Controleer het antwoord om te controleren of er de gegevens van de gebruikers-ID die u hebt ingevoerd, en niet een fout.

12. Als dit lukt, kopieert u het XML-bestand van de **antwoord** deelvenster en sla deze op als een XML-bestand.

13. Selecteer in de opdracht balk van Workday Studio **bestand > bestand openen...**  en open het XML-bestand dat u hebt opgeslagen. Deze actie wordt het bestand geopend in de Workday Studio XML-editor.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio3.png)

14. Navigeer in de bestandsstructuur van het via   **/env: Envelop > env: Body > wd:Get_Workers_Response > wd:Response_Data > Word: Werknemer** om gegevens van uw gebruikers te vinden.

15. Onder **Word: Werknemer**, zoek het kenmerk dat u wilt toevoegen en selecteer deze.

16. Kopieer de XPath-expressie voor het geselecteerde kenmerk van de **documentpad** veld.

17. Verwijder de **/env:Envelope / env:Body / wd:Get_Workers_Response / wd:Response_Data /** voorvoegsel van de gekopieerde expressie.

18. Als het laatste item in de gekopieerde expressie een knooppunt is (voorbeeld: "/ Word: Birth_Date'), Voeg **/text()** aan het einde van de expressie. Dit is niet nodig als het laatste item een kenmerk is (voorbeeld: "/@wd: type ').

19. Het resultaat moet er ongeveer als `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Deze waarde is wat u kopieert naar de Azure-portal.

**Uw aangepaste Workday gebruikerskenmerk toevoegen aan uw provisioning-configuratie:**

1. Start de [Azure-portal](https://portal.azure.com), en navigeer naar de sectie inrichting van uw werkdag inrichting van toepassing, zoals eerder in deze zelfstudie wordt beschreven.

2. Stel **Inrichtingsstatus** te **uit**, en selecteer **opslaan**. Deze stap helpt Zorg ervoor dat uw wijzigingen worden doorgevoerd alleen wanneer u klaar bent.

3. Onder **toewijzingen**, selecteer **op lokale Active Directory-Workday medewerkers synchroniseren** (of **Workday werknemers synchroniseren naar Azure AD**).

4. Ga naar de onderkant van het volgende scherm en selecteer **geavanceerde opties weergeven**.

5. Selecteer **kenmerkenlijst bewerken voor Workday**.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio_aad1.png)

6. Ga naar de onderkant van de lijst met kenmerken aan waar de invoervelden.

7. Voor **naam**, voer een weergavenaam voor het kenmerk.

8. Voor **Type**,-type selecteren dat op de juiste wijze komt met het kenmerk overeen (**tekenreeks** is gebruikelijk).

9. Voor **API-expressie**, geef de XPath-expressie die u hebt gekopieerd uit Workday Studio. Voorbeeld: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Selecteer **kenmerk toevoegen**.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio_aad2.png)

11. Selecteer **opslaan** hierboven, en vervolgens **Ja** naar het dialoogvenster. Sluit het scherm kenmerktoewijzing als deze nog steeds geopend is.

12. Terug op de hoofdpagina **Provisioning** tabblad **op lokale Active Directory-Workday medewerkers synchroniseren** (of **werknemers synchroniseren naar Azure AD**) opnieuw.

13. Selecteer **nieuwe toewijzing toevoegen**.

14. Het nieuwe kenmerk wordt nu weergegeven in de **bronkenmerk** lijst.

15. Een toewijzing voor het nieuwe kenmerk toevoegen naar wens.

16. Wanneer u klaar bent, moet u om in te stellen **Inrichtingsstatus** terug naar **op** en op te slaan.

### <a name="exporting-and-importing-your-configuration"></a>De configuratie importeren en exporteren

In deze sectie wordt beschreven hoe u de Microsoft Graph API en Graph Explorer gebruiken om uw kenmerktoewijzingen inrichten van Workday en het schema aan een JSON-bestand exporteren en importeren in Azure AD.

#### <a name="step-1-retrieve-your-workday-provisioning-app-service-principal-id-object-id"></a>Stap 1: Ophalen van uw werkdag inrichting App Service-Principal-ID (Object-ID)

1. Start de [Azure-portal](https://portal.azure.com), en navigeer naar de sectie met eigenschappen van uw werkdag inrichting van toepassing.
1. In het gedeelte Eigenschappen van uw provisioning-app, kopieert u de GUID-waarde die is gekoppeld aan de *Object-ID* veld. Deze waarde wordt ook wel genoemd de **ServicePrincipalId** van uw App en deze wordt gebruikt in Graph Explorer-bewerkingen.

   ![WorkDay App Service-Principal-ID](./media/workday-inbound-tutorial/wd_export_01.png)

#### <a name="step-2-sign-into-microsoft-graph-explorer"></a>Stap 2: Meld u aan bij Microsoft Graph Explorer

1. Start [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)
1. Klik op de knop 'Aanmelden met Microsoft' en meld u met behulp van Azure AD-hoofdbeheerder of beheer van App-referenties.

    ![Meld u in de grafiek](./media/workday-inbound-tutorial/wd_export_02.png)

1. Aanmelden bij een geslaagde ziet u de gegevens van de gebruiker in het linkerdeelvenster.

#### <a name="step-3-retrieve-the-provisioning-job-id-of-the-workday-provisioning-app"></a>Stap 3: De inrichting taak-ID van de werkdag inrichting App ophalen

Uitvoeren in Microsoft Graph Explorer, de volgende GET-query [servicePrincipalId] vervangen met de **ServicePrincipalId** geëxtraheerd uit de [stap 1](#step-1-retrieve-your-workday-provisioning-app-service-principal-id-object-id).

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs
```

U ontvangt een antwoord, zoals hieronder weergegeven. Kopieer het kenmerk' id' aanwezig in het antwoord. Deze waarde is de **ProvisioningJobId** en wordt gebruikt om op te halen de metagegevens van het onderliggende schema.

   [![Inrichting van taak-Id](./media/workday-inbound-tutorial/wd_export_03.png)](./media/workday-inbound-tutorial/wd_export_03.png#lightbox)

#### <a name="step-4-download-the-provisioning-schema"></a>Stap 4: Het inrichtingsproces Schema downloaden

In de Microsoft Graph Explorer, voer de volgende GET-query, vervangen [servicePrincipalId] en [ProvisioningJobId] met de ServicePrincipalId en de ProvisioningJobId in de vorige stappen hebt opgehaald.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

Kopieer het JSON-object van het antwoord en sla deze op een bestand te maken van een back-up van het schema.

#### <a name="step-5-import-the-provisioning-schema"></a>Stap 5: De inrichting Schema importeren

> [!CAUTION]
> Voer deze stap alleen als u nodig hebt om het schema voor de configuratie die niet kan worden gewijzigd met behulp van de Azure-portal te wijzigen of als u wilt herstellen van de configuratie van een eerder back-ups van bestand met een geldig en een werkende schema.

In de Microsoft Graph Explorer, configureert u de volgende query PUT vervangen [servicePrincipalId] en [ProvisioningJobId] met de ServicePrincipalId en de ProvisioningJobId die u in de vorige stappen.

```http
    PUT https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

Kopieer de inhoud van het JSON-schema-bestand op het tabblad 'Aanvraagtekst'.

   [![Aanvraagtekst](./media/workday-inbound-tutorial/wd_export_04.png)](./media/workday-inbound-tutorial/wd_export_04.png#lightbox)

Voeg het kenmerk van de header Content-Type met de waarde ' application/json' op het tabblad "Aanvraagheaders"

   [![Aanvraagheaders](./media/workday-inbound-tutorial/wd_export_05.png)](./media/workday-inbound-tutorial/wd_export_05.png#lightbox)

Klik op de knop 'Query uitvoeren' voor het importeren van het nieuwe schema.

## <a name="managing-personal-data"></a>Persoonlijke gegevens beheren

De oplossing wordt ingericht voor Active Directory Workday vereist een inrichtingsagent worden geïnstalleerd op een on-premises Windows server en deze agent logboeken gemaakt in het Windows-gebeurtenislogboek dat persoonlijke gegevens, afhankelijk van uw werkdag aan AD-kenmerk kan bevatten toewijzingen. Om te voldoen aan de gebruiker privacy verplichtingen, kunt u ervoor zorgen dat er geen gegevens worden bewaard in het geval van Logboeken na 48 uur door het instellen van een Windows geplande taak om te wissen van het gebeurtenislogboek wordt geschreven.

De Azure AD-inrichtingsservice valt in de **gegevensverwerker** categorie van GDPR-classificatie. Als een pijplijn gegevensverwerker biedt de service services voor gegevensverwerking op belangrijke partners en klanten te beëindigen. Azure AD-inrichtingsservice genereert geen gebruikersgegevens en heeft geen onafhankelijke controle over welke persoonlijke gegevens worden verzameld en hoe deze wordt gebruikt. Het ophalen van gegevens, aggregatie, analyse en rapportage in Azure AD-inrichtingsservice zijn gebaseerd op bestaande gegevens op ondernemingsniveau.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

Met betrekking tot het bewaren van gegevens, de Azure AD-inrichtingsservice niet genereren van rapporten, analyses uitvoeren, of inzichten langer dan 30 dagen te kunnen bieden. Daarom Azure AD-inrichtingsservice niet opslaan, verwerken of alle gegevens langer dan 30 dagen bewaren. Dit ontwerp is compatibel met de GDPR voorschriften, Microsoft privacy de regelgeving en bewaarbeleid voor Azure AD-gegevens.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)
* [Meer informatie over het configureren van eenmalige aanmelding tussen Workday en Azure Active Directory](workday-tutorial.md)
* [Meer informatie over andere SaaS-toepassingen integreren met Azure Active Directory](tutorial-list.md)
* [Informatie over het gebruik van Microsoft Graph-API's voor het beheren van inrichting configuraties](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
