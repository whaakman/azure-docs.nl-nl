---
title: Externe bewaking access control - Azure | Microsoft Docs
description: In dit artikel bevat informatie over hoe u op rollen gebaseerd toegangsbeheer (RBAC) in de oplossingsverbetering voor externe controle kunt configureren
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/06/2018
ms.topic: conceptual
ms.openlocfilehash: 56fbb5d45e55e63ae887d915367cfc649e531095
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51820219"
---
# <a name="configure-role-based-access-controls-in-the-remote-monitoring-solution-accelerator"></a>Op rollen gebaseerd toegangsbeheer in de oplossingsverbetering voor externe controle configureren

In dit artikel bevat informatie over het configureren van op rollen gebaseerd toegangsbeheer in de oplossingsverbetering voor externe controle. Op rollen gebaseerd toegangsbeheer kunnen u toegang voor afzonderlijke gebruikers beperken tot specifieke functies in de oplossing.

## <a name="default-settings"></a>Standaardinstellingen

Wanneer u eerst de oplossing voor externe controle implementeert, zijn er twee rollen: **Admin** en **alleen-lezen**.

Een gebruiker in de **Admin** rol heeft volledige toegang tot de oplossing, met inbegrip van de volgende machtigingen hieronder. Een gebruiker in de **alleen-lezen** rol hebben alleen toegang tot de oplossing bekijken.

| Machtiging            | Gemeente | Alleen-lezen |
|----------------       |-------|-----------|
| Weergave-oplossing         | Ja   | Ja       |
| Waarschuwingen bijwerken         | Ja   | Nee        |
| Waarschuwingen verwijderen         | Ja   | Nee        |
| Apparaten maken        | Ja   | Nee        |
| Apparaten bijwerken        | Ja   | Nee        |
| Apparaten verwijderen        | Ja   | Nee        |
| Apparaatgroepen maken  | Ja   | Nee        |
| Apparaatgroepen bijwerken  | Ja   | Nee        |
| Apparaatgroepen verwijderen  | Ja   | Nee        |
| Regels maken          | Ja   | Nee        |
| Update-regels          | Ja   | Nee        |
| Regels verwijderen          | Ja   | Nee        |
| Taken maken           | Ja   | Nee        |
| Updatebeheer voor SIM-kaart | Ja   | Nee        |

Standaard wordt de gebruiker die de oplossing geïmplementeerd automatisch toegewezen de **Admin** rol en de eigenaar van een Azure Active Directory-toepassing is. Als de toepassingseigenaar van een, kunt u rollen toewijzen aan andere gebruikers via de Azure-portal. Als u een andere gebruiker wilt het toewijzen van rollen in de oplossing, moeten ze ook worden ingesteld als de toepassingseigenaar van een in Azure portal.

> [!NOTE]
> De gebruiker die de oplossing wordt geïmplementeerd, is de **alleen persoon** die het kunnen bekijken onmiddellijk na de zijn gemaakt. Anderen toegang te verlenen om de toepassing als een alleen-lezen, Admin, of een aangepaste rol, Zie de volgende instructies hieronder op toevoegen of verwijderen van gebruikers weer te geven.

## <a name="add-or-remove-users"></a>Gebruikers toevoegen of verwijderen

Als de eigenaar van een Azure Active Directory-toepassing kunt u de Azure-portal toevoegen of verwijderen van een gebruiker aan een rol van de oplossing voor externe controle. De volgende stappen uitvoeren om de [Azure Active Directory-bedrijfstoepassing](../active-directory/manage-apps/add-application-portal.md#find-your-azure-ad-tenant-application) die is gemaakt tijdens de implementatie van de oplossing voor externe controle.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Controleer de [gebruiker zich in de map](../active-directory/fundamentals/add-users-azure-active-directory.md) u gebruikt. U hebt ervoor gekozen de map moet worden gebruikt wanneer u aangemeld bij de [Microsoft Azure IoT-oplossingsversnellers](https://www.azureiotsolutions.com/Accelerators) site. Naam van de map is zichtbaar in de rechterbovenhoek van de [pagina](https://www.azureiotsolutions.com/Accelerators).

1. Zoek de **bedrijfstoepassing** voor uw oplossing in Azure portal. Zodra er, filter de lijst door in te stellen **toepassingstype** naar **alle toepassingen**. Zoeken naar de naam van uw toepassing door de toepassing. Naam van de toepassing is de naam van uw oplossing voor externe controle. In de volgende schermopname is de oplossing en de toepassing weergavenamen zijn **contoso-rm4**.

    ![Bedrijfstoepassing](media/iot-accelerators-remote-monitoring-rbac/appregistration.png)

1. U bent eigenaar van de toepassing door te klikken op de toepassing en vervolgens te klikken op controleren **eigenaren**. In de volgende schermafbeelding **admin Contoso** is eigenaar van de **contoso-rm4** toepassing:

    ![Eigenaren](media/iot-accelerators-remote-monitoring-rbac/owners.png)

    Als u niet een eigenaar, moet u de eigenaar van een bestaand u toe te voegen aan de lijst met vragen. Alleen eigenaren toepassingsrollen zoals toewijzen kunnen **Admin** of **alleen-lezen** aan andere gebruikers.

1. De lijst met gebruikers die zijn toegewezen aan rollen in de toepassing wilt bekijken, klikt u op **gebruikers en groepen**.

1. Een gebruiker toevoegen, klikt u op **+ gebruiker toevoegen**, en klik vervolgens op **gebruikers en groepen, geen geselecteerd** om te selecteren van een gebruiker uit de map.

1. Als u wilt de gebruiker toewijzen aan een rol, klikt u op **rol selecteren, geen geselecteerd** en kiest u de **Admin** of **alleen-lezen** rol voor de gebruiker. Klik op **Selecteer**, en klik vervolgens op **toewijzen**.

    ![Rol selecteren](media/iot-accelerators-remote-monitoring-rbac/selectrole.png)

1. De gebruiker kan nu toegang tot de oplossing voor externe controle met de machtigingen die zijn gedefinieerd door de rol.

1. U kunt gebruikers van de toepassing verwijderen op de **gebruikers en groepen** pagina in de portal.

## <a name="create-a-custom-role"></a>Een aangepaste rol maken

De oplossing voor externe controle bevat de **Admin** en **alleen-lezen** rollen wanneer het eerst wordt geïmplementeerd. U kunt aangepaste rollen met verschillende sets machtigingen toevoegen. Voor het definiëren van een aangepaste rol, moet u naar:

- Een nieuwe rol toevoegen aan de toepassing in Azure portal.
- Definieer een beleid voor de nieuwe rol in de verificatie en autorisatie-microservice.
- Bijwerken van de oplossing-Webgebruikersinterface.

### <a name="define-a-custom-role-in-the-azure-portal"></a>Een aangepaste rol definiëren in Azure portal

De volgende stappen wordt beschreven hoe u een rol toevoegt aan een toepassing in Azure Active Directory. In dit voorbeeld maakt u een nieuwe functie waarmee u leden wilt maken, bijwerken en verwijderen van apparaten in de oplossing voor externe controle.

1. Zoek de **App-registratie** voor uw oplossing in Azure portal. Naam van de toepassing is de naam van uw oplossing voor externe controle. In de volgende schermopname is de oplossing en de toepassing weergavenamen zijn **contoso-rm4**.

    ![App-registratie](media/iot-accelerators-remote-monitoring-rbac/appregistration2.png)

1. Selecteer uw toepassing en klik vervolgens op **Manifest**. U ziet de twee bestaande [app-rollen](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) gedefinieerd voor de toepassing:

    ![Weergave-manifest](media/iot-accelerators-remote-monitoring-rbac/viewmanifest.png)

1. Bewerken van het manifest als u wilt toevoegen van een rol met de naam **ManageDevices** zoals wordt weergegeven in het volgende codefragment. U moet een unieke tekenreeks, zoals een GUID voor de nieuwe rol-ID. U kunt een nieuwe GUID met behulp van een service zoals genereren de [Online GUID Generator](https://www.guidgenerator.com/):

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Admin",
        "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "isEnabled": true,
        "description": "Administrator access to the application",
        "value": "Admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Read Only",
        "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "isEnabled": true,
        "description": "Read only access to device information",
        "value": "ReadOnly"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "ManageDevices",
        "id": "ADD A NEW GUID HERE",
        "isEnabled": true,
        "description": "A new role for the application.",
        "value": "ManageDevices"
      }
    ],
    ```

    Sla de wijzigingen op.

### <a name="define-a-policy-for-the-new-role"></a>Een beleid voor de nieuwe rol definiëren

Nadat de functie toevoegen aan de app in Azure portal, moet u een beleid in definiëren [roles.json](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/auth/Services/data/policies/roles.json) voor de rol die de machtiging die nodig zijn om apparaten te beheren.

1. Kloon de [Microservices voor externe bewaking](https://github.com/Azure/remote-monitoring-services-dotnet) opslagplaats van GitHub naar uw lokale computer.

1. Bewerk de **auth/Services/data/policies/roles.json** bestand om toe te voegen van het beleid voor de **ManageDevices** rol zoals wordt weergegeven in het volgende codefragment. De **ID** en **rol** waarden moeten overeenkomen met de roldefinitie van de in het app-manifest van de vorige sectie. De lijst met toegestane acties kan iemand in de **ManageDevices** rol die u wilt maken, bijwerken en verwijderen van apparaten die zijn verbonden met de oplossing:

    ```json
    {
    "Items": [
      {
        "Id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "Role": "admin",
        "AllowedActions": [
          "UpdateAlarms",
          "DeleteAlarms",
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices",
          "CreateDeviceGroups",
          "UpdateDeviceGroups",
          "DeleteDeviceGroups",
          "CreateRules",
          "UpdateRules",
          "DeleteRules",
          "CreateJobs",
          "UpdateSimManagement"
        ]
      },
      {
        "Id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "Role": "readOnly",
        "AllowedActions": []
      },
      {
        "Id": "GUID FROM APP MANIFEST",
        "Role": "ManageDevices",
        "AllowedActions": [
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices"
        ]
      }
    ]
    }
    ```

1. Wanneer u klaar bent met bewerken de **Services/data/policies/roles.json** bestand, opnieuw maken en implementeren van de verificatie en autorisatie microservice uw oplossingenaccelerator.

### <a name="how-the-web-ui-enforces-permissions"></a>Hoe machtigingen worden afgedwongen in de web-UI

De web UI gebruikt de [verificatie en autorisatie microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth) om te bepalen welke acties van een gebruiker is toegestaan om door te nemen en welke besturingselementen worden weergegeven in de gebruikersinterface. Bijvoorbeeld, als uw oplossing heet **contoso-rm4**, de web-UI wordt een lijst van toegestane acties voor de huidige gebruiker opgehaald door de volgende aanvraag te verzenden:

```http
http://contoso-rm4.azurewebsites.net/v1/users/current
headers:
X-Source: true
Authorization: Bearer <JWT Token from ADAL>
```

Voor een gebruiker met de naam **Apparaatbeheer** in de **ManageDevices** rol, het antwoord bevat de volgende JSON in de hoofdtekst:

```json
{
  "Id": "userIdExample",
  "Email": "devicemanager@contoso.com",
  "Name": "Device Manager",
  "AllowedActions": [
    "CreateDevices",
    "UpdateDevices",
    "DeleteDevices"
  ]
}
```

Het volgende fragment van [deviceDelete.js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/pages/devices/flyouts/deviceDelete/deviceDelete.js) in de [web-UI van](https://github.com/Azure/pcs-remote-monitoring-webui/) laat zien hoe de machtigingen declaratief wordt toegepast:

```json
<FlyoutContent>
  <Protected permission={permissions.deleteDevices}>
    <form className="device-delete-container" onSubmit={this.deleteDevices}>
      ...
    </form>
  </Protected>
</FlyoutContent>
```

Zie voor meer informatie, [beveiligde onderdelen](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/shared/protected/README.md). Kunt u extra machtigingen in de [authModel.js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/services/models/authModels.js) bestand.

### <a name="how-the-microservices-enforce-permissions"></a>Hoe de machtigingen voor het afdwingen van de microservices

De microservices ook controleren machtigingen om te beveiligen tegen onbevoegde API-aanvragen. Wanneer een microservice een API-aanvraag ontvangt, decodeert en valideert de JWT-token om op te halen van de gebruikers-ID en de machtigingen die zijn gekoppeld aan de gebruikersrol.

Het volgende fragment van het [DevicesController.cs](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/iothub-manager/WebService/v1/Controllers/DevicesController.cs) -bestand in de [IoTHub Manager microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager), ziet u hoe de machtigingen worden toegepast:

```csharp
[HttpDelete("{id}")]
[Authorize("DeleteDevices")]
public async Task DeleteAsync(string id)
{
    await this.devices.DeleteAsync(id);
}
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe op rollen gebaseerde toegang besturingselementen zijn geïmplementeerd in de oplossingsverbetering voor externe controle.

Zie [toegangsbeheer configureren voor de Verkenner van Time Series Insights](iot-accelerators-remote-monitoring-rbac-tsi.md) voor informatie over het beheren van toegang tot de Time Series Insights explorer in de oplossingsverbetering voor externe controle.

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Zie voor meer informatie over het aanpassen van de oplossing voor externe controle [aanpassen en opnieuw implementeren van een microservice](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->