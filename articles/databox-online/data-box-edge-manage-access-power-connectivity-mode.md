---
title: Toegang van Azure Data Box-Edge-apparaat, kracht en verbindingsmodus | Microsoft Docs
description: Hierin wordt beschreven hoe u voor het beheren van toegang, kracht en verbindingsmodus voor de Azure Data Box-Edge-apparaat dat helpt gegevens naar Azure overbrengen
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 06/03/2019
ms.author: alkohli
ms.openlocfilehash: 8937f4c47f0fa84d4ec371e951cff8a2fdaa8481
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66476900"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge"></a>Beheren van toegang, kracht en verbindingsmodus voor uw Azure Data Box-Edge

In dit artikel wordt beschreven hoe u de modus voor toegang, kracht en connectiviteit voor uw Azure Data Box-Edge beheren. Deze bewerkingen worden uitgevoerd via de lokale web-UI of de Azure-portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Toegang tot het apparaat beheren
> * Verbindingsmodus beheren
> * Energiebeheer


## <a name="manage-device-access"></a>Toegang tot het apparaat beheren

De toegang tot uw gegevens in het Edge-apparaat wordt bepaald door het gebruik van het wachtwoord van een apparaat. U kunt het wachtwoord via de lokale webgebruikersinterface wijzigen. U kunt ook het wachtwoord van het apparaat in Azure portal opnieuw.

### <a name="change-device-password"></a>Wachtwoord voor apparaat wijzigen

Volg deze stappen in de gebruikersinterface van de lokale wachtwoord van het apparaat te wijzigen.

1. Ga in de lokale webgebruikersinterface naar **onderhoud > wachtwoordwijziging**.
2. Voer in het huidige wachtwoord en klik vervolgens op het nieuwe wachtwoord. Het opgegeven wachtwoord moet tussen 8 en 16 tekens lang zijn. Het wachtwoord moet bestaan 3 van de volgende tekens bevatten: hoofdletters, kleine letters, cijfers en speciale tekens. Controleer of het nieuwe wachtwoord.

    ![Wachtwoord wijzigen](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. Selecteer **wachtwoord wijzigen**.
 
### <a name="reset-device-password"></a>Wachtwoord van apparaat opnieuw instellen

De werkstroom opnieuw instellen is niet vereist voor de gebruiker aan de hand van het oude wachtwoord en is handig als u het wachtwoord is verloren gegaan. Deze werkstroom wordt uitgevoerd in Azure portal.

1. In de Azure-portal, gaat u naar **overzicht > beheerderswachtwoord opnieuw instellen van**.

    ![Wachtwoord opnieuw instellen](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Voer het nieuwe wachtwoord en bevestigt u dit. Het opgegeven wachtwoord moet tussen 8 en 16 tekens lang zijn. Het wachtwoord moet bestaan 3 van de volgende tekens bevatten: hoofdletters, kleine letters, cijfers en speciale tekens. Selecteer **opnieuw**.

    ![Wachtwoord opnieuw instellen](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-resource-access"></a>Toegang tot resources beheren

Voor het maken van uw gegevens in Edge/Data Box Gateway, IoT-Hub en Azure Storage-resource, moet u machtigingen als Inzender of hoger op het niveau van een resource. U moet ook de bijbehorende resourceproviders worden geregistreerd. Voor alle bewerkingen die betrekking hebben op de activeringscode en referenties, zijn ook machtigingen voor Azure Active Directory Graph API vereist. Deze worden beschreven in de volgende secties.

### <a name="manage-microsoft-azure-active-directory-graph-api-permissions"></a>Microsoft Azure Active Directory Graph API-machtigingen beheren

Bij het genereren van activeringscode voor de gegevens in het Edge-apparaat of het uitvoeren van bewerkingen die referenties vereist, moet u machtigingen voor Azure Active Directory Graph API. De bewerkingen die referenties nodig kunnen zijn:

-  Het maken van een share met een gekoppelde storage-account.
-  Het maken van een gebruiker aan wie toegang heeft tot de shares op het apparaat.

U moet beschikken over een `User` toegang krijgen tot op Active Directory-tenant, als u nodig hebt om te kunnen `Read all directory objects`. U kunt een gastgebruiker kan niet als ze niet gemachtigd om te `Read all directory objects`. Als u een gast en vervolgens de bewerkingen zoals het genereren van een activeringscode, het maken van een share op uw gegevens in het Edge-apparaat, mislukt het maken van een gebruiker alle.

Zie voor meer informatie over het bieden van toegang voor gebruikers voor Azure Active Directory Graph API [standaard toegang voor beheerders, gebruikers en gastgebruikers](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).

### <a name="register-resource-providers"></a>Registreren van resourceproviders

Voor het inrichten van een resource in Azure (in de Azure Resource Manager-model), moet u een resourceprovider die ondersteuning biedt voor het maken van die resource. Bijvoorbeeld, voor het inrichten van een virtuele machine, moet u een 'Microsoft.Compute-resourceprovider beschikbaar in het abonnement hebt.
 
Resourceproviders worden geregistreerd op het niveau van het abonnement. Een nieuw Azure-abonnement is standaard, vooraf geregistreerd met een lijst met veelgebruikte resourceproviders. De resourceprovider voor 'Microsoft.DataBoxEdge' is niet opgenomen in deze lijst.

U hoeft niet te verlenen van machtigingen voor toegang tot het abonnement voor gebruikers mogelijk te maken van resources, zoals 'Microsoft.DataBoxEdge' binnen de resourcegroepen die beschikken over eigenaarsrechten, zolang de resourceproviders voor deze resources is al geregistreerd.

Voordat u probeert te maken van een resource, ervoor zorgen dat de resourceprovider is geregistreerd in het abonnement. Als de resourceprovider niet is geregistreerd, moet u om ervoor te zorgen dat de gebruiker die de nieuwe resource maakt heeft onvoldoende rechten om de vereiste resourceprovider op abonnementsniveau te registreren. Als u dit ook nog niet hebt gedaan, ziet u de volgende fout:

*Het abonnement <Subscription name> geen machtigingen voor het registreren van resourceproviders: Microsoft.DataBoxEdge.*


Als u een lijst met geregistreerde resourceproviders in het huidige abonnement, moet u de volgende opdracht uitvoeren:

```PowerShell
Get-AzResourceProvider -ListAvailable |where {$_.Registrationstate -eq "Registered"}
```

Voor gegevens in het Edge-apparaat, `Microsoft.DataBoxEdge` moeten worden geregistreerd. Om u te registreren `Microsoft.DataBoxEdge`, beheerder van abonnement moet de volgende opdracht uitvoeren:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.DataBoxEdge
```

Zie voor meer informatie over het registreren van een resourceprovider [oplossen voor registratie van de resourceprovider](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="manage-connectivity-mode"></a>Verbindingsmodus beheren

Uw apparaat kunt naast de standaard volledig verbonden modus, ook uitvoeren in de modus gedeeltelijk verbonden of volledig niet-verbonden. Elk van deze modi wordt hieronder beschreven:

- **Volledig wordt verbonden** -dit is de normale standaardmodus waarin het apparaat werkt. De cloud uploaden en downloaden van gegevens is in deze modus ingeschakeld. U kunt de Azure portal of de lokale webgebruikersinterface voor het beheren van het apparaat.

- **Gedeeltelijk verbroken** : In deze modus wordt het apparaat kan niet uploaden of downloaden van een share die gegevens echter kunnen worden beheerd via de Azure-portal.

    In deze modus wordt doorgaans gebruikt wanneer u op een netwerk met een datalimiet satelliet en het doel is om het gebruik van netwerkbandbreedte te minimaliseren. Minimale netwerkverbruik nog steeds optreden voor het controleren van de bewerkingen van apparaten.

- **De verbinding verbroken** : In deze modus wordt het apparaat is volledig losgekoppeld van de cloud en zowel de cloud uploads en downloads zijn uitgeschakeld. Het apparaat kan alleen worden beheerd via de lokale webgebruikersinterface.

    In deze modus wordt doorgaans gebruikt wanneer u offline wilt halen uw apparaat.

Apparaatmodus, Ga als volgt te werk:

1. In de lokale webgebruikersinterface van uw apparaat, gaat u naar **Configuration > Cloudinstellingen**.
2. In de vervolgkeuzelijst, selecteer de modus die u wilt werken met het apparaat in. U kunt kiezen uit **volledig wordt verbonden**, **gedeeltelijk verbonden**, en **volledig losgekoppeld**. Inschakelen om uit te voeren van het apparaat in de gedeeltelijk niet-verbonden modus, **beheer van Azure portal**.

    ![Verbindingsmodus](media/data-box-edge-manage-access-power-connectivity-mode/connectivity-mode.png)
 
## <a name="manage-power"></a>Energiebeheer

U kunt afsluiten of opnieuw opstarten van uw fysieke apparaat met behulp van de lokale webgebruikersinterface. We aanbevolen die u opnieuw start, neemt u even voordat de shares offline op de server en klik vervolgens op het apparaat. Deze actie wordt geminimaliseerd een mogelijkheid van beschadiging van gegevens.

1. Ga in de lokale webgebruikersinterface naar **onderhoud > energie-instellingen**.
2. Selecteer **afsluiten** of **opnieuw** , afhankelijk van wat u van plan bent om te doen.

    ![Energie-instellingen](media/data-box-edge-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Wanneer u hierom wordt gevraagd om bevestiging, selecteert u **Ja** om door te gaan.

> [!NOTE]
> Als u het fysieke apparaat afsluit, moet u push-knop op het apparaat deze in te schakelen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheren van bestandsshares](data-box-edge-manage-shares.md).