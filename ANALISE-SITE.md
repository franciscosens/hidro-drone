# Análise do site atual — Hidro Drone LP

> Data da análise: 02/08/2026
> Arquivo analisado: `index.html` (779 linhas, ~34 KB, página única auto-contida)
> Observação: as fotos/vídeos atuais em `assets/instagram/` foram **ignoradas** nesta análise — serão substituídas por outro material.

---

## 1. Visão geral

Landing page de página única, em português (pt-BR), para a **Hidro Drone** — empresa de lavagem de fachadas e limpeza industrial com drones de jato de alta pressão. Todo o site (HTML + CSS + JS) está em um único arquivo, sem dependências de build, frameworks ou bibliotecas externas (exceto Google Fonts).

**Objetivo de conversão:** pedido de orçamento via WhatsApp (`wa.me/5541996620072`), com mensagem pré-preenchida.

---

## 2. Estrutura da página (seções na ordem)

| # | Seção | ID | Conteúdo |
|---|-------|-----|----------|
| 1 | Header (sticky) | — | Logo, nav (Serviços, Como funciona, Vantagens, Contato), CTA "Pedir orçamento" |
| 2 | Hero | — | Headline "Fachada limpa. Ninguém pendurado.", lead, 2 CTAs, specs (+100 m, 250 bar, 0 pessoas em altura) e **widget interativo de fachada** |
| 3 | Faixa de telemetria | — | 4 métricas em fundo escuro: velocidade (5× mais rápido), segurança, consumo de água, estrutura |
| 4 | Serviços | `#servicos` | 3 cards: Fachadas prediais, Limpeza industrial, Difícil acesso (cada um com lista de aplicações) |
| 5 | Como funciona | `#como-funciona` | 4 etapas: Visita técnica → Plano de voo → Execução → Relatório com imagens |
| 6 | Vantagens | `#vantagens` | Tabela comparativa: Hidro Drone × Rapel × Andaime/balancim (5 critérios) |
| 7 | CTA final / Contato | `#contato` | Orçamento por foto no WhatsApp, telefone, site, redes sociais, selos de confiança (ANAC/DECEA, seguro RC, pilotos certificados) |
| 8 | Footer | — | Copyright, redes sociais, "Atendemos todo o Brasil" |

---

## 3. Identidade visual

### Paleta (CSS custom properties em `:root`)
- **Fundo:** `--paper #F3F7F9` (cinza-azulado claro), cards brancos
- **Texto:** `--ink #0C2836` (azul-marinho escuro), secundário `--ink-2 #41606D`
- **Marca/água:** `--water #2BB3D2` (ciano), `--water-deep #17708C`
- **CTA:** verde `#16A34A` / `#15803D` — deliberadamente na cor do WhatsApp
- **Sujeira (widget):** tons terrosos `#8F8B78` / `#6F6957`

### Tipografia
- **Archivo** (variável, largura 62–125) — títulos em uppercase com `wdth` expandido (112–118), peso 800–850. Dá aspecto industrial/técnico.
- **IBM Plex Mono** — usada na classe `.mono` para eyebrows, labels, specs e cabeçalhos de tabela (uppercase, letter-spacing largo). Reforça o tom "telemetria/técnico".

### Tom geral
Clean, técnico, com estética "engenharia/aviação" (mono + telemetria + specs). Copy direta e com personalidade ("Ninguém pendurado", "Onde o drone chega, a sujeira sai").

---

## 4. Elemento de destaque: widget interativo da fachada

A assinatura visual do site é uma **demo interativa antes/depois** no hero:

- Prédio desenhado 100% em CSS (gradientes + `repeating-linear-gradient` para esquadrias/vidros), com camada "suja" recortada via `clip-path: inset()` controlada pela variável `--x`.
- Drone em SVG inline com rotores animados (`@keyframes spin`), jato d'água animado (`spray`) e mangueira até o solo.
- **Interação:** arrastar (pointer events com `setPointerCapture`) ou setas do teclado; implementado como `role="slider"` com `aria-valuenow` — acessível.
- **Animação de entrada:** o drone "lava sozinho" de 15% a 42% em 1,6 s, cancelada ao primeiro toque do usuário.
- Hint "Arraste o drone e limpe o prédio" que desaparece após interação.
- Respeita `prefers-reduced-motion` (pula animações e posiciona direto em 42%).

Este widget substitui a necessidade de foto no hero — comunica o serviço de forma memorável sem imagem real.

---

## 5. Aspectos técnicos

### Pontos fortes
- **Zero dependências JS**: vanilla JS (~65 linhas), sem frameworks. Carregamento leve.
- **Acessibilidade acima da média para uma LP:** `aria-label` em todos os links de ícone, `focus-visible` estilizado, slider com semântica ARIA, `prefers-reduced-motion` respeitado em CSS e JS, `scope="col"` na tabela.
- **Responsivo:** breakpoints em 900/860/820/760/560 px; grids colapsam para 1 coluna; tabela com scroll horizontal (`overflow-x`).
- **Reveal on scroll** com `IntersectionObserver` e fallback para navegadores sem suporte.
- **Performance:** `preconnect` para Google Fonts, backdrop-filter só no header, sombras moderadas.

### Pontos fracos / lacunas técnicas
1. **SEO incompleto:**
   - Sem `og:image` (compartilhamento em redes sociais fica sem preview visual) e sem `og:url` / `og:type`.
   - Sem Twitter Cards.
   - Sem dados estruturados (JSON-LD `LocalBusiness`/`Service`) — relevante para negócio local.
   - Sem `canonical`.
2. **Sem favicon dedicado** — usa o `logo.png` direto (sem tamanhos, sem `apple-touch-icon`, sem manifest).
3. **Nenhuma imagem real do serviço** — todo o site é ilustrativo (CSS/SVG). Não há seção de portfólio, antes/depois real, nem vídeos de operação. *(As novas fotos/vídeos vão preencher exatamente essa lacuna.)*
4. **Menu mobile inexistente:** em telas < 760 px os links de navegação simplesmente somem (`display:none`), restando só o logo e o botão de orçamento. Sem hambúrguer.
5. **Fontes externas do Google** — dependência de terceiro e possível questão LGPD/GDPR; poderiam ser self-hosted.
6. **Sem analytics/pixel** — nenhum rastreamento de conversão (GA4, Meta Pixel, etc.). Não dá para medir cliques no WhatsApp.
7. **Link `hidrodrone.com` no bloco de contato** aponta para o próprio domínio — redundante dentro do próprio site.
8. **Sem formulário alternativo** — WhatsApp e telefone são os únicos canais; não há e-mail nem form para quem prefere não usar WhatsApp.
9. **Ano fixo no footer** ("© 2026") — hardcoded.

---

## 6. Análise de conteúdo e conversão

### O que funciona
- **Proposta de valor clara em 1 frase** no H1 + lead: o benefício (sem andaime/rapel/pessoas em altura) vem antes da tecnologia.
- **CTA único e consistente:** todos os caminhos levam ao WhatsApp com mensagem pré-preenchida; botão verde-WhatsApp destacado no header, hero e CTA final.
- **Redução de atrito no orçamento:** "mande uma foto do prédio e receba estimativa" + "visita técnica gratuita".
- **Prova técnica:** specs numéricas (+100 m, 250 bar), tabela comparativa honesta com métodos tradicionais, selos de conformidade (ANAC/DECEA, seguro RC).
- **Diferencial comunicado:** relatório com foto/vídeo aéreo incluso (linha da tabela + etapa 4).

### O que falta (oportunidades)
- **Prova social:** nenhum depoimento, logo de cliente, número de obras realizadas ou avaliação. É a lacuna mais crítica de persuasão.
- **Portfólio real:** seção antes/depois com fotos/vídeos reais (o material novo do Instagram pode alimentar isso — há vídeos de operação disponíveis nos assets).
- **FAQ:** perguntas típicas (respinga nos vizinhos? faz barulho? precisa fechar a rua? funciona com chuva? que produtos usa?) não são respondidas.
- **Área de atendimento ambígua:** o footer diz "Atendemos todo o Brasil", mas o DDD 41 sugere Curitiba/PR — não há cidade/base declarada, o que enfraquece SEO local.
- **Sem captura de lead alternativa:** quem não está pronto para chamar no WhatsApp não tem o que fazer (ex.: baixar checklist, receber e-mail).
- **Instagram como prova:** os links sociais existem, mas nenhum conteúdo do Instagram é trazido para dentro da página.

---

## 7. Resumo executivo

| Dimensão | Avaliação |
|----------|-----------|
| Design/identidade | ★★★★☆ — identidade própria, técnica e memorável; widget interativo é o destaque |
| Código/técnica | ★★★★☆ — leve, acessível, sem dependências; falta menu mobile |
| SEO | ★★☆☆☆ — meta básico ok; sem og:image, JSON-LD, canonical, SEO local |
| Conteúdo/persuasão | ★★★☆☆ — proposta de valor e CTA fortes; zero prova social e zero imagem real |
| Conversão/medição | ★★☆☆☆ — funil claro para WhatsApp, mas nada é medido |

### Prioridades sugeridas (em ordem de impacto)
1. **Adicionar portfólio real** (antes/depois + vídeos de operação) com o novo material — resolve a maior fraqueza de credibilidade.
2. **Prova social** — depoimentos, clientes atendidos, números.
3. **`og:image` + JSON-LD + SEO local** (cidade-base + área de cobertura).
4. **Menu mobile** (hambúrguer) e favicon adequado.
5. **Medição de conversão** — eventos nos cliques de WhatsApp (GA4 ou similar).
6. **FAQ** respondendo objeções comuns.
