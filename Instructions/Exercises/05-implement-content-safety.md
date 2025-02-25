---
lab:
  title: "Implémenter Azure\_AI Sécurité du Contenu"
---

# Implémenter Azure AI Sécurité du Contenu

Dans cet exercice, vous allez approvisionner une ressource Sécurité du Contenu, la tester dans Azure AI Studio et la tester dans le code.

## Approvisionner une ressource *Sécurité du Contenu*

Si vous n’en avez pas encore, vous devez configurer une ressource **Sécurité du Contenu** dans votre abonnement Azure.

1. Ouvrez le portail Azure à l’adresse `https://portal.azure.com` et connectez-vous avec le compte Microsoft associé à votre abonnement Azure.
1. Sélectionnez **Créer une ressource**.
1. Dans le champ de recherche, recherchez **Content Safety**. Ensuite, dans les résultats, sélectionnez **Créer** sous **Azure AI Sécurité du Contenu**.
1. Configurez la ressource avec les paramètres suivants :
    - **Abonnement** : *votre abonnement Azure*.
    - **Groupe de ressources** : *créez ou sélectionnez un groupe de ressources*.
    - **Région** : sélectionnez **USA Est**.
    - **Nom** : *entrez un nom unique.*
    - **Niveau tarifaire** : sélectionnez **F0** (*gratuit*) ou **S** (*standard*) si F0 n’est pas disponible.
1. Sélectionnez **Vérifier + créer**, puis **Créer** pour provisionner la ressource.
1. Attendez la fin du déploiement, puis accédez à la ressource.
1. Sélectionnez **Contrôle d’accès** dans le menu de navigation de gauche, puis **+ Ajouter** et **Ajouter une attribution de rôle**.
1. Faites défiler vers le bas pour choisir le rôle **Utilisateur Cognitive Services** et sélectionnez **Suivant**.
1. Ajoutez votre compte à ce rôle, puis sélectionnez **Vérifier + attribuer**.
1. Sélectionnez **Gestion des ressources** dans la barre de navigation de gauche, puis **Clés et points de terminaison**. Laissez cette page ouverte pour pouvoir copier les clés ultérieurement.

## Utiliser les boucliers d’invite d’Azure AI Sécurité du Contenu

Dans cet exercice, vous allez utiliser Azure AI Studio pour tester les boucliers d’invite de Sécurité du Contenu avec deux exemples d’entrées. L’un simule une invite utilisateur, et l’autre un document avec du texte potentiellement dangereux incorporé dans celui-ci.

1. Dans un autre onglet de navigateur, ouvrez la page Sécurité du Contenu d’[Azure AI Studio](https://ai.azure.com/explore/contentsafety) et connectez-vous.
1. Sous **Modérer le contenu d’un texte**, sélectionnez **Faire un essai**.
1. Dans la page **Modérer le contenu d’un texte**, sous **Azure AI Services**, sélectionnez la ressource Sécurité du Contenu que vous avez créée précédemment.
1. Sélectionnez **Plusieurs catégories de risques dans une phrase**. Passez en revue le texte du document pour identifier les problèmes potentiels.
1. Sélectionnez **Exécuter un test** et examinez les résultats.
1. Si vous le souhaitez, modifiez les niveaux de seuil et resélectionnez **Exécuter un test**.
1. Dans la barre de navigation de gauche, sélectionnez **Détection de matériel protégé pour le texte**.
1. Sélectionnez **Paroles protégées** et notez qu’il s’agit des paroles d’une chanson publiée.
1. Sélectionnez **Exécuter un test** et examinez les résultats.
1. Dans la barre de navigation de gauche, sélectionnez **Modérer le contenu d’une image**.
1. Sélectionnez **Contenu auto-nuisible**.
1. Notez que toutes les images sont floues par défaut dans AI Studio. Vous devez également être conscient que le contenu sexuel dans les exemples est très léger.
1. Sélectionnez **Exécuter un test** et examinez les résultats.
1. Dans la barre de navigation de gauche, sélectionnez **Boucliers d’invite**.
1. Dans la **page Boucliers d’invite**, sous **Azure AI Services**, sélectionnez la ressource Sécurité du Contenu que vous avez créée précédemment.
1. Sélectionnez **Contenu d’attaque d’invite et de document**. Examinez l’invite de l’utilisateur et le texte du document pour identifier les problèmes potentiels.
1. Sélectionnez **Exécuter le test**.
1. Dans **Afficher les résultats**, vérifiez que les attaques jailbreak ont été détectées à la fois dans l’invite de l’utilisateur et dans le document.

    > [!TIP]
    > Le code est disponible pour tous les exemples dans AI Studio.

1. Sous **Étapes suivantes**, sous **Afficher le code**, sélectionnez **Afficher le code**. La fenêtre **Exemple de code** s’affiche.
1. Utilisez la flèche vers le bas pour sélectionner Python ou C#, puis sélectionnez **Copier** pour copier l’exemple de code dans le presse-papiers.
1. Fermez l’écran **Exemple de code**.

### Configuration de votre application

Vous allez maintenant créer une application en C# ou Python.

#### C#

##### Prérequis

* [Visual Studio Code](https://code.visualstudio.com/) sur l’une des [plateformes prises en charge](https://code.visualstudio.com/docs/supporting/requirements#_platforms).
* [.NET 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) est le framework cible de cet exercice.
* [Extension C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) pour Visual Studio Code.

##### Configuration

Procédez comme suit pour préparer Visual Studio Code pour l’exercice.

1. Démarrez Visual Studio Code et, dans l’affichage Explorateur, cliquez sur **Créer un projet .NET** en sélectionnant **Application console**.
1. Sélectionnez un dossier sur votre ordinateur, puis donnez un nom au projet. Sélectionnez **Créer un projet** et accusez réception du message d’avertissement.
1. Dans le volet Explorateur, développez Explorateur de solutions et sélectionnez **Program.cs**.
1. Générez et exécutez le projet en sélectionnant **Exécuter** -> **Exécuter sans débogage**. 
1. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet C# et sélectionnez **Ajouter un package NuGet**.
1. Recherchez **Azure.AI.TextAnalytics** et sélectionnez la dernière version.
1. Recherchez un second package NuGet : **Microsoft.Extensions.Configuration.Json 8.0.0**. Le fichier projet doit maintenant répertorier deux packages NuGet.

##### Ajout de code

1. Collez l’exemple de code que vous avez copié précédemment sous la section **ItemGroup**.
1. Faites défiler vers le bas pour rechercher *Remplacer par votre propre clé d’abonnement et point de terminaison*.
1. Dans le portail Azure, dans la page Clés et point de terminaison, copiez l’une des clés (1 ou 2). Remplacez **<your_subscription_key>** par cette valeur.
1. Dans le portail Azure, dans la page Clés et point de terminaison, copiez le point de terminaison. Collez cette valeur dans votre code pour remplacer **<your_endpoint_value>**.
1. Dans **Azure AI Studio**, copiez la valeur de l’**invite utilisateur**. Collez-la dans votre code pour remplacer **<test_user_prompt>**.
1. Faites défiler jusqu’à **<this_is_a_document_source>** et supprimez cette ligne de code.
1. Dans **Azure AI Studio**, copiez la valeur du **document**.
1. Faites défiler jusqu’à **<this_is_another_document_source>** et collez la valeur de votre document.
1. Sélectionnez **Exécuter** -> **Exécuter sans débogage** et vérifiez qu’une attaque a été détectée. 

#### Python

##### Prérequis

* [Visual Studio Code](https://code.visualstudio.com/) sur l’une des [plateformes prises en charge](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* L’[extension Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) est installé pour Visual Studio Code.

* Le [module des demandes](https://pypi.org/project/requests/) est installé.

1. Créez un fichier Python avec une extension **.py** et donnez-lui un nom approprié.
1. Collez l’exemple de code que vous avez copié précédemment.
1. Faites défiler jusqu’à la section intitulée *Remplacer par votre propre clé d’abonnement et point de terminaison*.
1. Dans le portail Azure, dans la page Clés et point de terminaison, copiez l’une des clés (1 ou 2). Remplacez **<your_subscription_key>** par cette valeur.
1. Dans le portail Azure, dans la page Clés et point de terminaison, copiez le point de terminaison. Collez cette valeur dans votre code pour remplacer **<your_endpoint_value>**.
1. Dans **Azure AI Studio**, copiez la valeur de l’**invite utilisateur**. Collez-la dans votre code pour remplacer **<test_user_prompt>**.
1. Faites défiler jusqu’à **<this_is_a_document_source>** et supprimez cette ligne de code.
1. Dans **Azure AI Studio**, copiez la valeur du **document**.
1. Faites défiler jusqu’à **<this_is_another_document_source>** et collez la valeur de votre document.
1. À partir du terminal intégré de votre fichier, exécutez le programme, par exemple :

    - `.\prompt-shield.py`

1. Vérifiez qu’une attaque est détectée.
1. Si vous le souhaitez, vous pouvez tester différents contenus de test et valeurs de document.
