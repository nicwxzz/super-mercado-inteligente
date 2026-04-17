# Sistema de supermercado

## Sobre o Projeto

Interface de PDV que simula o consumo de uma API REST Java no backend. O projeto foi construído com foco em **boas práticas reais de desenvolvimento frontend**: estado centralizado, zero handlers inline, manipulação de DOM segura e aritmética financeira correta.

---

## Funcionalidades

- Catálogo de produtos com cards clicáveis
- Carrinho com adição e remoção de itens
- Cálculo de total em tempo real
- Modal de pagamento com validação de valor
- Cálculo e composição do troco (notas e moedas)
- Comprovante de venda ao finalizar
- Layout responsivo (mobile e desktop)

---

## Decisões Técnicas

| Decisão | Por quê |
|---|---|
| Preços em **centavos (inteiros)** | Evita erros de ponto flutuante (`0.1 + 0.2 ≠ 0.3` em JS) |
| **Store pub/sub** centralizado | Estado previsível, sem variáveis globais soltas |
| **`createElement`** em vez de `innerHTML` | Elimina risco de XSS com dados dinâmicos |
| **Event delegation** no catálogo e carrinho | Um único listener para N elementos — mais eficiente |
| **`DocumentFragment`** na renderização | Insere o DOM em uma operação só, evitando reflows |
| Zero **handlers inline** no HTML | Separação clara entre estrutura e comportamento |

---

## Arquitetura do Código

```
supermercado.html
│
├── DADOS
│   ├── PRODUCTS[]          → Catálogo (priceCents: inteiros)
│   └── DENOMINATIONS[]     → Notas e moedas para o troco
│
├── STORE                   → Estado centralizado (pub/sub)
│   ├── addItem(productId)
│   ├── removeItem(cartItemId)
│   ├── clearCart()
│   └── getTotalCents()
│
├── LÓGICA DE NEGÓCIO       → Funções puras, sem tocar no DOM
│   ├── formatBRL(cents)
│   ├── parseCents(rawValue)
│   └── calcChange(total, received)
│
├── COMPONENTES DE RENDERIZAÇÃO
│   ├── Catalog             → Monta o catálogo (executado uma vez)
│   ├── CartView            → Re-renderiza o carrinho a cada mudança
│   ├── Modal               → Controla abertura, validação e pagamento
│   └── ReceiptView         → Renderiza o comprovante de venda
│
├── bindEvents()            → Todos os event listeners centralizados
└── App.init()              → Bootstrap da aplicação
```

---

## Como Executar

Abra o VsCode, abra um novo terminal e siga o passo a passo logo abaixo:

```bash
# Clone o repositório
git clone https://github.com/nicwxzz/super-mercado-inteligente

# Entre na pasta
cd super-mercado-inteligente

# Abra o arquivo (ou dê dois cliques nele)

Para Windows:
open supermercado.html

Para macOS:
open supermercado.html

Para Linux:
xdg-open supermercado.html

```

> Compatível com qualquer navegador moderno (Chrome, Firefox, Edge, Safari).

---

## Integração com Backend Java

O frontend está estruturado para consumir uma API REST. Para conectar ao backend, substitua o array `PRODUCTS` por uma chamada real:

```js
// Antes (mock local)
const PRODUCTS = [ { id: 1, name: 'Arroz', priceCents: 1250, icon: '🌾' }, ... ];

// Depois (API Java / Spring Boot)
const PRODUCTS = await fetch('http://localhost:8080/api/produtos')
  .then(res => res.json());
```

A estrutura esperada da resposta JSON:

```json
[
  { "id": 1, "name": "Arroz", "priceCents": 1250, "icon": "🌾" }
]
```

---

## 🔭 Próximos Passos

- [ ] Conectar à API Java (Spring Boot)
- [ ] Busca e filtragem de produtos
- [ ] Suporte a quantidades por item no carrinho
- [ ] Histórico de vendas
- [ ] Autenticação de operador de caixa
- [ ] Impressão de comprovante

---

## 🛠️ Tecnologias

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

## 📁 Estrutura do Repositório

```
supermarket-system/
├── supermercado.html   ← aplicação completa (frontend)
└── README.md
```
