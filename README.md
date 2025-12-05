# Tá Certo Isso AI? - Fluxo de Automação n8n

## Como funciona o Fluxo (n8n)

O arquivo `n8n.json` contém a lógica completa do bot. O fluxo opera da seguinte maneira:

1.  **Recebimento de Mensagens**: O fluxo é acionado via Webhook (integrado à **Evolution API**) sempre que uma nova mensagem é recebida no WhatsApp.
2.  **Verificação de Contexto**: O sistema verifica se a mensagem foi enviada diretamente para o bot (privado) ou se ele foi mencionado em um grupo, garantindo que ele só responda quando solicitado.
3.  **Classificação de Conteúdo**: O sistema identifica automaticamente o tipo de mídia recebida:
    *   **Texto**: Analisado diretamente.
    *   **Áudio**: Transcrito para texto utilizando **OpenAI Whisper**.
    *   **Imagem**: Analisada via modelos de visão da **OpenAI**.
    *   **Vídeo**: Processado e analisado utilizando **Google Gemini**.
4.  **Análise de Veracidade**: O conteúdo extraído é enviado para nossa **API proprietária**, que utiliza modelos de IA (OpenAI GPT / Gemini), web scraping e técnicas de fact-checking para identificar inconsistências, tom alarmista ou informações falsas.
5.  **Resposta ao Usuário**: O resultado da análise é enviado de volta para o usuário no WhatsApp, em formato de texto ou áudio, mantendo a acessibilidade.

## Tecnologias e Integrações

Este workflow utiliza os seguintes nós e serviços principais:

*   **[n8n](https://n8n.io/)**: Orquestrador de automação.
*   **Evolution API**: Gateway para conexão com o WhatsApp.
*   **OpenAI API**:
    *   Modelo de Chat (GPT) para análise de texto e raciocínio.
    *   Whisper para transcrição de áudio.
    *   TTS (Text-to-Speech) para gerar respostas em áudio.
*   **Google Gemini (PaLM)**: Para análise multimodal de vídeos.
*   **HTTP Requests**: Para comunicação com APIs externas e serviços auxiliares.

## Configuração e Instalação

Para rodar este fluxo em sua própria instância do n8n:

1.  **Importar o Workflow**:
    *   Baixe o arquivo `n8n.json` deste repositório.
    *   No seu painel do n8n, vá em "Workflows" > "Import from File" e selecione o arquivo.

2.  **Configurar Credenciais**:
    Você precisará configurar as seguintes credenciais no n8n:
    *   `OpenAi Api`: Chave de API da OpenAI.
    *   `Google Gemini(PaLM) Api`: Chave de API do Google.
    *   `Evolution Api`: Credenciais de acesso à sua instância da Evolution API.

3.  **Ajustar Variáveis**:
    Verifique os nós de "Set" ou variáveis de ambiente para garantir que as URLs dos serviços (como a Evolution API) apontem para a sua infraestrutura correta.

## Privacidade e Dados

O projeto foi desenhado com foco em privacidade. O fluxo processa as mensagens para verificação, e a visão de longo prazo inclui a anonimização de dados para a construção de uma plataforma de analytics para pesquisadores, respeitando a LGPD e as boas práticas de segurança.