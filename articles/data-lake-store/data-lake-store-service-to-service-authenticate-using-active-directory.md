---
title: 'Verificatie van de service-naar-service: Data Lake Store met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van de service to service-verificatie met Data Lake Store met Azure Active Directory
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 0b3f19bb92d1eeb214150bf118d546cd1c67cd78
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="service-to-service-authentication-with-data-lake-store-using-azure-active-directory"></a>Verificatie met Data Lake Store met Azure Active Directory-Services
> [!div class="op_single_selector"]
> * [Verificatie van de eindgebruiker](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Verificatie van service-tot-service](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
>  

Azure Data Lake Store maakt gebruik van Azure Active Directory voor verificatie. Vóór het ontwerpen van een toepassing die met Azure Data Lake Store werkt, moet u bepalen hoe u verificatie van uw toepassing met Azure Active Directory (Azure AD). De twee belangrijkste opties zijn beschikbaar:

* Verificatie van de eindgebruiker 
* Verificatie van de service-naar-service (in dit artikel) 

Deze beide opties leiden in uw toepassing wordt geleverd met een OAuth 2.0-token, dat wordt gekoppeld aan elke aanvraag aan Azure Data Lake Store.

In dit artikel wordt gesproken over het maken van een **Azure AD-webtoepassing voor verificatie van de service to service**. Zie voor instructies over de configuratie van Azure AD-toepassing voor verificatie van de eindgebruiker, [eindgebruiker verificatie met Data Lake Store met Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>Stap 1: Maak een Active Directory-webtoepassing

Maken en configureren van een Azure AD-webtoepassing voor service to service-verificatie met Azure Data Lake Store met Azure Active Directory. Zie voor instructies [maken van een Azure AD-toepassing](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Zorg ervoor dat u selecteert bij de instructies in de voorgaande link volgen, **Web-App / API** voor toepassingstype, zoals wordt weergegeven in de volgende schermafbeelding:

![Web-app maken](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "web-app maken")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>Stap 2: Toepassings-ID en verificatiesleutel tenant-ID ophalen
Wanneer u programmatisch zich aanmeldt, moet u de ID voor uw toepassing. Als de toepassing wordt uitgevoerd onder zijn eigen referenties, moet u ook een verificatiesleutel.

* Zie voor instructies over het ophalen van de toepassing-ID en -verificatie-sleutel (ook wel het clientgeheim) voor uw toepassing [Get-ID en verificatie Toepassingssleutel](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Zie voor instructies over het ophalen van de tenant-ID [tenant-ID ophalen](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder"></a>Stap 3: De Azure AD-toepassing met het Azure Data Lake Store-accountbestand of map toewijzen


1. Meld u aan bij [Azure Portal](https://portal.azure.com). Open de Azure Data Lake Store-account die u wilt koppelen aan de Azure Active Directory-toepassing die u eerder hebt gemaakt.
2. Klik op de blade van het Data Lake Store-account op **Gegevensverkenner**.
   
    ![Mappen maken in Data Lake Store-account](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "mappen maken in Data Lake-account")
3. In de **Data Explorer** blade, klikt u op het bestand of map waarvoor u wilt toegang bieden tot de Azure AD-toepassing en klik vervolgens op **toegang**. Voor het configureren van toegang tot een bestand, klikt u op **toegang** van de **bestandsvoorbeeld** blade.
   
    ![ACL's ingesteld op Data Lake-bestandssysteem](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "ACL's ingesteld op Data Lake-bestandssysteem")
4. De **toegang** blade bevat de standaard toegang en aangepaste toegang is al toegewezen aan de hoofdmap. Klik op de **toevoegen** pictogram Aangepast niveau ACL's toevoegen.
   
    ![Standaard- en aangepaste toegang lijst](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "lijst standaard en aangepaste toegang")
5. Klik op de **toevoegen** pictogram openen de **aangepaste toegang toevoegen** blade. Klik op deze blade **gebruiker of groep selecteren**, en klik vervolgens in **gebruiker of groep selecteren** blade, zoekt u de Azure Active Directory-toepassing die u eerder hebt gemaakt. Als u veel groepen om te zoeken vanaf hebt, gebruikt u in het tekstvak boven kunt u filteren op naam van de groep. Klik op de groep die u wilt toevoegen en klik vervolgens op **Selecteer**.
   
    ![Een groep toevoegen](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "een groep toevoegen")
6. Klik op **Selecteer machtigingen**, selecteer de machtigingen en of u de machtigingen toewijzen als een standaard ACL wilt, toegang krijgen tot ACL of beide. Klik op **OK**.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "machtigingen toewijzen aan de groep")
   
    Zie voor meer informatie over de machtigingen in Data Lake Store en/toegang ACL's, [toegangsbeheer in Data Lake Store](data-lake-store-access-control.md).
7. In de **aangepaste toegang toevoegen** blade, klikt u op **OK**. De toegevoegde groep met de bijbehorende machtigingen worden vermeld in de **toegang** blade.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "machtigingen toewijzen aan de groep")

> [!NOTE]
> Als u de SDK's gebruiken om een Data Lake Store-account te maken wilt, moet u de Azure AD-webtoepassing als een rol toewijzen aan de resourcegroep waarin u het Data Lake Store-account maken.
> 
>

## <a name="step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications"></a>Stap 4: Het eindpunt van het OAuth 2.0-token ophalen (alleen voor Java-toepassingen)

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en klikt u op Active Directory in het linkerdeelvenster.

2. Klik in het linkerdeelvenster op **App registraties**.

3. Klik vanaf de bovenkant van het tabblad App-registraties **eindpunten**.

    ![OAuth-token eindpunt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "token OAuth-eindpunt")

4. Kopieer het OAuth 2.0-tokeneindpunt uit de lijst met eindpunten.

    ![OAuth-token eindpunt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "token OAuth-eindpunt")   

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt een Azure AD-webtoepassing gemaakt en die worden verzameld door de informatie die u nodig hebt in uw clienttoepassingen te ontwerpen met behulp van de SDK voor .NET, Java, Python, REST-API, enzovoort. U kunt nu doorgaan met de volgende artikelen die communiceren over het gebruik van de systeemeigen Azure AD-toepassing eerst worden geverifieerd met Data Lake Store en andere bewerkingen uit te voeren in de store.

* [Service to service-verificatie met Data Lake Store met Java](data-lake-store-service-to-service-authenticate-java.md)
* [Service to service-verificatie met Data Lake Store met .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md)
* [Service to service-verificatie met Data Lake Store met behulp van Python](data-lake-store-service-to-service-authenticate-python.md)
* [Verificatie met Data Lake Store met REST-API-services](data-lake-store-service-to-service-authenticate-rest-api.md)


