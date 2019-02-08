---
title: Toegang tot het apparaat van de Microsoft Azure Data Box-Gateway, kracht en verbindingsmodus | Microsoft Docs
description: Hierin wordt beschreven hoe u voor het beheren van toegang, kracht en verbindingsmodus voor de Azure Data Box-Gateway-apparaat dat helpt gegevens naar Azure overbrengen
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: 0ad94799320e25d88f616117f1bfcf9f0513aadf
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873016"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-gateway-preview"></a>Beheren van toegang, kracht en verbindingsmodus voor uw Azure Data Box-Gateway (preview)

In dit artikel wordt beschreven hoe u de modus voor toegang, kracht en connectiviteit voor uw Azure Data Box-Gateway te beheren. Deze bewerkingen worden uitgevoerd via de lokale web-UI of de Azure-portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Toegang tot het apparaat beheren
> * Verbindingsmodus beheren
> * Energiebeheer

> [!IMPORTANT]
> Data Box Gateway verkeert in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert.

## <a name="manage-device-access"></a>Toegang tot het apparaat beheren

De toegang tot uw gegevens in het Gateway-apparaat wordt bepaald door het gebruik van een beheerderswachtwoord voor het apparaat. U kunt het administrator-wachtwoord via de lokale webgebruikersinterface wijzigen. U kunt ook het beheerderswachtwoord voor het apparaat in Azure portal opnieuw.

### <a name="change-device-administrator-password"></a>Beheerderswachtwoord voor het apparaat wijzigen

Volg deze stappen in de lokale gebruikersinterface te wijzigen van het beheerderswachtwoord voor het apparaat.

1. Ga in de lokale webgebruikersinterface naar **onderhoud > wachtwoordwijziging**.
2. Voer in het huidige wachtwoord en klik vervolgens op het nieuwe wachtwoord. Het opgegeven wachtwoord moet tussen 8 en 16 tekens lang zijn. Het wachtwoord moet bestaan 3 van de volgende tekens bevatten: hoofdletters, kleine letters, cijfers en speciale tekens. Controleer of het nieuwe wachtwoord.

    ![Wachtwoord wijzigen](media/data-box-gateway-manage-access-power-connectivity-mode/change-password-1.png)

3. Klik op **wachtwoord wijzigen**.
 
### <a name="reset-device-administrator-password"></a>Beheerderswachtwoord voor het apparaat opnieuw instellen

De werkstroom opnieuw instellen is niet vereist voor de gebruiker aan de hand van het oude wachtwoord en is handig als u het wachtwoord is verloren gegaan. Deze werkstroom wordt uitgevoerd in Azure portal.

1. In de Azure-portal, gaat u naar **overzicht > beheerderswachtwoord opnieuw instellen van**.

    ![Wachtwoord opnieuw instellen](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-1.png)

 
2. Voer het nieuwe wachtwoord en bevestigt u dit. Het opgegeven wachtwoord moet tussen 8 en 16 tekens lang zijn. Het wachtwoord moet bestaan 3 van de volgende tekens bevatten: hoofdletters, kleine letters, cijfers en speciale tekens. Klik op **opnieuw**.

    ![Wachtwoord opnieuw instellen](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-connectivity-mode"></a>Verbindingsmodus beheren

Uw apparaat kunt naast de normale standaardmodus ook uitvoeren in de modus gedeeltelijk niet-verbonden of niet-verbonden. Elk van deze modi wordt hieronder beschreven:

- **Gedeeltelijk verbroken** : In deze modus wordt het apparaat kan niet uploaden gegevens naar de shares echter kunnen worden beheerd via de Azure-portal.

    In deze modus wordt doorgaans gebruikt wanneer u op een netwerk met een datalimiet satelliet en het doel is om het gebruik van netwerkbandbreedte te minimaliseren. Minimale netwerkverbruik nog steeds optreden voor het controleren van de bewerkingen van apparaten.

- **De verbinding verbroken** : In deze modus wordt het apparaat is volledig losgekoppeld van de cloud en zowel de cloud uploads en downloads zijn uitgeschakeld. Het apparaat kan alleen worden beheerd via de lokale webgebruikersinterface.

    In deze modus wordt doorgaans gebruikt wanneer u offline wilt halen uw apparaat.

Apparaatmodus, Ga als volgt te werk:

1. In de lokale webgebruikersinterface van uw apparaat, gaat u naar **Configuration > Cloudinstellingen**.
2. Schakel de **uploaden en downloaden in de Cloud**.
3. Inschakelen om uit te voeren van het apparaat in de gedeeltelijk niet-verbonden modus, **beheer van Azure portal**.

    ![Verbindingsmodus](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-1.png)
 
4. Voor het uitvoeren van het apparaat in de niet-verbonden modus, uitschakelen **beheer van Azure portal**. Het apparaat kan nu alleen worden beheerd via de lokale webgebruikersinterface.

    ![Verbindingsmodus](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-2.png)

## <a name="manage-power"></a>Energiebeheer

U kunt afsluiten of opnieuw opstarten van uw fysieke en virtuele apparaten met behulp van de lokale webgebruikersinterface. We raden u aan de shares vóór de herstart offline te zetten op de host en vervolgens op het apparaat. Deze actie wordt geminimaliseerd een mogelijkheid van beschadiging van gegevens.

1. Ga in de lokale webgebruikersinterface naar **onderhoud > energie-instellingen**.
2. Klik op **afsluiten** of **opnieuw** , afhankelijk van wat u van plan bent om te doen.

    ![Energie-instellingen](media/data-box-gateway-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja** om door te gaan.

> [!NOTE]
> Als u het virtuele apparaat uitschakelen, moet u start het apparaat via de-hypervisor.
