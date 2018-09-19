---
title: 'Service-naar-serviceverificatie: Azure Data Lake Storage Gen1 met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van service-naar-serviceverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory
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
ms.openlocfilehash: ae1fdb6239b7d04dc366d023a26d1307cbae68f3
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123722"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Service-naar-serviceverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory
> [!div class="op_single_selector"]
> * [Verificatie van de eindgebruiker](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Verificatie van service-tot-service](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
>  

Azure Data Lake Storage Gen1 gebruikt Azure Active Directory voor verificatie. Vóór het ontwerpen van een toepassing die met Data Lake Storage Gen1 werkt, moet u bepalen hoe u aan het verifiëren van uw toepassing met Azure Active Directory (Azure AD). De twee belangrijkste opties die beschikbaar zijn:

* Verificatie van de eindgebruiker 
* Service-naar-serviceverificatie (in dit artikel) 

Deze beide opties leiden tot uw toepassing wordt geleverd met een OAuth 2.0-token opgehaald die zijn gekoppeld aan elke aanvraag aan de Data Lake Storage Gen1.

In dit artikel wordt besproken hoe u maakt een **Azure AD-webtoepassing voor service-naar-serviceverificatie**. Zie voor instructies voor de configuratie van de Azure AD-toepassing voor verificatie van eindgebruikers, [eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>Stap 1: Een Active Directory-webtoepassing maken

Maken en configureren van een Azure AD-webtoepassing voor verificatie van de service-naar-service met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory. Zie voor instructies [maken van een Azure AD-toepassing](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Zorg ervoor dat u selecteert bij de instructies in de vorige koppeling te volgen, **Web-App / API** voor het type van de toepassing, zoals wordt weergegeven in de volgende schermafbeelding:

![Web-app maken](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "web-app maken")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>Stap 2: Toepassings-ID, de verificatiesleutel en de tenant-ID ophalen
Wanneer u zich programmatisch aanmeldt, moet u de ID voor uw toepassing. Als de toepassing wordt uitgevoerd onder een eigen referenties, moet u ook een verificatiesleutel nodig.

* Zie voor instructies over het ophalen van de toepassing-ID en verificatiesleutel-sleutel (ook wel het clientgeheim) voor uw toepassing [Get-ID en -verificatie Toepassingssleutel](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Zie voor instructies over het ophalen van de tenant-ID [tenant-ID ophalen](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-storage-gen1-account-file-or-folder"></a>Stap 3: De Azure AD-toepassing naar het Azure Data Lake Storage Gen1 accountbestand of map toewijzen


1. Meld u aan bij [Azure Portal](https://portal.azure.com). Open het Data Lake Storage Gen1-account dat u wilt koppelen aan de Azure Active Directory-toepassing die u eerder hebt gemaakt.
2. Klik in de blade van het Data Lake Storage Gen1-account op **Data Explorer**.
   
    ![Mappen maken in Data Lake Storage Gen1 account](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "mappen maken in Data Lake-account")
3. In de **Data Explorer** blade, klikt u op het bestand of map waarvoor u wilt toegang bieden tot de Azure AD-toepassing en klik vervolgens op **toegang**. Voor het configureren van toegang tot een bestand, moet u klikken op **toegang** uit de **bestandsvoorbeeld** blade.
   
    ![ACL's ingesteld op Data Lake-bestandssysteem](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "ACL's in Data Lake-bestandssysteem instellen")
4. De **toegang** blade bevat de standaard toegang en aangepaste toegang is al toegewezen aan de hoofdmap. Klik op de **toevoegen** pictogram voor het toevoegen van aangepast niveau ACL's.
   
    ![Lijst met standaardentiteiten en aangepaste toegang](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "lijst met standaardentiteiten en aangepaste toegang")
5. Klik op de **toevoegen** pictogram opent de **aangepaste toegang toevoegen** blade. Klik op deze blade **Select User or Group**, en klik vervolgens in **Select User or Group** blade, zoekt u de Azure Active Directory-toepassing die u eerder hebt gemaakt. Hebt u veel groepen wilt zoeken, gebruikt u in het tekstvak aan de bovenkant om te filteren op naam van de groep. Klik op de groep die u wilt toevoegen en klik vervolgens op **Selecteer**.
   
    ![Een groep toevoegen](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "een groep toevoegen")
6. Klik op **Selecteer machtigingen**, selecteert u de machtigingen en of u de machtigingen toewijzen als een standaard-ACL wilt, toegang krijgen tot ACL, of beide. Klik op **OK**.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "machtigingen toewijzen aan de groep")
   
    Zie voor meer informatie over de machtigingen in Data Lake Storage Gen1 en standaard/toegangs-ACL's, [Access Control in Data Lake Storage Gen1](data-lake-store-access-control.md).
7. In de **aangepaste toegang toevoegen** blade, klikt u op **OK**. De zojuist toegevoegde groep met de bijbehorende machtigingen worden vermeld in de **toegang** blade.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "machtigingen toewijzen aan de groep")

> [!NOTE]
> Als u van plan bent over het beperken van uw Azure Active Directory-toepassing naar een specifieke map, u moet ook te geven die dezelfde Azure Active directory-toepassing **Execute** machtiging in de hoofdmap om in te schakelen toegang tot het maken van bestanden via de .NET SDK.

> [!NOTE]
> Als u de SDK's gebruiken om een Data Lake Storage Gen1-account te maken wilt, moet u de Azure AD-web-App als een rol toewijzen aan de resourcegroep waarin u het Data Lake Storage Gen1-account maken.
> 
>

## <a name="step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications"></a>Stap 4: Het OAuth 2.0-tokeneindpunt ophalen (alleen voor Java-toepassingen)

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en klik op Active Directory in het linkerdeelvenster.

2. Klik in het linkerdeelvenster op **App-registraties**.

3. Vanaf de bovenkant van het tabblad App-registraties, klikt u op **eindpunten**.

    ![OAuth-token-eindpunt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "OAuth-token-eindpunt")

4. Kopieer het OAuth 2.0-tokeneindpunt uit de lijst met eindpunten.

    ![OAuth-token-eindpunt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "OAuth-token-eindpunt")   

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt een Azure AD-webtoepassing gemaakt en die worden verzameld door de informatie die u nodig hebt in uw client-toepassingen te ontwerpen met behulp van de SDK voor .NET, Java, Python, REST-API, enzovoort. U kunt nu doorgaan naar de volgende artikelen die spreken over het gebruik van de systeemeigen Azure AD-toepassing eerst worden geverifieerd met Data Lake Storage Gen1 en andere bewerkingen uit te voeren in de store.

* [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Java](data-lake-store-service-to-service-authenticate-java.md)
* [Service-naar-serviceverificatie met Data Lake Storage Gen1 met .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md)
* [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Python](data-lake-store-service-to-service-authenticate-python.md)
* [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-service-to-service-authenticate-rest-api.md)


