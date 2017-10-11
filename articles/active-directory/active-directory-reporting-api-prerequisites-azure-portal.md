---
title: Vereisten voor toegang tot de Azure AD rapportage-API | Microsoft Docs
description: Meer informatie over de vereisten voor toegang tot de Azure AD rapportage-API
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5fafd83c337e3c73260d89cdad7409a01ce5855b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Vereisten voor toegang tot de Azure AD rapportage-API

De [Azure AD rapportage-API's](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) u programmatische toegang bieden tot de gegevens via een set op basis van REST-API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

De rapportage-API maakt gebruikt [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) toegang verlenen aan de web-API's. 

Als u toegang tot de rapportagegegevens via de API, moet u een van de volgende rollen toegewezen:

- Beveiliging lezer
- De beheerder beveiliging
- Globale beheerder


Als u met het voorbereiden van uw toegang tot de rapportage-API, moet u het volgende doen:

1. Een toepassing registreren 
2. Machtigingen toekennen 
3. Verzamelen van configuratie-instellingen 

Voor vragen, problemen of feedback, neemt u [een ondersteuningsticket bestand](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="register-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing registreren

U moet een app registreren, zelfs als u de rapportage-API met een script opent. Hiermee krijgt u een **toepassings-ID**, die is vereist voor een aanroep van de autorisatie en kunnen uw code te ontvangen van tokens.

Voor het configureren van uw directory voor toegang tot de Azure AD rapportage-API, moet u zich aanmelden bij de Azure-portal met een Azure-beheerdersaccount die ook lid zijn van de **hoofdbeheerder** directory-rol in uw Azure AD-tenant.

> [!IMPORTANT]
> Toepassingen die worden uitgevoerd onder-referenties met bevoegdheden als volgt 'admin' kunnen zeer krachtig zijn, dus zorg ervoor dat de toepassing-ID/geheim referenties veilig te houden.
> 


**Een Azure Active Directory-toepassing registreren:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Op de **Azure Active Directory** blade, klikt u op **App registraties**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. Op de **App registraties** blade in de werkbalk bovenaan, klikt u op **registratie van de nieuwe toepassing**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. Op de **maken** blade, voer de volgende stappen uit:

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    a. In de **naam** textbox type `Reporting API application`.

    b. Als **toepassingstype**, selecteer **Web-app / API**.

    c. In de **aanmeldings-URL** textbox type `https://localhost`.

    d. Klik op **Create**. 


## <a name="grant-permissions"></a>Machtigingen toekennen 

Het doel van deze stap is het verlenen van uw toepassing **mapgegevens lezen** machtigingen voor de **Windows Azure Active Directory** API.

![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

**Uw toepassing om machtiging te verlenen aan de API gebruiken:**

1. Op de **App registraties** blade in de lijst met apps, klikt u op **rapportage-API-toepassing**.

2. Op de **rapportage-API-toepassing** blade in de werkbalk bovenaan, klikt u op **instellingen**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. Op de **instellingen** blade, klikt u op **vereist machtigingen**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. Op de **vereist machtigingen** blade in de **API** lijst, klikt u op **Windows Azure Active Directory**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. Op de **toegang inschakelen** blade Selecteer **mapgegevens lezen**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. Klik in de werkbalk bovenaan op **opslaan**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a>Verzamelen van configuratie-instellingen 
Deze sectie wordt beschreven hoe u de volgende instellingen van uw directory ophalen:

* Domeinnaam
* Client-ID
* Clientgeheim

U moet deze waarden bij het configureren van de rapportage-API aanroepen. 

### <a name="get-your-domain-name"></a>Uw domeinnaam ophalen

**Uw domeinnaam ophalen:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Op de **Azure Active Directory** blade, klikt u op **domeinnamen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. Kopieer uw domeinnaam van de lijst met domeinen.


### <a name="get-your-applications-client-id"></a>Haal client-ID van uw toepassing

**Ophalen van uw toepassing de client-ID:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Op de **App registraties** blade in de lijst met apps, klikt u op **rapportage-API-toepassing**.

3. Op de **rapportage-API-toepassing** blade op de **toepassings-ID**, klikt u op **Klik hier om te kopiëren**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a>Uw toepassing clientgeheim ophalen
Als u uw toepassing clientgeheim, moet u een nieuwe sleutel maken en opslaan van de waarde bij het opslaan van de nieuwe sleutel, omdat het is niet mogelijk is deze waarde later meer ophalen.

**Ophalen van uw toepassing clientgeheim:**

1. In de [Azure-portal](https://portal.azure.com), klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Op de **App registraties** blade in de lijst met apps, klikt u op **rapportage-API-toepassing**.


3. Op de **rapportage-API-toepassing** blade in de werkbalk bovenaan, klikt u op **instellingen**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. Op de **instellingen** blade in de **APIR toegang** sectie, klikt u op **sleutels**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. Op de **sleutels** blade, voer de volgende stappen uit:

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    a. In de **beschrijving** textbox type `Reporting API`.

    b. Als **verloopt**, selecteer **In 2 jaar**.

    c. Klik op **Opslaan**.

    d. Kopieer de waarde van de sleutel.


## <a name="next-steps"></a>Volgende stappen
* Wilt u de toegang tot de gegevens van de Azure AD rapportage-API op een programmatische manier? Bekijk [aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).
* Als u meer informatie over Azure Active Directory-rapportage wilt, raadpleegt u de [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  

