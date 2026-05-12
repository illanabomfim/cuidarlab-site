# CuidarLab — Site Institucional + CMS

**Laboratório de Tecnologias de Cuidados Sociais · UFBA · EAUFBA**

---

## Estrutura do Projeto

```
cuidarlab/
│
├── index.html              ← Site principal (landing page)
├── package.json
├── vercel.json             ← Configuração de rotas para Vercel
│
├── admin/
│   ├── index.html          ← Painel Decap CMS
│   └── config.yml          ← Coleções editáveis do CMS
│
├── content/                ← TODO o conteúdo editável (Markdown)
│   ├── config/
│   │   ├── geral.md        ← Nome, e-mail, redes sociais
│   │   ├── hero.md         ← Textos da seção Hero
│   │   ├── sobre.md        ← Seção Sobre
│   │   └── manifesto.md    ← Manifesto
│   ├── blog/               ← Posts do blog
│   ├── publicacoes/        ← Artigos, relatórios, pesquisas
│   ├── podcasts/           ← Episódios do podcast
│   ├── eixos/              ← Eixos de atuação
│   └── equipe/             ← Membros do laboratório
│
├── public/
│   └── uploads/            ← Imagens e arquivos enviados pelo CMS
│
├── scripts/
│   ├── build.js            ← Compilador Markdown → HTML
│   └── dev-server.js       ← Servidor local de desenvolvimento
│
└── dist/                   ← Saída do build (gerada automaticamente)
```

---

## Deploy no Vercel + GitHub

### 1. Criar repositório no GitHub

```bash
git init
git add .
git commit -m "feat: CuidarLab site + CMS"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/cuidarlab.git
git push -u origin main
```

### 2. Conectar ao Vercel

1. Acesse [vercel.com](https://vercel.com) → **New Project**
2. Importe o repositório do GitHub
3. Configure:
   - **Framework**: Other
   - **Build Command**: `node scripts/build.js`
   - **Output Directory**: `dist`
   - **Install Command**: `npm install`
4. Clique em **Deploy**

### 3. Configurar autenticação do CMS

O Decap CMS precisa de um provider de autenticação OAuth para funcionar com GitHub.

#### Opção A — Netlify Identity (mais simples)

Mesmo hospedando no Vercel, você pode usar o Netlify só para autenticação:

1. Crie uma conta em [netlify.com](https://netlify.com)
2. Novo site → conecte o mesmo repositório
3. Ative **Netlify Identity** (Site Settings → Identity → Enable)
4. Convide seu e-mail como usuário
5. Em `admin/config.yml`, o `base_url: https://api.netlify.com` já está configurado

#### Opção B — GitHub OAuth App (recomendado para produção)

1. GitHub → Settings → Developer settings → OAuth Apps → **New OAuth App**
2. **Homepage URL**: `https://seu-site.vercel.app`
3. **Callback URL**: `https://seu-site.vercel.app/api/auth`
4. Copie o **Client ID** e gere um **Client Secret**
5. Em `admin/config.yml`, altere:

```yaml
backend:
  name: github
  repo: SEU_USUARIO/cuidarlab
  branch: main
  auth_endpoint: /api/auth
```

6. Crie as variáveis de ambiente no Vercel:
   - `GITHUB_CLIENT_ID`
   - `GITHUB_CLIENT_SECRET`

---

## Usar o CMS

### Acessar o painel

```
https://seu-site.vercel.app/admin
```

### O que é editável pelo CMS

| Seção | Caminho |
|-------|---------|
| ⚙️ Config geral, redes, e-mail | Configurações → Informações Gerais |
| 🏠 Textos do Hero | Configurações → Seção Hero |
| 📝 Posts do Blog | Blog → Novo post |
| 📚 Publicações | Publicações → Nova publicação |
| 🎙️ Episódios de podcast | Podcasts → Novo episódio |
| 🔬 Eixos de atuação | Eixos → Editar |
| 👥 Equipe | Equipe → Adicionar membro |

### Workflow de publicação

Com `publish_mode: editorial_workflow` ativado:
1. Novo conteúdo → salvo como **Draft**
2. Revisão → marcado como **In Review**
3. Aprovação → **Ready** → cria Pull Request no GitHub
4. Merge do PR → Vercel faz rebuild automático → publicado

---

## Desenvolvimento Local

```bash
# Instalar dependências
npm install

# Rodar com watch mode
npm run dev
# → http://localhost:3000

# Apenas build
npm run build
```

---

## Adicionar novo conteúdo manualmente (sem CMS)

Crie um arquivo `.md` na pasta correta com o frontmatter adequado.

### Blog

```markdown
---
title: "Título do post"
date: 2025-06-01
autor:
  nome: "Nome do Autor"
  cargo: "Cargo"
categoria: politica  # politica | formacao | pesquisa | tecnologia-social | noticias | opiniao
resumo: "Texto curto para a listagem"
draft: false         # true = não aparece no site
---

Conteúdo em Markdown aqui...
```

### Publicação

```markdown
---
title: "Título da publicação"
ano: 2025
tipo: relatorio  # artigo | relatorio | diagnostico | nota-tecnica | guia | material | tese
autores:
  - "Autor 1"
  - "Autor 2"
resumo: "Abstract/resumo"
pdf: /uploads/publicacoes/arquivo.pdf
---
```

### Podcast

```markdown
---
title: "Título do episódio"
episodio: 2
temporada: 1
date: 2025-06-01
duracao: "52min"
plataformas:
  spotify: "https://..."
  embed: "https://open.spotify.com/embed/episode/..."
resumo: "Sobre o episódio"
---
```

---

## Tecnologias

| Tecnologia | Uso |
|------------|-----|
| HTML/CSS/JS puro | Site principal (zero dependências pesadas) |
| Decap CMS (ex-Netlify CMS) | Painel de edição visual |
| gray-matter | Leitura de frontmatter Markdown |
| marked | Conversão Markdown → HTML |
| Node.js | Build script |
| Vercel | Hospedagem + CI/CD |
| GitHub | Repositório + backend do CMS |

---

## Suporte

**CuidarLab · UFBA · EAUFBA**  
📧 cuidarlab@ufba.br

---

*"Cuidado é direito. Cuidar é responsabilidade coletiva."*
