## <a name="create-a-project-zip-file"></a>Een ZIP-bestand van het project maken

Maak een ZIP-archief van alle bestanden in uw project. De volgende opdracht maakt gebruik van het standaardhulpprogramma in de terminal:

```
# Bash
zip -r myAppFiles.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath myAppFiles.zip
``` 

U kunt dit ZIP-bestand later uploaden naar Azure en dit implementeren in App Service.