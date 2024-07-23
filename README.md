# Fluxo de Trabalho do Harness CI/CD

![Toolbox Playground](img/toolbox-playground.png)

## Pré-requisitos

1. Certifique-se de ter o Docker instalado em sua máquina. Você pode baixar e instalar o Docker a partir do site oficial: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/).

2. Certifique-se de ter o Git instalado em sua máquina. Você pode baixar e instalar o Git a partir do site oficial: [https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

3. Crie uma conta no Docker Hub, [https://hub.docker.com/signup/](https://hub.docker.com/signup/), e crie um repositório chamado **pipelines-harness-exemplo-basico**.

4. Faça um fork do repositório [https://github.com/toolbox-playground/pipelines-harness-exemplo-basico](https://github.com/toolbox-playground/pipelines-harness-exemplo-basico). Siga os passos abaixo para fazer um fork:

a. Acesse o repositório [https://github.com/toolbox-playground/pipelines-harness-exemplo-basico](https://github.com/toolbox-playground/pipelines-harness-exemplo-basico).

b. No canto superior direito da página, clique no botão "Fork".

c. Na janela que ira aparecer, acrescente **-fork** no final de **pipelines-harness-exemplo-basico**, ficando o repo com o seguinte nome **pipelines-harness-exemplo-basico-fork**, muito importante colocar esse nome pois o arquivo da pipeline, [toolboxplaygroundharness.yaml](./.harness/pipelines/toolboxplaygroundharness.yaml), esta setado para esse nome. Verifique no arquivo o trecho `repoName: pipelines-harness-exemplo-basico-fork`.

c. Isso criará uma cópia do repositório em sua conta do GitHub.

5. Depois de ter feito o fork, você pode clonar o repositório para a sua máquina local com o seguinte comando:

```bash
git clone https://github.com/seu-usuario/pipelines-harness-exemplo-basico-fork
```
Lembre-se de substituir `seu-usuario` pelo seu usuário do GitHub.

6. Acesse o diretório que você acabou de clonar: `pipelines-harness-exemplo-basico-fork`.

7. Acesse o arquivo [toolboxplaygroundharness.yaml](./.harness/pipelines/toolboxplaygroundharness.yaml) e altere o trecho `repo: toolboxplayground/pipelines-harness-exemplo-basico` substituindo **toolboxplayground** pelo seu namespace do DockerHub, que deve ser o seu usuário.

Faca o commit e o Push

Crie uma conta no Harness em [https://app.harness.io/auth/#/signup?utm_source=harness_io&utm_medium=cta&utm_campaign=platform&utm_content=main_nav](https://app.harness.io/auth/#/signup?utm_source=harness_io&utm_medium=cta&utm_campaign=platform&utm_content=main_nav)

Clique em **Continuos Integration**

No canto superior esquerdo existe uma caixa de seleção, clique nela e escolha **Accounts** e clique na sua conta, depois vá em **Account Settings**, depois clique em **Default Settings** e em **Git Experience**, marque **Enable Bi-Directional Sync**.

Clique em **Projects** e depois clique em **Default Project**.

Vá até o **Project Settings** e clique em **Connectors**

Iremos criar dois conectores, um para o GitHub, usando o [**PAT**](https://docs.github.com/pt/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) (personal access token), para acessarmos o repositório que queremos rodar a pipeline e para criar um webhook que irá informar o harness sobre qualquer alteração por PUSH ou PULL REQUEST.

O segundo conector será para o Docker Hub, para podermos enviar a imagem gerada pelo docker para lá.

Clique em **New Connector**

Digite GitHub em Search e selecione o GItHUb

Em name coloque `Toolbox_GitHub`

Em **GitHub Account URL** coloque a URL da sua conta no GitHub, **https://github.com/seu-suario**, substituindo o `seu-usuario` por seu usuário no GitHub.

Em **Test Repository** coloque respositorio que fizemos o fork `pipelines-harness-exemplo-basico`.

Clique em continue.

Em **Authentication**, coloque o seu **username** do GitHUb 
Antes de cadastrar o **Personal Access Token**, vamos criar o PAT no GitHub usado o seguinte link: [https://github.com/settings/tokens/new?scopes=repo,user,admin:org_hook,admin:repo_hook](https://github.com/settings/tokens/new?scopes=repo,user,admin:org_hook,admin:repo_hook). Devem estar marcadas as opções repo, user, admin:org_hook e admin:repo_hook.

Na página do github qe irá aprecer, em **note** digite `Harness` e depois clique em **Generate token**, copie o token para usá-lo como secret

De volta a página do Harness, clique  em **Create or Select a Secret**, no modal que irá aparecer clique em **New Secret Text**. Em **Secret Name**  digite **GitHub PAT** e em **Secret Value** cole o token gerado no Github e clique em **Save**

Marque o **Enable API access (recommended)** e em **API Authentication**  clique  em **Create or Select a Secret** e selecione o **GitHub PAT** e depois clique em Continue. 

Em **Connect to the provider** selecione **Connect through Harness Platform** e clique em **Save and Continue**.

Aguarde a **Connection Test** terminar e aparecer **Verification successful** e clique em **Finish**.

Agora vamos criar um Conector para o Docker Hub.

Clique novamente em **New Conector**

Digite Docker em Search e selecione o Docker Registry

Em name coloque `Docker_Toolbox`

Em **Provider Type** selecione **DockerHub**

Em **Docker Registry URL** preencar com `https://index.docker.io/v2/`

Em **Authentication**, coloque o seu **username** do Docker Hub. 
Antes de cadastrar o **Password**, vamos criar o token no Docker Hub. Para criar o token acesse [https://hub.docker.com/settings/security](https://hub.docker.com/settings/security) e clique em **New Access Token**.
Preencha o **Access Token Description** com `Harness Token` e clique em **generate**.

Copie o token gerado.

De volta a página do Harness, clique  em **Create or Select a Secret**, no modal que irá aparecer clique em **New Secret Text**. Em **Secret Name**  digite `Docker Token` e em **Secret Value** cole o token gerado no Docker Hub e clique em **Save**.

Clique em Continue. 

Em **Connect to the provider** selecione **Connect through Harness Platform** e clique em **Save and Continue**.

Aguarde a **Connection Test** terminar e aparecer **Verification successful** e clique em **Finish**.

Clique em **Pipelines** e depois clique na **seta para baixo** no **+ Create a Pipeline** e clique em **Import from Git** 

Clique em **Third-party Git provider**, depois selecione o **Git Connector** **Toolbox_GitHub**, em **Repository** selecione o repositório que você fez o fork, `pipelines-harness-exemplo-basico-fork`.

Em **Git Branch** escolha **main** e em YAML path coloque `.harness/pipelines/toolboxplaygroundharness.yaml` e clique em **Import**

Clique me **pipelines-harness-exemplo-basico**, você verá visualmente a pipeline no **Pipeline Studio**.

Clique **NodeJS**, e em **Infrastructure**, clique em Update Card e coloque um cartão de crédito para rodar a pipeline na **Harness Cloud**, que é o que este exemplo irá fazer. Não será feita nenhuma cobraça pois estamos no plano free e temos direito a 100 builds por mês, para saber mais acesse [https://www.harness.io/pricing](https://www.harness.io/pricing).

Depois selecione **Cloud** e clique em **Continue**

"Caso não queira usar o cartão de crédito ou não queira rodar na **Harness Cloud**, existem as opções de rodar em um **Kubernets** e **Local**. Em **Local** siga o passo-a-passo para configurar conforme o link [https://developer.harness.io/docs/continuous-integration/use-ci/set-up-build-infrastructure/define-a-docker-build-infrastructure/](https://developer.harness.io/docs/continuous-integration/use-ci/set-up-build-infrastructure/define-a-docker-build-infrastructure/). Se optar por local, deverão ser alterados os conectores também, de **Connect through a Harness Platform** para **Connect through a Harness Delegate**"

Vamos agora adicionar os **Input Sets**, serão dois, um para **Pull Request** e outro para **Push**.

Clique em **Input Sets**, em **New Input Set**, clique na **seta para baixo** e selecione **Import From Git**.

Em **Name** coloque `pipelines-harness-exemplo-basico-pr-trigger-input-set` e em **YAML Path** coloque `.harness/pipelines-harness-exemplo-basico-pr-trigger-input-set.yaml` e clique em **Import**. Acabamos de importar o input set para **Pull Request**.

Novamente em **New Input Set**, clique na **seta para baixo** e selecione **Import From Git** novamente.

Em **Name** coloque `pipelines-harness-exemplo-basico-push-trigger-input-set` e em **YAML Path** coloque `.harness/pipelines-harness-exemplo-basico-push-trigger-input-set.yaml` e clique em **Import**. Acabamos de importar o input set para **Push**.

Agora vamos configurar os **Triggers** para que a pipeline seja executada quando houver um **Push** ou um **Pull Request**.

Clique em **Triggers** e depois em **New Trigger**, selecione o **GitHub** em **Webhook**, em **Name** digite `PR Trigger`, em **Connector** selecione o **Toolbox GitHub**. Em **Repository Name** coloque `pipelines-harness-exemplo-basico-fork`. Em **Event**, selecione **Pull Request** e clique em **Continue**. A próxima tela é para configurar as condições de execução, iremos deixar em branco e clicar em **Continue** novamente. Em **Pipeline Input** colocaremos o **Input Set** para **Pull Request** clicando em **+ Select Input Set**, selecionado o **pipelines-harness-exemplo-basico-pr-trigger-input-set**. Agora clique em **Create Trigger**.

Clique novamente em **New Trigger**, selecione o **GitHub** em **Webhook**, em **Name** digite `Push Trigger`, em **Connector** selecione o **Toolbox GitHub**. Em **Repository Name** coloque `pipelines-harness-exemplo-basico-fork`. Em **Event**, selecione **Push** e clique em **Continue**. A próxima tela é para configurar as condições de execução, iremos deixar em branco e clicar em **Continue** novamente. Em **Pipeline Input** colocaremos o **Input Set** para **Push** clicando em **+ Select Input Set**, selecionado o **pipelines-harness-exemplo-basico-push-trigger-input-set**. Agora clique em **Create Trigger**.

Pronto está tudo configurado para a pipeline ser disparada com um **Push** ou **Pull Request** para o seu repo. 

Para testar, faça uma alteração no [README.md](README.md), colocando seu nome no final e faça um Commit e um Push.

Feito isso clique em **Pipelines** e veja a pipeline rodar.



# estou alterando o readme.md para o lab


# mais um novo commit
