In de voorgaande stappen hebt u Azure-resources in een resourcegroep gemaakt. Als u deze resources in de toekomst waarschijnlijk niet nodig hebt, kunt u ze verwijderen door de resourcegroep te verwijderen.
 
1. Voer de volgende opdracht uit om te controleren of de resourcegroep geen resources bevat die u wilt opslaan:

  ```azurecli
  az group show --name myResourceGroup
  ```

2. Als de weergegeven resources de resources zijn die u wilt verwijderen, voert u de volgende opdracht uit:
 
  ```azurecli
  az group delete --name myResourceGroup
  ```
