# Gerenciando Acesso ao AKS

O Azure suporta 2 tipos de interação ao API server :

1. Acesso de Usuário
2. Acesso de Aplicação

Os acessos de usuário são gerenciados externamente via cliente SSO. Aplicações utilizam de service account. O processo de controle passa por fases especificas e ocorre da seguinte forma :

```
- Usuário ou aplicação >> ## API SERVER ## - usuário ou aplicação cria uma requisição >> Autenticação >> Autorização >> Admissão >>>>> # Worker Node # Kublet >> Container

```

1. Na primeira fase o cliente faz uma requisição de alteração no cluster já deployado
2. Na fase de 'Autenticação' o ApiServer entenden quem é ou qual aplicação deseja fazer alteração ( Via Token )
3. Na fase de autorização é checado se o 'usuário' ou 'app' tem permissão para executar a ação desejada.
4. Na fase de adimissão, o usuário com a devida permissão de execução terá sua mudança aplicada ao estado atual do kubernetes podendo criar novos deployments, services e etc.

>'Exaplanação' : O Kublet é o agente em cada um dos nodes que se comunica com o API server no control plane e envia o sinal para o runtime ( Containerd, CRI-O ou Docker em versões mais antigas )

## Kubeconfig

O centro de permissionamento de nosso clister esta no nosso arquivo Kubeconfig. Esse arquivo fica no home do usuário que tem permissão de execução em nosso cluster. O Kubeconfig é responsável por nos dar contextos de execução. Cada contexto representa um diferente cluster no qual poderá fazer alteração.

Imagine o seguinte cenário :

 Temos 2 clusters cl01 e cl02 devidamente configurado no Azure AKS e eu preciso executar deployments em ambos.
 Rodando *kubectl config get-clusters* vejo que tenho somente cl01 configurado devidamente na minha sessão de shell atual.

 Com o comando **az aks list --query "[?contains(name, 'cl')].{Name:name,State:properties.state}"** nós conseguimos a informação dos nossos clusters atuais no azure.


 ```
 [
  {
    "name": "cl01",
    "State": null
  },
  {
    "name": "cl02",
    "State": null
  },
 ]

 ```
Confirmado que o segundo cluster esta ativo, podemos adiciona-lo de forma direta ao nosso Kubeconfig através do comando :

```
az aks get-credentials --ressource-group SeuRgDoAKS --name cl02
Merged "cl02" as current context in /home/rosthan/.kube/config
```

Agora conseguimos listar nossos clusters e veremos 2 clusters ao inves de 1
e ao fim de nosso kubeconfig vemos que um novo contexto foi adicionado ( cat ~/.kube/config  para ver o contexto)
Para executar ações no segundo cluster precisamos alterar o contexto :
```
#Exibe contexto atual, no caso será o segundo cluster pois o contexto é açterado após a adição de um novo cluster
kubectl config current context
cl02

#Altera contexto para deployments no cl01
Kubectl config use-context cl01

#Exibe contextos existentes
Kubectl config get-context

```

Dessa forma, o kubeconfig esta liberado e qualquer pessoa pode ter acesso ao mesmo e fazer alterações no seu cluster Kubernetes. Para controle do cluster temos duas opções nativas do azure. São elas, azureAD e Azure Rbac ( Veremos mais a frente). 
