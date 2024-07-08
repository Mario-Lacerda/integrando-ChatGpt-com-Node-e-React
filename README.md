# Desafio Dio - Integrando o ChatGPT com Node e React



**Projeto ainda mais abrangente com mais códigos para integrar o ChatGPT com Node.js e React:**

**Introdução**

Neste projeto, construiremos uma aplicação de chat completa que usa o ChatGPT para gerar respostas a perguntas dos usuários. A aplicação será construída usando Node.js, React, Socket.IO e MongoDB.



### **Pré-requisitos**

- Conta da OpenAI
- Node.js 14 ou superior
- npm 6 ou superior
- MongoDB



### **Configuração**



1. #### Crie um novo projeto Node.js:

   plaintext

   

   #### npm init -y

   

2. #### Instale as seguintes dependências:

   plaintext

   

   ```plaintext
   npm install express socket.io body-parser mongoose
   ```

   

3. #### Crie um arquivo `server.js` com o seguinte código:

   javascript

   

   ```javascript
   const express = require('express')
   const socketIO = require('socket.io')
   const bodyParser = require('body-parser')
   const mongoose = require('mongoose')
   const fetch = require('node-fetch')
   
   const app = express()
   
   app.use(bodyParser.json())
   
   mongoose.connect('mongodb://localhost:27017/chat', {
     useNewUrlParser: true,
     useUnifiedTopology: true
   })
   
   const MessageSchema = new mongoose.Schema({
     prompt: String,
     response: String
   })
   
   const Message = mongoose.model('Message', MessageSchema)
   
   const server = app.listen(3000, () => {
     console.log('Servidor rodando na porta 3000')
   })
   
   const io = socketIO(server)
   
   io.on('connection', (socket) => {
     console.log('Novo usuário conectado')
   
     socket.on('chat message', async (prompt) => {
       const response = await fetch('https://generativelanguage.googleapis.com/v1beta2/models/chat-bison-001:generateMessage?key={{API_KEY}}', {
         method: 'POST',
         headers: {
           'Content-Type': 'application/json'
         },
         body: JSON.stringify({
           input: {
             text: prompt
           }
         })
       })
   
       const data = await response.json()
   
       const message = new Message({
         prompt: prompt,
         response: data.candidates[0].content
       })
   
       message.save()
   
       io.emit('chat message', data.candidates[0].content)
     })
   })
   ```

   

4. #### Crie um novo projeto React:

   plaintext

   

   ```plaintext
   npx create-react-app chat
   ```

   

5. #### No diretório do projeto React, instale as dependências `socket.io-client` e `axios`:

   plaintext

   

   ```plaintext
   npm install socket.io-client axios
   ```

   

6. #### No arquivo `App.js`, substitua o código existente pelo seguinte:

   javascript

   

   ```javascript
   import React, { useState, useEffect } from 'react'
   import socketIOClient from 'socket.io-client'
   import axios from 'axios'
   
   const App = () => {
     const [prompt, setPrompt] = useState('')
     const [response, setResponse] = useState('')
   
     useEffect(() => {
       const socket = socketIOClient('http://localhost:3000')
   
       socket.on('chat message', (msg) => {
         setResponse(msg)
       })
   
       return () => {
         socket.disconnect()
       }
     }, [])
   
     const handleSubmit = async (e) => {
       e.preventDefault()
   
       const res = await axios.post('http://localhost:3000/chat', { prompt })
   
       setResponse(res.data.response)
     }
   
     return (
       <div>
         <h1>ChatGPT</h1>
         <form onSubmit={handleSubmit}>
           <label htmlFor="prompt">Pergunta:</label>
           <input type="text" id="prompt" value={prompt} onChange={(e) => setPrompt(e.target.value)} />
           <button type="submit">Enviar</button>
         </form>
         <div>
           {response && <p>{response}</p>}
         </div>
       </div>
     )
   }
   
   export default App
   ```

   

7. Rode o servidor Node.js:

   plaintext

   

   ```plaintext
   node server.js
   ```

   

8. #### Rode a aplicação React:

   plaintext

   

   ```plaintext
   cd chat
   npm start
   ```



### **Uso**

Abra o navegador e vá para `http://localhost:3000`. Você verá um formulário onde poderá digitar uma pergunta para o ChatGPT. Clique no botão "Enviar" para enviar sua pergunta. O ChatGPT irá gerar uma resposta e exibi-la na página.



## **Conclusão**

Este é um projeto abrangente que demonstra como integrar o ChatGPT com Node.js, React, Socket.IO e MongoDB. Este projeto é mais abrangente do que os anteriores porque usa o Socket.IO para comunicação em tempo real entre o servidor e o cliente, e o MongoDB para persistência de dados. Você pode expandir este projeto adicionando recursos como autenticação do usuário, persistência de dados e muito mais.



## Observação:   



### Guia Resumido:



criar um projeto completo que integra o **ChatGPT** com **Node.js** e **React**. Abaixo, apresento algumas opções e recursos para você explorar:



1. #### **Repositório do Curso Dio.me**:

   - Existe um repositório no GitHub chamado “[Integrando o ChatGPT com Node e React](https://github.com/EriAssunPereira/Integrando-o-ChatGPT-com-Node-e-React)” que aborda exatamente esse desafio.

   - Ele oferece uma base para integrar o ChatGPT com Node.js (usando Express) e React. [Você pode explorar o código-fonte e seguir os passos para entender como a integração foi realizada](https://github.com/EriAssunPereira/Integrando-o-ChatGPT-com-Node-e-React)[1](https://github.com/EriAssunPereira/Integrando-o-ChatGPT-com-Node-e-React).

     

2. #### **Tutorial Kinsta**:

   - O blog da Kinsta publicou um tutorial detalhado sobre como construir e implantar um aplicativo clone do ChatGPT com React e a API da OpenAI.

   - [Esse tutorial abrange desde a criação do aplicativo até a implantação na plataforma de Hospedagem de Aplicativos da Kinsta](https://github.com/hfbricio10/integrando-chat-gpt-node-react)[2](https://github.com/hfbricio10/integrando-chat-gpt-node-react).

     

3. #### **Guia Sorfy**:

   - O guia “Desencadeando a Magia Conversacional: Integrando o ChatGPT com React.js e Node.js” explora a integração do ChatGPT com React.js no frontend e Node.js no backend.

   - [Ele pode fornecer insights adicionais sobre como conectar essas tecnologias](https://github.com/cayoesn/dio-lab-node-react-chatgpt)[3](https://github.com/cayoesn/dio-lab-node-react-chatgpt).

     

4. #### **Biblioteca JavaScript GPT-3**:

   - Se você deseja integrar o ChatGPT em um aplicativo React Native, pode usar a biblioteca JavaScript GPT-3.
   - Essa biblioteca permite que você acesse facilmente a API GPT-3 de dentro do seu código JavaScript.
