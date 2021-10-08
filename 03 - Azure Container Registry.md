# ACR ( Azure Container Registry)

O ACR ( **Azure Container Registry** ) é um repositório privado de imagens de container que armazena seus dados nos datacenters da Azure.

Ao rodar um deployment, por segurança é padrão que o repositório seja de sua propriedade ou de sua empresa.
com o ACR temos uma forma propria de lidar com as imagens e buildar seu codigo.

## Criando um Registry

Para criação de um registry digite :

```
#criando um ACR com SKU Basico, para mais informações sobre os niveis de SKU consulte o link : [Acr Niveis de Serviço](https://docs.microsoft.com/pt-br/azure/container-registry/container-registry-skus)
az acr create --ressource-group RgAcr --nome AcrDoMau --sku Basic --admin-enabled true
```

buildando uma imagem :


```
az acr build --registry AcrDoMau --image img:latest .

```

Criando seu primeiro deployment :

```
kubectl run app --image=AcrDoMau.azureacr.io/img:latest --replicas=3 --port=80
```
