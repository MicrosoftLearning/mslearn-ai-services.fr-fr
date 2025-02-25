---
lab:
  title: Surveiller Azure AI Services
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Surveiller Azure AI Services

Azure AI Services peut s’avérer être un élément essentiel d’une infrastructure d’application globale. Il est important de pouvoir surveiller l’activité et d’obtenir des alertes concernant les problèmes qui nécessitent votre attention.

## Cloner le référentiel dans Visual Studio Code

Vous allez développer votre code en utilisant Visual Studio Code. Les fichiers de code de votre application ont été fournis dans un référentiel GitHub.

> **Conseil** : Si vous avez déjà cloné le référentiel **mslearn-ai-services**, ouvrez-le dans Visual Studio Code. Dans le cas contraire, procédez comme suit pour le cloner dans votre environnement de développement.

1. Démarrez Visual Studio Code.
2. Ouvrez la palette (Maj+CTRL+P) et exécutez une commande **Git : Cloner** pour cloner le référentiel `https://github.com/MicrosoftLearning/mslearn-ai-services` vers un dossier local (peu importe quel dossier).
3. Lorsque le référentiel a été cloné, ouvrez le dossier dans Visual Studio Code.
4. Si nécessaire, attendez que les fichiers supplémentaires soient installés pour prendre en charge les projets de code C# dans le référentiel

    > **Remarque** : si vous êtes invité à ajouter des ressources requises pour générer et déboguer, sélectionnez **Not Now** (Pas maintenant).

5. Développez le dossier `Labfiles/03-monitor-ai-services`.

## Provisionner une ressource Azure AI Services

Si vous n’en avez pas encore dans votre abonnement, vous devez configurer une ressource **Azure AI Services**.

1. Ouvrez le portail Azure à l’adresse `https://portal.azure.com` et connectez-vous avec le compte Microsoft associé à votre abonnement Azure.
2. Dans la barre de recherche supérieure, recherchez *Azure AI Services*, sélectionnez **Compte multiservice Azure AI Services** et créez une ressource avec les paramètres suivants :
    - **Abonnement** : *votre abonnement Azure*
    - **Groupe de ressources** : *Choisissez ou créez un groupe de ressources. (Si vous utilisez un abonnement restreint, vous n’avez peut-être pas l’autorisation de créer un groupe de ressources. Dans ce cas, utilisez le groupe fourni.)*
    - **Région** : *choisissez n’importe quelle région disponible*
    - **Nom** : *Entrez un nom unique.*
    - **Niveau tarifaire** : Standard S0
3. Cochez les cases nécessaires et créez la ressource.
4. Attendez la fin du déploiement, puis visualisez les détails du déploiement.
5. Une fois la ressource déployée, accédez-y et affichez sa page **Clés et point de terminaison**. Notez l’URI de point de terminaison. Vous en aurez besoin ultérieurement.

## Configurer une alerte

Commençons la surveillance en définissant une règle d’alerte afin de détecter l’activité dans votre ressource Azure AI Services.

1. Dans le portail Azure, accédez à votre ressource Azure AI Services et affichez sa page **Alertes** (dans la section **Surveillance**).
2. Sélectionnez la liste déroulante **+ Créer**, puis sélectionnez **Règle d’alerte**.
3. Dans la page **Créer une règle d’alerte**, sous **Étendue**, vérifiez que la ressource Azure AI Services est répertoriée. (Fermez le volet **Sélectionner un signal** s’il est ouvert.)
4. Sélectionnez l’onglet **Condition**, puis sur le lien **Afficher tous les signaux** afin de faire apparaître le volet **Sélectionner un signal** qui s’affiche à droite, où vous pouvez sélectionner un type de signal à surveiller.
5. Dans la liste de **Type de signal**, faites défiler vers le bas jusqu’à la section **Journal d’activité**, puis sélectionnez **Clés de liste (compte d’API Cognitive Services)**. Sélectionnez ensuite **Appliquer**.
6. Passez en revue l’activité des 6 dernières heures.
7. Sélectionnez l’onglet **Actions**. Notez que vous pouvez spécifier un *groupe d’actions*. Cela vous permet de configurer des actions automatisées lorsqu’une alerte est déclenchée, par exemple en envoyant une notification par e-mail. Nous ne le ferons pas dans cet exercice; mais il peut être utile de le faire dans un environnement de production.
8. Sous l’onglet **Détails**, définissez le **Nom de la règle d’alerte** sur **Alerte de liste de clés**.
9. Sélectionnez **Revoir + créer**. 
10. Passez en revue la configuration de l’alerte. Sélectionnez **Créer**, puis attendez la fin de la création de la règle d’alerte.
11. Vous pouvez maintenant utiliser la commande suivante pour obtenir la liste des clés Azure AI Services, en remplaçant *&lt;resourceName&gt;* par le nom de votre ressource Azure AI Services et *&lt;resourceGroup&gt;* par le nom du groupe de ressources dans lequel vous l’avez créée.

    ```
    az cognitiveservices account keys list --name <resourceName> --resource-group <resourceGroup>
    ```

    La commande retourne une liste des clés de votre ressource Azure AI Services.

    > **Remarque** Si vous ne vous êtes pas connecté à Azure CLI, vous devrez peut-être exécuter `az login` avant que la commande de clés de liste fonctionne.

12. Revenez au navigateur contenant le Portail Azure et actualisez votre **page Alertes**. Vous devez voir une alerte **Sev 4** répertoriée dans la table (si ce n’est pas le cas, attendez cinq minutes et actualisez à nouveau).
13. Sélectionnez l’alerte pour afficher les détails la concernant.

## Visualiser une métrique

En plus de définir des alertes, vous pouvez afficher les métriques de votre ressource Azure AI Services pour surveiller son utilisation.

1. Dans le portail Azure, sur la page de votre ressource Azure AI Services, sélectionnez **Métriques** (dans la section **Surveillance**).
2. S’il n’existe aucun graphique existant, sélectionnez **+ Nouveau graphique**. Ensuite, dans la liste **Métriques**, passez en revue les métriques possibles que vous pouvez visualiser et sélectionnez **Nombre total d’appels**.
3. Dans la liste **Agrégation**, sélectionnez **Nombre**.  Cela vous permettra de superviser le nombre total d’appels à votre ressource Azure AI Services, ce qui s’avère utile pour déterminer l’utilisation du service sur une période donnée.
4. Pour générer certaines demandes à votre service Azure AI Services, vous allez utiliser **curl**, un outil de ligne de commande pour les requêtes HTTP. Dans votre éditeur, ouvrez **rest-test.cmd** et modifiez la commande **curl** qu’il contient (illustrée ci-dessous), en remplaçant *&lt;yourEndpoint&gt;* et *&lt;yourKey&gt;* par votre URI de point de terminaison et votre clé **Key1** pour utiliser l’API Analyse de texte dans votre ressource Azure AI services.

    ```
    curl -X POST "<your-endpoint>/language/:analyze-text?api-version=2023-04-01" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <your-key>" --data-ascii "{'analysisInput':{'documents':[{'id':1,'text':'hello'}]}, 'kind': 'LanguageDetection'}"
    ```

5. Enregistrez vos modifications, puis exécutez la commande suivante :

    ```
    ./rest-test.cmd
    ```

    La commande retourne un document JSON contenant des informations sur la langue détectée dans les données d’entrée (qui doit être l’anglais).

6. Réexécutez la commande **rest-test** plusieurs fois pour générer une activité d’appel (vous pouvez utiliser la clé **^** pour parcourir les commandes précédentes).
7. Revenez à la page **Métriques** de le Portail Azure et actualisez le graphique **Nombre total d’appels**. Plusieurs minutes peuvent être nécessaires pour que les appels que vous avez effectués à l’aide de *curl* soient répercutés dans le graphique. Actualisez le graphique jusqu’à ce qu’il soit mis à jour pour les inclure.

## Nettoyer les ressources

Si vous n’utilisez pas les ressources Azure créées dans ce labo pour d’autres modules de formation, vous pouvez les supprimer pour éviter d’autres frais.

1. Ouvrez le Portail Azure à l'adresse `https://portal.azure.com` et, dans la barre de recherche supérieure, recherchez les ressources que vous avez créées dans ce labo.

2. Dans la page de ressources, sélectionnez **Supprimer** et suivez les instructions pour supprimer la ressource. Vous pouvez également supprimer l’intégralité du groupe de ressources pour nettoyer toutes les ressources en même temps.

## Plus d’informations

L’une des options de surveillance d’Azure AI Services consiste à utiliser la *journalisation des diagnostics*. Une fois activée, la journalisation des diagnostics capture des informations enrichies sur l’utilisation de votre ressource Azure AI Services et peut constituer un outil de supervision et de débogage utile. La génération d’informations peut prendre plus d’une heure après la configuration de la journalisation des diagnostics. C’est pourquoi nous n’avons pas exploré cet aspect dans cet exercice ; mais vous pouvez en savoir plus dans la [documentation d’Azure AI Services](https://docs.microsoft.com/azure/ai-services/diagnostic-logging).
