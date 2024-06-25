---
lab:
  title: "Gérer la sécurité d’Azure\_AI\_Services"
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Gérer la sécurité d’Azure AI Services

La sécurité est une considération essentielle pour toute application. En tant que développeur, vous devez vous assurer que l’accès aux ressources telles qu’Azure AI Services est limité aux seules personnes qui en ont besoin.

L’accès à Azure AI Services est généralement contrôlé par le biais de clés d’authentification générées lorsque vous créez initialement une ressource Azure AI Services.

## Cloner le référentiel dans Visual Studio

Vous allez développer votre code en utilisant Visual Studio Code. Les fichiers de code de votre application ont été fournis dans un référentiel GitHub.

> **Conseil** : Si vous avez déjà cloné le référentiel **mslearn-ai-services** récemment, ouvrez-le dans Visual Studio Code. Dans le cas contraire, procédez comme suit pour le cloner dans votre environnement de développement.

1. Démarrez Visual Studio Code.
2. Ouvrez la palette (Maj+CTRL+P) et exécutez une commande **Git : Cloner** pour cloner le référentiel `https://github.com/MicrosoftLearning/mslearn-ai-services` vers un dossier local (peu importe quel dossier).
3. Lorsque le référentiel a été cloné, ouvrez le dossier dans Visual Studio Code.
4. Si nécessaire, attendez que les fichiers supplémentaires soient installés pour prendre en charge les projets de code C# dans le référentiel

    > **Remarque** : si vous êtes invité à ajouter des ressources requises pour générer et déboguer, sélectionnez **Not Now** (Pas maintenant).

5. Développez le dossier `Labfiles/02-ai-services-security`.

Le code pour C# et pour Python a été fourni. Développez le dossier de votre langage préféré.

## Provisionner une ressource Azure AI Services

Si vous n’en avez pas encore dans votre abonnement, vous devez configurer une ressource **Azure AI Services**.

1. Ouvrez le portail Azure à l’adresse `https://portal.azure.com` et connectez-vous avec le compte Microsoft associé à votre abonnement Azure.
2. Dans la barre de recherche supérieure, recherchez *Azure AI services*, sélectionnez **Azure AI Services** et créez une ressource de compte multiservices Azure AI services avec les paramètres suivants :
    - **Abonnement** : *votre abonnement Azure*
    - **Groupe de ressources** : *Choisissez ou créez un groupe de ressources. (Si vous utilisez un abonnement restreint, vous n’avez peut-être pas l’autorisation de créer un groupe de ressources. Dans ce cas, utilisez le groupe fourni.)*
    - **Région** : *choisissez n’importe quelle région disponible*
    - **Nom** : *Entrez un nom unique.*
    - **Niveau tarifaire** : Standard S0
3. Cochez les cases nécessaires et créez la ressource.
4. Attendez la fin du déploiement, puis visualisez les détails du déploiement.

## Gérer les clés d'authentification

Lorsque vous avez créé votre ressource Azure AI Services, deux clés d’authentification ont été générées. Vous pouvez les gérer dans le portail Azure ou en utilisant l'interface de ligne de commande (CLI) Azure.

1. Dans le portail Azure, accédez à votre ressource Azure AI Services et affichez sa page **Clés et point de terminaison**. Cette page contient les informations dont vous aurez besoin pour vous connecter à votre ressource et l’utiliser à partir d’applications que vous développez. Plus précisément :
    - *Point de terminaison* HTTP auquel les applications clientes peuvent envoyer des requêtes.
    - Deux *clés* qui peuvent être utilisées pour l’authentification (les applications clientes peuvent utiliser l’une des clés. Une pratique courante consiste à en utiliser une pour le développement, et une autre pour la production. Vous pouvez facilement régénérer la clé de développement une fois que les développeurs ont terminé leur travail pour empêcher l’accès continu.
    - *Emplacement* dans lequel la ressource est hébergée. Cela est nécessaire pour les demandes adressées à certaines API (mais pas toutes).
2. Vous pouvez maintenant utiliser la commande suivante pour obtenir la liste des clés Azure AI Services, en remplaçant *&lt;resourceName&gt;* par le nom de votre ressource Azure AI Services et *&lt;resourceGroup&gt;* par le nom du groupe de ressources dans lequel vous l’avez créée.

    ```
    az cognitiveservices account keys list --name <resourceName> --resource-group <resourceGroup>
    ```

    La commande retourne une liste des clés de votre ressource Azure AI Services : il existe deux clés nommées **key1** et **key2**.

    > **Conseil** : Si vous ne vous êtes pas encore authentifié auprès d’Azure CLI, exécutez `az login` et connectez-vous à votre compte.

3. Pour tester votre service Azure AI, vous pouvez utiliser **curl**, un outil de ligne de commande pour les requêtes HTTP. Dans le dossier **02-ai-services-security**, ouvrez **rest-test.cmd** et modifiez la commande **curl** qu’il contient (montrée ci-dessous), en remplaçant *&lt;yourEndpoint&gt;* et *&lt;yourKey&gt;* par l’URI et la clé **Key1** de votre point de terminaison pour utiliser l’API Analyse de texte dans votre ressource Azure AI services.

    ```bash
    curl -X POST "<yourEndpoint>/language/:analyze-text?api-version=2023-04-01" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: 81468b6728294aab99c489664a818197" --data-ascii "{'analysisInput':{'documents':[{'id':1,'text':'hello'}]}, 'kind': 'LanguageDetection'}"
    ```

4. Enregistrez vos modifications, puis exécutez la commande suivante :

    ```
    ./rest-test.cmd
    ```

La commande retourne un document JSON contenant des informations sur la langue détectée dans les données d’entrée (qui doivent être anglais).

5. Si une clé devient compromise ou si les développeurs n’ont plus besoin d’accès, vous pouvez la régénérer dans le portail ou à l’aide d’Azure CLI. Exécutez la commande suivante pour régénérer votre **clé key1** (en remplaçant *&lt;resourceName&gt;* et *&lt;resourceGroup&gt;* par les informations de votre ressource).

    ```
    az cognitiveservices account keys regenerate --name <resourceName> --resource-group <resourceGroup> --key-name key1
    ```

La liste des clés de votre ressource Azure AI Services est retournée : notez que **key1** a changé depuis la dernière récupération.

6. Réexécutez la commande **rest-test** avec l’ancienne clé (vous pouvez utiliser la flèche **^** sur votre clavier pour parcourir en boucle les commandes précédentes) : vous pouvez maintenant constater qu’elle échoue.
7. Modifiez la commande *curl* dans **rest-test.cmd** en remplaçant la clé par la nouvelle valeur **key1**, puis enregistrez les modifications. Réexécutez ensuite la commande **rest-test** et vérifiez qu’elle réussit.

> **Conseil** : Dans cet exercice, vous avez utilisé les noms complets des paramètres Azure CLI, tels que **--resource-group**.  Vous pouvez également utiliser des alternatives plus courtes, telles que **-g**, pour rendre vos commandes moins détaillées (mais un peu plus difficile à comprendre).  La [référence des commandes CLI d’Azure AI Services](https://docs.microsoft.com/cli/azure/cognitiveservices?view=azure-cli-latest) répertorie les options de paramètre pour chaque commande CLI d’Azure AI Services.

## Sécuriser l’accès à la clé avec Azure Key Vault

Vous pouvez développer des applications qui consomment Azure AI Services à l’aide d’une clé pour l’authentification. Toutefois, cela signifie que le code de l’application doit être en mesure d’obtenir la clé. Une option consiste à stocker la clé dans une variable d’environnement ou un fichier de configuration où l’application est déployée, mais cette approche laisse la clé vulnérable à un accès non autorisé. Une meilleure approche lors du développement d’applications sur Azure consiste à stocker la clé en toute sécurité dans Azure Key Vault et à fournir l’accès à la clé via une *identité managée* (en d’autres termes, un compte d’utilisateur utilisé par l’application elle-même).

### Créer un coffre de clés et ajouter un secret

Tout d’abord, vous devez créer un coffre de clés et ajouter un *secret* pour la clé d’Azure AI Services.

1. Notez la valeur **key1** de votre ressource Azure AI Services (ou copiez-la dans le Presse-papiers).
2. Dans le Portail Azure, dans la page **Accueil**, sélectionnez le bouton **&#65291;Créer une ressource**, recherchez *Key Vault* et créez une ressource **Key Vault** avec les paramètres suivants :

    - Onglet **Informations de base**
        - **Abonnement** : *votre abonnement Azure*
        - **Groupe de ressources** : * le même groupe de ressources que votre ressource de service Azure AI*
        - **Nom du coffre de clés** : *Entrez un nom unique*
        - **Région** : *la même région que votre ressource Azure AI Services*
        - **Niveau tarifaire** : Standard
    
    - Onglet **Configuration de l’accès**
        -  **Modèle d’autorisation** : stratégie d’accès au coffre
        - Faites défiler jusqu’à la section **Stratégies d’accès** et sélectionnez votre utilisateur à l’aide de la case à cocher sur la gauche. Sélectionnez ensuite **Vérifier + créer**, puis **Créer** pour créer votre ressource.
     
3. Attendez que le déploiement soit terminé, puis accédez à votre ressource de coffre de clés.
4. Dans le volet de navigation gauche, sélectionnez **Secrets** (dans la section Objets).
5. Sélectionnez **+ Générer/Importer** et ajoutez un nouveau secret avec les paramètres suivants :
    - **Options de chargement** : Manuelle
    - **Nom** : AI-Services-Key *(il est important que ceci corresponde exactement car plus tard, vous allez exécuter du code qui récupère le secret en fonction de ce nom)*
    - **Valeur** : *votre clé **key1** Azure AI Services*
6. Sélectionnez **Créer**.

### Créer un principal du service

Pour accéder au secret dans le coffre de clés, votre application doit utiliser un principal de service qui a accès au secret. Vous allez utiliser l’interface de ligne de commande Azure (CLI) pour créer le principal de service, rechercher son ID d’objet et accorder l’accès au secret dans Azure Vault.

1. Exécutez la commande Azure CLI suivante, en remplaçant *&lt;spName&gt;* par un nom unique approprié pour une identité d’application (par exemple *ai-app* avec vos initiales ajoutées à la fin ; le nom doit être unique dans votre locataire). Remplacez également *&lt;subscriptionId&gt;* et *&lt;resourceGroup&gt;* par les valeurs appropriées pour votre ID d’abonnement et le groupe de ressources contenant vos ressources Azure AI Services et de coffre de clés :

    > **Conseil** : Si vous n’êtes pas sûr de votre ID d’abonnement, utilisez la commande **az account show** pour récupérer vos informations d’abonnement : l’ID d’abonnement est l’attribut **id** dans la sortie. Si une erreur s’affiche, indiquant que l’objet existe déjà, choisissez un autre nom unique.

    ```
    az ad sp create-for-rbac -n "api://<spName>" --role owner --scopes subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>
    ```

La sortie de cette commande inclut des informations sur votre nouveau principal de service. Il doit ressembler à ceci :

    ```
    {
        "appId": "abcd12345efghi67890jklmn",
        "displayName": "api://ai-app-",
        "password": "1a2b3c4d5e6f7g8h9i0j",
        "tenant": "1234abcd5678fghi90jklm"
    }
    ```

Prenez note des valeurs de **appId**, **password** et **tenant**, car vous en aurez besoin plus tard (si vous fermez ce terminal, vous ne serez pas en mesure de récupérer le mot de passe : il est donc important de noter les valeurs maintenant.Vous pouvez coller la sortie dans un nouveau fichier texte sur votre machine locale pour être sûr de trouver les valeurs dont vous avez besoin plus tard.)

2. Pour obtenir **l’ID d’objet** de votre principal de service, exécutez la commande Azure CLI suivante, en remplaçant *&lt;appId&gt;* par la valeur de l’ID d’application de votre principal de service.

    ```
    az ad sp show --id <appId>
    ```

3. Copiez la valeur de `id` dans le JSON retourné en réponse. 
3. Pour attribuer à votre nouveau principal de service l’autorisation d’accéder aux secrets de votre coffre de clés, exécutez la commande Azure CLI suivante, en remplaçant *&lt;keyVaultName&gt;* par le nom de votre ressource Azure Key Vault et *&lt;objectId&gt;* par la valeur de l’ID de principal de service que vous venez de copier.

    ```
    az keyvault set-policy -n <keyVaultName> --object-id <objectId> --secret-permissions get list
    ```

### Utiliser le principal de service dans une application

Vous êtes maintenant prêt à utiliser l’identité du principal de service dans une application, afin qu’elle puisse accéder à la clé secrète Azure AI Services dans votre coffre de clés et l’utiliser pour vous connecter à votre ressource Azure AI Services.

> **Remarque** : Dans cet exercice, nous allons stocker les informations d’identification du principal de service dans la configuration de l’application et les utiliser pour authentifier une identité **ClientSecretCredential** dans votre code d’application. Cela est parfait pour le développement et le test, mais dans une application de production réelle, un administrateur affecterait une *identité managée* à l’application afin qu’elle utilise l’identité du principal de service pour accéder aux ressources, sans mettre en cache ou stocker le mot de passe.

1. Dans votre terminal, accédez au dossier **C-Sharp** ou **Python**, selon le langage que vous avez choisi, en exécutant `cd C-Sharp` ou `cd Python`. Exécutez ensuite `cd keyvault_client` pour accéder au dossier de l’application.
2. Ensuite, installez les packages que vous devez utiliser pour Azure Key Vault et l’API Analyse de texte dans votre ressource Azure AI services en exécutant la commande appropriée pour le langage que vous avez choisi :

    **C#**

    ```
    dotnet add package Azure.AI.TextAnalytics --version 5.3.0
    dotnet add package Azure.Identity --version 1.5.0
    dotnet add package Azure.Security.KeyVault.Secrets --version 4.2.0-beta.3
    ```

    **Python**

    ```
    pip install azure-ai-textanalytics==5.3.0
    pip install azure-identity==1.5.0
    pip install azure-keyvault-secrets==4.2.0
    ```

3. Affichez le contenu du dossier **keyvault-client**, et notez qu’il contient un fichier pour les paramètres de configuration :
    - **C#**  : appsettings.json
    - **Python** : .env

    Ouvrez le fichier de configuration et mettez à jour les valeurs de configuration qu’il contient pour refléter les paramètres suivants :
    
    - **Point de terminaison** de votre ressource Azure AI Services
    - Nom de votre ressource **Azure Key Vault**
    - ID de **locataire** pour votre principal de service
    - **appid** pour votre principal de service
    - **Mot de passe** pour votre principal de service

     Enregistrez vos modifications en appuyant sur **Ctrl+S**.
4. Notez que le dossier **keyvault-client** contient un fichier de code pour l’application cliente :

    - **C#** : Program.cs
    - **Python** : keyvault-client.py

    Ouvrez le fichier de code et passez en revue le code qu’il contient, en notant les détails suivants :
    - L’espace de noms du kit de développement logiciel (SDK) que vous avez installé est importé
    - Le code de la fonction **Main** récupère les paramètres de configuration de l’application, puis utilise les informations d’identification du principal de service pour obtenir la clé Azure AI Services à partir du coffre de clés.
    - La fonction **GetLanguage** utilise le kit de développement logiciel (SDK) pour créer un client pour le service, puis utilise le client pour détecter la langue du texte entré.
5. Entrez la commande suivante pour exécuter le programme :

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python keyvault-client.py
    ```

6. Lorsque vous y êtes invité, entrez du texte et passez en revue la langue détectée par le service. Par exemple, essayez d’entrer « Hello », « Bonjour » et « Gracias ».
7. Lorsque vous avez terminé de tester l’application, entrez « quit » pour arrêter le programme.

## Nettoyer les ressources

Si vous n’utilisez pas les ressources Azure créées dans ce labo pour d’autres modules de formation, vous pouvez les supprimer pour éviter d’autres frais.

1. Ouvrez le Portail Azure à l'adresse `https://portal.azure.com` et, dans la barre de recherche supérieure, recherchez les ressources que vous avez créées dans ce labo.

2. Dans la page de ressources, sélectionnez **Supprimer** et suivez les instructions pour supprimer la ressource. Vous pouvez également supprimer l’intégralité du groupe de ressources pour nettoyer toutes les ressources en même temps.

## Plus d’informations

Pour plus d’informations sur la sécurisation d’Azure AI Services, consultez la [documentation de la sécurité d’Azure AI Services](https://docs.microsoft.com/azure/ai-services/security-features).
