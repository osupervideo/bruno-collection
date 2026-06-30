# SuperVideo API Collection 🐶

Coleção de requisições oficiais do **Bruno** para integração com a API pública B2B do **SuperVideo**.

Esta coleção foi projetada para testes rápidos e integração com ferramentas de automação (como n8n, Make, ou integrações backend locais) utilizando autenticação simplificada por **API Key**.

---

## 🚀 Como Importar e Configurar

### 1. Importar no Bruno
1. Baixe e instale o [Bruno](https://www.usebruno.com/).
2. Na tela inicial, clique em **"Open Collection"** (Abrir Coleção).
3. Selecione a pasta raiz desta coleção (onde se encontra o arquivo `bruno.json`).

---

### 2. Configurar Variáveis de Ambiente
1. No canto superior direito do aplicativo, clique no menu suspenso de ambientes (*No Environment*).
2. Selecione o ambiente **development**.
3. Clique no ícone de engrenagem ao lado do nome do ambiente para configurar as seguintes variáveis:
   - `baseUrl`: A URL base da API do SuperVideo (ex: `http://localhost:3001` localmente ou sua URL de produção).
   - `apiKey`: Sua chave de API B2B.
     > 💡 **Dica**: Por padrão, as requisições da coleção enviam a chave via cabeçalho `X-API-Key`. Se você estiver integrando via WordPress ou proxies restritivos que barram headers customizados, a API também aceita a autenticação via padrão HTTP `Authorization: Bearer <SUA_CHAVE>`.

---

## ⛓️ Fluxo de Teste Sugerido

As requisições contam com scripts de pós-resposta configurados. Isso significa que as variáveis dinâmicas (como `roomId` ou `guestSocketId`) são preenchidas automaticamente a partir das respostas anteriores.

Execute na seguinte ordem:

1. **Configurar Webhook (`Set Webhook Config`):** Registre a URL do seu receptor de webhooks (ex: n8n listener).
2. **Criar Sala (`Create Room`):** Cria uma nova sala de vídeo. O `roomId` retornado será salvo e injetado nos próximos requests.
3. **Monitorar/Encerrar (`Get Room Status` / `End Room`):** Valide o estado em tempo real da videoconferência ou force o encerramento da chamada.
4. **Controle da Sala de Espera (Bypass / Admit / Deny):**
   - Quando um convidado pede para entrar, o webhook envia o evento `participant.knocking` com seu `guestSocketId`.
   - Você pode admitir (`Admit Guest`) ou rejeitar (`Deny Guest`) o convidado.
5. **Gravações e IA (`List Recordings` / `Get Recording Details`):** Liste ou consulte detalhes estruturados das transcrições, resumos automáticos e segmentação de speakers das reuniões.
