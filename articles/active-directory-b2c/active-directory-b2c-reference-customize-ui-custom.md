---
title: De gebruikersinterface van een gebruikersbeleving met aangepast beleid aanpassen | Microsoft Docs
description: Meer informatie over Azure Active Directory B2C aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8c3684f93bbf5b9fe9f5ea9167396b9822e70c48
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52841622"
---
# <a name="customize-the-ui-of-a-user-journey-with-custom-policies"></a>De gebruikersinterface van een gebruikersbeleving met aangepast beleid aanpassen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> In dit artikel is een geavanceerde beschrijving van de werking van UI-aanpassing en hoe u met Azure AD B2C aangepaste beleidsregels, met behulp van de Identity-Ervaringsframework inschakelt.


Een naadloze gebruikerservaring is essentieel voor elke business-to-consumer-oplossing. Een naadloze gebruikerservaring is een ervaring op apparaat of de browser, waarbij een gebruikersbeleving via de service is te onderscheiden van die van de klantenservice die ze gebruiken.

## <a name="understand-the-cors-way-for-ui-customization"></a>Informatie over de manier CORS voor UI-aanpassing

Azure AD B2C kunt u het uiterlijk-en-de werking van de gebruikerservaring (UX) op de verschillende pagina's die worden geleverd en worden weergegeven door Azure AD B2C aanpassen met behulp van uw aangepaste beleidsregels.

Om die reden wordt Azure AD B2C wordt uitgevoerd de code in de browser van uw consumenten en maakt gebruik van de benadering van moderne en standard [Cross-Origin Resource Sharing (CORS)](https://www.w3.org/TR/cors/) aangepaste inhoud laden uit een specifieke URL die u in een aangepast beleid om te verwijzen opgeeft naar uw HTML5/CSS-sjablonen. CORS is een mechanisme waarmee beperkte resources, zoals lettertypen, op een webpagina worden aangevraagd vanuit een ander domein buiten het domein waaruit de bron afkomstig is.

In vergelijking met de oude traditionele methode, waarbij sjabloon pagina's zijn eigendom van de oplossing waar u beperkte tekst en afbeeldingen hebt opgegeven, waarbij beperkte beheermogelijkheden van indeling en u is aangeboden leiden tot meer dan problemen bij het realiseren van een naadloze ervaring, de CORS-manier biedt ondersteuning voor HTML5- en CSS- en kunt u naar:

- Host van de inhoud en de oplossing injects de besturingselementen met client-side-script.
- Hiermee hebt u volledige controle over elke pixel van indeling en stijl.

U kunt zoveel inhoudspagina's als u wilt met het samenstellen van HTML5/CSS-bestanden waar nodig opgeven.

> [!NOTE]
> Uit veiligheidsoverwegingen is het gebruik van JavaScript momenteel geblokkeerd voor aanpassing. 

In elk van uw sjablonen HTML5/CSS, bieden u een *anker* element, dat overeenkomt met de vereiste `<div id=”api”>` element in de HTML-code of de pagina-inhoud, zoals hierna illustreren. Azure AD B2C is vereist dat alle inhoud pagina's deze specifieke div. hebben

```
<!DOCTYPE html>
<html>
  <head>
    <title>Your page content’s tile!</title>
  </head>
  <body>
    <div id="api"></div>
  </body>
</html>
```

Azure AD B2C-gerelateerde inhoud voor de pagina is opgenomen in deze div-element, terwijl de rest van de pagina kunt kiezen om te bepalen is. De Azure AD B2C-JavaScript-code in uw inhoud ophaalt en HTML injects in deze specifieke div-element. Azure AD B2C injects de volgende besturingselementen waar nodig: account kiezer besturingselement, zich aanmelden, multi-factor Authentication (op dit moment telefoon-besturingselementen op basis van) en kenmerk verzameling besturingselementen. Azure AD B2C zorgt ervoor dat alle besturingselementen zijn HTML5 compatibel is en toegankelijk is, alle besturingselementen kunnen volledig worden opgemaakt en die een versie van het besturingselement niet gaat.

De samengevoegde inhoud wordt uiteindelijk weergegeven als de dynamische document aan uw consumenten.

Om ervoor te zorgen dat alles werkt zoals verwacht, moet u:

- Zorg ervoor dat uw inhoud is HTML5 compatibel is en toegankelijk is
- Zorg ervoor dat uw server voor webinhoud is ingeschakeld voor CORS.
- Inhoud leveren via HTTPS.
- Gebruik de absolute URL's, zoals https://yourdomain/content voor alle koppelingen en CSS-inhoud.

> [!TIP]
> Controleer of de site die u bij het hosten van uw inhoud op heeft CORS is ingeschakeld en testen van CORS-aanvragen, kunt u de site https://test-cors.org/. Dankzij deze site, kunt u de CORS-aanvraag verzenden naar een externe server (om te testen, als u CORS wordt ondersteund) of de CORS-aanvraag verzenden naar een testserver (voor bepaalde functies van CORS verkennen).

> [!TIP]
> De site https://enable-cors.org/ ook vormt een meer dan nuttige informatiebronnen voor CORS.

Dankzij deze benadering CORS op basis van de eindgebruikers beschikken over consistente ervaring tussen uw toepassing en de pagina's bediend door Azure AD B2C.

## <a name="create-a-storage-account"></a>Create a storage account

Als een vereiste moet u een opslagaccount maken. U moet een Azure-abonnement te maken van een Azure Blob Storage-account. U kunt zich aanmelden met een gratis proefversie op de [Azure-website](https://azure.microsoft.com/pricing/free-trial/).

1. Open een browsersessie en navigeer naar de [Azure-portal](https://portal.azure.com).
2. Meld u aan met uw beheerdersreferenties.
3. Klik op **een resource maken** > **opslag** > **opslagaccount**.  Een **storage-account maken** deelvenster wordt geopend.
4. In **naam**, Geef een naam op voor de storage-account, bijvoorbeeld *contoso369b2c*. Deze waarde wordt later aangeduid als *storageAccountName*.
5. Kies de gewenste selecties voor de prijscategorie, de resourcegroep en het abonnement. Zorg ervoor dat u hebt de **vastmaken aan Startboard** optie is ingeschakeld. Klik op **Create**.
6. Ga terug naar het Startboard en klik op het opslagaccount dat u hebt gemaakt.
7. In de **Services** sectie, klikt u op **Blobs**. Een **Blob service deelvenster** wordt geopend.
8. Klik op **+ Container**.
9. In **naam**, Geef een naam op voor de container, bijvoorbeeld *b2c*. Deze waarde later wordt aangeduid als *containerName*.
9. Selecteer **Blob** als de **toegangstype**. Klik op **Create**.
10. De container die u hebt gemaakt, wordt weergegeven in de lijst op de **Blob service deelvenster**.
11. Sluit de **Blobs** deelvenster.
12. Op de **storage account deelvenster**, klikt u op de **sleutel** pictogram. Een **toegang tot sleutels deelvenster** wordt geopend.  
13. Noteer de waarde van **key1**. Deze waarde later wordt verwezen als *key1*.

## <a name="downloading-the-helper-tool"></a>Het hulpprogramma voor downloaden

1.  Download het hulpprogramma helper van [GitHub](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).
2.  Sla de *B2C-Azure BLOB Storage-Client-master.zip* bestand op uw lokale computer.
3.  Pak de inhoud van de B2C-Azure BLOB Storage-Client-master.zip-bestand op uw lokale schijf, bijvoorbeeld onder de **UI-aanpassing-Pack** map, maakt u een *B2C-Azure BLOB Storage-Client-master*submap.
4.  Open die map en de inhoud van het bestand uit te pakken *B2CAzureStorageClient.zip* daarin.

## <a name="upload-the-ui-customization-pack-sample-files"></a>De UI-aanpassing-Pack-voorbeeld-bestanden uploaden

1.  Met behulp van Windows Verkenner, Ga naar de map *B2C-Azure BLOB Storage-Client-master* bevindt zich onder de *UI-aanpassing-Pack* map in de vorige sectie hebt gemaakt.
2.  Voer de *B2CAzureStorageClient.exe* bestand. Dit programma worden alle bestanden in de map die u naar uw opslagaccount opgeeft en CORS-toegang voor deze bestanden geüpload.
3.  Geef desgevraagd de: een.  De naam van uw opslagaccount, *storageAccountName*, bijvoorbeeld *contoso369b2c*.
    b.  De primaire toegangssleutel van uw azure-blobopslag *key1*, bijvoorbeeld *contoso369b2c*.
    c.  De naam van uw opslag blob storage-container *containerName*, bijvoorbeeld *b2c*.
    d.  Het pad van de *Beginnerspakket* voorbeeldbestanden, bijvoorbeeld *... \B2CTemplates\wingtiptoys*.

Als u de voorgaande stappen hebt gevolgd de HTML5 en CSS-bestanden van de *UI-aanpassing-Pack* voor het fictieve bedrijf **wingtiptoys** nu verwijst naar uw storage-account.  U kunt controleren dat de inhoud correct is geüpload door het openen van het deelvenster gerelateerde container in Azure portal. U kunt ook kunt u controleren dat de inhoud correct is geüpload door de pagina vanuit een browser te openen. Zie voor meer informatie, [Azure Active Directory B2C: een hulpprogramma gebruikt om te demonstreren van de pagina gebruiker gebruikersinterface (UI) aanpassing functie](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-the-storage-account-has-cors-enabled"></a>Zorg ervoor dat het opslagaccount heeft CORS ingeschakeld

CORS (Cross-Origin Resource Sharing) moet zijn ingeschakeld op uw eindpunt voor Azure AD B2C om uw inhoud te laden. Dit komt doordat de inhoud wordt gehost op een ander domein dan het domein wordt Azure AD B2C worden met de pagina uit.

Om te controleren of de opslag die u bij het hosten van uw inhoud op CORS ingeschakeld heeft, gaat u verder met de volgende stappen uit:

1. Open een browsersessie en navigeer naar de pagina *unified.html* met behulp van de volledige URL van de locatie in uw opslagaccount `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Bijvoorbeeld https://contoso369b2c.blob.core.windows.net/b2c/unified.html.
2. Navigeer naar https://test-cors.org. Deze site kunt u controleren of de pagina die u met CORS ingeschakeld heeft.  
<!--
![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
-->

3. In **externe URL**, voer de volledige URL voor uw inhoud unified.html en klikt u op **aanvraag verzenden**.
4. Controleer de uitvoer in de **resultaten** sectie bevat *XHR status: 200*, wat aangeeft dat CORS is ingeschakeld.
<!--
![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
-->
Het opslagaccount moet nu een blobcontainer met de naam bevatten *b2c* in de afbeelding met de volgende wingtiptoys sjablonen van de *Beginnerspakket*.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

De volgende tabel beschrijft het doel van de voorgaande HTML5-pagina's.

| HTML5-sjabloon | Beschrijving |
|----------------|-------------|
| *phonefactor.HTML* | Deze pagina kan worden gebruikt als een sjabloon voor een multi-factor authentication-pagina. |
| *resetpassword.html* | Deze pagina kan worden gebruikt als een sjabloon voor een pagina voor vergeten wachtwoorden. |
| *selfasserted.html* | Deze pagina kan worden gebruikt als een sjabloon voor een sociale account aanmelden pagina, de aanmeldpagina voor een lokaal account of een lokaal account aanmelden pagina. |
| *Unified.HTML* | Deze pagina kan worden gebruikt als een sjabloon voor een uniforme registreren of aanmelden pagina. |
| *updateprofile.HTML* | Deze pagina kan worden gebruikt als een sjabloon voor een update profielpagina. |

## <a name="add-a-link-to-your-html5css-templates-to-your-user-journey"></a>Een koppeling naar uw HTML5/CSS-sjablonen naar uw gebruikersbeleving toevoegen

U kunt een koppeling naar uw HTML5/CSS-sjablonen toevoegen aan uw gebruikersbeleving door een aangepast beleid rechtstreeks te bewerken.

De aangepaste sjablonen die HTML5/CSS gebruiken in de gebruikersbeleving moeten worden opgegeven in een lijst van inhoudsdefinities die kunnen worden gebruikt in deze gebruiker reizen. Om die reden wordt een optionele *<ContentDefinitions>* XML-element moet worden gedeclareerd onder de *<BuildingBlocks>* sectie van het aangepaste beleid XML-bestand.

De volgende tabel beschrijft de set met inhoud roldefinitie-id's wordt herkend door de identiteit van de Azure AD B2C-engine en het type van pagina's dat is gekoppeld aan deze optreden.

| De definitie van de inhoud-ID | Beschrijving |
|-----------------------|-------------|
| *api.error* | **Foutpagina**. Deze pagina wordt weergegeven wanneer er een uitzondering of een fout is opgetreden. |
| *api.idpselections* | **Pagina voor het id-provider selecteren**. Deze pagina bevat een lijst met id-providers die de gebruiker uit tijdens het aanmelden kiezen kan. Deze providers zijn enterprise id-providers, sociale id-providers, zoals Facebook en Google + of lokale accounts (op basis van e-mailadres of gebruikersnaam naam). |
| *api.idpselections.signup* | **Selectie van de id-provider voor aanmelding bij**. Deze pagina bevat een lijst met id-providers die de gebruiker uit tijdens de registratie kiezen kan. Deze providers zijn enterprise id-providers, sociale id-providers, zoals Facebook en Google + of lokale accounts (op basis van e-mailadres of gebruikersnaam naam). |
| *api.localaccountpasswordreset* | **Pagina voor vergeten wachtwoorden**. Deze pagina bevat een formulier dat de gebruiker heeft om in te vullen voor het starten van hun wachtwoord opnieuw instellen.  |
| *api.localaccountsignin* | **Lokaal account aanmelden pagina**. Deze pagina bevat een aanmeldingsformulier waartoe de gebruiker heeft om in te vullen wanneer u zich aanmeldt met een lokale account dat is gebaseerd op een e-mailadres of een gebruikersnaam opgeven. Een tekstvak voor invoer en wachtwoord vermelding in bevatten het formulier. |
| *api.localaccountsignup* | **Pagina voor het registreren van lokale account**. Deze pagina bevat een aanmeldingsformulier hebt ingevuld die de gebruiker heeft om in te vullen bij het aanmelden voor een lokaal account dat is gebaseerd op een e-mailadres of een gebruikersnaam opgeven. Het formulier kan verschillende besturingselementen voor tekstinvoer zoals tekstinvoervak, wachtwoordinvoervak, keuzerondje, enkelvoudige selectie vervolgkeuzelijsten en meervoudige selectie selectievakjes bevatten. |
| *api.phonefactor* | **Multi-factor authentication-pagina**. Gebruikers kunnen hun telefoonnummers (via SMS of spraak) controleren tijdens het registreren of aanmelden op deze pagina. |
| *api.selfasserted* | **Aanmeldpagina voor sociaal account**. Deze pagina bevat een aanmeldingsformulier hebt ingevuld die de gebruiker heeft om in te vullen wanneer u zich aanmeldt met behulp van een bestaand account van een sociale id-provider zoals Facebook of Google +. Deze pagina is vergelijkbaar met de voorgaande sociaal account registratiepagina met uitzondering van de velden van de vermelding wachtwoord. |
| *api.selfasserted.profileupdate* | **Update-profielpagina**. Deze pagina bevat een formulier die de gebruiker gebruiken kunt om hun profiel bijwerken. Deze pagina is vergelijkbaar met de voorgaande sociaal account registratiepagina met uitzondering van de velden van de vermelding wachtwoord. |
| *api.signuporsignin* | **Uniforme pagina voor registreren of aanmelden**.  Deze pagina verwerkt zowel registreren en aanmelden van gebruikers, die zakelijke id-providers, sociale id-providers zoals Facebook of Google + of lokale accounts kunnen gebruiken.

## <a name="next-steps"></a>Volgende stappen
[Naslaginformatie: Inzicht in hoe aangepaste beleidsregels werken met de Identiteitservaring-Framework in B2C](active-directory-b2c-reference-custom-policies-understanding-contents.md)
