# **Tutorial: Como Deployar um Aplicativo Streamlit no Azure App Service**

Este tutorial irá guiá-lo passo a passo para realizar o deploy de uma aplicação **Streamlit** no **Microsoft Azure App Service**, sem utilizar Docker ou Azure CLI. O deploy será integrado com o GitHub para permitir CI/CD (Integração e Deploy Contínuos). 

---

## **Passo 1: Configurar o App Service no Azure**

1. **Acesse o Portal do Azure**
   - Entre no [Portal do Azure](https://portal.azure.com).
   - No menu lateral, busque por **App Services** e clique na opção.

2. **Criar um Novo App Service**
   - Clique em **Create** (Criar).
   - Preencha as seguintes informações:
     - **Resource Group**: Escolha ou crie um grupo de recursos.
     - **App Name**: Escolha um nome único (será usado como parte do URL).
     - **Publish**: Selecione **Code** (Código).
     - **Runtime Stack**: Escolha **Python 3.8** (ou a versão do Python usada na sua aplicação).
     - **Operating System**: Escolha **Linux**.
     - **Region**: Escolha a região mais próxima de seus usuários.

3. **Plano de Serviço**
   - Escolha um plano de serviço:
     - **Dev/Test** → **B1 Basic**: É necessário, pois o plano gratuito (F1) não suporta WebSockets, que são usados pelo Streamlit.
   - Clique em **Apply** (Aplicar).

4. **Habilitar CI/CD (Continuous Deployment)**
   - Clique em **Next: Deployment**.
   - Habilite a opção **Enable Continuous Deployment**.
   - Conecte sua conta GitHub ou GitLab.
   - Escolha o repositório e a branch que contém o código do aplicativo.

5. **Revisar e Criar**
   - Clique em **Review + Create**.
   - Após revisar as configurações, clique em **Create** (Criar).

---

## **Passo 2: Configurar a Aplicação**

1. **Adicionar o Comando de Inicialização**
   - No App Service, vá até **Configuration** → **General Settings**.
   - Na opção **Startup Command**, insira o comando para iniciar o Streamlit:
     ```bash
     streamlit run app.py --server.port=8000 --server.address=0.0.0.0
     ```
   - Substitua `app.py` pelo nome do arquivo principal da sua aplicação Streamlit.

2. **Salvar Configurações**
   - Clique em **Save** e confirme a atualização.

---

## **Passo 3: Configurar o Repositório GitHub**

1. **Criar o Arquivo `requirements.txt`**
   - No repositório da aplicação, crie um arquivo chamado `requirements.txt` com as dependências do Python necessárias para rodar o aplicativo. Exemplo:
     ```plaintext
     streamlit
     pandas
     numpy
     matplotlib
     ```
   - Certifique-se de listar todas as dependências usadas na aplicação.

2. **Commit e Push**
   - Faça o commit do arquivo `requirements.txt` e envie para o repositório conectado ao Azure.

---

## **Passo 4: Verificar o Deploy**

1. **Acesse o App Service**
   - No Portal do Azure, vá até o App Service criado.
   - Clique em **Browse** ou acesse o URL fornecido.

2. **Validar a Aplicação**
   - Verifique se a aplicação Streamlit está rodando corretamente.
   - Caso veja a página padrão do Azure, confirme se a **Startup Command** foi configurada corretamente.

---

## **Passo 5: Testar CI/CD**

1. **Realizar Alterações**
   - Faça uma alteração no código no repositório GitHub, como mudar o título do dashboard.
   - Exemplo:
     ```python
     st.title("Dashboard Atualizado!")
     ```

2. **Verificar o Deploy**
   - O GitHub Actions será automaticamente acionado, e o Azure App Service será atualizado com a nova versão do código.
   - Após alguns minutos, as alterações estarão disponíveis na aplicação.

---

## **Passo 6: Escalar ou Excluir o App Service**

1. **Escalar**
   - No Azure Portal, vá até **Scale Up (App Service Plan)**.
   - Escolha um plano de serviço superior, caso necessário.

2. **Excluir**
   - Para remover o App Service, clique em **Delete**.
   - Insira o nome do App Service e confirme.

---

## **Resumo**

- **Objetivo**: Deploy de uma aplicação Streamlit no Azure App Service.
- **Configurações**: Configuramos o runtime, o plano de serviço e CI/CD com GitHub.
- **Resultado**: A aplicação está disponível publicamente e atualiza automaticamente com o CI/CD.

Se tiver dúvidas ou problemas, deixe um comentário! 🚀