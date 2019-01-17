---
title: Vereisten voor toegang tot de Azure Active Directory reporting API | Microsoft Docs
description: Meer informatie over de vereisten voor toegang tot de rapportage-API van Azure AD
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 03acd7c283fd1296af06dd19d0170a4b3c65eeb3
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352492"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Vereisten voor toegang tot de Azure Active Directory reporting API

De [Azure Active Directory (Azure AD) rapportage-API's](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

De rapportage-API maakt gebruikt [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) toegang verlenen aan de web-API's.

Om voor te bereiden uw toegang tot de rapportage-API, moet u naar:

1. [Rollen toewijzen](#assign-roles)
2. [Een toepassing registreren](#register-an-application)
3. [Machtigingen verlenen](#grant-permissions)
4. [Verzamelen van configuratie-instellingen](#gather-configuration-settings)

## <a name="assign-roles"></a>Rollen toewijzen

Voor toegang tot de rapportagegegevens via de API, moet u beschikken over een van de volgende rollen toegewezen:

- Beveiligingslezer

- Beveiligingsbeheerder

- Globale beheerder


## <a name="register-an-application"></a>Een toepassing registreren

U moet een toepassing registreert, zelfs als u de rapportage-API met behulp van een script. Dit biedt u een **toepassings-ID**, die is vereist voor de autorisatie-aanroepen en kunt uw code voor het ontvangen van tokens.

Voor het configureren van uw directory voor toegang tot de rapportage-API van Azure AD, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com) met een Azure-beheerder-account dat is ook lid is van de **hoofdbeheerder** directory-rol in uw Azure AD-tenant.

> [!IMPORTANT]
> Toepassingen die worden uitgevoerd onder referenties met administrator-bevoegdheden kunnen zeer krachtig zijn, dus houd er rekening te houden van de toepassing-ID en geheim referenties op een veilige locatie.
> 

**Het registreren van een Azure AD-toepassing:**

1. In de [Azure-portal](https://portal.azure.com), selecteer **Azure Active Directory** in het navigatiedeelvenster links.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. In de **Azure Active Directory** weergeeft, schakelt **App-registraties**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Uit de **App-registraties** weergeeft, schakelt **nieuwe toepassing registreren**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. In de **maken** pagina, voert u de volgende stappen uit:

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. In de **naam** tekstvak, type `Reporting API application`.

    b. Als **toepassingstype**, selecteer **Web-app / API**.

    c. In de **aanmeldings-URL** tekstvak, type `https://localhost`.

    d. Selecteer **Maken**. 


## <a name="grant-permissions"></a>Machtigingen verlenen 

Afhankelijk van de API die u wilt openen, moet u uw app in de volgende machtigingen te verlenen:  

| API | Machtiging |
| --- | --- |
| Windows Azure Active Directory | Adreslijstgegevens lezen |
| Microsoft Graph | Lees dat alle logboekgegevens controleren |


![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/36.png)

De volgende sectie bevat de stappen voor beide API's. Als u niet dat toegang tot een van de API's wilt, kunt u de bijbehorende stappen overslaan.

**Uw om Toepassingsmachtigingen te verlenen aan de API's gebruiken:**

1. Selecteer uw toepassing uit de **App-registraties** pagina en selecteer **instellingen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. Op de **instellingen** weergeeft, schakelt **vereiste machtigingen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. Op de **vereiste machtigingen** pagina, in de **API** lijst, klikt u op **Windows Azure Active Directory**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. Op de **toegang inschakelen** weergeeft, schakelt **mapgegevens lezen** en schakelt u **aanmelden en gebruikersprofiel lezen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Klik in de werkbalk bovenaan op **opslaan**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/15.png)

6. Op de **vereiste machtigingen** in de werkbalk bovenaan op de pagina, klikt u op **toevoegen**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/32.png)

7. Op de **API-toegang toevoegen** pagina, klikt u op **Select an API**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/31.png)

8. Op de **Select an API** pagina, klikt u op **Microsoft Graph**, en klik vervolgens op **Selecteer**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/33.png)

9. Op de **toegang inschakelen** weergeeft, schakelt **lezen alle gegevens van een audit**, en klik vervolgens op **Selecteer**.  

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/34.png)

10. Op de **API-toegang toevoegen** pagina, klikt u op **gedaan**.  

11. Op de **vereiste machtigingen** in de werkbalk bovenaan de pagina. Klik op **machtigingen verlenen**, en klik vervolgens op **Ja**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/17.png)


## <a name="gather-configuration-settings"></a>Verzamelen van configuratie-instellingen 

Deze sectie leest u hoe u de volgende instellingen van uw directory:

- Domeinnaam
- Client-id
- Clientgeheim

U hebt deze waarden nodig bij het configureren van aanroepen naar de rapportage-API. 

### <a name="get-your-domain-name"></a>Uw domeinnaam ophalen

**Uw domeinnaam ophalen:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Op de **Azure Active Directory** weergeeft, schakelt **aangepaste-domeinnamen**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Kopieer de domeinnaam van uw uit de lijst met domeinen.


### <a name="get-your-applications-client-id"></a>Client-ID van uw toepassing ophalen

**Ophalen van uw toepassing de client-ID:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Selecteer uw toepassing uit de **App-registraties** pagina.

3. Vanuit de toepassingspagina, navigeert u naar **toepassings-ID** en selecteer **Klik om te kopiëren**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Clientgeheim van uw toepassing ophalen
Als u het clientgeheim van uw toepassing, moet u een nieuwe sleutel maken en opslaan van de waarde bij het opslaan van de nieuwe sleutel omdat het is niet mogelijk om op te halen deze waarde later niet meer.

**Ophalen van het clientgeheim van uw toepassing:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Selecteer uw toepassing uit de **App-registraties** pagina.

3. Selecteer op de toepassingspagina, in de werkbalk bovenaan op de **instellingen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. Op de **instellingen** pagina, in de **API-toegang** sectie, klikt u op **sleutels**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. Op de **sleutels** pagina, voert u de volgende stappen uit:

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/14.png)

    a. In de **beschrijving** tekstvak, type `Reporting API`.

    b. Als **verloopt**, selecteer **na twee jaar**.

    c. Klik op **Opslaan**.

    d. Kopieer de sleutelwaarde.

## <a name="troubleshoot-errors-in-the-reporting-api"></a>Oplossen van fouten in de rapportage-API

Deze sectie vindt u de algemene foutberichten die u ondervindt mogelijk bij het openen van de activiteitenrapporten met behulp van de MS Graph API en de stappen voor de oplossing.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>500 HTTP-interne serverfout opgetreden tijdens het openen van Microsoft Graph-V2-eindpunt

We ondersteunen momenteel geen de v2-eindpunt voor Microsoft Graph - Zorg ervoor dat u toegang tot de activiteitenlogboeken met behulp van de Microsoft Graph-v1-eindpunt.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Fout: Kan geen gebruikersrollen ophalen uit AD Graph

U mag deze foutmelding krijgt bij het openen van aanmeldingen met behulp van Graph Explorer. Zorg ervoor dat u bent aangemeld bij uw account met behulp van de knoppen voor aanmelding bij in de Graph Explorer-gebruikersinterface, zoals wordt weergegeven in de volgende afbeelding. 

![Graph Explorer](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Fout: Kan geen premium-licentie om uit te voeren vanuit AD Graph 

Als u dit foutbericht opgetreden tijdens het openen van aanmeldingen met behulp van Graph Explorer, kies **wijzigingsmachtigingen** onder uw account in het navigatievenster aan de linkerkant en selecteer **Tasks.ReadWrite** en **Directory.Read.All**. 

![Machtigingen voor gebruikersinterface wijzigen](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Fout: De tenant is B2C noch tenant heeft geen premium-licentie

Toegang tot rapporten-aanmelding vereist een Azure Active Directory premium 1 (P1) licentie. Als u dit foutbericht ziet tijdens het openen van aanmeldingen, zorg ervoor dat uw tenant in licentie met een Azure AD P1-licentie gegeven wordt.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Fout: Gebruiker is niet in de toegestane rollen 

Als u dit foutbericht ziet opgetreden tijdens het openen van de logboeken voor controle of aanmeldingen met behulp van de API, zorg ervoor dat uw account deel van uitmaakt de **Beveiligingslezer** of **rapportlezer** rol in uw Azure Active Directory tenant. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Fout: Toepassing AAD 'directory-gegevens lezen-machtiging ontbreekt 

Volg de stappen in de [vereisten voor toegang tot de Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md) om te controleren of uw toepassing wordt uitgevoerd met de juiste set machtigingen. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Fout: Toepassing MSGraph API 'Lezen alle logboekgegevens controleren' machtiging ontbreekt

Volg de stappen in de [vereisten voor toegang tot de Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md) om te controleren of uw toepassing wordt uitgevoerd met de juiste set machtigingen. 

## <a name="next-steps"></a>Volgende stappen

* [Ophalen van gegevens met behulp van de Azure Active Directory reporting API met certificaten](tutorial-access-api-with-certificates.md)
* [Controle van de API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Rapport van aanmeldingsactiviteiten API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
