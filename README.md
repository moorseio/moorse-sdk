# Moorse SDK

O moorse sdk surge como solução ao problema que muitos desenvolvedores enfrentam de necessitar criar suas próprias requisições http à API da Moorse, com ele, torna-se possível enviar requests à nossa API de forma simplificada, sem a necessidade de configurar todos os requests do início ao fim.

## Sumário

1. [Início rápido](#1-inicio-rapido)
2. [Objeto moorse](#2-objeto-moorse)
    1. [Criação](#21-criação)
    2. [Atributos do construtor](#22-atributos-do-moorseobject)
3. [Métodos síncronos](#3-métodos-síncronos)
    1. [getToken](#31-gettoken)
    2. [setToken](#32-settoken)
    3. [getIntegrationId](#33-getIntegrationId)
    4. [setIntegrationId](#34-setIntegrationId)
4. [Métodos assíncronos](#4-métodos-assíncronos)
    1. [login](#41-login)
    2. [Métodos do whatsapp](#42-métodos-do-whatsapp)
        1. [sendText](#421-sendtext)
        2. [sendFile](#422-sendfile)
        3. [sendButtons](#423-sendbuttons)
        4. [sendListMenu](#424-sendlistmenu)
        5. [sendImage](#425-sendimage)
        6. [sendVideo](#426-sendvideo)
        7. [sendVoice](#427-sendvoice)
        8. [sendAudio](#428-sendaudio)
        9. [getCredits](#429-getcredits)
        10. [getIntegrationStatus](#4210-getintegrationstatus)
    3. [Métodos de webhook](#43-métodos-de-webhook)
        1. [getWebhookById](#431-getwebhookbyid)
        2. [createWebhook](#432-createwebhook)
        3. [deleteWebhook](#433-deletewebhook)
        4. [updateWebhook](#434-updatewebhook)
    4. [Métodos de template](#44-métodos-de-template)
        1. [createTemplate](#441-createtemplate)
        2. [getTemplates](#442-gettemplates)
        3. [getTemplateById](#443-gettemplatebyid)
        4. [deleteTemplateById](#444-deletetemplatebyid)
    5. [Métodos de faturamento](#45-métodos-de-faturamento)
        1. [totalMessages](#451-totalmessages)
        2. [totalMessagesPerChannel](#452-totalmessagesperchannel)
        3. [totalMessagesTimeline](#453-totalmessagestimeline)
    6. [Métodos de Sms](#46-métodos-de-sms)
        1. [sendSms](#461-sendsms)



## 1. Início rápido

Antes de tudo, inicie um projeto nodejs em typescript e instale o pacote da moorse nele utilizando o npm:

```bash
npm i moorseio
```

Caso esteja usando typescript, inclua o pacote da moorse no seu código, crie o objeto moorse e chame o método sendText para enviar uma mensagem

```typescript
import { Moorse } from "moorseio";

const moorse: Moorse = new Moorse({
    channelType: "whatsapp",
    integrationId: "some-moorse-integration-id",
    email: "sample.email@gmail.com",
    password: "strongpass123"
});
moorse.sendText({
    to: "5583996843637",
    body: "wow, it's a moorse message!"
}).then((answer)=>{
    console.log(answer);
});
```

Ao executar este trecho de código, o sdk usará seu email e senha para resgatar seu token da moorse e enviar uma mensagem ao número te telefone passado no método sendText().

## 2. Objeto moorse

O objeto Moorse é a parte central do SDK, nele estão guardados todas os métodos que futuramente servirão para consumir alguma rota da Moorse.

### 2.1. Criação

A criação se dá como a de qualquer outro objeto, cria-se uma instância chamando o construtor e passando todos os atributos necessários. 

```typescript
import { Moorse } from "moorseio";

const moorse: Moorse = new Moorse({
    channelType: "whatsapp",
    email: "seuemail@gmail.com",
    password: "senhaforte123"
});
```

### 2.2. Atributos do MoorseObject

Para o construtor da moorse usa-se apenas um objeto chamado de **MoorseObject** nele, podem/devem ser definidos os seguintes atributos:

| atributo      |  tipo   | obrigatório? | descrição |
|---------------|---------|--------------|-----------|
| channelType   | string  | sim          |tipo de canal de comunicação desejado (apenas **whatsapp** é suportado atualmente)|
| integrationId | string  | não          |id de uma integração sua da moorse|
| email         | string  | não          |email do usuário cadastrado no site da moorse|
| password      | string  | não          | senha do usuário cadastrado no site da moorse|
| token         | string  | não          | token do usuário cadastrado no site da moorse (pode ser encontrado no dashboard do site)
| onLogin       | função  | não          | uma função que é executada quando usuário da moorse for logado (isso só acontece caso seja passado apenas email e senha e não o token no **MoorseObject** )



## 3. Métodos síncronos

### 3.1. getToken

**Definição**:
```typescript
getToken(): string
```
**Exemplo**: 
```typescript
moorse.getToken();
```
**Retorno**: `string`

### 3.2. setToken

**Definição**:
```typescript
setToken(token: string): void
```
**Exemplo**:
```typescript
moorse.setToken("novo-token-desejado");
```
**Retorno**: `void`

### 3.3. getIntegrationId

**Definição**: 
```typescript
getIntegrationId(): string
```
**Exemplo**: 
```typescript
moorse.getIntegrationId();
```

**Retorno**: `string`

### 3.4. setIntegrationId

**Definição**:
```typescript
setIntegrationId(integrationId: string): void
```
**Exemplo**:
```typescript
moorse.setTokenIntegrationId("novo-integrationid-desejado");
```
**Retorno**: `void`

## 4. Métodos assíncronos

### 4.1. login

**Definição**:
```typescript
login(email: string, password: string): Promise<MoorseResponse>
```
**Exemplo**:
```typescript
moorse.login("exemplo@mail.com", "senhaforte").then((data: any)=>{
    console.log(data);
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

### 4.2. Métodos do whatsapp

#### 4.2.1. sendText

**Definição**:
```typescript
sendText(args: SendTextObject): Promise<MoorseResponse>
```
**SendTextObject**
```typescript
type SendTextObject ={
    to: string;
    body: string;
    integrationId?: string;
}
```
**Exemplo**:
```typescript
moorse.sendText({
    to: "655645198411"
    body: "olá"
});
moorse.sendText({
    to: "655645198411"
    body: "olá",
    integrationId: "54198498-498-4984984-984"
});
```
**Retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.2.2. sendFile

**Definição**:
```typescript
sendFile(args: SendFileObject): Promise<MoorseResponse>
```

**SendFileObject**

```typescript
type SendFileObject = {
    to: string;
    body: string;
    filename: string;
    caption: string;
    integrationId?: string;
};
```

**Exemplo**:
```typescript
moorse.sendFile({
    to: "6485456184",
    body: base64EncodedImage,
    filename: "gatinho.png"
    caption: "olha essa foto!"
});
```
**Objeto do retorno**:
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.2.3. sendButtons

**Definição**:
```typescript
sendButtons(args: SendButtonsObject): Promise<MoorseResponse>
```
**SendButtonsObject**
```typescript
type SendButtonsObject = {
    buttons: string[],
    title: string,
    subtitle: string,
    to: string
    integrationId?: string;
}
```
**Exemplo**:
```typescript
moorse.sendButtons({
    buttons: ["a", "b", "c"],
    title: "titulo do botão",
    subtitle: "subtitulo do botão",
    to: "54954196852",
    integrationId: "6548541-854-84984"
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.2.4. sendListMenu

**Definição**:
```typescript
sendListMenu(args: SendListMenuObject): Promise<MoorseResponse>
```
**SendListMenuObject**
```typescript
type SendListMenuObject = {
    integrationId?: string,
    to: string,
    body: string,
    action: {
        sections: {
            title: string,
            rows: {
                id: string;
                title: string;
            }[],
        }[],
    }
};
```

**Exemplo**:
```typescript
moorse.sendListMenu({
  to: "5511999999999",
  body: "Olá 🙂\nQue bom que você chegou!\n\nAqui no Zap Americanas temos ofertas incríveis e personalizadas pra você ❤️",
  action: {
    sections: [
      {
        title: "OFERTA",
        rows: [
          {
            id: "MEUS_CUPONS",
            title: "Meus cupons"
          },
          {
            id: "PRODUTOS_ALTA",
            title: "Produtos em alta"
          }
        ]
      },
      {
        title: "OUTROS",
        rows: [
          {
            id: "SUA_OPNIAO",
            title: "Dê sua opnião"
          },
          {
            id: "FALAR_PESSOA",
            title: "Falar com uma pessoa"
          }
        ]
      }
    ]
  }
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.2.5. sendImage

**Definição**:
```typescript
sendImage(args: SendImageObject): Promise<MoorseResponse>
```
**SendImageObject**
```typescript
type SendImageObject = {
    to: string;
    body: string;
    filename: string;
    caption: string;
    integrationId?: string;
};
```
**Exemplo**:
```typescript
moorse.sendImage({
    to: "6168546518",
    body: base64EncodedAudio,
    filename: "imagem.jpg",
    caption: "teste",
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.2.6. sendVideo

**Definição**:
```typescript
sendVideo(args: SendVideoObject): Promise<MoorseResponse>
```
**SendVideoObject**
```typescript
type SendVideoObject = {
    to: string;
    body: string;
    filename: string;
    caption: string;
    integrationId?: string;
};
```
**Exemplo**:
```typescript
moorse.sendVideo({
    to: "6168546518",
    body: base64EncodedVideo,
    filename: "video.mp4",
    caption: "teste",
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.2.7. sendVoice

**Definição**:
```typescript
sendVoice(args: SendVoiceObject): Promise<MoorseResponse>
```
**SendVoiceObject**

```typescript
type SendVoiceObject = {
    to: string;
    body: string;
    filename: string;
    caption: string;
    integrationId?: string;
};
```
**Exemplo**:
```typescript
moorse.sendVoice({
    to: "6168546518",
    body: base64EncodedVoice,
    filename: "voice.mp3",
    caption: "teste",
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.2.8. sendAudio

**Definição**:
```typescript
sendAudio(args: SendAudioObject): Promise<MoorseResponse>
```
**SendAudioObject**

```typescript
type SendAudioObject = {
    to: string;
    body: string;
    filename: string;
    caption: string;
    integrationId?: string;
};
```
**Exemplo**:
```typescript
moorse.sendAudio({
    to: "6168546518",
    body: base64EncodedAudio,
    filename: "audio.mp3",
    caption: "teste",
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.2.9. getCredits

**Definição**:
```typescript
getCredits(argsIntegrationId?: string):Promise<MoorseResponse>
```
**Exemplo**:
```typescript
moorse.getCredits();
moorse.getCredits("16541659814-87451984-56165496");
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.2.10. getIntegrationStatus

**Definição**:
```typescript
getIntegrationStatus(argsIntegrationId: string): Promise<MoorseResponse>
```
**Exemplo**:
```typescript
moorse.getIntegrationStatus();
moorse.getIntegrationStatus("16541659814-87451984-56165496");
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

### 4.3. Métodos de webhook

#### 4.3.1. getWebhookById

**Definição**:
```typescript
getWebhookById(webhookId: string): Promise<MoorseResponse>
```
**Exemplo**:
```typescript
moorse.getWebhookById("webhook-id");
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.3.2. createWebhook

**Definição**:
```typescript
createWebhook(args: MoorseWebhookConfig): Promise<MoorseResponse>
```
**MoorseWebhookConfig**
```typescript
type MoorseWebhookConfig = {
    name: string,
    url: string,
    method: string,
    active: boolean,
    integrations?: string[],
    headers?: {
        key: string,
        value: string
    }[],
    answered: boolean,
    readed: boolean,
    received: boolean,
    sended: boolean,
    retries: number,
    timeout: number
}
```

**Exemplo**:
```typescript
moorse.createWebhook({
    name: "webhook teste",
    url: "www.moorse.com",
    method: "POST",
    active: true,
    answered: false,
    readed: false,
    received: true,
    sended: false,
    retries: 2,
    timeout: 30
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.3.3. deleteWebhook

**Definição**:
```typescript
deleteWebhook(webhookId: string): Promise<MoorseResponse>
```
**Exemplo**:
```typescript
moorse.deleteWebhook("webhook-id");
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.3.4. updateWebhook

**Definição**:
```typescript
updateWebhook(webhookId: string, args: MoorseWebhookConfig): Promise<MoorseResponse>
```
**MoorseWebhookConfig**
```typescript
type MoorseWebhookConfig = {
    name: string,
    url: string,
    method: string,
    active: boolean,
    integrations?: string[],
    headers?: {
        key: string,
        value: string
    }[],
    answered: boolean,
    readed: boolean,
    received: boolean,
    sended: boolean,
    retries: number,
    timeout: number
}
```

**Exemplo**:
```typescript
moorse.update("webhook-id",{
    name: "webhook teste",
    url: "www.moorse.com",
    method: "POST",
    active: true,
    answered: false,
    readed: false,
    received: true,
    sended: false,
    retries: 2,
    timeout: 30
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

### 4.4. Métodos de template

#### 4.4.1. createTemplate

**Definição**:
```typescript
createTemplate(args: CreateTemplate): Promise<MoorseResponse>
```

**CreateTemplate**
```typescript
type CreateTemplate = {
    name: string,
    description?: string,
    type: string,
    integrationId: string,
    category: string,
    language: string,
    components: {
        type: string,
        text?: string,
        format?: string,
        example?: any,
        buttons?: string[]
    }[]
}
```

**Exemplo**:
```typescript
moorse.createTemplate({
    name: "template teste",
    type: "STANDARD",
    integrationId: "id-da-integração"
    category: "ACCOUNT_UPDATE",
    language: "pt_BR",
    components: [
        {
            type: "BODY",
            text: "teste"
        }
    ]
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.4.2. getTemplates

**Definição**:
```typescript
getTemplates(args?: GetTemplatesQueryParams): Promise<MoorseResponse>
```
**GetTemplatesQueryParams**
```typescript
type GetTemplatesQueryParams = {
    category?: string,
    clientId?: string, 
    integrationId?: string,
    name?: string,
    status?: string,
    type?: string
}
```
**Exemplo**:
```typescript
moorse.getTemplates();
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.4.3. getTemplateById

**Definição**:
```typescript
getTemplateById(templateId: string): Promise<MoorseResponse>
```
**Exemplo**:
```typescript
moorse.getTemplateById("id-do-template");
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.4.4. deleteTemplateById

**Definição**:
```typescript
deleteTemplateById(templateId: string): Promise<MoorseResponse>
```
**Exemplo**:
```typescript
moorse.deleteTemplateById("id-do-template");
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

### 4.5. Métodos de faturamento

#### 4.5.1 totalMessages

**Definição**:
```typescript
totalMessages(args: TotalMessagesObject): Promise<MoorseResponse>
```

**TotalMessagesObject**
```typescript
type TotalMessagesObject = {
    beginDate: string;
    endDate: string;
};
```

**Exemplo**:
```typescript
moorse.totalMessages({
    beginDate: "2021-06-01",
    endDate: "2021-07-01"
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.5.2 totalMessagesPerChannel

**Definição**:
```typescript
totalMessagesPerChannel(args: TotalMessagesPerChannelObject): Promise<MoorseResponse>
```
**TotalMessagesPerChannelObject**
```typescript
type TotalMessagesPerChannelObject = {
    beginDate: string;
    endDate: string;
};
```
**Exemplo**:
```typescript
moorse.totalMessagesPerChannel({
    beginDate: "2021-06-01",
    endDate: "2021-07-01"
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

#### 4.5.3 totalMessagesTimeline

**Definição**:
```typescript
totalMessagesTimeline(args: TotalMessagesTimelineObject): Promise<MoorseResponse>
```
**TotalMessagesTimelineObject**
```typescript
type TotalMessagesTimelineObject = {
    beginDate: string;
    endDate: string;
};
```
**Exemplo**:
```typescript
moorse.totalMessagesTimeline({
    beginDate: "2021-06-01",
    endDate: "2021-07-01"
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```

### 4.6. Métodos de Sms

#### 4.6.1 sendSms

**Definição**:
```typescript
sendSms(args: SendTextObject): Promise<MoorseResponse>
```
**SendTextObject**
```typescript
type SendTextObject ={
    to: string;
    body: string;
    integrationId?: string;
}
```
**Exemplo**:
```typescript
moorse.sendSms({
    to: "16985498456",
    body: "olá"
});
```
**Objeto do retorno**: 
```typescript
{
    data: any
    errors: any
    links: any
}
```