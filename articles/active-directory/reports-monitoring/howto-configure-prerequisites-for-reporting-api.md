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
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 8d610dc74b7e2ef10295bc0a3407cf7c3d781b51
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "42055256"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Vereisten voor toegang tot de Azure Active Directory reporting API

De [Azure Active Directory (Azure AD) rapportage-API's](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

De rapportage-API maakt gebruikt [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) toegang verlenen aan de web-API's.

Om voor te bereiden uw toegang tot de rapportage-API, moet u naar:

1. Rollen toewijzen
2. Een toepassing registreren
3. Machtigingen verlenen
4. Verzamelen van configuratie-instellingen



## <a name="assign-roles"></a>Rollen toewijzen

Voor toegang tot de rapportagegegevens via de API, moet u beschikken over een van de volgende rollen toegewezen:

- Beveiligingslezer

- Beveiligingsbeheerder

- Globale beheerder




## <a name="register-an-application"></a>Een toepassing registreren

U moet een app te registreren, zelfs als u de rapportage-API met behulp van een script. Dit biedt u een **toepassings-ID**, die is vereist voor een aanroep van de autorisatie en zorgt ervoor dat uw code voor het ontvangen van tokens.

Voor het configureren van uw directory voor toegang tot de rapportage-API van Azure AD, moet u zich aanmelden bij de Azure portal met een Azure-beheerdersaccount die ook lid is van de **hoofdbeheerder** directory-rol in uw Azure AD-tenant.

> [!IMPORTANT]
> Toepassingen die worden uitgevoerd onder referenties met bevoegdheden 'admin' als volgt kunnen worden zeer krachtige, dus zorg ervoor dat u van de toepassing-ID en-geheim voor referenties te beveiligen.
> 


**Het registreren van een Azure Active Directory-toepassing:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Op de **Azure Active Directory** pagina, klikt u op **App-registraties**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Op de **App-registraties** in de werkbalk bovenaan op de pagina, klikt u op **nieuwe toepassing registreren**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. Op de **maken** pagina, voert u de volgende stappen uit:

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. In de **naam** tekstvak, type `Reporting API application`.

    b. Als **toepassingstype**, selecteer **Web-app / API**.

    c. In de **aanmeldings-URL** tekstvak, type `https://localhost`.

    d. Klik op **Create**. 


## <a name="grant-permissions"></a>Machtigingen verlenen 

Afhankelijk van de API die u wilt openen, moet u uw app in de volgende machtigingen te verlenen:  

| API | Machtiging |
| --- | --- |
| Windows Azure Active Directory | Adreslijstgegevens lezen |
| Microsoft Graph | Lees dat alle logboekgegevens controleren |


![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/36.png)


De volgende sectie bevat de stappen voor beide API's. Als u niet dat toegang tot een van de API's wilt, kunt u de bijbehorende stappen overslaan.
 

**Uw om Toepassingsmachtigingen te verlenen aan de API's gebruiken:**

1. Op de **App-registraties** pagina in de lijst met apps, klikt u op **rapportage-API-toepassing**.

2. Op de **rapportage-API-toepassing** in de werkbalk bovenaan op de pagina, klikt u op **instellingen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/05.png)

3. Op de **instellingen** pagina, klikt u op **vereiste machtigingen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/06.png)

4. Op de **vereiste machtigingen** pagina, in de **API** lijst, klikt u op **Windows Azure Active Directory**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/07.png)

5. Op de **toegang inschakelen** weergeeft, schakelt **mapgegevens lezen** en schakelt u **aanmelden en gebruikersprofiel lezen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/08.png)

6. Klik in de werkbalk bovenaan op **opslaan**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/15.png)

7. Op de **vereiste machtigingen** in de werkbalk bovenaan op de pagina, klikt u op **toevoegen**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/32.png)

8. Op de **API-toegang toevoegen** pagina, klikt u op **Select an API**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/31.png)

9. Op de **Select an API** pagina, klikt u op **Microsoft Graph**, en klik vervolgens op **Selecteer**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/33.png)

10. Op de **toegang inschakelen** weergeeft, schakelt **lezen alle gegevens van een audit**, en klik vervolgens op **Selecteer**.  

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/34.png)


11. Op de **API-toegang toevoegen** pagina, klikt u op **gedaan**.  

12. Op de **vereiste machtigingen** in de werkbalk bovenaan de pagina. Klik op **machtigingen verlenen**, en klik vervolgens op **Ja**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/17.png)


## <a name="gather-configuration-settings"></a>Verzamelen van configuratie-instellingen 

Deze sectie leest u hoe u de volgende instellingen van uw directory:

- Domeinnaam
- Client-id
- Clientgeheim

U hebt deze waarden nodig bij het configureren van aanroepen naar de rapportage-API. 

### <a name="get-your-domain-name"></a>Uw domeinnaam ophalen

**Uw domeinnaam ophalen:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Op de **Azure Active Directory** pagina, klikt u op **aangepaste-domeinnamen**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Kopieer de domeinnaam van uw uit de lijst met domeinen.


### <a name="get-your-applications-client-id"></a>Client-ID van uw toepassing ophalen

**Ophalen van uw toepassing de client-ID:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Op de **App-registraties** pagina in de lijst met apps, klikt u op **rapportage-API-toepassing**.

3. Op de **rapportage-API-toepassing** pagina op de **toepassings-ID**, klikt u op **Klik om te kopiëren**.

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/11.png) 



### <a name="get-your-applications-client-secret"></a>Clientgeheim van uw toepassing ophalen
Als u het clientgeheim van uw toepassing, moet u een nieuwe sleutel maken en opslaan van de waarde bij het opslaan van de nieuwe sleutel omdat het is niet mogelijk om op te halen deze waarde later niet meer.

**Ophalen van het clientgeheim van uw toepassing:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Azure Active Directory**.
   
    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Op de **App-registraties** pagina in de lijst met apps, klikt u op **rapportage-API-toepassing**.


3. Op de **rapportage-API-toepassing** in de werkbalk bovenaan op de pagina, klikt u op **instellingen**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. Op de **instellingen** pagina, in de **APIR toegang** sectie, klikt u op **sleutels**. 

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/12.png)


5. Op de **sleutels** pagina, voert u de volgende stappen uit:

    ![Toepassing registreren](./media/howto-configure-prerequisites-for-reporting-api/14.png)

    a. In de **beschrijving** tekstvak, type `Reporting API`.

    b. Als **verloopt**, selecteer **na twee jaar**.

    c. Klik op **Opslaan**.

    d. Kopieer de sleutelwaarde.


## <a name="next-steps"></a>Volgende stappen

* [Ophalen van gegevens met behulp van de Azure Active Directory reporting API met certificaten](tutorial-access-api-with-certificates.md)
* [Een eerste indruk van de rapportage-API 's krijgen](concept-reporting-api.md)
* [Controle van de API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Rapport van aanmeldingsactiviteiten API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
