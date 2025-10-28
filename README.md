# 🚀 Aula Prática: Criando um Servidor Express conectado à API da OpenAI

## 🧑‍🏫 Sobre a Aula

Nesta aula, você vai aprender a **criar um servidor Node.js com Express** e integrá-lo à **API da OpenAI**.  
Esse é o primeiro passo para sair do uso de automações visuais (como n8n) e passar a construir **suas próprias APIs inteligentes via código**.

Ao final, você terá uma API que:

- Recebe uma pergunta em JSON
- Envia essa pergunta à OpenAI
- Retorna uma resposta gerada por IA em tempo real ✨

---

## 📦 Estrutura do Projeto

```bash
express-openai-demo/
│
├── server.js          # Código principal do servidor Express
├── .env               # Variáveis de ambiente (chave da OpenAI, porta, etc.)
├── .env.example       # Exemplo do arquivo de ambiente
└── package.json       # Configurações e dependências do projeto
```

## ⚙️ 1. Preparando o Ambiente

### 🧰 Requisitos

- Node.js instalado (versão 18 ou superior)
- Uma conta na [OpenAI](https://platform.openai.com)
- Uma chave de API (`OPENAI_API_KEY`)

---

### 🔧 Passos iniciais

1. Crie uma nova pasta e acesse:

```bash
   mkdir express-openai-demo
   cd express-openai-demo
```

 2.Inicie o projeto Node.js:

```bash
npm init -y
```

 3.Instale as dependências:

```bash
npm install express openai dotenv
```

 4.Crie um arquivo .env na raiz:

```bash
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxx
PORT=3000
```

⚠️ Importante: nunca suba sua chave da OpenAI no GitHub. Use sempre o .env e adicione ele ao .gitignore.

⸻

🧠 2. Criando o Servidor Express

Crie o arquivo server.js com o conteúdo abaixo:

```bash
const express = require("express");
const dotenv = require("dotenv");
const OpenAI = require("openai");

dotenv.config();

const app = express();
app.use(express.json());

// Inicializa cliente da OpenAI
const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

// Rota de teste (GET)
app.get("/", (req, res) => {
  res.send("🚀 Servidor Express rodando com OpenAI!");
});

// Rota principal (POST)
app.post("/ask", async (req, res) => {
  const { question } = req.body;

  try {
    const completion = await openai.chat.completions.create({
      model: "gpt-4o-mini",
      messages: [
        { role: "system", content: "Você é um assistente técnico e conciso." },
        { role: "user", content: question },
      ],
    });

    res.json({
      response: completion.choices[0].message.content,
    });
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: "Erro ao conectar com a OpenAI" });
  }
});

app.listen(process.env.PORT || 3000, () => {
  console.log(`✅ Servidor rodando em http://localhost:${process.env.PORT || 3000}`);
});
```

---

🚀 3. Executando o Projeto

Inicie o servidor com:

```bash
node server.js
```

Se tudo estiver certo, o terminal mostrará:

```bash
✅ Servidor rodando em <http://localhost:3000>
```

Abra no navegador:
👉 <http://localhost:3000>

Você verá:

🚀 Servidor Express rodando com OpenAI!

⸻

🧪 4. Testando a API no Postman ou Terminal

Usando curl:

```bash
curl -X POST <http://localhost:3000/ask> \
-H "Content-Type: application/json" \
-d '{"question": "Explique o que é o Express.js"}'
```

Resposta esperada:

```bash
{
  "response": "Express.js é um framework minimalista para criar APIs e servidores com Node.js."
}
```

Se aparecer “Cannot POST /”, é porque você fez o POST na rota errada.
Certifique-se de enviar para /ask e não apenas /.

⸻

🧩 5. Conceitos Ensinados

Conceito Explicação
Express.js Framework Node.js para criar APIs e servidores
Rota (endpoint) Caminho que o servidor escuta e responde (ex: /ask)
Método HTTP Tipo de ação (GET, POST, PUT, DELETE)
Middleware express.json() Permite receber e interpretar dados em JSON
dotenv Carrega variáveis de ambiente de um arquivo .env
OpenAI SDK Biblioteca oficial para interagir com a API da OpenAI
req.body Acessa os dados enviados pelo cliente
res.json() Retorna dados em formato JSON para o cliente

⸻

💡 6. Criando Outras Rotas Inteligentes

🖼️ Gerar uma imagem

```bash
app.post("/image", async (req, res) => {
  const { prompt } = req.body;

  try {
    const image = await openai.images.generate({
      model: "gpt-image-1",
      prompt,
      size: "512x512",
    });

    res.json({ url: image.data[0].url });
  } catch (error) {
    res.status(500).json({ error: "Erro ao gerar imagem" });
  }
});
```

🌍 Traduzir texto

```bash
app.post("/translate", async (req, res) => {
  const { text, language } = req.body;

  try {
    const completion = await openai.chat.completions.create({
      model: "gpt-4o-mini",
      messages: [
        { role: "system", content: `Traduza o texto para ${language}.` },
        { role: "user", content: text },
      ],
    });

    res.json({ translation: completion.choices[0].message.content });
  } catch (error) {
    res.status(500).json({ error: "Erro ao traduzir o texto" });
  }
});
```

⸻

🧭 7. Desafio Final

Crie uma rota /summary que:
 • Receba um texto longo
 • Use a OpenAI para resumir esse texto
 • Retorne o resumo como resposta JSON

Exemplo de body:

```bash
{
  "text": "A inteligência artificial está revolucionando a forma como interagimos com dados..."
}
```

⸻

🔍 8. Resumo da Aula
 • Você aprendeu a criar um servidor Express
 • Criou rotas GET e POST
 • Entendeu como o servidor se comunica com a API da OpenAI
 • Testou sua própria API de IA local
 • Viu como expandir para novos endpoints inteligentes (imagem, tradução, resumo)

⸻

⸻

👨‍💻 Autor

Rodrigo Paiva
Formador e Engenheiro de Software
📍 França | 🇧🇷 Brasil
💡 Apaixonado por IA, programação e criação de produtos digitais inteligentes.

⸻

🧾 Esta aula foi desenvolvida para a formação em desenvolvimento com foco em automações inteligentes, mostrando na prática como transformar fluxos n8n em código real usando Node.js e Express.
