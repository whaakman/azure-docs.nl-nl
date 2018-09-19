---
title: 'Verificatie van eindgebruikers: Azure Data Lake Storage Gen1 met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van eindgebruikersverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 2f0638b2449bfd582cb68e26d2043b7bc85342b6
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125949"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Eindgebruikersverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory
> [!div class="op_single_selector"]
> * [Verificatie van de eindgebruiker](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Verificatie van service-tot-service](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Storage Gen1 gebruikt Azure Active Directory voor verificatie. Vóór het ontwerpen van een toepassing die met Data Lake Storage Gen1 of Azure Data Lake Analytics werkt, moet u bepalen hoe u aan het verifiëren van uw toepassing met Azure Active Directory (Azure AD). De twee belangrijkste opties die beschikbaar zijn:

* Verificatie van eindgebruikers (in dit artikel)
* Service-naar-serviceverificatie (Kies deze optie in de bovenstaande vervolgkeuzelijst)

Deze beide opties leiden tot uw toepassing wordt geleverd met een OAuth 2.0-token opgehaald die zijn gekoppeld aan elke aanvraag aan de Data Lake Storage Gen1 of Azure Data Lake Analytics.

In dit artikel wordt besproken hoe u maakt een **systeemeigen Azure AD-toepassing voor verificatie van eindgebruikers**. Zie voor instructies voor de configuratie van de Azure AD-toepassing voor service-naar-serviceverificatie, [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* Uw abonnements-ID. U kunt het ophalen van de Azure-portal. Het is bijvoorbeeld beschikbaar is via de blade Data Lake Storage Gen1-account.
  
    ![Abonnements-ID ophalen](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* De naam van uw Azure AD-domein. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. In de onderstaande schermafbeelding, de domeinnaam is **contoso.onmicrosoft.com**, en de GUID tussen haakjes is de tenant-ID. 
  
    ![AAD-domein ophalen](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

* Uw Azure-tenant-ID. Zie voor instructies over het ophalen van de tenant-ID [ophalen van de tenant-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="end-user-authentication"></a>Verificatie van de eindgebruiker
Dit verificatiemechanisme is de aanbevolen methode als u wilt dat een eindgebruiker zich aanmelden bij uw toepassing via Azure AD. Uw toepassing is vervolgens toegang krijgen tot Azure-resources met hetzelfde niveau van toegang als de gebruiker die in het logboek geregistreerd. De eindgebruiker moet naar hun referenties periodiek in volgorde voor uw toepassing om toegang te behouden.

Het resultaat dat de aanmelding door eindgebruikers is dat uw toepassing een toegangstoken en een vernieuwingstoken is opgegeven. Het toegangstoken opgehaald die zijn gekoppeld aan elke aanvraag aan de Data Lake Storage Gen1 of Data Lake Analytics en deze is geldig gedurende één uur standaard. Het vernieuwingstoken dat kan worden gebruikt om een nieuw toegangstoken ophalen en deze is geldig voor maximaal twee weken standaard. U kunt twee verschillende methoden gebruiken voor aanmelding door eindgebruikers.

### <a name="using-the-oauth-20-pop-up"></a>Met behulp van het pop-upvenster OAuth 2.0
Uw toepassing kan resulteren in een OAuth 2.0 machtiging pop, waarin de gebruiker zijn referenties kunt invoeren. Dit pop-upvenster werkt ook met het proces van Azure AD tweeledige verificatie (2FA), indien nodig. 

> [!NOTE]
> Deze methode wordt nog niet ondersteund in de Azure AD Authentication Library (ADAL) voor Python of Java.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Het rechtstreeks doorgeven in de referenties van gebruiker
Uw toepassing kan rechtstreeks gebruikersreferenties bieden aan Azure AD. Deze methode werkt alleen met een gebruikersaccount voor de organisatie-ID; het is niet compatibel met persoonlijk / 'live ID' gebruikersaccounts, inclusief de accounts die eindigen op @outlook.com of @live.com. Bovendien is deze methode is niet compatibel is met een gebruikersaccount waarvoor Azure AD tweeledige verificatie (2FA).

### <a name="what-do-i-need-for-this-approach"></a>Wat heb ik nodig voor deze benadering?
* Azure AD-domeinnaam. Deze vereiste is al opgenomen in de vereisten van dit artikel.
* Azure AD-tenant-ID. Deze vereiste is al opgenomen in de vereisten van dit artikel.
* Azure AD **systeemeigen toepassing**
* Toepassings-ID voor de systeemeigen Azure AD-toepassing
* Omleidings-URI voor de systeemeigen Azure AD-toepassing
* Gedelegeerde machtigingen instellen


## <a name="step-1-create-an-active-directory-native-application"></a>Stap 1: Een systeemeigen Active Directory-toepassing maken

Maken en configureren van een systeemeigen Azure AD-toepassing voor eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory. Zie voor instructies [maken van een Azure AD-toepassing](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Zorg ervoor dat u selecteert bij de instructies in de koppeling te volgen, **systeemeigen** voor het type van de toepassing, zoals wordt weergegeven in de volgende schermafbeelding:

![Web-app maken](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "systeemeigen app maken")

## <a name="step-2-get-application-id-and-redirect-uri"></a>Stap 2: Ophalen van toepassings-ID en omleidings-URI

Zie [de toepassings-ID ophalen](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) om op te halen van de toepassings-ID.

Als u wilt de omleidings-URI ophalen, moet u de volgende stappen uitvoeren.

1. Selecteer in de Azure-portal **Azure Active Directory**, klikt u op **App-registraties**, en zoeken en klikt u op de systeemeigen Azure AD-toepassing die u hebt gemaakt.

2. Uit de **instellingen** blade voor de toepassing, klikt u op **omleidings-URI's**.

    ![Get omleidings-URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Kopieer de waarde die wordt weergegeven.


## <a name="step-3-set-permissions"></a>Stap 3: Machtigingen voor het instellen

1. Selecteer in de Azure-portal **Azure Active Directory**, klikt u op **App-registraties**, en zoeken en klikt u op de systeemeigen Azure AD-toepassing die u hebt gemaakt.

2. Uit de **instellingen** blade voor de toepassing, klikt u op **vereiste machtigingen**, en klik vervolgens op **toevoegen**.

    ![client-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. In de **API-toegang toevoegen** blade, klikt u op **Select an API**, klikt u op **Azure Data Lake**, en klik vervolgens op **Selecteer**.

    ![client-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  In de **API-toegang toevoegen** blade, klikt u op **machtigingen selecteren**, schakel het selectievakje in om te geven **volledige toegang tot Data Lake Store**, en klik vervolgens op **selecteren**.

    ![client-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Klik op **Gereed**.

5. Herhaal de laatste twee stappen voor het verlenen van machtigingen voor **Windows Azure Service Management API** ook.
   
## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt een systeemeigen Azure AD-toepassing gemaakt en die worden verzameld door de informatie die u nodig hebt in uw client-toepassingen te ontwerpen met behulp van de SDK voor .NET, Java-SDK, REST-API, enzovoort. U kunt nu doorgaan naar de volgende artikelen die spreken over het gebruik van de Azure AD-webtoepassing eerst worden geverifieerd met Data Lake Storage Gen1 en andere bewerkingen uit te voeren in de store.

* [Eindgebruiker End-User-authentication met Data Lake Storage Gen1 met behulp van Java-SDK](data-lake-store-end-user-authenticate-java-sdk.md)
* [Eindgebruikersverificatie met Data Lake Storage Gen1 met .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md)
* [Eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van Python](data-lake-store-end-user-authenticate-python.md)
* [Eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-end-user-authenticate-rest-api.md)

