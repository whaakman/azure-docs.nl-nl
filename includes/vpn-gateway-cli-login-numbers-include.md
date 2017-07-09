1. Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm. Zie [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) voor meer informatie over aanmelden.

  ```azurecli
  az login
  ```
2. Als u meer dan één Azure-abonnement hebt, worden alle abonnementen voor het account weergegeven.

  ```azurecli
  az account list --all
  ```
3. Geef het abonnement op dat u wilt gebruiken.

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```