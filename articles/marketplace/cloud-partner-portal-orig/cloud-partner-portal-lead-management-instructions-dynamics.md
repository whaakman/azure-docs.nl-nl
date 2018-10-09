---
title: Dynamics CRM | Microsoft Docs
description: Beheer van leads voor Dynamics CRM configureren.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 8c432146d33db992a0ae612dfc56ace9460ade17
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48870856"
---
# <a name="configure-lead-management-for-dynamics-crm-online"></a>Leadbeheer configureren voor Dynamics CRM online

In dit artikel wordt beschreven hoe u Dynamics CRM Online voor het verwerken van verkoopleads instellen.

## <a name="prerequisites"></a>Vereisten

De volgende gebruikersmachtigingen zijn nodig voor het voltooien van de stappen in dit artikel:
- U moet een beheerder zijn op uw Dynamics CRM Online-instantie voor het installeren van een oplossing.
- U moet een tenantbeheerder om te maken van een nieuw serviceaccount voor lead-service.

<a name="install-the-solution"></a>De oplossing installeren
--------------------

1.  Download de [Microsoft Marketplace leiden Writer oplossing](https://mpsapiprodwus.blob.core.windows.net/documentation/MicrosoftMarketplacesLeadIntegrationSolution_1_0_0_0_target_CRM_6.1_managed.zip) en lokaal opslaan.

2.  Open Dynamics CRM Online en Ga naar instellingen.
    >[!NOTE]
    >Als u de opties in de volgende schermopname niet ziet, hebt u de machtigingen die u nodig hebt.
 
       ![Dynamics-instelling weergeven](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline1.png)

3.  Selecteer **importeren**, en selecteer vervolgens de oplossing die u hebt gedownload in stap 1.
 
    ![Dynamics importeren optie](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline2.png)

4.  Voltooi de installatie van de oplossing.

## <a name="configure-user-permissions"></a>Gebruikersmachtigingen configureren

Leads in uw Dynamics CRM-exemplaar dat u hebt een service-account met ons delen en machtigingen voor het account configureren om te schrijven.

Gebruik de volgende stappen uit de serviceaccount maken en toewijzen van machtigingen. U kunt **Azure Active Directory** of **Office 365**.

### <a name="azure-active-directory"></a>Azure Active Directory

Deze optie wordt aangeraden omdat u het voordeel van nooit hoeven bijwerken van uw gebruikersnaam en wachtwoord wilt blijven ontvangen leads. Voor het gebruik van de Azure Active Directory-optie bieden u de App-Id, de sleutel van de toepassing en de map-Id van uw Active Directory-toepassing.

Gebruik de volgende stappen voor het configureren van Azure Active Directory voor Dynamics CRM.

1.  Aanmelden bij [Azure-portal](https://portal.azure.com/) en selecteer vervolgens de Azure Active Directory-service.

2.  Selecteer **eigenschappen** en kopieer vervolgens de **map-Id**. Dit is uw tenant-account-id die u nodig hebt in de Cloud Partner-Portal gebruiken.
    ![Map-ID ophalen](./media/cloud-partner-portal-lead-management-instructions-dynamics/directoryid.png)

3.  Selecteer **App-registraties**, en selecteer vervolgens **nieuwe toepassing registreren**.
4.  Voer de toepassingsnaam in.
5.  Selecteer bij Type **Web-app / API**.
6.  Geef een URL. Dit veld niet nodig is voor de leads, maar is vereist voor het maken van een toepassing.
7. Selecteer **Maken**.
8.  Nu dat uw toepassing is geregistreerd, selecteer **eigenschappen** en selecteer vervolgens **kopiëren van de toepassings-Id**. U gebruikt deze verbindingsgegevens in de Cloud Partner-Portal.
9.  In de eigenschappen van de toepassing instellen als een multitenant en selecteer vervolgens **opslaan**.

10. Selecteer **sleutels** en maak een nieuwe sleutel met de duur is ingesteld op *verloopt nooit*. Selecteer **opslaan** om de sleutel te maken. 
11. Selecteer in het menu sleutels **Kopieer de sleutelwaarde.** Sla een kopie van deze waarde, omdat u hebt deze nodig voor de Cloud Partner-Portal.
    
    ![Dynamics ophalen geregistreerde sleutel](./media/cloud-partner-portal-lead-management-instructions-dynamics/registerkeys.png)
12. Selecteer **vereiste machtigingen** en selecteer vervolgens **toevoegen**. 
13. Selecteer **Dynamics CRM Online** als de nieuwe API, en controleer de machtiging voor *toegang CRM Online als organisatiegebruikers*.

14. In Dynamics CRM, gaat u naar gebruikers en selecteer de vervolgkeuzelijst 'Gebruikers ingeschakeld' overschakelen op **toepassingsgebruikers**.
    
    ![Gebruikers van de toepassing](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuserfirst.PNG)

15. Selecteer **nieuw** om een nieuwe gebruiker te maken. Selecteer de **gebruiker: TOEPASSINGSGEBRUIKER** vervolgkeuzelijst.
    
    ![Nieuwe toepassingsgebruiker toevoegen](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuser.PNG)

16. In **nieuwe gebruiker**, geeft u de naam en e-dat u wilt gebruiken voor deze verbinding. Plak het **toepassings-Id** voor de app die u hebt gemaakt in Azure portal.
     ![Nieuwe gebruiker configureren](./media/cloud-partner-portal-lead-management-instructions-dynamics/leadgencreateuser.PNG)

17. Ga naar 'Instellingen' in dit artikel voor het configureren van de verbinding voor deze gebruiker is voltooid.

### <a name="office-365"></a>Office 365

Als u niet wilt gebruiken van Azure Active Directory, kunt u een nieuwe gebruiker op de Office 365-beheerportal kunt registreren. U zult zijn vereist voor het bijwerken van uw gebruikersnaam en wachtwoord om de 90 dagen om door te gaan met het ophalen van leads.

Gebruik de volgende stappen voor het configureren van Office 365 voor Dynamics CRM.

1. Aanmelden bij de [Microsoft Office 365-beheerportal](https://go.microsoft.com/fwlink/?LinkId=225975).

2. Selecteer de **Admin** tegel ![beheerder van Office Online](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline3.png)

3. Selecteer **toevoegen van een gebruiker**.

    ![Een gebruiker toevoegen](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline4.png)

4. Maak een nieuwe gebruiker voor de lead writer-service. Configureer de volgende instellingen:

    -   Geef een wachtwoord op en schakel de optie 'Maak deze gebruiker het wachtwoord wijzigen wanneer ze zich eerst aanmelden'.
    -   Selecteer 'Gebruiker (geen beheerderstoegang)' als de rol voor de gebruiker.
    -   Selecteer de productlicentie die wordt weergegeven in de volgende schermopname. In rekening gebracht voor de licentie die u kiest. De oplossing werkt ook met Dynamics CRM Online Basic-licentie.
    ![Gebruikersmachtigingen en licentie configureren](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline5.png)

## <a name="security-settings"></a>Beveiligingsinstellingen

De laatste stap is om in te schakelen van de gebruiker die u hebt gemaakt voor het schrijven van de leads.

1.  Aanmelden bij Dynamics CRM online.
2.  Op **instellingen**, selecteer **Security**.
    
    ![Beveiligingsinstellingen](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline6.png)

3.  Selecteer de gebruiker die u hebt gemaakt in **gebruikersmachtigingen**, en selecteer vervolgens **gebruikersrollen beheren**. Controleer **Microsoft Marketplace leiden Writer** aan de rol toe te wijzen.
    ![Gebruikersrollen toewijzen](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline7.png)\

    >[!NOTE]
    >Deze rol is gemaakt door de oplossing geïmporteerd uit te voeren en alleen machtigingen om te schrijven van de leads en om bij te houden van de versie van de oplossing om ervoor te zorgen compatibiliteit heeft.

4.  Selecteer in de beveiliging, **beveiligingsrollen** en zoek de rol van de Microsoft Marketplace leiden Writer.
    ![Beveiliging lead schrijver configureren](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline10.jpg)\

5. Selecteer de **Kernrecords** tabblad. Schakel maken/lezen/schrijven voor de gebruikersentiteit van gebruikersinterface.

    ![Maken/lezen/schrijven voor de gebruiker inschakelen](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline11.jpg)\

## <a name="wrap-up"></a>Samenvatten

Klaar bent met het configureren van Dynamics CRM voor lead management door toe te voegen van de gegenereerde accountgegevens voor de Cloud Partner-Portal. Bijvoorbeeld:

-   **Azure Active Directory** - **toepassings-Id** (voorbeeld: *23456052-aaaa-bbbb-8662-1234df56788f*), **map-Id** (voorbeeld: *12345678-8af1-4asf-1234-12234d01db47*), en **Toepassingssleutel** (voorbeeld: *1234ABCDEDFRZ/G/FdY0aUABCEDcqhbLn/ST122345nBc =*).
-   **Office 365** - **Url** (voorbeeld: *https://contoso.crm4.dynamics.com*), **gebruikersnaam** (voorbeeld: *contoso\@ Contoso.onmicrosoft.com*), en **wachtwoord** (voorbeeld: *P\@ssw0rd*).
