---
title: Vereisten voor toegang tot de Azure Active Directory rapportage-API | Microsoft Docs
description: Meer informatie over de vereisten voor toegang tot de Azure AD rapportage-API
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/03/2018
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 72372a37c4ddcc6135fc7477807e4ff94ae90c77
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Vereisten voor toegang tot de Azure Active Directory rapportage-API

De [Azure Active Directory (Azure AD) API's reporting](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) u programmatische toegang bieden tot de gegevens via een set op basis van REST-API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

De rapportage-API maakt gebruikt [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) toegang verlenen aan de web-API's. 

Als u toegang tot de rapportagegegevens via de API, moet u een van de volgende rollen toegewezen:

- Beveiligingslezer
- Beveiligingsbeheerder
- Globale beheerder


Als u met het voorbereiden van uw toegang tot de rapportage-API, moet u het volgende doen:

1. Een toepassing registreren 
2. Machtigingen verlenen 
3. Verzamelen van configuratie-instellingen 

Voor vragen, problemen of feedback, [een ondersteuningsticket bestand](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="register-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing registreren

U moet een app registreren, zelfs als u bij het openen van de rapportage-API met een script. Hiermee krijgt u een **toepassings-ID**, die is vereist voor een aanroep van de autorisatie en kunnen uw code te ontvangen van tokens.

Voor het configureren van uw directory voor toegang tot de Azure AD rapportage-API, moet u zich aanmelden bij de Azure-portal met een Azure-beheerdersaccount die ook lid zijn van de **hoofdbeheerder** directory-rol in uw Azure AD-tenant.

> [!IMPORTANT]
> Toepassingen die worden uitgevoerd onder-referenties met bevoegdheden als volgt 'admin' kunnen zeer krachtig zijn, dus zorg ervoor dat de toepassing-ID/geheim referenties veilig te houden.
> 


**Een Azure Active Directory-toepassing registreren:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Op de **Azure Active Directory** pagina, klikt u op **App registraties**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. Op de **App registraties** pagina in de werkbalk bovenaan, klikt u op **registratie van de nieuwe toepassing**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. Op de **maken** pagina, voert u de volgende stappen uit:

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    a. In de **naam** textbox type `Reporting API application`.

    b. Als **toepassingstype**, selecteer **Web-app / API**.

    c. In de **aanmeldings-URL** textbox type `https://localhost`.

    d. Klik op **Create**. 


## <a name="grant-permissions"></a>Machtigingen verlenen 

Afhankelijk van de API die u wilt openen, moet u uw app in de volgende machtigingen verlenen:  

| API | Machtiging |
| --- | --- |
| Windows Azure Active Directory | Mapgegevens lezen |
| Microsoft Graph | Lees dat alle logboekgegevens controleren |


![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/36.png)


De volgende sectie bevat de stappen voor beide API's. Als u niet wilt voor toegang tot een van de API's, kunt u de bijbehorende stappen overslaan.
 

**Uw om Toepassingsmachtigingen te verlenen aan de API's gebruiken:**

1. Op de **App registraties** pagina in de lijst met apps, klikt u op **rapportage-API-toepassing**.

2. Op de **rapportage-API-toepassing** pagina in de werkbalk bovenaan, klikt u op **instellingen**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. Op de **instellingen** pagina, klikt u op **vereist machtigingen**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. Op de **vereist machtigingen** pagina in de **API** lijst, klikt u op **Windows Azure Active Directory**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. Op de **toegang inschakelen** pagina **mapgegevens lezen** en schakelt u **aanmelden en gebruikersprofiel lezen**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. Klik in de werkbalk bovenaan op **opslaan**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

7. Op de **vereist machtigingen** pagina in de werkbalk bovenaan, klikt u op **toevoegen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/32.png)

8. Op de **API toevoegen toegang** pagina, klikt u op **selecteert u een API**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/31.png)

9. Op de **selecteert u een API** pagina, klikt u op **Microsoft Graph**, en klik vervolgens op **Selecteer**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/33.png)

10. Op de **toegang inschakelen** pagina **lezen alle logboekgegevens audit**, en klik vervolgens op **Selecteer**.  

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/34.png)


11. Op de **API toevoegen toegang** pagina, klikt u op **gedaan**.  

12. Op de **vereist machtigingen** pagina in de werkbalk bovenaan. Klik op **machtiging verlenen**, en klik vervolgens op **Ja**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/17.png)


## <a name="gather-configuration-settings"></a>Verzamelen van configuratie-instellingen 
Deze sectie wordt beschreven hoe u de volgende instellingen van uw directory ophalen:

* Domeinnaam
* Client-id
* Clientgeheim

U moet deze waarden bij het configureren van de rapportage-API aanroepen. 

### <a name="get-your-domain-name"></a>Uw domeinnaam ophalen

**Uw domeinnaam ophalen:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Op de **Azure Active Directory** pagina, klikt u op **aangepaste domeinnamen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. Kopieer uw domeinnaam van de lijst met domeinen.


### <a name="get-your-applications-client-id"></a>Haal client-ID van uw toepassing

**Ophalen van uw toepassing de client-ID:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Op de **App registraties** pagina in de lijst met apps, klikt u op **rapportage-API-toepassing**.

3. Op de **rapportage-API-toepassing** pagina op de **toepassings-ID**, klikt u op **Klik hier om te kopiëren**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a>Uw toepassing clientgeheim ophalen
Als u uw toepassing clientgeheim, moet u een nieuwe sleutel maken en opslaan van de waarde bij het opslaan van de nieuwe sleutel, omdat het is niet mogelijk is deze waarde later meer ophalen.

**Ophalen van uw toepassing clientgeheim:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Op de **App registraties** pagina in de lijst met apps, klikt u op **rapportage-API-toepassing**.


3. Op de **rapportage-API-toepassing** pagina in de werkbalk bovenaan, klikt u op **instellingen**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. Op de **instellingen** pagina in de **APIR toegang** sectie, klikt u op **sleutels**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. Op de **sleutels** pagina, voert u de volgende stappen uit:

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    a. In de **beschrijving** textbox type `Reporting API`.

    b. Als **verloopt**, selecteer **In 2 jaar**.

    c. Klik op **Opslaan**.

    d. Kopieer de waarde van de sleutel.


## <a name="next-steps"></a>Volgende stappen
* Wilt u de toegang tot de gegevens van de Azure AD rapportage-API op een programmatische manier? Bekijk [aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).
* Als u meer informatie over Azure Active Directory-rapportage wilt, raadpleegt u de [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  

