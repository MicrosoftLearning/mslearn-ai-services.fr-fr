---
lab:
  title: "Commencer à utiliser Azure\_AI\_Services"
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Commencer à utiliser Azure AI Services

Dans cet exercice, vous allez commencer à utiliser Azure AI Services en créant une ressource **Azure AI Services** dans votre abonnement Azure et en l’utilisant à partir d’une application cliente. L’objectif de l’exercice n’est pas d’acquérir de l’expertise dans un service particulier, mais plutôt de se familiariser avec un modèle général pour le provisionnement et l’utilisation d’Azure AI Services en tant que développeur.

## Cloner le référentiel dans Visual Studio Code

Vous allez développer votre code en utilisant Visual Studio Code. Les fichiers de code de votre application ont été fournis dans un référentiel GitHub.

> **Conseil** : Si vous avez déjà cloné le référentiel **mslearn-ai-services**, ouvrez-le dans Visual Studio Code. Dans le cas contraire, procédez comme suit pour le cloner dans votre environnement de développement.

1. Démarrez Visual Studio Code.
2. Ouvrez la palette (Maj+CTRL+P) et exécutez une commande **Git : Cloner** pour cloner le référentiel `https://github.com/MicrosoftLearning/mslearn-ai-services` vers un dossier local (peu importe quel dossier).
3. Lorsque le référentiel a été cloné, ouvrez le dossier dans Visual Studio Code.
4. Si nécessaire, attendez que les fichiers supplémentaires soient installés pour prendre en charge les projets de code C# dans le référentiel

    > **Remarque** : si vous êtes invité à ajouter des ressources requises pour générer et déboguer, sélectionnez **Not Now** (Pas maintenant).

5. Développez le dossier `Labfiles/01-use-azure-ai-services`.

Le code pour C# et pour Python a été fourni. Développez le dossier de votre langage préféré.

## Provisionner une ressource Azure AI Services

Azure AI Services est un ensemble de services cloud qui encapsulent des fonctionnalités d’intelligence artificielle que vous pouvez incorporer dans vos applications. Vous pouvez provisionner des ressources Azure AI Services individuelles pour des API spécifiques (par exemple, **Langue** ou **Vision**), ou vous pouvez provisionner une seule ressource **Azure AI Services** qui fournit l’accès à plusieurs API Azure AI Services via un point de terminaison et une clé uniques. Dans ce cas, vous allez utiliser une seule ressource **Azure AI Services**.

1. Ouvrez le portail Azure à l’adresse `https://portal.azure.com` et connectez-vous avec le compte Microsoft associé à votre abonnement Azure.
2. Dans la barre de recherche supérieure, recherchez *Azure AI Services*, sélectionnez **Compte multiservice Azure AI Services** et créez une ressource avec les paramètres suivants :
    - **Abonnement** : *votre abonnement Azure*
    - **Groupe de ressources** : *Choisissez ou créez un groupe de ressources. (Si vous utilisez un abonnement restreint, vous n’avez peut-être pas l’autorisation de créer un groupe de ressources. Dans ce cas, utilisez le groupe fourni.)*
    - **Région** : *choisissez n’importe quelle région disponible*
    - **Nom** : *Entrez un nom unique.*
    - **Niveau tarifaire** : Standard S0
3. Cochez les cases nécessaires et créez la ressource.
4. Attendez la fin du déploiement, puis visualisez les détails du déploiement.
5. Accédez à la ressource et affichez sa page **Clés et point de terminaison**. Cette page contient les informations dont vous aurez besoin pour vous connecter à votre ressource et l’utiliser à partir d’applications que vous développez. Plus précisément :
    - *Point de terminaison* HTTP auquel les applications clientes peuvent envoyer des requêtes.
    - Deux *clés* qui peuvent être utilisées pour l’authentification (les applications clientes peuvent utiliser une clé pour s’authentifier).
    - *Emplacement* dans lequel la ressource est hébergée. Cela est nécessaire pour les demandes adressées à certaines API (mais pas toutes).

## Utiliser une interface REST

Les API Azure AI Services sont basées sur REST. Vous pouvez donc les utiliser en envoyant des requêtes JSON via HTTP. Dans cet exemple, vous allez explorer une application console qui utilise l’API REST **Langue** pour effectuer la détection de la langue, mais le principe de base est le même pour toutes les API prises en charge par la ressource Azure AI Services.

> **Remarque** : Dans cet exercice, vous pouvez choisir d’utiliser l’API REST à partir de **C#** ou **Python**. Dans les étapes qui suivent, effectuez les actions appropriées pour votre langage préféré.

1. Dans Visual Studio Code, développez le dossier **C-Sharp** ou **Python** en fonction de votre préférence de langage.
2. Affichez le contenu du dossier **rest-client**, et notez qu’il contient un fichier pour les paramètres de configuration :

    - **C#**  : appsettings.json
    - **Python** : .env

    Ouvrez le fichier de configuration et mettez à jour les valeurs de configuration qu’il contient pour refléter le **point de terminaison** et une **clé** d’authentification pour votre ressource Azure AI services. Enregistrez vos modifications.

3. Notez que le dossier **rest-client** contient un fichier de code pour l’application cliente :

    - **C#** : Program.cs
    - **Python** : rest-client.py

    Ouvrez le fichier de code et passez en revue le code qu’il contient, en notant les détails suivants :
    - Différents espaces de noms sont importés pour activer la communication HTTP
    - Le code de la fonction **Main** récupère le point de terminaison et la clé de votre ressource Azure AI Service. Ceux-ci seront utilisés pour envoyer des requêtes REST au service Analyse de texte.
    - Le programme accepte l’entrée utilisateur et utilise la fonction **GetLanguage** pour appeler l’API REST de détection de langue Analyse de texte pour votre point de terminaison Azure AI Services afin de détecter la langue du texte entré.
    - La requête envoyée à l’API se compose d’un objet JSON contenant les données d’entrée, dans ce cas, une collection d’objets de **document**, chacune ayant un **ID** et du **texte**.
    - La clé de votre service est incluse dans l’en-tête de demande pour authentifier votre application cliente.
    - La réponse du service est un objet JSON, que l’application cliente peut analyser.

4. Cliquez avec le bouton droit sur le dossier **rest-client**, sélectionnez *Ouvrir dans un terminal intégré*, puis exécutez la commande suivante :

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    pip install python-dotenv
    python rest-client.py
    ```

5. Lorsque vous y êtes invité, entrez du texte et passez en revue la langue détectée par le service, qui est retournée dans la réponse JSON. Par exemple, essayez d’entrer « Hello », « Bonjour » et « Hola ».
6. Lorsque vous avez terminé de tester l’application, entrez « quit » pour arrêter le programme.

## Utiliser un Kit de développement logiciel (SDK)

Vous pouvez écrire du code qui consomme directement les API REST d’Azure AI services, mais il existe des Kits de développement logiciel (SDK) pour de nombreux langages de programmation populaires, notamment Microsoft C#, Python, Java et Node.js. L’utilisation d’un kit de développement logiciel (SDK) peut simplifier considérablement le développement d’applications qui consomment Azure AI Services.

1. Dans Visual Studio Code, développez le dossier **sdk-client** sous le dossier **C-Sharp** ou **Python** en fonction de votre préférence de langage. Ensuite, exécutez `cd ../sdk-client` pour effectuer la modification dans le dossier sdk-client** approprié**.

2. Installez ensuite le package du kit de développement logiciel (SDK) d’analyse de texte en exécutant la commande appropriée pour votre préférence de langage :

    **C#**

    ```
    dotnet add package Azure.AI.TextAnalytics --version 5.3.0
    ```

    **Python**

    ```
    pip install azure-ai-textanalytics==5.3.0
    ```

3. Affichez le contenu du dossier **sdk-client**, et notez qu’il contient un fichier pour les paramètres de configuration :

    - **C#**  : appsettings.json
    - **Python** : .env

    Ouvrez le fichier de configuration et mettez à jour les valeurs de configuration qu’il contient pour refléter le **point de terminaison** et une **clé** d’authentification pour votre ressource Azure AI services. Enregistrez vos modifications.
    
4. Notez que le dossier **sdk-client** contient un fichier de code pour l’application cliente :

    - **C#** : Program.cs
    - **Python** : sdk-client.py

    Ouvrez le fichier de code et passez en revue le code qu’il contient, en notant les détails suivants :
    - L’espace de noms du kit de développement logiciel (SDK) que vous avez installé est importé
    - Le code de la fonction **Main** récupère le point de terminaison et la clé de votre ressource Azure AI Services. Ceux-ci seront utilisés avec le kit de développement logiciel (SDK) pour créer un client pour le service Analyse de texte.
    - La fonction **GetLanguage** utilise le kit de développement logiciel (SDK) pour créer un client pour le service, puis utilise le client pour détecter la langue du texte entré.

5. Revenez au terminal, vérifiez que vous vous trouvez dans le dossier **sdk-client**, puis entrez la commande suivante pour exécuter le programme :

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python sdk-client.py
    ```

6. Lorsque vous y êtes invité, entrez du texte et passez en revue la langue détectée par le service. Par exemple, essayez d’entrer « Goodbye », « Au revoir » et « Hasta la vista ».
7. Lorsque vous avez terminé de tester l’application, entrez « quit » pour arrêter le programme.

> **Remarque** : Certaines langues nécessitant des jeux de caractères Unicode peuvent ne pas être reconnues dans cette application console simple.

## Nettoyer les ressources

Si vous n’utilisez pas les ressources Azure créées dans ce labo pour d’autres modules de formation, vous pouvez les supprimer pour éviter d’autres frais.

1. Ouvrez le Portail Azure à l'adresse `https://portal.azure.com` et, dans la barre de recherche supérieure, recherchez les ressources que vous avez créées dans ce labo.

2. Dans la page de ressources, sélectionnez **Supprimer** et suivez les instructions pour supprimer la ressource. Vous pouvez également supprimer l’intégralité du groupe de ressources pour nettoyer toutes les ressources en même temps.

## Plus d’informations

Pour plus d’informations sur Azure AI Services, consultez la [Documentation sur Azure AI Services](https://docs.microsoft.com/azure/ai-services/what-are-ai-services).
