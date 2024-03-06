---
lab:
  title: Configuration d'un environnement de labo
  module: Setup
---

# Configuration d'un environnement de labo

Les exercices sont destinés à être réalisés dans un environnement de laboratoire hébergé. Si vous souhaitez les terminer sur votre propre ordinateur, vous pouvez le faire en installant le logiciel suivant. Vous pouvez rencontrer des dialogues et un comportement inattendus lors de l’utilisation de votre propre environnement. En raison de la large gamme de configurations locales possibles, l’équipe de formation ne peut pas prendre en charge les problèmes que vous pouvez rencontrer dans votre propre environnement.

> **Remarque** : Les instructions ci-dessous s’appliquent à un ordinateur équipé de Windows 11. Vous pouvez également utiliserLinux ou MacOS. Vous devrez peut-être adapter les instructions du labo pour votre système d’exploitation choisi.

### Système d’exploitation de base (Windows 11)

#### Windows 10

Installez Windows 11 et effectuez toutes les mises à jour.

#### Edge

Installez [Edge (Chromium)](https://microsoft.com/edge)

### SDK .NET Core

1. Procédez au téléchargement et à l’installation depuis https://dotnet.microsoft.com/download (téléchargez le kit SDK .NET Core, pas seulement le runtime). Si vous exécutez les labos de ce cours sur votre propre ordinateur, vous aurez besoin d’installer .NET 7.0.

### C++ Redistributable

1. Téléchargez et installez le Visual C++ Redistributable (x64) à partir de https://aka.ms/vs/16/release/vc_redist.x64.exe.

### Node.JS

1. Téléchargez la dernière version de LTS à partir de https://nodejs.org/en/download/ 
2. Installez la version à l’aide des options par défaut

### Python (et packages requis)

1. Téléchargez la version 3.11 à partir de https://docs.conda.io/en/latest/miniconda.html 
2. Exécutez le programme d'installation **Important**: Sélectionnez les options pour ajouter Miniconda à la variable PATH et pour l'enregistrer comme environnement Python par défaut.
3. Après l’installation, ouvrez l’invite Anaconda et entrez les commandes suivantes pour Installez les packages : 

```
pip install flask requests python-dotenv pylint matplotlib pillow
pip install --upgrade numpy
```

### Azure CLI

1. Téléchargez depuis https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest 
2. Installez la version à l’aide des options par défaut

### Git

1. Télécharger et Installez à partir de https://git-scm.com/download.html, à l’aide des options par défaut


### Visual Studio Code (et extensions)

1. Téléchargez depuis https://code.visualstudio.com/Download 
2. Installez la version à l’aide des options par défaut 
3. Après l'installation, démarrez Visual Studio Code et sous l'onglet **Extensions** (CTRL+Maj+X), recherchez et installez les extensions suivantes depuis Microsoft :
    - Python
    - C#
    - Azure Functions
    - PowerShell
