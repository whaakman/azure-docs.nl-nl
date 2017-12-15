---
title: Met CSP-partnergegevens registeren bij Azure Cost Management | Microsoft Docs
description: Gebruik uw CSP-partnergegevens om u te registreren bij Azure Cost Management by Cloudyn.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/13/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 84f2fec61f791d4fc9264eaa01e24180696da853
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="register-with-the-csp-partner-program-and-view-cost-data"></a>Registreren met het CSP-partnerprogramma en kostengegevens weergeven

Als CSP-partner kunt u zich registreren bij Azure Cost Management by Cloudyn. Uw registratie biedt toegang tot de Cloudyn-portal. In deze Quick Start vindt u de details van de registratieprocedure voor het maken van een Cloudyn-proefabonnement en het aanmelden bij de Cloudyn-portal. U vindt er ook informatie over het weergeven van kostengegevens.


>[!NOTE]

>Alleen directe CSP-partners en indirecte CSP-providers kunnen zich bij Cloudyn registeren.
>
>Voor verificatie en toegang tot gegevens is het vereist de Partner Center API te configureren. Een Partner Center Global Administrator-account is nodig voor het inrichten van de API-toegang.
Zie [Verbinding maken met de Partner Center API](https://msdn.microsoft.com/library/partnercenter/mt709136.aspx) voor meer informatie.
>
>Indirecte CSP-resellers kan toegang worden gegeven tot Cloudyn nadat hun indirecte CSP-provider zich bij Cloudyn heeft geregistreerd. Indirecte CSP-resellers kunnen vervolgens zorgen dat Azure-klanten en abonnementen toegang hebben tot Cloudyn.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

- Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-trial-registration"></a>Registreren voor een proefabonnement

1. Klik in Azure Portal, in de lijst met services, op **Cost Management en facturering**.
2. Klik onder **Overzicht** op **Cost Management**  
    ![Pagina Cost Management](./media/quick-register-csp/cost-mgt-billing-service.png)
3. Klik op de pagina **Cost Management** op **Go to Cost Management** om de Cloudyn-registratiepagina in een nieuw venster te openen.
4. Typ op de pagina voor de registratie van het proefabonnement in de Cloudyn-portal de naam van uw bedrijf en selecteer **Microsoft CSP Partner Program Administrator** en klik op **Volgende**.  
5. Voer een **toepassings-id**, een **handels-id**, **geheime sleutel van de toepassing** in, en selecteer het **standaardprijsplan**. Als u de gegevens niet bij die hand hebt, meldt u zich aan bij de portal Partnercentrum [https://partnercenter.microsoft.com](https://partnercenter.microsoft.com) met uw primair beheerdersaccount en voert u de volgende stappen uit:
  1. Ga naar **Dashboard** > **Accountinstellingen** > **App-beheer**.
  2. Als u eerder al een web-app hebt gemaakt, kunt u deze stap overslaan. Klik anders op **Nieuwe web-app toevoegen** in de sectie **Web-app**.
  3. Kopieer de **app-id**-GUID van uw webtoepassing.
  4. Kopieer de **handels-id**-GUID van uw webtoepassing.
  5. Selecteer de geldigheidsduur van de sleutel desgewenst voor één of twee jaar. Selecteer **Sleutel toevoegen**, kopieer vervolgens de waarde voor de geheime sleutel en sla deze op.  
    ![CSP Partner Center](./media/quick-register-csp/csp-partner-center.png)
  6. Ga terug naar de registratiepagina en plak de gegevens erin.  
      ![CSP-accountreferenties](./media/quick-register-csp/csp-reg.png)
6. Accepteer de Gebruiksvoorwaarden en controleer uw gegevens. Klik op **Volgende** om Cloudyn te autoriseren Azure-resourcegegevens te verzamelen. De gegevens die worden verzameld zijn onder meer gegevens over gebruik, prestaties, facturering en tags van uw abonnementen.  
7. Onder **Invite other stakeholders** kunt u gebruikers toevoegen door hun e-mailadressen in te voeren. Klik op **Volgende** als u klaar bent. Het duurt ongeveer twee uur voordat uw factureringsgegevens aan Cloudyn zijn toegevoegd.
8. Klik op **Go to Cloudyn** om de Cloudyn-portal te openen. Op de pagina **Cloud Accounts Management** zou u de geregistreerde CSP-accountgegevens moeten kunnen zien.

## <a name="configure-indirect-csp-access-in-cloudyn"></a>Indirecte CSP-toegang in Cloudyn configureren

De Partner Center API is standaard alleen toegankelijk voor directe CSP's. Een directe CSP-provider kan echter toegang configureren voor hun indirecte CSP-klanten of -partners met behulp van entiteitsgroepen in Cloudyn.

Volg de stappen in [Registreren voor een proefabonnement](#create-a-trial-registration) voor het geven van toegang aan indirecte CSP-klanten of -partners, om een registratie voor een proefabonnement in te stellen. Voer vervolgens de volgende stappen uit voor het segmenteren van indirecte CSP-gegevens met behulp van Cloudyn-entiteitsgroepen. Wijs daarna de juiste gebruikersmachtigingen toe aan de entiteitsgroepen.

1. Maak met deze gegevens een entiteitsgroep via [Entiteiten maken](tutorial-user-access.md#create-entities).
2. Volg de stappen in [Abonnementen toewijzen aan kostentiteiten](https://support.cloudyn.com/hc/en-us/articles/115005139425-Video-Assigning-subscriptions-to-Cost-Entities). Koppel de indirecte CSP-klant en de bijbehorende Azure-abonnementen aan de entiteit die u eerder hebt gemaakt.
3. Volg de stappen in [Een gebruiker met beheerderstoegang maken](tutorial-user-access.md#create-a-user-with-admin-access) om een gebruikersaccount met beheerderstoegang te maken. Controleer vervolgens of het gebruikersaccount beheerderstoegang heeft tot de specifieke entiteiten die u eerder hebt gemaakt voor het indirecte account.

Indirecte CSP-partners kunnen zich aanmelden bij de Cloudyn-portal met behulp van de accounts die u voor hen hebt gemaakt.


[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u de CSP-gegevens van uw Azure-abonnement gebruikt om u te registreren bij Cost Management. U hebt zich ook aangemeld bij de Cloudyn-portal en de gegevenskosten weergegeven. Ga door naar de zelfstudie voor Cost Management voor meer informatie over Azure Cost Management by Cloudyn.

> [!div class="nextstepaction"]
> [Gebruik en kosten controleren](./tutorial-review-usage.md)
