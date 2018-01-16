# <a name="persist-files-in-azure-cloud-shell"></a>Bestanden in de Azure-Cloud-Shell behouden
Azure File storage om te blijven behouden bestanden over de sessies maakt gebruik van cloud-Shell.

## <a name="set-up-a-clouddrive-file-share"></a>Een bestandsshare clouddrive instellen
Op de eerste start vraagt Cloud-Shell u om te koppelen van een nieuwe of bestaande bestandsshare om te blijven behouden bestanden over de sessies.

> [!NOTE]
> Delen dezelfde bestandsshare Bash en PowerShell. Slechts één bestandsshare kan worden gekoppeld aan het automatisch koppelen in de Cloud-Shell.

### <a name="create-new-storage"></a>Maken van nieuwe opslag

Wanneer u basisinstellingen en alleen een abonnement selecteren, maakt Cloud Shell drie bronnen in de ondersteunde regio die het dichtst bij u namens jou:
* Resourcegroep: `cloud-shell-storage-<region>`
* Storage-account:`cs<uniqueGuid>`
* Bestandsshare:`cs-<user>-<domain>-com-<uniqueGuid>`

![De instelling voor Cloudabonnement](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

De bestandsshare koppelt als `clouddrive` in uw `$Home` directory. Dit is een eenmalige bewerking en de bestandsshare koppelt automatisch in de volgende sessies. 

In Bash, bevat de bestandsshare ook een afbeelding 5 GB automatisch voor u die gemaakt wordt zich blijft voordoen gegevens in uw `$Home` directory. 

### <a name="use-existing-resources"></a>Bestaande bronnen gebruiken

U kunt bestaande resources koppelen met behulp van de geavanceerde optie. Wanneer de opslag setup prompt wordt weergegeven, selecteert u **weergeven geavanceerde instellingen** om extra opties weer te geven. De vervolgkeuzemenu's worden voor uw toegewezen Shell Cloud-regio en lokaal redundante opslag en geo-redundant storage-accounts gefilterd.

Bestaande bestandsshares ontvangt Bash, een installatiekopie van 5 GB gemaakt voor u om vast te leggen uw `$Home` directory.

![De instelling voor groep](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Bron maken met een Azure-resource beleid beperken
Storage-accounts die u in de Cloud-Shell maakt zijn gelabeld met `ms-resource-usage:azure-cloud-shell`. Als u wilt weigeren gebruikers van de storage-accounts maken in de Cloud-Shell, maakt u een [Azure bronbeleid voor tags](../articles/azure-policy/json-samples.md) die worden geactiveerd door deze specifieke tag.

## <a name="supported-storage-regions"></a>Ondersteunde opslagregio
Gekoppelde accounts moeten zich bevinden in dezelfde regio bevinden als de Cloud Shell-machine die u ze kunt koppelen aan Azure storage.

U kunt uw toegewezen regio vinden:
* De opmerking in het dialoogvenster 'Geavanceerde Opslaginstellingen' weergeven
* Raadpleeg de naam van het opslagaccount voor u gemaakt (ex: `cloud-shell-storage-westus`)
* Voer `env` en zoek de variabele`ACC_LOCATION`

Cloud-Shell bestaat in de volgende gebieden:
|Onderwerp|Regio|
|---|---|
|Noord- en Zuid-Amerika|VS-Oost, Zuid-centraal VS, VS-West|
|Europa|Noord-Europa, West-Europa|
|Azië en Stille Oceaan|India centraal, Zuidoost-Azië|

