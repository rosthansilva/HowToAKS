# ACR ( Azure Container Registry)

O ACR ( **Azure Container Registry** ) é um repositório privado de imagens de container que armazena seus dados nos datacenters da Azure.

Ao rodar um deployment, por segurança é padrão que o repositório seja de sua propriedade ou de sua empresa.
com o ACR temos uma forma propria de lidar com as imagens e buildar seu codigo.

## Criando um Registry

Para criação de um registry digite

```
az acr create --ressource-group RgAcr --nome AcrDoMau --sku Basic --admin-enabled true
```
