# Service Principal e Grencia de recursos

  Service Principal é um gestor de identidades do Azure que permite a aplicações como o o k8s a criar recursos no Azure. Dentro do contexto do azure, o service principal permite ao kubernetes ter mais de um cluster gerenciado, multiplicar seu numero de nodes e ter acesso restgrito ou irrestrito ao ACR.

  1 - Para criar um service principal vá em no menu e pesquise por Azure Active Directory ( AzureAD ).
  2 - No menu a esquerda clique em **APP Registration**
  3 - Clique em New Rergistration.
  4 - Digite um nome e selecione quem terá acesso a essa API ou aplicação
  5 - Finalize e é isso :)

Agra com nosso service pincipal configurado podemos criar novos secrets para nossa aplicação no K8s.
Em certificates and secrets podemos fazer agora upload de sertificados e criar secrets.


## Criando Service Principal via AzCli

Faça login no portal azure ou em seu terminal de preferencia já com o azure cli disponivel. De preferencia para o azure cloud shell devido a praticidade.

Quando criamos um cluster diretamente no azure cloud shell o azure cria um gerenciador de identidade proprio para o kubernetes, para que possamos utilizar o azure service principal precisamos cria-lo manualmente via az cli.

Para cria-lo rode o seguinte comando :

```
az ad sp create-for-rbac --skip-assignment --name ClusterDoMau

```

Uma saida no formato Json aparecerá na sua Tela da seguinte forma :

> {
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "ClusterDoMau",
  "name": "http://ClusterDoMau",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}

Para usar um Service Pincipal existente quando você criar um cluster AKS usando o cli, use os parâmetros  para especificar a saída do comando az ad sp create-for-rbac:
--service-principal, --client-secret, appIdpassword

```
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>

```
É de suma importância ressaltar que um service principal é um serviço do azure que não tem ligação direta com o azure. Logo, quando deletamos nosso cluster ou recriamos, nada ocorre com o service principal. A função do SP é dar autonomia ao cluester, não o inverso.
