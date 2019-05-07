---
title: Maak een Azure AD-app-registratie en de gebruiker voor Azure Red Hat OpenShift | Microsoft Docs
description: Informatie over het maken van een service-principal, een client-geheim en de verificatie callback-URL te genereren en maak een nieuwe Active Directory-gebruiker voor het testen van apps op uw Microsoft Azure Red Hat OpenShift-cluster.
author: tylermsft
ms.author: twhitney
ms.service: openshift
manager: jeconnoc
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/06/2019
ms.openlocfilehash: de3f3c30848d26ea399bcccc29a6149a149f6a55
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078518"
---
# <a name="create-an-azure-ad-app-registration-and-user-for-azure-red-hat-openshift"></a>Maak een Azure AD-app-registratie en de gebruiker voor Azure Red Hat OpenShift

Microsoft Azure Red Hat OpenShift moet machtigingen voor het uitvoeren van taken namens uw cluster. Als uw organisatie niet al een app-registratie voor Azure Active Directory (Azure AD) om te gebruiken als de service-principal hebt, volgt u deze instructies voor het maken van een.

In dit onderwerp wordt afgesloten met instructies voor het maken van een nieuwe Azure AD-gebruiker, moet u toegang tot apps die worden uitgevoerd op uw cluster Azure Red Hat OpenShift.

Als u een Azure AD-tenant al hebt gemaakt, volg de aanwijzingen in [maken van een Azure AD-tenant voor Azure Red Hat OpenShift](howto-create-tenant.md) voordat u doorgaat met deze instructies.

## <a name="create-a-new-app-registration"></a>Maak een nieuwe app-registratie

Een toepassing die met behulp van de mogelijkheden van Azure AD moet eerst worden geregistreerd in een Azure AD-tenant. Deze registratieproces omvat het met Azure AD gedetailleerde informatie over uw toepassing, zoals de URL waar de app zich bevindt, de URL voor het verzenden van antwoorden nadat een gebruiker is geverifieerd, de URI die u de app, enzovoort identificeert.

1. In de [Azure-portal](https://portal.azure.com), zorg ervoor dat uw tenant wordt weergegeven onder de naam van de gebruiker in de rechterbovenhoek van de portal:

    ![Schermafbeelding van portal met de tenant die worden vermeld in de rechterbovenhoek][tenantcallout] als de verkeerde tenant wordt weergegeven, klikt u op de naam van de gebruiker in de rechterbovenhoek en klik vervolgens op **schakelen tussen mappen**, en selecteer de juiste tenant van de **alle Mappen** lijst.

2. Open de [blade App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) en klikt u op **nieuwe toepassing registreren**.
3. In de **maken** in het deelvenster een beschrijvende naam voor uw toepassingsobject.
4. Zorg ervoor dat **toepassingstype** is ingesteld op *Web-app/API*.
5. Maak een **aanmeldings-URL** met behulp van het volgende patroon:

    ```
    https://<cluster-name>.<azure-region>.cloudapp.azure.com
    ```

    . . . waar `<cluster-name>` is de gewenste naam van uw cluster Azure Red Hat OpenShift en `<azure-region>` is de [Azure-regio die als host fungeert voor uw cluster Azure Red Hat OpenShift](supported-resources.md#azure-regions). Bijvoorbeeld, als de clusternaam van uw moet worden `contoso`, en maakt u deze in de `eastus` regio, de volledig gekwalificeerde domeinnaam (FQDN) die u moet invoeren voor de **aanmeldings-URL** zou zijn `https://contoso.eastus.cloudapp.azure.com`

    > [!IMPORTANT]
    > Naam van het cluster moet alleen kleine letters bevatten, en de URL van de FQDN-naam moet uniek zijn.
    > Zorg ervoor dat u kiest een voldoende unieke naam voor uw cluster.

    Noteer de clusternaam van uw en aanmeldings-URL, moet u ze later naar toegang tot apps die worden uitgevoerd op uw cluster. Verwijzen we naar de naam van het cluster als `CLUSTER_NAME`, en dit aanmeldings-URL als `FQDN` in de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

6. Zorg ervoor dat uw **aanmeldings-URL** waarde wordt gevalideerd met een groen vinkje. (Druk op de Tab-toets naar de focus van de *aanmeldings-URL* veld en voer de validatie uit.)
7. Klik op de **maken** knop om de Azure AD-toepassing-object te maken.
8. Op de **App geregistreerd** pagina die wordt weergegeven, noteer de **toepassings-ID**. Er wordt verwezen naar deze waarde als `APPID` in de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

    ![Schermopname van het tekstvak van de toepassing-ID][appidimage]
    
Zie voor meer informatie over Azure toepassingsobjecten [toepassing en service-principalobjecten in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals).

Voor meer informatie over het maken van een nieuwe Azure AD-toepassing, Zie [een app registreren met het eindpunt van de Azure Active Directory v1.0](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

### <a name="create-a-client-secret"></a>Een clientgeheim maken

U bent nu klaar voor het genereren van een clientgeheim voor het verifiëren van uw app naar Azure Active Directory.

1. Uit de **App geregistreerd** pagina, klikt u op **instellingen** om de instellingen voor uw geregistreerde app te openen.
2. Op de **instellingen** deelvenster dat verschijnt, klikt u op **sleutels**.  De **sleutels** deelvenster wordt weergegeven.
![Schermafbeelding van de belangrijkste pagina maken in de portal][createkeyimage]
3. Geef een sleutel **beschrijving**.
4. Een waarde instellen voor **verloopt**, bijvoorbeeld *na twee jaar*.
5. Klik op **opslaan** en waarde van de sleutel wordt weergegeven.
6. Noteer de sleutelwaarde. Er wordt verwezen naar deze waarde als `SECRET` in de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

### <a name="create-a-reply-url"></a>Maken van een antwoord-URL

Azure AD maakt gebruik van het app-object *antwoord-URL* om op te geven van de callback die moet worden gebruikt wanneer de app te autoriseren. In het geval van een web-API of webtoepassing is de antwoord-URL de locatie waar Azure AD het verificatieantwoord stuurt, met inbegrip van een token als de verificatie gelukt is.

Zelfs de kleinste mismatch (afsluitende schuine streep ontbreekt, verschillend hoofdlettergebruik) zorgt ervoor dat de bewerking voor token-uitgifte mislukt en kunt u zich niet aanmelden.

1. Ga terug naar de **instellingen** deelvenster (u kunt klikken op **instellingen** in het koppelingenmenu bovenaan de portal), en klikt u op **antwoord-URL's** aan de rechterkant.  De **antwoord-URL's** deelvenster wordt weergegeven.
2. De eerste antwoord-URL in de lijst worden de `FQDN` -waarde uit stap 6 in [maken van een nieuwe app-registratie](#create-a-new-app-registration). Deze bewerken en toevoeg- `/oauth2callback/Azure%20AD`.  Bijvoorbeeld, ziet de antwoord-URL er ongeveer als `https://mycluster.eastus.cloudapp.azure.com/oauth2callback/Azure%20AD`
3. Klik op **opslaan** om op te slaan de antwoord-URL.

## <a name="create-a-new-active-directory-user"></a>Maak een nieuwe Active Directory-gebruiker

Maak een nieuwe gebruiker in Active Directory gebruiken om aan te melden bij de app in uw cluster Azure Red Hat OpenShift.

1. Ga naar de [gebruikers: alle gebruikers](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers) blade.
2. Klik op **+ nieuwe gebruiker**. De **gebruiker** deelvenster wordt weergegeven.
3. Voer een **naam** die u wilt dat voor deze gebruiker.
4. Maak een **gebruikersnaam** op basis van de naam van de tenant die u hebt gemaakt met `.onmicrosoft.com` toegevoegd aan het einde. Bijvoorbeeld `yourUserName@yourTenantName.onmicrosoft.com`. Noteer de naam van deze gebruiker. U moet deze zich aanmeldt bij de app op uw cluster gebruiken.
5. Klik op **maprol** en selecteer **hoofdbeheerder** en klik vervolgens op **Ok** aan de onderkant van het deelvenster.
6. In het midden van de **gebruiker** deelvenster, klikt u op **wachtwoord weergeven** en noteer het tijdelijke wachtwoord. Nadat u de eerste keer aanmelden, wordt u gevraagd opnieuw in te stellen.
7. Klik aan de onderkant van het deelvenster **maken** om de gebruiker te maken.

## <a name="resources"></a>Resources

* [Toepassingen en service-principalobjecten in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)  
* [Snelstart: Een app registreren met het eindpunt van de Azure Active Directory v1.0](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)  

[appidimage]: ./media/howto-create-tenant/get-app-id.png
[createkeyimage]: ./media/howto-create-tenant/create-key.png
[tenantcallout]: ./media/howto-create-tenant/tenant-callout.png

## <a name="next-steps"></a>Volgende stappen

Als u al hebt voldaan de [Azure Red Hat OpenShift vereisten](howto-setup-environment.md), u bent klaar om uw eerste cluster te maken!

Raadpleeg de zelfstudie:
> [!div class="nextstepaction"]
> [Een Azure Red Hat OpenShift-cluster maken](tutorial-create-cluster.md)
