---
title: Vereisten voor toegang tot de rapportage-API van Azure Active Directory | Microsoft Docs
description: Meer informatie over de vereisten voor toegang tot de Azure AD Reporting-API
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: f925a86504d68fd08b83c63e4da8b37b4aa25f85
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68989905"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Vereisten voor toegang tot de API voor Azure Active Directory rapportage

De [Azure Active Directory (Azure AD) rapportage-API's](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

De rapportage-API maakt gebruik van [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) om toegang tot de Web-api's te verlenen.

U moet het volgende doen om de toegang tot de rapportage-API voor te bereiden:

1. [Rollen toewijzen](#assign-roles)
2. [Een toepassing registreren](#register-an-application)
3. [Machtigingen verlenen](#grant-permissions)
4. [Configuratie-instellingen verzamelen](#gather-configuration-settings)

## <a name="assign-roles"></a>Rollen toewijzen

Als u toegang wilt krijgen tot de rapportage gegevens via de API, moet een van de volgende rollen zijn toegewezen:

- Beveiligingslezer

- Beveiligingsbeheerder

- Globale beheerder


## <a name="register-an-application"></a>Een toepassing registreren

U moet een toepassing registreren, zelfs als u gebruikmaakt van de rapportage-API met behulp van een script. Dit geeft u een **toepassings-id**die is vereist voor de autorisatie aanroepen en de code in staat stelt om tokens te ontvangen.

Als u uw Directory wilt configureren voor toegang tot de Azure AD Reporting-API, moet u zich aanmelden bij de [Azure Portal](https://portal.azure.com) met een Azure-beheerders account dat ook lid is van de rol **globale beheerder** in uw Azure AD-Tenant.

> [!IMPORTANT]
> Toepassingen die worden uitgevoerd onder referenties met beheerders bevoegdheden, kunnen zeer krachtig zijn. Zorg er dus voor dat u de ID en geheime referenties van de toepassing op een veilige locatie behoudt.
> 

**Een Azure AD-toepassing registreren:**

1. Selecteer **Azure Active Directory** in het navigatie deel venster links In het [Azure Portal](https://portal.azure.com).
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Selecteer op de pagina **Azure Active Directory** **app-registraties**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Selecteer op de pagina **app-registraties** de optie **nieuwe toepassing registreren**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. Voer de volgende stappen uit op de pagina **maken** :

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. Typ`Reporting API application`in het tekstvak **naam** .

    b. Selecteer **Web-app/API**als **toepassings type**.

    c. Typ`https://localhost`in het tekstvak **URL voor aanmelden** .

    d. Selecteer **Maken**. 


## <a name="grant-permissions"></a>Machtigingen verlenen 

Afhankelijk van de API die u wilt gebruiken, moet u uw app de volgende machtigingen verlenen:  

| API | Machtiging |
| --- | --- |
| Windows Azure Active Directory | Adreslijstgegevens lezen |
| Microsoft Graph | Alle audit logboek gegevens lezen |


![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/36.png)

In de volgende sectie worden de stappen voor beide Api's vermeld. Als u geen toegang tot een van de Api's wilt, kunt u de gerelateerde stappen overs Laan.

**Uw toepassings machtigingen verlenen voor het gebruik van de Api's:**

1. Selecteer uw toepassing op de pagina **app** -registraties en selecteer **instellingen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. Selecteer op de pagina **instellingen** de optie **vereiste machtigingen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. Klik op de pagina **vereiste machtigingen** in de lijst **API** op **Windows Azure Active Directory**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. Selecteer op de pagina **toegang inschakelen** de optie **Directory gegevens lezen** en Schakel **Aanmelden en gebruikers profiel lezen uit**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Klik in de werk balk bovenaan op **Opslaan**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/15.png)

6. Klik op de pagina **vereiste machtigingen** in de werk balk aan de bovenkant op **toevoegen**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/32.png)

7. Klik op de pagina **API-toegang toevoegen** op **een API selecteren**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/31.png)

8. Klik op de pagina **een API selecteren** op **Microsoft Graph**en klik vervolgens op **selecteren**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/33.png)

9. Selecteer op de pagina **toegang inschakelen** de optie **alle audit logboek gegevens lezen**en klik vervolgens op **selecteren**.  

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/34.png)

10. Klik op de pagina **API-toegang toevoegen** op **gereed**.  

11. Op de pagina **vereiste machtigingen** in de werk balk aan de bovenkant. Klik op **machtigingen verlenen**en klik vervolgens op **Ja**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/17.png)


## <a name="gather-configuration-settings"></a>Configuratie-instellingen verzamelen 

In deze sectie wordt beschreven hoe u de volgende instellingen uit uw Directory kunt ophalen:

- Domeinnaam
- Client-id
- Clientgeheim

U hebt deze waarden nodig bij het configureren van aanroepen naar de rapportage-API. 

### <a name="get-your-domain-name"></a>Uw domein naam ophalen

**Uw domein naam ophalen:**

1. Selecteer **Azure Active Directory**in de [Azure Portal](https://portal.azure.com)in het navigatie deel venster links.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Selecteer op de pagina **Azure Active Directory** **aangepaste domein namen**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Kopieer uw domein naam uit de lijst met domeinen.


### <a name="get-your-applications-client-id"></a>De client-ID van uw toepassing ophalen

**De client-ID van uw toepassing ophalen:**

1. Klik in het [Azure Portal](https://portal.azure.com)op **Azure Active Directory**in het navigatie deel venster aan de linkerkant.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Selecteer uw toepassing op de pagina **app** -registraties.

3. Ga op de pagina toepassing naar **toepassings-id** en selecteer **klikken om te kopiëren**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Het client geheim van uw toepassing ophalen
Als u het client geheim van uw toepassing wilt ophalen, moet u een nieuwe sleutel maken en de waarde ervan opslaan bij het opslaan van de nieuwe sleutel, omdat het niet mogelijk is om deze waarde later meer op te halen.

**Het client geheim van uw toepassing ophalen:**

1. Klik in het [Azure Portal](https://portal.azure.com)op **Azure Active Directory**in het navigatie deel venster aan de linkerkant.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Selecteer uw toepassing op de pagina **app** -registraties.

3. Selecteer op de pagina toepassing in de werk balk bovenaan de optie **instellingen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. Klik op de pagina **instellingen** in de sectie **API-toegang** op **sleutels**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. Voer de volgende stappen uit op de pagina **sleutels** :

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/14.png)

    a. Typ`Reporting API`in het tekstvak **Beschrijving** .

    b. Als **verlopen**, selecteert u **in 2 jaar**.

    c. Klik op **Opslaan**.

    d. Kopieer de sleutel waarde.

## <a name="troubleshoot-errors-in-the-reporting-api"></a>Fouten in de rapportage-API oplossen

In deze sectie vindt u de algemene fout berichten die u kunt uitvoeren bij het openen van activiteiten rapporten met behulp van de MS Graph API en de stappen voor de oplossing.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>500 interne HTTP-server fout bij het openen van Microsoft Graph v2-eind punt

Het Microsoft Graph v2-eind punt wordt momenteel niet ondersteund. Zorg ervoor dat u toegang hebt tot de activiteiten logboeken met behulp van het Microsoft Graph v1-eind punt.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Fout: Kan de gebruikers rollen niet ophalen uit de AD-grafiek

Dit fout bericht kan worden weer gegeven wanneer u probeert toegang te krijgen tot aanmeldingen met Graph Explorer. Zorg ervoor dat u bent aangemeld bij uw account met behulp van beide aanmeld knoppen in de interface van Graph Verkenner, zoals wordt weer gegeven in de volgende afbeelding. 

![Graph Explorer](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Fout: Kan geen Premium-licentie controle uitvoeren vanuit de AD-grafiek 

Als u in dit fout bericht wordt weer gegeven terwijl u probeert toegang te krijgen tot aanmeldingen met behulp van Graph Explorer, kiest u **machtigingen voor wijzigen** onder uw account in het navigatie venster links en selecteert u **taken. readwrite** en **Directory. Read. all**. 

![Machtigingen voor gebruikers interface wijzigen](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Fout: Geen van de tenants is B2C of de Tenant heeft geen Premium-licentie

Voor toegang tot aanmeldings rapporten is een licentie voor Azure Active Directory Premium 1 (P1) vereist. Als dit fout bericht wordt weer gegeven tijdens het openen van de aanmeldingen, moet u ervoor zorgen dat uw Tenant is gelicentieerd met een Azure AD P1-licentie.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Fout: De gebruiker bevindt zich niet in de toegestane rollen 

Als dit fout bericht wordt weer gegeven tijdens het openen van controle Logboeken of-aanmeldingen met behulp van de API, moet u ervoor zorgen dat uw account deel uitmaakt van de rol **beveiligings lezer** of **rapport lezer** in uw Azure Active Directory Tenant. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Fout: De machtiging voor het lezen van mapgegevens van de toepassing AAD ontbreekt 

Volg de stappen in de [vereisten voor toegang tot de API voor Azure Active Directory rapportage](howto-configure-prerequisites-for-reporting-api.md) om ervoor te zorgen dat uw toepassing wordt uitgevoerd met de juiste set machtigingen. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Fout: MSGraph-API voor ontbrekende toepassing alle controle logboek gegevens lezen

Volg de stappen in de [vereisten voor toegang tot de API voor Azure Active Directory rapportage](howto-configure-prerequisites-for-reporting-api.md) om ervoor te zorgen dat uw toepassing wordt uitgevoerd met de juiste set machtigingen. 

## <a name="next-steps"></a>Volgende stappen

* [Gegevens ophalen met behulp van de API voor Azure Active Directory rapportage met certificaten](tutorial-access-api-with-certificates.md)
* [Controle-API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [API-naslag informatie voor aanmeld activiteiten](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
