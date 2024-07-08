# Fluxo de Trabalho do Harness CI/CD

![Toolbox Playground](img/toolbox-playground.png)

Crie uma conta no Harness em [https://app.harness.io/auth/#/signup?utm_source=harness_io&utm_medium=cta&utm_campaign=platform&utm_content=main_nav](https://app.harness.io/auth/#/signup?utm_source=harness_io&utm_medium=cta&utm_campaign=platform&utm_content=main_nav)

Clique em **Continuos Integration**

Vá até o **Project Settings** e clique em **Connectors**

Iremos criar dois conectores, um para o GitHub, usando o [**PAT**](https://docs.github.com/pt/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) (personal access token), para acessarmos o repositório que queremos rodar a pipeline e para criar um webhook que irá informar o harness sobre qualquer alteração por PUSH ou PULL REQUEST.

O segundo conector será para o Docker Hub, para podermos enviar a imagem gerada pelo docker para lá.

Clique em **New Connector**
Será neessário cadastrar um cartão de crédito para rodar as pipeline na Harness Cloud, caso não tenha, você irá rodar localmente usando o delegates. Este exemplo fará uso da Harness Cloud.



Habilite o `Enable Bi-Directional Sync` em `Account Settings`

Vá em `pipelines`

