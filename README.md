# Guild DIVINE (Guild Wars 2)

Site estático com os guias da guilda **DIVINE**: Raids, Fractals, Dungeons, Endgame e Recursos Úteis.
Cada guia funciona sozinho (CSS embutido), abre em qualquer navegador e tem a versão em PDF.

```
divine-gw2-manual/
├── index.html              ← página inicial (grid com os 5 guias)
├── assets/
│   ├── divine.css          ← folha de estilo única (fonte da verdade; é embutida nos guias)
│   ├── fonts/              ← Barlow Condensed, Source Sans 3, IBM Plex Mono (OFL, auto-hospedadas)
│   └── img/                ← cópias locais otimizadas de todas as imagens (JPEG/WebP)
├── guias/
│   ├── raids.html
│   ├── fractals.html
│   ├── dungeons.html
│   ├── endgame.html
│   └── recursos-uteis.html
├── pdfs/                   ← mesmos guias em PDF (A4)
└── README.md
```

> **Não edite os arquivos de `guias/` à mão.** Eles são gerados pelo `build_site.ps1` (na pasta-mãe)
> a partir dos HTMLs originais + `_build/fixes.ps1`. Para mudar um texto: edite o original
> (ou adicione uma correção em `_build/fixes.ps1`) e rode a build de novo.

---

## Opção 1 — GitHub Pages (recomendado)

Grátis, com histórico de versões e URL estável (`https://<usuario>.github.io/divine-gw2-manual/`).

**Pelo navegador (sem instalar nada):**

1. Entre em <https://github.com/new> e crie o repositório **`divine-gw2-manual`** (público).
2. Na página do repositório: **Add file → Upload files** e arraste **o conteúdo** desta pasta
   (`index.html`, `assets/`, `guias/`, `pdfs/`, `README.md`). Clique em **Commit changes**.
3. Vá em **Settings → Pages** → *Build and deployment* → **Source: Deploy from a branch** →
   Branch **`main`** / pasta **`/ (root)`** → **Save**.
4. Em 1–2 minutos o site estará em `https://<seu-usuario>.github.io/divine-gw2-manual/`.

**Pelo terminal (com Git instalado):**

```bash
cd divine-gw2-manual
git init -b main
git add .
git commit -m "Manual da Guilda DIVINE"
git remote add origin https://github.com/<seu-usuario>/divine-gw2-manual.git
git push -u origin main
```

Depois ative o Pages como no passo 3.

**Para atualizar:** rode a build de novo e faça upload/commit dos arquivos alterados.

## Opção 2 — Cloudflare Pages

1. <https://dash.cloudflare.com> → **Workers & Pages → Create → Pages → Upload assets**.
2. Nome do projeto: `divine-gw2-manual` → arraste esta pasta → **Deploy**.
3. URL: `https://divine-gw2-manual.pages.dev` (dá para ligar a um domínio próprio depois).

Também aceita conectar o repositório do GitHub para deploy automático a cada commit.

## Opção 3 — Netlify

1. <https://app.netlify.com/drop> → arraste esta pasta.
2. Pronto: URL `https://<nome-aleatorio>.netlify.app` (renomeável em *Site configuration*).

## Comparativo

| | GitHub Pages | Cloudflare Pages | Netlify |
|---|---|---|---|
| Custo | Grátis | Grátis | Grátis (limite de banda generoso) |
| Deploy | Upload/commit no repo | Arrastar pasta ou Git | Arrastar pasta ou Git |
| Histórico de versões | ✅ completo (Git) | ✅ por deploy | ✅ por deploy |
| Velocidade global | Boa | Excelente (CDN própria) | Muito boa |
| Edição colaborativa | ✅ PRs / vários editores | Via Git | Via Git |
| Conta necessária | GitHub | Cloudflare | Netlify |
| Melhor para | Guilda com mais de um editor | Desempenho / domínio próprio | Publicar em 30 segundos |

**Google Drive** serve só para guardar os PDFs: ele não renderiza HTML como site.

---

## Como editar

O passo a passo completo para mudar textos, imagens, logos e visual está em **`COMO_EDITAR.md`**, na pasta-mãe do projeto (fora desta pasta publicada). Lá também estão exemplos prontos de cada bloco.

## Design

O visual segue o design system **Codex DIVINE**, documentado em `DESIGN.md` na pasta-mãe (formato do blueprint TypeUI). Lá estão as fontes, os tokens de cor, os componentes e as regras. Qualquer mudança de estilo deve começar por esse arquivo.

## Tema claro e escuro

- Os guias seguem o tema do sistema (claro ou escuro).
- O botão na barra do topo troca o tema, e a escolha fica salva no navegador.
- **PDFs e impressão saem sempre no tema claro.**
- Todas as cores vêm de tokens no topo de `assets/divine.css`. Para mudar a paleta, edite só ali.

## Gerar de novo os HTMLs e PDFs

Requisitos: Node.js. Na primeira vez, instale as dependências da build:

```bash
cd "_build"
npm install
```

Depois, na pasta **`Guias Divine`** (pasta-mãe), no PowerShell:

```powershell
powershell -ExecutionPolicy Bypass -File .\build_site.ps1 -Pdf
```

O que a build faz:
1. `build_site.ps1` lê os HTMLs originais e aplica as correções de `_build/fixes.ps1`, o CSS, as âncoras e a logo.
2. `_build/postprocess.mjs` organiza a estrutura semântica, troca emojis por ícones SVG (Lucide, licença ISC), revisa o texto e gera as cópias locais otimizadas das imagens.
3. Com `-Pdf`, o Microsoft Edge imprime os PDFs (A4, sem cabeçalho nem rodapé do navegador).

Sem `-Pdf`, gera só os HTMLs. Para conferir a qualidade (contraste, celular, tema escuro):

```bash
node _build/qa_audit.js divine-gw2-manual
```

## Imagens

- **Todas as imagens são servidas de `assets/img/`.** O site não depende de hotlink do Discord, Imgur ou da Wiki para funcionar.
- As fontes originais são a **Wiki oficial** (boons, conditions, atributos, moedas, loading screens), o **Imgur da guilda** (logo e artes) e alguns emojis do Discord sem equivalente oficial (títulos de Fractal, Attuned/Infused, CM/LM). A build baixa uma vez (cache em `_build/cache/`) e gera cópias no tamanho de exibição ×2:
  - **JPEG** para fotos e artes;
  - **WebP** para ícones com transparência.
- Os banners decorativos têm todos a mesma proporção (2:1). Screenshots, mapas e infográficos mantêm o tamanho natural.
