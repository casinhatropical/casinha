# Guia: sair da Wix e gerir o site sozinho

Site: **Casinha Tropical** · `casinhatropical.com`

Este guia é para o **dono do site** passar a editar e publicar tudo **sem Wix**, usando:

- **GitHub** (código do site)
- **Vercel** (site no ar)
- **Cursor gratuito** (editar no computador)
- **Cloudflare** (domínio + DNS, substituindo a Wix)

Você **já criou** o repositório no seu GitHub (baixou os arquivos e subiu). Este guia continua a partir daí.

---

## Visão geral

```
Você edita no Cursor (grátis)
        ↓
Salva no GitHub (seu repo)
        ↓
Sua Vercel publica automaticamente
        ↓
casinhatropical.com (DNS no Cloudflare, não na Wix)
```

---

## O que você vai precisar

- [ ] Email seu (o mesmo em GitHub, Vercel e Cloudflare facilita)
- [ ] Conta **GitHub** (grátis): https://github.com/signup
- [ ] Conta **Vercel** (grátis): https://vercel.com/signup
- [ ] **Cursor** (grátis): https://cursor.com — plano Free/Hobby basta
- [ ] Conta **Cloudflare** (grátis): https://dash.cloudflare.com/sign-up
- [ ] Acesso ao **painel Wix** (só para copiar DNS e transferir o domínio)
- [ ] Acesso ao **Zoho Mail** (email `@casinhatropical.com` — não muda, só os registros DNS)

---

## PARTE 1 — GitHub (confirme que o repo está completo)

Você já fez: baixou os arquivos e criou um repo novo no **seu** GitHub. ✅

Antes de seguir, abra o repo e confira se tem **tudo isto na raiz**:

| Arquivo / pasta | Obrigatório? |
|-----------------|--------------|
| `index.html` | ✅ Sim — é o site |
| `casinha-logo.png` | ✅ Sim — logo no topo |
| `deluxe/` (fotos `.jpg`) | ✅ Sim — fotos do Casal Deluxe |
| `GUIA-SAIR-WIX.md` | Opcional — este guia |

Se só tiver `index.html`, peça à Liana a **pasta completa** (zip) ou baixe de https://github.com/thehighlightsmethod-debug/casinhatropical → **Code → Download ZIP** e suba de novo.

**Subir arquivos no GitHub (se faltar algo):**

1. Abra seu repo → **Add file → Upload files**
2. Arraste `casinha-logo.png` e a pasta `deluxe/`
3. **Commit changes**

Anote a URL do seu repo, ex.:  
`https://github.com/SEU-USUARIO/NOME-DO-REPO`

---

## PARTE 2 — Vercel **sua conta** (publicar na internet)

O site hoje pode ainda estar na Vercel da Liana. Você vai criar **seu** projeto.

1. Entre em https://vercel.com com **sua conta** (crie se ainda não tiver).
2. Clique **Add New → Project**.
3. **Import** o **seu** repositório GitHub (autorize o GitHub se pedir).
4. Configuração:
   - **Framework Preset:** Other
   - **Root Directory:** `./` (raiz)
   - **Build Command:** deixe vazio
   - **Output Directory:** deixe vazio
5. Clique **Deploy** e espere ficar verde.
6. Teste o link `https://casinhatropical-xxxx.vercel.app` — deve abrir o site novo.

### Ligar o domínio na sua Vercel

1. No projeto → **Settings → Domains**.
2. Adicione:
   - `casinhatropical.com`
   - `www.casinhatropical.com`
3. A Vercel mostrará registros DNS. **Anote** (vai usar na Parte 4).
4. Quando o domínio estiver na **sua** Vercel, o site deixa de depender da conta da Liana.

---

## PARTE 3 — Cursor gratuito (editar o site)

1. Instale o Cursor: https://cursor.com/download
2. Crie conta / entre no plano **Free** (não precisa pagar).
3. **File → Clone Repository** (ou terminal) — use **seu** repo:

   ```bash
   git clone https://github.com/SEU-USUARIO/NOME-DO-REPO.git
   cd NOME-DO-REPO
   ```

4. Abra a pasta no Cursor.
5. Edite o arquivo **`index.html`** (textos, FAQ, links, etc.).
6. Para publicar, no terminal do Cursor:

   ```bash
   git add index.html
   git commit -m "Atualiza textos do site"
   git push
   ```

7. Em **1–2 minutos** a Vercel atualiza o site sozinha.

### O que dá para mudar sozinho

- Textos e FAQ
- Links (WhatsApp, reservas)
- Fotos na pasta `deluxe/` (e trocar o nome no HTML)

### O que é mais difícil sem ajuda

- Mudar layout, cores ou criar seções novas

---

## PARTE 4 — Sair da Wix (domínio + DNS)

**Importante:** faça isso **antes** de cancelar a Wix. **Não apague** os registros de email.

### Passo 4.1 — Copiar DNS da Wix (backup)

No Wix → **Domínios → casinhatropical.com → DNS**, copie ou tire print de **tudo**, especialmente:

| Tipo | Para quê |
|------|----------|
| MX | Email Zoho |
| TXT (SPF, DKIM, zoho-verification) | Email Zoho |
| A / CNAME | Site (vai mudar para Vercel) |

### Passo 4.2 — Transferir domínio para Cloudflare

1. Crie conta em https://dash.cloudflare.com
2. **Domain Registration → Transfer domains** → digite `casinhatropical.com`
3. Na **Wix**:
   - Desbloqueie a transferência do domínio
   - Peça o **código AUTH/EPP** (chega por email)
4. Cole o código no Cloudflare e pague a transferência (1 ano de registro).
5. Aguarde **5–7 dias** (o site continua no ar na Wix até terminar).

### Passo 4.3 — DNS no Cloudflare (sem Wix)

Quando o domínio estiver no Cloudflare → **DNS → Records**:

**Site (valores que a SUA Vercel mostrar em Settings → Domains):**

| Tipo | Nome | Valor (exemplo) |
|------|------|-----------------|
| A | `@` | `76.76.21.21` (ou o IP que a Vercel indicar) |
| CNAME | `www` | `cname.vercel-dns.com` (ou o que a Vercel indicar) |

**Email Zoho (copie igual estava na Wix):**

| Tipo | Nome | Valor |
|------|------|--------|
| MX | `@` | registros MX do Zoho |
| TXT | `@` | SPF (`v=spf1 include:zohomail...`) |
| TXT | `zmail._domainkey` | DKIM do Zoho |
| TXT | `@` | `zoho-verification=...` |

Salve. Propagação: **30 min a 24 h**.

### Passo 4.4 — Testar antes de cancelar a Wix

- [ ] `https://casinhatropical.com` abre o site novo
- [ ] `https://www.casinhatropical.com` abre o site novo
- [ ] Vercel → Domains → **Valid** (sem aviso vermelho)
- [ ] Email: envie e receba um teste em `@casinhatropical.com`

### Passo 4.5 — Cancelar a Wix

1. Cancele o **plano de site** Wix.
2. Só cancele o **domínio** na Wix **depois** que a transferência para Cloudflare terminar.
3. Ignore o Editor Wix — não use mais **Publicar** lá.

---

## PARTE 5 — Checklist final (autonomia total)

- [ ] Repositório GitHub na **minha** conta (com index.html, logo e deluxe/)
- [ ] Projeto Vercel na **minha** conta, ligado ao **meu** repo
- [ ] Domínio apontando para a **minha** Vercel (não a da Liana)
- [ ] Domínio no **Cloudflare** (não na Wix)
- [ ] DNS: site → Vercel, email → Zoho
- [ ] Cursor instalado; testei editar `index.html` e dar `git push`
- [ ] Site e email funcionando
- [ ] Wix cancelada

---

## Resumo em 5 linhas (WhatsApp)

> 1. Já tenho o site no **meu** GitHub (index.html + logo + pasta deluxe).  
> 2. Crio conta na **Vercel** e importo **meu** repo.  
> 3. Edito no **Cursor grátis** e dou push — minha Vercel publica sozinha.  
> 4. Transfiro o domínio da Wix pro **Cloudflare** e copio MX/TXT do Zoho.  
> 5. Aponto DNS pro **meu** projeto Vercel, testo site + email, cancelo Wix.

---

## Ajuda

- **Site não abre:** confira DNS no Cloudflare e Domains na Vercel.
- **Email parou:** faltou algum MX/TXT do Zoho no Cloudflare — compare com o backup da Wix.
- **Deploy não atualizou:** Vercel → Deployments — deve aparecer deploy verde após o `git push`.

---

*Guia criado para Casinha Tropical — GitHub + Vercel + Cursor (plano gratuito).*
