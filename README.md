# MarkItDown MCP Server on Azure Functions

A [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server running on Azure Flex Consumption Plan that provides document conversion capabilities using Microsoft's MarkItDown library.

## Qu'est-ce que MarkItDown ?

[MarkItDown](https://github.com/microsoft/markitdown) est une bibliothèque Python développée par Microsoft qui convertit divers formats de fichiers en Markdown. Elle permet de transformer des documents Office, PDF, images, pages web et bien d'autres formats en texte Markdown facilement exploitable.

### Origines

MarkItDown a été créé par l'équipe Microsoft pour faciliter l'intégration de documents de différents formats dans des workflows d'IA et de traitement de texte.  La bibliothèque est open-source et maintenue activement sur GitHub.

### Formats supportés

MarkItDown peut convertir les formats suivants :
- **Documents Office** : Word (. docx), PowerPoint (.pptx), Excel (.xlsx)
- **Documents PDF** (. pdf)
- **Images** (.jpg, .png, etc.) avec extraction de texte via OCR
- **Pages web** (HTML)
- **Fichiers texte** et bien d'autres formats

## Architecture du projet

Ce projet combine : 
- **MarkItDown** : pour la conversion de documents
- **MCP (Model Context Protocol)** : pour exposer les capacités via un protocole standardisé
- **Azure Functions (Flex Consumption Plan)** : pour l'hébergement serverless scalable et économique

## Installation

### Prérequis

- Python 3.10 ou supérieur
- Un compte Azure (pour le déploiement)
- Azure Functions Core Tools
- Azure CLI

### Installation locale

1. **Cloner le repository**
   ```bash
   git clone https://github.com/zecloud/markitdown-mcp-azurefunctions.git
   cd markitdown-mcp-azurefunctions
   ```

2. **Créer un environnement virtuel**
   ```bash
   python -m venv . venv
   source .venv/bin/activate  # Sur Windows: .venv\Scripts\activate
   ```

3. **Installer les dépendances**
   ```bash
   pip install -r requirements.txt
   ```

4. **Installer MarkItDown**
   ```bash
   pip install markitdown
   ```

5. **Configurer les variables d'environnement**
   ```bash
   cp local. settings.json.example local.settings.json
   # Éditer local.settings.json avec vos configurations
   ```

6. **Lancer en local**
   ```bash
   func start
   ```

### Déploiement sur Azure

1. **Se connecter à Azure**
   ```bash
   az login
   ```

2. **Créer les ressources Azure**
   ```bash
   # Créer un groupe de ressources
   az group create --name markitdown-mcp-rg --location francecentral

   # Créer un compte de stockage
   az storage account create --name markitdownstorage --resource-group markitdown-mcp-rg --location francecentral

   # Créer l'application Function (Flex Consumption)
   az functionapp create --name markitdown-mcp-function \
     --resource-group markitdown-mcp-rg \
     --storage-account markitdownstorage \
     --runtime python \
     --runtime-version 3.10 \
     --functions-version 4 \
     --consumption-plan-location francecentral
   ```

3. **Déployer le code**
   ```bash
   func azure functionapp publish markitdown-mcp-function
   ```

## Utilisation

Une fois déployé, le serveur MCP expose les capacités de conversion de MarkItDown via le protocole MCP. Les clients MCP peuvent alors utiliser ces fonctionnalités pour convertir des documents à la volée.

### Exemple d'utilisation avec un client MCP

```python
# Exemple de conversion d'un fichier PDF en Markdown
result = mcp_client.convert_document(
    file_path="document.pdf",
    output_format="markdown"
)
print(result)
```

## Avantages du Flex Consumption Plan

- **Coût optimisé** : Vous ne payez que pour l'exécution réelle
- **Scalabilité automatique** : S'adapte automatiquement à la charge
- **Démarrage rapide** : Temps de démarrage à froid réduit
- **Intégration Azure** : Accès facile aux autres services Azure

## Licence

Ce projet est sous licence MIT.  MarkItDown est également sous licence MIT.

## Ressources

- [Documentation MarkItDown](https://github.com/microsoft/markitdown)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Azure Functions Documentation](https://docs.microsoft.com/azure/azure-functions/)
