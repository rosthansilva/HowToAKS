# Qual a diferença do Kubernetes Gerenciado pela Azure para o Kubernetes comum ?

Assim como muitos dos pre reqs de segurança dos clouds são divididos entre os clientes e os clouds prublicos,
com o kubernetes não é diferente. Parte da responsabilidade é Dividida entre você, dono do cluester e a Microsoft.

Segurança :
   - A Segurança do cluster é gerenciada pela Microsoft

Manutenção :
   - A Manutenção do cluster, updates e Self healings são feitos pela Microsoft.

Escalabilidade :
   - A auta disponibilidade ( HA ) e a escalabilidade do cluster é de responsabilidade da Microsoft.

Monitoração :
   - A saúde e garantia de continuidade do cluster é gerenciado pela microsoft.


## Easy as F*

Não há duvida alguma que a pior parte do K8s ( Kubernetes ) é mante-lo. Fazer backup do etcd e manter seus updates é muito difícil. Além de toda essa vantagem com a gestão da microsoft ela ainda nos da a possibilidade de ter um cluster onde eu não pago nem vejo o control plane ... Isso é maravilhoso e muito diferente se comparado a outras soluções como o próprio GKE do google. Com AKS não preciso mais me preocupar em colocar o etcd externo e nem fazer backup do mesmo. Eu só preciso ter o kubectl ( Comando que controla o kubernetes) configurado e utilizar o meu cluster pra rodar meus deploys.

## Deploy rapido de um clusters

Abra o azure cloudshell em sua conta Azure e digite as seguintes linhas :

```
crie um rbac para o cluster ( Rbac é o controle de privilégios de usuários sobre o cluster )

az ad sp create-for-rbac \
--skip-assignment

export RG=SeuRessourceGroup

export ClusterName=NomeDoCluster

export Nodes=1

export Addons=monitoring

export ServicePrincipal="SeuServicePrincipal"

export Secret="ClientSecret"

az aks create --ressource-group $RG --name $ClusterName --node-count $Nodes --enable-addons $addons --generate-ssh-keys --service-principal $ServicePrincipal --client-secret $ClientSecret

```
