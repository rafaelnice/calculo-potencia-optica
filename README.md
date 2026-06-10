# Cálculo de Potência Óptica — Método Desbalanceado

Ferramenta web para cálculo de orçamento de potência (link budget) de redes FTTH/PON construídas pelo **método desbalanceado**, com suporte a topologia em árvore (ramais), sugestão automática de splitters, visualização interativa e exportação de relatórios.

> Aplicativo de página única (single-page). Funciona **offline**, em qualquer navegador moderno, sem instalação e sem servidor.

## Acesso

- **Online:** abra pelo GitHub Pages (veja a seção [Publicação](#publicação-github-pages)).
- **Local:** baixe o arquivo [`index.html`](index.html) e abra com duplo clique.

## Principais recursos

- **Cálculo em cascata** com fórmulas idênticas às de planilha, validadas casa a casa.
- **Topologia em árvore:** ramais alimentados por qualquer perna de qualquer caixa, herdando a potência automaticamente.
- **Distância por trecho** (entre caixas), com a distância acumulada até a OLT calculada sozinha.
- **Sugestão automática de splitters:** escolhe o desbalanceado ideal para o limite de atenção configurado.
- **Diagrama interativo** da rede, com zoom vetorial (arrastar, rolar, pinça no celular).
- **Status por caixa** (OK / Atenção / Crítico / Sem sinal) e barra de orçamento óptico.
- **Exportações:** PDF (relatório com diagrama), CSV (Excel) e JSON (salvar/abrir projeto).
- **Interface responsiva** (desktop, tablet e celular) com tema escuro.

## Nomenclatura das pernas

Como a rotulagem "P1/P2" varia entre fabricantes, o sistema usa nomes funcionais acompanhados da porcentagem da perna:

| Perna | Significado | Exemplo (splitter 25/75) |
|------|-------------|--------------------------|
| **Derivação** | Perna de menor porcentagem (maior perda). Atende a caixa local. | 25% |
| **Passagem** | Perna de maior porcentagem (menor perda). Segue para a próxima caixa. | 75% |

## Como usar

1. Configure os **parâmetros do enlace** (saída da OLT, atenuação da fibra, limites).
2. Monte o **tronco** adicionando caixas (CTO/CE) com a distância de cada trecho.
3. Crie **ramais** clicando em ⑂ na linha de uma caixa.
4. Use **Sugerir splitters** para otimizar e **Exportar PDF** para o relatório.

> **Importante:** o sistema não grava nada sozinho. Use **Exportar projeto** (.json) ao fim de cada sessão para salvar seu trabalho.

A documentação completa está na pasta [`docs/`](docs):

- 📘 **Manual do usuário** (Word) — guia completo com fórmulas para auditoria.
- 📊 **Apresentação** (PowerPoint) — visão geral do sistema.

## Publicação (Vercel)

O deploy é feito importando este repositório do GitHub. A cada novo commit, a Vercel republica o site automaticamente.

1. Suba este projeto para um repositório no GitHub.
2. Acesse [vercel.com](https://vercel.com) e entre com a conta do GitHub.
3. Clique em **Add New… → Project** e selecione **Import** no repositório.
4. Em **Framework Preset**, mantenha **Other** (é um site estático, sem build).
5. Deixe os campos de *Build Command* e *Output Directory* em branco e clique em **Deploy**.
6. Em instantes a Vercel gera a URL pública (ex.: `https://NOME-DO-PROJETO.vercel.app`).

Como o arquivo principal se chama `index.html` na raiz, ele abre automaticamente ao acessar a URL. O arquivo [`vercel.json`](vercel.json) já define as configurações do site (URLs limpas e cabeçalhos de segurança).

## Referência de cálculo

```
Chegada (1ª caixa do tronco) = OLT − dist × dB/km − perda inicial − perda por trecho
Chegada (demais)             = passagem anterior − trecho × dB/km − perda por trecho
Derivação                    = chegada − perda da derivação do Splitter 1
Passagem                     = chegada − perda da passagem do Splitter 1
Cliente                      = derivação − perda do Splitter 2 (atendimento)
```

## Tecnologia

HTML, CSS e JavaScript puro (vanilla), sem dependências ou build. Todo o estado vive na sessão do navegador; persistência via exportação/importação de JSON.

## Licença

Distribuído sob a licença MIT. Veja [`LICENSE`](LICENSE).
