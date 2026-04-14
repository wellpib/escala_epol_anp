# 🚀 Como Hospedar o Portal no GitHub Pages

## 📋 Visão Geral

Este guia explica como publicar o Portal ePol no GitHub Pages para acesso via web.

## 🔧 Pré-requisitos

- Conta no GitHub
- Git instalado no computador
- Portal configurado com Firebase (opcional, mas recomendado)

## 📝 Passo a Passo

### 1. Criar Repositório no GitHub

1. Acesse [GitHub](https://github.com) e faça login
2. Clique no **+** no canto superior direito e selecione **"New repository"**
3. Configure o repositório:
   - **Repository name**: `portal-epol` (ou outro nome)
   - **Description**: "Portal de Gestão de Alocações ePol"
   - **Public** ou **Private** (conforme sua necessidade)
   - ✅ Marque **"Add a README file"**
4. Clique em **"Create repository"**

### 2. Preparar o Projeto

#### Criar .gitignore

Crie um arquivo `.gitignore` na pasta do projeto com o seguinte conteúdo:

```
# Arquivos sensíveis
config.local.js

# Arquivos temporários
*.tmp
*.log
~$*.xlsx

# Dados de teste (opcional - remova se quiser versionar)
*.xlsx
*.csv
*.xls

# Sistema operacional
.DS_Store
Thumbs.db
Desktop.ini

# Editor
.vscode/
.idea/
*.swp
*.swo
```

⚠️ **IMPORTANTE**: Se você deixou as credenciais do Firebase no código:
- Considere mover para um arquivo separado
- Ou garanta que o repositório seja **privado**
- Ou use autenticação do Firebase para proteger os dados

### 3. Fazer Upload dos Arquivos

#### Opção A: Via GitHub Web (Mais Fácil)

1. No seu repositório no GitHub, clique em **"Add file"** > **"Upload files"**
2. Arraste os arquivos:
   - `index.html`
   - `semana 13.csv` (se quiser incluir dados de exemplo)
   - `FIREBASE_CONFIG.md`
   - `README.md`
3. Adicione uma mensagem de commit: "Upload inicial do portal"
4. Clique em **"Commit changes"**

#### Opção B: Via Git (Mais Profissional)

```powershell
# No PowerShell, navegue até a pasta do projeto
cd d:\planilhaaula

# Inicialize o repositório Git
git init

# Adicione o remote do GitHub
git remote add origin https://github.com/SEU-USUARIO/portal-epol.git

# Adicione os arquivos
git add .

# Faça o commit
git commit -m "Upload inicial do portal"

# Configure a branch principal
git branch -M main

# Envie para o GitHub
git push -u origin main
```

### 4. Ativar o GitHub Pages

1. No seu repositório, clique em **"Settings"** (Configurações)
2. No menu lateral, clique em **"Pages"**
3. Em **"Source"** (Fonte), selecione:
   - **Branch**: `main`
   - **Folder**: `/ (root)`
4. Clique em **"Save"**
5. Aguarde alguns minutos (geralmente 1-5 minutos)
6. O site estará disponível em: `https://SEU-USUARIO.github.io/portal-epol/`

### 5. Testar o Site

1. Acesse a URL fornecida: `https://SEU-USUARIO.github.io/portal-epol/`
2. Verifique se:
   - A página carrega corretamente
   - O Firebase está conectado (verifique o console do navegador - F12)
   - Os arquivos CSV/Excel podem ser carregados
   - O CRUD de servidores funciona

## 🔒 Configurações de Segurança

### Repositório Privado

Se você optou por um repositório privado:
- O site ainda será **público** no GitHub Pages
- Apenas o código-fonte é privado
- Configure autenticação no Firebase para proteger os dados

### Autenticação Firebase (Recomendado para Produção)

1. No Firebase Console, ative **"Authentication"**
2. Habilite o método de login desejado:
   - **E-mail/Senha**: Para usuários internos
   - **Google**: Login com conta Google
   - **SAML**: Para integração com sistemas corporativos

3. Adicione o código de autenticação no `index.html`:

```javascript
// Adicione após a inicialização do Firebase
firebase.auth().onAuthStateChanged(function(user) {
  if (user) {
    // Usuário logado
    console.log('Usuário logado:', user.email);
    document.body.style.display = 'block';
  } else {
    // Usuário não logado - redirecionar para login
    window.location.href = 'login.html';
  }
});
```

4. Atualize as regras do Firestore:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /servidores/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### Restringir Acesso por Domínio

No Firebase Console:
1. Vá em **"Authentication"** > **"Settings"** > **"Authorized domains"**
2. Adicione apenas: `SEU-USUARIO.github.io`
3. Remova outros domínios não utilizados

## 🔄 Atualizações Futuras

### Atualizar o Site (Git)

```powershell
cd d:\planilhaaula

# Adicione as mudanças
git add .

# Faça o commit
git commit -m "Descrição da atualização"

# Envie para o GitHub
git push
```

O GitHub Pages atualiza automaticamente em alguns minutos.

### Atualizar via GitHub Web

1. Navegue até o arquivo que deseja editar
2. Clique no ícone de **lápis** (editar)
3. Faça as alterações
4. Clique em **"Commit changes"**

## 📱 Domínio Personalizado (Opcional)

Se você possui um domínio próprio:

1. No GitHub Pages settings, adicione seu **"Custom domain"**
2. Configure o DNS do seu domínio:
   - **CNAME**: `portal.suaorganizacao.gov.br` → `SEU-USUARIO.github.io`
   - Ou **A Records** para os IPs do GitHub
3. Aguarde a propagação do DNS (pode levar até 48h)

**IPs do GitHub Pages:**
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

## 🆘 Solução de Problemas

### Site não carrega

1. Verifique se o GitHub Pages está ativado em Settings > Pages
2. Aguarde alguns minutos após o primeiro deploy
3. Limpe o cache do navegador (Ctrl+F5)
4. Verifique se não há erros no console (F12)

### Erro 404

1. Certifique-se de que o arquivo `index.html` está na raiz do repositório
2. Verifique se a branch correta está selecionada (main)
3. Tente acessar diretamente: `https://SEU-USUARIO.github.io/portal-epol/index.html`

### Firebase não conecta

1. Verifique se as credenciais estão corretas no código
2. Adicione o domínio do GitHub Pages (`SEU-USUARIO.github.io`) nos domínios autorizados do Firebase
3. Verifique as regras de segurança do Firestore

### Mudanças não aparecem

1. Aguarde 1-5 minutos após o push
2. Limpe o cache do navegador (Ctrl+Shift+Delete)
3. Tente em modo anônimo/privado do navegador
4. Use Ctrl+F5 para forçar atualização

## 🎨 Melhorias Recomendadas

### 1. Adicionar Página de Login

Crie `login.html` para autenticação antes de acessar o portal.

### 2. Adicionar Favicon

```html
<link rel="icon" href="favicon.ico" type="image/x-icon">
```

### 3. Adicionar Meta Tags para SEO/Compartilhamento

```html
<meta name="description" content="Portal de Gestão de Alocações da Academia Nacional de Polícia">
<meta property="og:title" content="Portal ePol">
<meta property="og:description" content="Sistema de gerenciamento de servidores e alocações">
<meta property="og:image" content="https://SEU-USUARIO.github.io/portal-epol/logo.png">
```

### 4. PWA (Progressive Web App)

Transforme o portal em um app instalável no celular:
- Adicione `manifest.json`
- Adicione Service Worker para funcionamento offline
- Configure ícones para diferentes tamanhos

## 📊 Monitoramento

### Google Analytics (Opcional)

1. Crie uma conta no [Google Analytics](https://analytics.google.com/)
2. Adicione o código de rastreamento no `<head>` do `index.html`:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### Firebase Analytics

Já incluído automaticamente quando você usa o Firebase!

## 🌐 Alternativas ao GitHub Pages

Se preferir outras plataformas de hospedagem gratuita:

1. **Netlify**
   - Drag & drop de arquivos
   - CI/CD automático
   - HTTPS grátis

2. **Vercel**
   - Otimizado para performance
   - Deploy automático do GitHub
   - Domínio grátis

3. **Firebase Hosting**
   - Integração total com Firebase
   - CDN global
   - HTTPS grátis

4. **Cloudflare Pages**
   - CDN global da Cloudflare
   - Deploy do GitHub
   - Analytics incluído

## ✅ Checklist Final

Antes de disponibilizar o portal para uso:

- [ ] Firebase configurado e testado
- [ ] Regras de segurança do Firestore configuradas
- [ ] Autenticação ativada (se necessário)
- [ ] Domínio do GitHub Pages adicionado aos domínios autorizados do Firebase
- [ ] CRUD de servidores testado
- [ ] Upload de arquivos testado
- [ ] Relatórios testados
- [ ] Site acessível via HTTPS
- [ ] Testes em diferentes navegadores (Chrome, Firefox, Edge)
- [ ] Testes em dispositivos móveis
- [ ] Documentação compartilhada com a equipe

## 📚 Recursos Adicionais

- [Documentação do GitHub Pages](https://docs.github.com/pages)
- [Sobre GitHub Pages e HTTPS](https://docs.github.com/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https)
- [Domínios personalizados](https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site)

---

**🎉 Pronto! Seu portal está no ar!**

Acesse: `https://SEU-USUARIO.github.io/portal-epol/`
