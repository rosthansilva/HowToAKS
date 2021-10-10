#Gerenciando Acesso ao AKS

O Azure suporta 2 tipos de interação ao API server :

1. Acesso de Usuário
2. Acesso de Aplicação

Os acessos de usuário são gerenciados externamente via cliente SSO. Aplicações utilizam de service account. O processo de controle passa por fases especificas e ocorre da seguinte forma :

```
- usuário ou aplicação >> ## API SERVER ## - usuário ou aplicação cria uma requisição >> Autenticação >> Autorização >> Admissão >>>>> # Worker Node # Kublet >> Container

```

1. Na primeira fase o cliente faz uma requisição de alteração no cluster já deployado
2. Na fase de 'Autenticação' o ApiServer entendende quem é ou qual aplicação deseja fazer alteração ( Via Token )
3. Na fase de autorização é checado se o 'usuário' ou 'app' tem permissão para executar a ação desejada.
4. Na fase de adimissão, o usuário com a devida permissão de execução terá sua mudança aplicada ao estado atual do kubernetes podendo criar novos deployments, services e etc.
