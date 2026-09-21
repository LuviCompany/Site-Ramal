# RamalVirtual no WordPress + Elementor (versão gratuita)

Esta pasta tem tudo o que é preciso para levar as duas páginas (Home e Funcionalidades) para o WordPress.

**Plugins usados:** Elementor (gratuito), Ultimate Addons for Elementor (header e footer), Fluent Forms (formulário). O Royal Addons é opcional, só se quiserem colar o CSS por ele.

```
wordpress/
  1-css-global.css            CSS de todo o site (colar uma única vez)
  imagens/                    todas as imagens, com nomes limpos (subir por FTP)
  blocos-header-footer/       header.html e footer.html (com o botão flutuante do WhatsApp)
  blocos-home/                8 blocos da Home, na ordem em que entram na página
  blocos-funcionalidades/     5 blocos da página Funcionalidades, na ordem
```

Todo bloco de HTML começa com um comentário dizendo em qual container ele entra.

---

## 1. Imagens (5 min)

Suba a pasta `imagens/` inteira, com as subpastas `clientes/` e `parceiros/`, para:

```
/wp-content/uploads/ramalvirtual/
```

Use FTP ou o Gerenciador de Arquivos da hospedagem. Os blocos já apontam para esse caminho, então **não precisa trocar nenhum link**.

Teste abrindo `https://SEU-SITE/wp-content/uploads/ramalvirtual/logo-header.png`. Se a imagem abrir, está certo.

> Se preferirem a Biblioteca de Mídia do WordPress, precisarão trocar os caminhos `/wp-content/uploads/ramalvirtual/...` nos blocos pelas URLs geradas por ela.

## 2. CSS global (2 min)

**Aparência → Personalizar → CSS adicional** → colar o conteúdo inteiro de `1-css-global.css` → Publicar.

- Todo o CSS fica dentro da classe `.rv`, então não interfere no tema nem no Elementor.
- A fonte Manrope já vem carregada pelo `@import` na primeira linha.
- Se usarem o CSS personalizado do Royal Addons, confirmem que ele carrega no site inteiro e não só em uma página.

## 3. Header e footer (10 min)

**Ultimate Addons → Header/Footer Builder → Adicionar novo**

| Tipo | Exibir em | Conteúdo |
|---|---|---|
| Cabeçalho | Site inteiro | Widget **HTML** com o conteúdo de `blocos-header-footer/header.html` |
| Rodapé | Site inteiro | Widget **HTML** com o conteúdo de `blocos-header-footer/footer.html` |

- O rodapé já inclui o botão flutuante do WhatsApp e o balão de boas-vindas.
- O link "Funcionalidades" fica em destaque sozinho quando a pessoa está nessa página.
- O CSS já deixa o header fixo no topo (`#masthead`). Se o tema também imprimir o header dele, ajustem em **Ultimate Addons → Configurações → compatibilidade de tema**.

## 4. Montar a página Home

1. Crie a página **Home** e defina em **Configurações → Leitura** como página inicial.
2. Editar com Elementor. Nas configurações da página: **Template = Elementor Largura Total** (não use "Canvas": ele esconde o header e o footer) e **Ocultar título**.
3. Monte os blocos abaixo, **nesta ordem**.

### Blocos 1 a 6 (só HTML)

Para cada um: **Adicionar container** com

- Largura do conteúdo: **Largura total**
- Direção: **Coluna**
- Avançado → Classes CSS: `rv-reset`

Dentro do container, coloque um widget **HTML** e cole o arquivo.

| # | Arquivo |
|---|---|
| 1 | `1-hero-e-faixa.html` |
| 2 | `2-problema.html` |
| 3 | `3-solucao.html` |
| 4 | `4-planos.html` |
| 5 | `5-como-funciona.html` |
| 6 | `6-logos-clientes-e-integracoes.html` |

A classe `rv-reset` zera o padding, o espaçamento e a largura máxima que o Elementor coloca por padrão. Se esquecerem dela, a seção vai aparecer com margens estranhas.

### Bloco 7: FAQ (nativo)

Container com:

- Largura total, Direção: **Coluna**
- Avançado → **ID CSS: `faq`**
- Avançado → **Classes CSS: `rv faq-section`** (não use `rv-reset` aqui)

Dentro dele, nesta ordem:

1. Widget **HTML** com `7-faq-titulo.html` (título da seção).
2. Widget **Sanfona** (o Acordeão do Elementor, cujos itens aparecem como "Item Nº 1", "Item Nº 2"...):
   - Avançado → Classes CSS: `faq-acc`
   - Conteúdo → **Itens**: 6 itens. Em cada um, o **Título** é a pergunta.
   - A **resposta** de cada item vai *dentro* do item: no painel Estrutura, abra o item, clique no container dele e adicione um widget **Editor de texto** com a resposta. Dica: monte o Item 1 completo e use **Duplicar** no item, depois só troque os textos.
   - Conteúdo → Interações: **um item aberto por vez** e estado padrão **todos recolhidos**.
   - Conteúdo → Ícone: use uma seta (fechado: seta para baixo; aberto: seta para cima). Posição e cores o CSS resolve, **não mexam na aba Estilo**.
   - Cadastre estas 6 perguntas:

| Pergunta | Resposta |
|---|---|
| O que eu preciso pra usar a RamalVirtual? | Basta internet estável e um dispositivo: computador, smartphone ou telefone IP. Nós configuramos a central em nuvem e entregamos os ramais prontos para uso. |
| Preciso trocar minha operadora? | Não. A RamalVirtual funciona com tudo incluso. Somos operadora e podemos portar seus números telefônicos, mesmo que seja um 0800. |
| A RamalVirtual é VoIP? | Sim, a tecnologia é VoIP em nuvem, com qualidade de áudio e recursos equivalentes aos de uma central tradicional, sem estrutura física. |
| Posso integrar com minha estrutura atual? | Sim. Integramos com sistemas administrativos, CRMs e equipamentos legados já utilizados pela sua empresa. |
| Posso usar meu ramal no celular? | Sim. O mesmo ramal acompanha você no aplicativo do celular, no computador e no telefone IP. |
| Como funciona o suporte? | Suporte especializado e humanizado por telefone, e-mail e WhatsApp, com prioridade de atendimento nos planos superiores. |

### Bloco 8: Contato (nativo)

Container externo com:

- Largura total, Direção: **Coluna**
- **ID CSS: `contato`**
- **Classes CSS: `rv contato`**

Dentro dele, um **segundo container** com Classes CSS `contato-inner` (Largura total, ou "Boxed": o CSS funciona nos dois casos), e dentro desse segundo container:

1. Widget **HTML** com `8-contato-texto.html` (textos da coluna esquerda).
2. Widget **Shortcode** com `[fluentform id="X"]` (o ID do formulário, veja a seção 6). **Crie o formulário antes**: enquanto o shortcode estiver vazio ou com ID inexistente, o Elementor mostra só uma barra cinza no lugar.

Resultado: texto à esquerda e formulário à direita no desktop; empilhados (texto em cima, formulário embaixo) no tablet e no celular.

## 5. Montar a página Funcionalidades

Crie a página com o endereço (slug) **`funcionalidades`**, template **Elementor Largura Total** e título oculto.

Os 5 blocos seguem a mesma receita dos blocos 1 a 6 da Home (container `rv-reset` + widget HTML), nesta ordem:

`1-hero.html` → `2-comparativo.html` → `3-aplicativos.html` → `4-inteligencia-artificial.html` → `5-integracoes-e-suporte.html`

Em **Configurações → Links permanentes**, escolha **Nome do post**. Isso dá o endereço `/funcionalidades` sem `.html`, como nos links do menu.

## 6. Formulário (Fluent Forms)

Crie um formulário em branco com estes campos (a ordem e o layout são os do site):

| Linha | Campo | Tipo | Obrigatório |
|---|---|---|---|
| 1 | Nome completo | Texto (placeholder "Seu nome") | Sim |
| 1 | Empresa | Texto (placeholder "Nome da empresa") | Sim |
| 2 | WhatsApp / Telefone | Texto simples (placeholder "(00) 00000-0000"); o campo "Telefone" é da versão Pro | Sim |
| 2 | E-mail | E-mail (placeholder "voce@empresa.com.br") | Sim |
| 3 | Número de ramais desejado | Lista suspensa | Sim |
| 4 | Mensagem (opcional) | Texto longo, 3 linhas | Não |
| 5 | Autorização LGPD | Caixa de seleção | Sim |
| 6 | Botão | Texto "Solicitar orçamento" | (botão) |

- **Campos lado a lado (importante para o tamanho do formulário):** nos **4 primeiros campos** (Nome, Empresa, WhatsApp, E-mail), clique no campo → **Personalização de entrada** → em *Opções avançadas* procure **Classe do contêiner** (Container Class) e escreva `rv-meia`. Isso deixa Nome e Empresa lado a lado, e WhatsApp e E-mail também, como no site. Nos outros campos, não coloque nada. No celular eles empilham sozinhos. Sem essa classe, todos os campos ficam empilhados e o formulário fica bem mais alto que o do site.
- **Lista de ramais:** "1 a 5 ramais", "6 a 15 ramais", "16 a 30 ramais", "31 a 100 ramais", "Mais de 100 ramais".
- **Texto do LGPD:** "Autorizo o contato da RamalVirtual Telecom e o tratamento dos meus dados conforme a LGPD.*"
- **Notificação por e-mail:** para `contato@ramalvirtual.com.br` (ajustem se o comercial usar outro e-mail).
- **Mensagem de sucesso:** "Recebemos seus dados. Um especialista entra em contato em breve."
- **Antispam:** deixem o honeypot ligado (é o padrão). Se houver spam, adicionem reCAPTCHA ou Cloudflare Turnstile.

> No site atual o formulário só valida no navegador e **não envia nada**. No WordPress ele passa a enviar de verdade e a guardar cada envio em **Fluent Forms → Entradas**.

## 7. Testes antes de publicar

- [ ] Header fixo ao rolar, menu mobile abrindo e fechando (largura menor que 860px).
- [ ] Todos os links do menu, do footer e os botões "Seja Parceiro" (abrem o WhatsApp com a mensagem pronta).
- [ ] Balão do WhatsApp aparece depois de ~2,5 s e não volta depois de fechado, na mesma sessão.
- [ ] FAQ: um item abre por vez. Formulário: enviar um teste e conferir o e-mail e a entrada.
- [ ] Sem barra de rolagem horizontal em 320, 375, 414, 768, 1024 e 1440 px.
- [ ] Página Funcionalidades: tabela comparativa no desktop, cards empilhados no celular, e o card "Chamada finalizada" animando ao rolar até ele.
- [ ] Se usarem plugin de cache, limpar o cache depois de colar o CSS e os blocos.

## 8. Onde a agência edita cada coisa depois

| O quê | Onde |
|---|---|
| Textos e imagens das seções | Widget HTML do bloco (editar o código, os textos ficam em texto puro) |
| Itens da tabela comparativa | No final de `2-comparativo.html`, na lista `COMPARE` |
| Perguntas do FAQ | Widget Sanfona (título = pergunta, Editor de texto dentro do item = resposta) |
| Campos e destino do formulário | Fluent Forms |
| Menu, footer, botão do WhatsApp | Templates do Ultimate Addons |
| Cores da marca | Início do CSS global (`--blue`, `--cyan`, `--ink`...) |
| Número do WhatsApp `551130900900` | Só em `header.html` (4 lugares) e `footer.html` (3 lugares). Procurar e trocar nos dois. |

**Regra de marca:** sempre "RamalVirtual" (uma palavra só), com artigo feminino ("a RamalVirtual").

## 9. Se algo sair diferente

| Sintoma | Causa provável |
|---|---|
| Seção com espaço em volta ou não ocupa a largura toda | Faltou `rv-reset` no container, ou ele não está em "Largura total" |
| Fundo azul do Contato sem cor / FAQ sem estilo | Faltou a classe `rv` junto de `contato` / `faq-section` |
| Textos do Contato invisíveis ou sobre um retângulo branco | Está com o CSS antigo. Cole de novo o `1-css-global.css` e recarregue o editor. |
| Formulário aparece embaixo do texto no desktop | Mesma causa acima (CSS antigo). Confirme também que o container interno tem a classe `contato-inner`. |
| Formulário sem estilo | O container externo do Contato precisa ter as classes `rv contato`. O CSS do formulário depende delas. |
| Header não fica fixo | Um ancestral do tema com `overflow` impede o `position:sticky`. Procurar por isso no CSS do tema. |
| Tudo com fonte diferente | O CSS global não carregou nessa página, ou um plugin de cache está servindo a versão antiga |
| Imagens quebradas | Confira o caminho `/wp-content/uploads/ramalvirtual/` e se as subpastas foram enviadas |

## Como isto foi validado

Antes de entregar, os blocos e o CSS foram testados localmente numa página que **simula um tema invasivo** (fonte serifada, títulos em caixa alta, margens em parágrafos, containers do Elementor com padding e gap padrão). Resultado:

- As medidas da Home ficaram idênticas às do site atual (posição e altura de cada seção, com diferença de 1 px em uma).
- Não houve rolagem horizontal na Home em 320, 375, 768, 1024 e 1440 px, nem em Funcionalidades em 320, 414, 834 e 1440 px.
- A tabela de 47 linhas, os cards do mobile, a animação da IA e o menu mobile funcionaram.

- O CSS da Sanfona foi testado com o CSS original do Elementor carregado na página (2 colunas, bordas, ícone à direita, abrir e fechar).

**O que ainda não foi testado num WordPress de verdade:** o Fluent Forms. O CSS dele foi escrito a partir da estrutura padrão do plugin. Na primeira montagem, pode ser preciso um ajuste fino de espaçamento nesse ponto.
