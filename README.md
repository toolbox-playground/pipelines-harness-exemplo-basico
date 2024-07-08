# Fluxo de Trabalho do Harness CI/CD

![Toolbox Playground](img/toolbox-playground.png)

## Pré-requisitos

1. Certifique-se de ter o Docker instalado em sua máquina. Você pode baixar e instalar o Docker a partir do site oficial: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/).

2. Certifique-se de ter o Git instalado em sua máquina. Você pode baixar e instalar o Git a partir do site oficial: [https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

3. Crie uma conta no Docker Hub, [https://hub.docker.com/signup/](https://hub.docker.com/signup/), e crie um repositório.


4. Faça um fork do repositório [https://github.com/toolbox-playground/pipelines-harness-exemplo-basico](https://github.com/toolbox-playground/pipelines-harness-exemplo-basico). Siga os passos abaixo para fazer um fork:

a. Acesse o repositório [https://github.com/toolbox-playground/pipelines-jenkins-exemplo-basico](https://github.com/toolbox-playground/pipelines-jenkins-exemplo-basico).

b. No canto superior direito da página, clique no botão "Fork".

c. Isso criará uma cópia do repositório em sua conta do GitHub.

5. Depois de ter feito o fork, você pode clonar o repositório para a sua máquina local com o seguinte comando:

```bash
git clone https://github.com/seu-usuario/pipelines-harness-exemplo-basico
```
Lembre-se de substituir `seu-usuario` pelo seu usuário do GitHub.

6. Acesse o diretório que você acabou de clonar: `pipelines-harness-exemplo-basico`.

Crie uma conta no Harness em [https://app.harness.io/auth/#/signup?utm_source=harness_io&utm_medium=cta&utm_campaign=platform&utm_content=main_nav](https://app.harness.io/auth/#/signup?utm_source=harness_io&utm_medium=cta&utm_campaign=platform&utm_content=main_nav)

Clique em **Continuos Integration**

Vá até o **Project Settings** e clique em **Connectors**

Iremos criar dois conectores, um para o GitHub, usando o [**PAT**](https://docs.github.com/pt/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) (personal access token), para acessarmos o repositório que queremos rodar a pipeline e para criar um webhook que irá informar o harness sobre qualquer alteração por PUSH ou PULL REQUEST.

O segundo conector será para o Docker Hub, para podermos enviar a imagem gerada pelo docker para lá.

Clique em **New Connector**

Digite GitHub em Search e selecione o GItHUb

Em name coloque **Toolbox GitHub**

Em **GitHub Account URL** coloque a URL da sua conta no GitHub, **https://github.com/seu-suario**, substituindo o `seu-usuario` por seu usuário no GitHub.

Em **Test Repository** coloque respositorio que fizemos o fork `pipelines-harness-exemplo-basico`.

Clique em continue.

Em **Authentication**, coloque o seu **username** do GitHUb 
Antes de cadastrar o **Personal Access Token**, vamos criar o PAT no GitHub usado o seguinte link: [https://github.com/settings/tokens/new?scopes=repo,user,admin:org_hook,admin:repo_hook](https://github.com/settings/tokens/new?scopes=repo,user,admin:org_hook,admin:repo_hook) 

Na página do github qe irá aprecer em note digite **Harness** e depois clique em **Generate token**, copie o token para usá-lo como secret

De volta a página do Harness, clique  em **Create or Select a Secret**, no modal que irá aparecer clique em **New Secret Text**. Em **Secret Name**  digite **GitHub PAT** e em **Secret Value** cole o token gerado no Github e clique em **Save**

Marque o **Enable API access (recommended)** e em **API Authentication**  clique  em **Create or Select a Secret** e selecione o **GitHub PAT** e depois clique em Continue. 

Em **Connect to the provider** selecione **Connect through Harness Platform** e clique em **Save and Continue**.

Aguarde a **Connection Test** terminar e aparecer **Verification successful** e clique em **Finish**.

Agora vamos criar um Conector para o Docker Hub.



Clique novamente em **New Conector**

Digite Docker em Search e selecione o Docker Registry

Em name coloque **Docker_Toolbox**

Em **Provider Type** selecione **DockerHub**

Em **Docker Registry URL** preencar com https://index.docker.io/v2/

Em **Authentication**, coloque o seu **username** do Docker Hub. 
Antes de cadastrar o **Password**, vamos criar o token no Docker Hub GitHub usado o seguinte link: [https://github.com/settings/tokens/new?scopes=repo,user,admin:org_hook,admin:repo_hook](https://github.com/settings/tokens/new?scopes=repo,user,admin:org_hook,admin:repo_hook) 

Na página do github qe irá aprecer em note digite **Harness** e depois clique em **Generate token**, copie o token para usá-lo como secret

De volta a página do Harness, clique  em **Create or Select a Secret**, no modal que irá aparecer clique em **New Secret Text**. Em **Secret Name**  digite **GitHub PAT** e em **Secret Value** cole o token gerado no Github e clique em **Save**

Marque o **Enable API access (recommended)** e em **API Authentication**  clique  em **Create or Select a Secret** e selecione o **GitHub PAT** e depois clique em Continue. 

Em **Connect to the provider** selecione **Connect through Harness Platform** e clique em **Save and Continue**.

Aguarde a **Connection Test** terminar e aparecer **Verification successful** e clique em **Finish**.


Será necessário cadastrar um cartão de crédito para rodar as pipeline na Harness Cloud, caso não tenha, você irá rodar localmente usando o delegates. Este exemplo fará uso da Harness Cloud.



Habilite o `Enable Bi-Directional Sync` em `Account Settings`

Vá em `pipelines`

