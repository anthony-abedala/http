# Relatório — Laboratório de Inspeção HTTP/HTTPS — Fluxo B (sem privilégio administrativo)

> **Como usar este template:** substitua os campos `[...]` pelas suas respostas,
> anexe as capturas de tela na pasta `evidencias/` e referencie-as onde indicado.
> Preserve a formatação markdown (tabelas, blocos de código) para facilitar a correção.
>
> **Observação:** toda a análise prática deste relatório é feita sobre tráfego **HTTP em texto claro**. A análise de HTTPS é teórica, baseada na fundamentação do `readme.md` do repositório.

---

## Identificação

| Campo       | Valor                  |
|-------------|------------------------|
| Nome        | [Anthony Abedala]    |
| RA          | [0050482411036]               |
| Disciplina  | Redes de Computadores  |
| Turma       | [Manhã]            |
| Data        | [09/05/2026]   |
| Fluxo       | **B — Aluno sem privilégio de administrador** |
| SO utilizado | [Windows 11] |
| Ferramenta de proxy | [Fiddler Classic per-user] |
| Navegador(es)       | [Chrome 124 / Firefox 125 / ...] |
| HTTPS-First Mode / HTTPS-Only desabilitado? | [sim] |

---

## Atividade 1 — Primeira captura (`http://example.com`)

<img width="886" height="468" alt="image" src="https://github.com/user-attachments/assets/c265f040-c85c-4b7c-80c7-4f1f1d3471af" />


**Request-line enviada:**

```http
[GET http://wholefunbeautifulmelody.neverssl.com/online/ HTTP/1.1]
```

**Status-line recebida:**

```http
[HTTP/1.1 200 OK]
```

### Pergunta 1.1
> Quantos cabeçalhos o navegador enviou no request? Liste-os.

**Resposta:**
[7]

Cabeçalhos:
- [Host:]
- [Connection:]
- [Upgrade-Insecure-Requests:]
- [User-Agent:]
- [Accept:]
- [Accept-Encoding:]
- [Accept-Language:]

### Pergunta 1.2
> Qual foi o `Content-Length` da resposta? Se ele não apareceu, registre `Transfer-Encoding`, versão do protocolo ou outro indício observado. O corpo retornado é HTML, texto puro, JSON ou binário? Como você descobriu?

**Resposta:** [Content-Length: 1173. Content-Type: text/html; O Corpo retornado é Html]

---

## Atividade 2 — Anatomia de um GET (`http://httpbin.org/get?...`)

<img width="886" height="497" alt="image" src="https://github.com/user-attachments/assets/26fcc229-cad4-4c69-960c-365df6e493a7" />


**Request-line completa:**

```http
[GET http://httpbin.org/get?aluno=ANTHONY_ABEDALA&curso=redes HTTP/1.1]
```

**Cabeçalhos-chave capturados:**

| Cabeçalho    | Valor                    |
|--------------|--------------------------|
| `Host`       | [httpbin.org]                    |
| `User-Agent` | [Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36]                    |
| `Accept`     | [text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7]                    |

**Campos do JSON de resposta:**

```json
{
  "args": {
    "aluno": "ANTHONY_ABEDALA", 
    "curso": "redes"
  }, 
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7", 
    "Accept-Encoding": "gzip, deflate", 
    "Accept-Language": "pt-BR,pt;q=0.9,en-US;q=0.8,en;q=0.7", 
    "Host": "httpbin.org", 
    "Upgrade-Insecure-Requests": "1", 
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36", 
    "X-Amzn-Trace-Id": "Root=1-69ff3847-30f396a37a442b57537bfaae"
  }, 
  "origin": "187.58.19.47", 
  "url": "http://httpbin.org/get?aluno=ANTHONY_ABEDALA&curso=redes"
}
```

### Pergunta 2.1
> O valor do campo `origin` corresponde a qual elemento da rede? Por que normalmente não é o IP local?

**Resposta:** [Corresponde ao IP. Porque é trafego passa pelo roteador]

### Pergunta 2.2
> Compare o `User-Agent` enviado com o que aparece no JSON da resposta. Coincidem?

**Resposta:** [Sim. O agent user enviado pelo navegador coincide com valor retornado pelo servidor no JSON]

### Pergunta 2.3
> Em `http://httpbin.org/headers`, liste até três cabeçalhos que o servidor vê mas **não aparecem** no Raw do request. De onde vêm? Se não encontrar três, explique por que o resultado pode variar.

**Resposta:**

| Cabeçalho visto pelo servidor | Origem provável | Observação |
|-------------------------------|-----------------|------------|
| [Host]                        | [Servidor]      | [...]      |
| [Upgrade-Insecure-Requests]   | [Servidor]      | [...]      |
| [User-Agent]                  | [Servidor]      | [...]      |

---

## Atividade 3 — POST e envio de formulário (`http://httpbin.org/forms/post` → `/post`)

<img width="886" height="643" alt="image" src="https://github.com/user-attachments/assets/18a5a21d-a55e-4a09-a0e2-700d8779c199" />


**Request-line do POST:**

```http
[POST http://httpbin.org/post HTTP/1.1]
```

**Cabeçalhos do request:**

| Cabeçalho        | Valor |
|------------------|-------|
| `Content-Type`   | [application/json] |
| `Content-Length` | [1170] |

**Corpo completo do request:**

```
[custname=Cleber&custtel=11+40028922&custemail=yudi%40play2.com&size=large&topping=bacon&topping=cheese&topping=onion&topping=mushroom&delivery=11%3A00&comments=Quebre+a+porta]
```

**Trecho do JSON de resposta (campo `form`):**

```json
"form": {
    "comments": "Quebre a porta", 
    "custemail": "yudi@play2.com", 
    "custname": "Cleber", 
    "custtel": "11 40028922", 
    "delivery": "11:00", 
    "size": "large", 
    "topping": [
      "bacon", 
      "cheese", 
      "onion", 
      "mushroom"
    ]
  }, 
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7", 
    "Accept-Encoding": "gzip, deflate", 
    "Accept-Language": "pt-BR,pt;q=0.9,en-US;q=0.8,en;q=0.7", 
    "Cache-Control": "max-age=0", 
    "Content-Length": "174", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "Origin": "http://httpbin.org", 
    "Referer": "http://httpbin.org/forms/post", 
    "Upgrade-Insecure-Requests": "1", 
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36", 
    "X-Amzn-Trace-Id": "Root=1-69ff3aee-0588a12b12fea29a619a52de"
  }
```

### Pergunta 3.1
> Qual o formato do corpo? Como esse formato codifica caracteres especiais (espaço, acentos)?

**Resposta:** [application/x-www-form-urlencoded. Espaços sao codificados como '+' ou '%20' e caracteres especiais sao codificados em URL enconding]

### Pergunta 3.2
> Comparando **Request → WebForms** e **Request → Raw**: qual das duas corresponde literalmente aos bytes enviados no socket TCP?

**Resposta:** [Aba raw corresponde literalmente aos bytes enviados pelo socket TCP]

### Pergunta 3.3 — Composer
> Envie manualmente via Composer um `POST` para `http://httpbin.org/post` com JSON. Registre a resposta. Qual campo do JSON confirma que o servidor interpretou o JSON?

<img width="469" height="358" alt="image" src="https://github.com/user-attachments/assets/2a68d73d-b9d9-4f50-91a5-9f09ecc97b62" />


**Response JSON (trecho relevante):**

```json
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Content-Length": "0", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "Fiddler", 
    "X-Amzn-Trace-Id": "Root=1-69ff3d81-377d6c9604269e552bb69073"
  }, 
  "json": null, 
  "origin": "187.58.19.47", 
  "url": "http://httpbin.org/post
}
```

**Resposta:** [...]

---

## Atividade 4 — Catálogo de status codes (`http://httpbin.org/...`)

<img width="381" height="511" alt="image" src="https://github.com/user-attachments/assets/6d4affcc-eb07-4e1e-94f3-94af102298ad" />


| # | Método | URL | Status-line | `Content-Length` / `Transfer-Encoding` | Body presente? |
|---|--------|-----|-------------|-----------------------------------------|----------------|
| 1 | GET    | `http://httpbin.org/status/200` | [HTTP/1.1 200 OK] | [0] | [não] |
| 2 | GET    | `http://httpbin.org/redirect-to?status_code=301&url=/get` | [HTTP/1.1 301 MOVED PERMANENTLY] | [0] | [não] |
| 3 | GET    | `http://httpbin.org/status/404` | [HTTP/1.1 404] | [7150] | [sim] |
| 4 | GET    | `http://httpbin.org/status/418` | [HTTP/1.1 418 I'M A TEAPOT] | [135] | [sim] |
| 5 | GET    | `http://httpbin.org/status/500` | [HTTP/1.1 500 INTERNAL SERVER ERROR] | [0] | [não] |
| 6 | GET    | `http://httpbin.org/status/503` | [HTTP/1.1 503 SERVICE UNAVAILABLE] | [0] | [não] |
| 7 | GET    | `http://httpbin.org/cache` com `If-Modified-Since` | [Gzip.] | [630] | [não] |

### Pergunta 4.1
> Em qual dos status o corpo está ausente/tamanho zero? Isso é obrigatório pela especificação ou depende do servidor?

**Resposta:** [200, 301, 500, 503 e 304. depende do servidor]

### Pergunta 4.2
> No `301`, qual cabeçalho da resposta informa para onde ir? O que aconteceria se estivesse ausente?

**Resposta:** [o cabeçalho informa o destino do redirecionamento sem ele o navegador nao saberia para onde redirecionar o user]

### Pergunta 4.3
> Diferença semântica entre `200`, `304` e `404` do ponto de vista do cache do navegador.

---

## Atividade 5 — Identificação de cabeçalhos (`http://httpbin.org/response-headers?...` + `/gzip`)

**Captura de tela (Inspectors → Headers):** `<img width="1298" height="687" alt="image" src="https://github.com/user-attachments/assets/53bcb17c-e9ff-4f2f-a552-5746a5f639af" />

`

| Cabeçalho                    | Req/Resp | Valor capturado | Função em uma frase |
|------------------------------|----------|------------------|----------------------|
| `Host`                       | [Req]    | [httpbin.org]            | [Indica qual domínio a requisição HTTP está sendo enviada]                |
| `User-Agent`                 | [Req]    | [Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/147.0.0.0 Safari/537.36]            |[Identifica o navegador, app ou cliente que fez a requisição]                |
| `Accept`                     | [Req]    | [text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7]            | [Informa quais tipos de conteúdo o cliente aceita receber]                |
| `Accept-Encoding`            | [Req]    | [gzip, deflate]            | [Diz quais formatos de compressão o cliente consegue entender]                |
| `Cookie`                     | [Req]    | [teste=1]            | [Envia cookies armazenados pelo navegador para manter sessão e preferências]                |
| `Server`                     | [Resp]    | [gunicorn/19.9.0]            | [Identifica o software/servidor que respondeu à requisição]                |
| `Content-Type`               | [Resp]    | [application/json]            | [Define o tipo de dado enviado no corpo da mensagem]                |
| `Content-Encoding`           | [-]    | [-]            | [-]
| `Set-Cookie`                 | [Resp]    | [teste=1]            | [Faz o servidor criar ou atualizar cookies no navegador do cliente.]                |
| `Cache-Control`              | [Resp]    | [max-age=3600]            | [Define regras de cache para navegador e proxies.]                |
| `Strict-Transport-Security`  | Não esperado em HTTP — ver Pergunta 5.3 | — | — |

### Pergunta 5.1
> `Content-Encoding: gzip`/`br` apareceu? Compare `Content-Length`, quando presente, com o conteúdo visível. O que explica a diferença?

**Resposta:** [Não houve gzip/br. Com compressão, o Content-Length fica menor por medir os dados comprimidos]

### Pergunta 5.2
> Cliente envia `Accept: application/json` mas o recurso só existe em `text/html`. Qual status code esperar?

**Resposta:** [O status esperado é 406 Not Acceptable.]

### Pergunta 5.3
> `Strict-Transport-Security` apareceu nas respostas HTTP? Por que esse cabeçalho está ausente neste fluxo? (Consulte a RFC 6797.) Qual é seu papel contra downgrades para HTTP puro?

**Resposta:** [O HSTS não apareceu porque só funciona em HTTPS. Ele evita downgrade para HTTP.]

---

## Atividade 6 — HTTP vs HTTPS (análise sem decriptação)

**Captura de tela HTTP (`neverssl.com`):** `<img width="1275" height="688" alt="image" src="https://github.com/user-attachments/assets/5882e316-9e45-4bd0-8353-f57ba0518fb9" />

`
**Captura de tela HTTPS (`https://httpbin.org/get`, apenas CONNECT):** `<img width="1132" height="623" alt="image" src="https://github.com/user-attachments/assets/c53f0c0e-e52a-4399-b9e3-908cd0e85572" />

`

### Pergunta 6.1
> Que método HTTP aparece na sessão do `https://httpbin.org/get`? O que ele faz e por que existe?

**Resposta:** [O método é CONNECT. Ele cria um túnel entre cliente e servidor para permitir tráfego HTTPS criptografado através de um proxy.]
### Pergunta 6.2
> Tabela comparativa dos campos visíveis ao Fiddler em cada caso:

| Campo                          | Visível em HTTP? | Visível em HTTPS (sem decriptação)? |
|--------------------------------|------------------|-------------------------------------|
| Método                         | [Get]            | [Connect]                           |
| URL completa (path + query)    | [Sim]            | [Nao]                               |
| Cabeçalhos de request          | [Sim]            | [Nao]                               |
| Corpo de request               | [Sim]            | [Nao]                               |
| Status code                    | [200 OK]         | [200 Connection Established]        |
| Cabeçalhos de response         | [Sim]            | [Nao]                               |
| Corpo de response              | [Sim]            | [Nao]                               |
| Host (via SNI, no `CONNECT`)   | [Sim]            | [Nao]                               |
| IP e porta de destino          | [Sim]            | [Nao]                               |

### Pergunta 6.3 (teórica)
> O que você **veria** no Fiddler se tivesse privilégio de administrador e pudesse habilitar *Decrypt HTTPS traffic*? Indique telas/abas e justifique por que essa inspeção exige a instalação de um certificado raiz.

**Resposta:** [Com Decrypt HTTPS traffic, o Fiddler mostra o tráfego HTTPS já descriptografado em Inspectors (Raw, Headers, TextView, JSON), permitindo ver requisição e resposta completas. Isso só funciona porque ele usa um certificado raiz instalado no sistema para interceptar e recriar conexões TLS.]

### Pergunta 6.4
> Por que a técnica de decriptação dos *debugging proxies* **não** funcionaria contra um usuário se um atacante a tentasse sem instalar o certificado?

**Resposta:** [Sem instalar o certificado raiz, o navegador não confia no proxy e bloqueia a interceptação. Por isso, a decriptação só funciona com cooperação do usuário, mantendo o HTTPS seguro contra interceptação não autorizada.]

---

## Atividade 7 — Cookies e sessão (`http://httpbin.org/cookies/...`)

**Captura de tela da sequência:** `<img width="1269" height="562" alt="image" src="https://github.com/user-attachments/assets/cfcdbea3-175e-4801-a06d-b67ba6606c14" />

`

| # | URL | `Set-Cookie` recebido | `Cookie` enviado |
|---|-----|-----------------------|-------------------|
| 1 | `/cookies/set?...`       | [disciplina=redes; Path=/professor=claudio; Path=/] | [nenhum] |
| 2 | `/cookies` (1ª visita)   | nenhum | [disciplina=redes; professor=claudio
]          |
| 3 | `/cookies` (reload 1)    | [nenhum] | [disciplina=redes; professor=claudio]          |
| 4 | `/cookies` (reload 2)    | [nenhum] | [disciplina=redes; professor=claudio]          |

### Pergunta 7.1
> `Set-Cookie` aparece uma vez ou em toda requisição? Justifique.

**Resposta:** [O Set-Cookie aparece apenas quando o servidor quer criar ou atualizar cookies (normalmente na primeira requisição). Nas requisições seguintes ele não aparece, porque o navegador já armazenou o cookie e passa a enviá-lo via cabeçalho Cookie]

### Pergunta 7.2
> Que atributos o `Set-Cookie` trouxe? Explique cada um presente. Para atributos não observados, registre `não observado`.

> **Nota:** o httpbin define cookies mínimos — apenas o atributo `Path=/` estará presente. Para cada atributo ausente, registre **não observado** e explique o comportamento padrão do navegador na sua ausência (ex.: sem `Expires`/`Max-Age` → cookie de sessão; sem `Secure` → pode ser enviado por HTTP; sem `SameSite` → o navegador aplica a política padrão da versão em uso).

**Resposta:**

| Atributo  | Valor | Função | Observado? |
|-----------|-------|--------|------------|
| `Path`    | `/`   | [Define em quais caminhos do site o cookie será enviado]  | Sim        |
| `Domain`  | —     | [Limita quais domínios podem receber o cookie; sem ele, usa o domínio atual]  | não observado |
| `Expires` | —     | [Define data de expiração; sem ele, o cookie é de sessão (some ao fechar o navegador)]  | não observado |
| `Max-Age` | —     | [Define tempo de vida em segundos; sem ele, comportamento de sessão]  | não observado |
| `Secure`  | —     | [Só envia o cookie em HTTPS; sem ele, pode ser enviado também em HTTP]  | não observado |
| `HttpOnly`| —     | [Impede acesso via JavaScript (proteção contra XSS); sem ele, JS pode ler o cookie]  | não observado |
| `SameSite`| —     | [Controla envio em requisições cross-site (proteção contra CSRF); sem ele, navegador usa política padrão (geralmente Lax)]  | não observado |

### Pergunta 7.3
> O atributo `Secure` pode aparecer num cookie recebido por HTTP puro? Qual seria o comportamento esperado? Relacione com o fato de que todo o tráfego desta atividade é visível em texto claro.

**Resposta:** [(a) O servidor pode enviar cookie com Secure mesmo em HTTP, não é proibido, mas é desencorajado pela RFC.(b) O navegador armazena o cookie, mas só o envia em HTTPS, ignorando em HTTP. Em HTTP, cookies sem proteção podem ser vistos por qualquer interceptador; o Secure evita isso ao restringir o envio a conexões criptografadas.]
### Pergunta 7.4
> Na aba **Inspectors → Cookies**, o cookie armazenado coincide com o campo `cookies` do JSON?

**Resposta:** [Sim, eles coincidem]

---

## Atividade 8 — Manipulação com breakpoints

> **Atividade exclusiva do Fiddler Classic.** Se você utilizou mitmproxy ou HTTP Toolkit, responda às questões 8.1 e 8.2 de forma teórica (sem capturas de tela), indicando que a ferramenta utilizada não suporta breakpoints interativos.

**Captura de tela da edição do User-Agent:** `<img width="735" height="572" alt="image" src="https://github.com/user-attachments/assets/dde08e9e-410e-4f5d-b338-3db790abc726" />

`

**JSON de resposta após edição:**

```json
{
  "user-agent": "[valor forjado]"
}
```

### Pergunta 8.1
> O servidor pode detectar que o `User-Agent` foi forjado? Discuta.

**Resposta:** [O servidor não consegue ter certeza de que o User-Agent foi forjado, pois ele pode ser facilmente alterado pelo cliente; no máximo ele pode inferir inconsistências.]

### Pergunta 8.2
> Após editar a status-line de `200 OK` para `404 Not Found`, o que o navegador exibe? Comente o papel do proxy como MITM.

**Captura de tela:** `evidencias/atv8_status_edit.png`

**Resposta:** [O navegador passa a mostrar erro 404 mesmo com resposta original 200, porque o Fiddler atua como MITM e altera a resposta antes de chegar ao cliente]

### Pergunta 8.3
> Confirme que todos os breakpoints foram desabilitados.

- [ ] Breakpoints desabilitados ao final (Shift+F11)

---

## Atividade 9 — Redirecionamento HTTP → HTTPS

**Captura de tela:** `<img width="960" height="641" alt="image" src="https://github.com/user-attachments/assets/ec6ab462-cf7d-4c63-b5a6-2d4e3d3a2ea5" />
<img width="1266" height="458" alt="image" src="https://github.com/user-attachments/assets/5fcf96c7-f110-4bcb-bf27-e37ec99befa7" />

`

**Status-line da resposta a `http://httpbin.org/redirect-to?status_code=301&url=https%3A%2F%2Fhttpbin.org%2Fget`:**

```http
[HTTP/1.1 301 MOVED PERMANENTLY]
```

**Cabeçalho `Location` da resposta:**

```
Location: [https://httpbin.org/get]
```

### Pergunta 9.1
> Código de status e cabeçalho que direcionaram o navegador para `https://`.

**Resposta:** [Código de status: 301 Moved Permanently / Cabeçalho responsável pelo redirecionamento: Location: https://httpbin.org/get]

### Pergunta 9.2
> Além do redirecionamento 3xx, qual outro mecanismo/cabeçalho faz o navegador passar a forçar HTTPS em visitas futuras? Cite a RFC.

**Resposta:** [O mecanismo é HSTS (Strict-Transport-Security), definido na RFC 6797, que força o uso de HTTPS.]

### Pergunta 9.3
> Se esse cabeçalho fosse enviado por uma resposta servida via HTTP puro, o navegador deveria obedecer? Justifique com base na RFC.

**Resposta:** [Não deve ser aceito em HTTP puro, pois a RFC 6797 exige HTTPS; caso contrário, o cabeçalho pode ser interceptado e falsificado.]



### 7. Impacto prático de `Cache-Control: no-store`.

[Impede que navegador e proxies armazenem a resposta.]

### 8. Como um debugging proxy decifra HTTPS sem violar a criptografia, e por que isso exige cooperação do usuário (e por que, justamente, você não pôde executar essa etapa)?

[Funciona como MITM com certificado raiz instalado pelo usuário; sem isso, o navegador bloqueia por falta de confiança, por isso exige cooperação e não foi possível executar sem privilégios.]

### 9. Exemplo de cabeçalho de request que o navegador envia automaticamente, sem a página pedir.

[User-Agent, enviado automaticamente pelo navegador.]

### 10. Se fosse automatizar a inspeção via script, qual ferramenta alternativa escolheria? Por quê?

[Usaria filtros, regras e exportação/logs (ou FiddlerScript) para capturar e analisar tráfego em massa]

### 11. (Exclusiva do Fluxo B) Três cabeçalhos de segurança que não aparecem ou não fazem sentido em respostas HTTP puro. Para cada um, o que aconteceria se enviado por um servidor HTTP? (Cite RFC 6797 para HSTS.)

**Resposta:**

| Cabeçalho | Comportamento esperado sobre HTTP | Referência |
|-----------|-----------------------------------|-----------|
| [Strict-Transport-Security]     | [Ignorado pelo navegador, pois só é válido em HTTPS; não estabelece política de segurança]                             | [RFC 6797]     |
| [Set-Cookie: Secure]     | [Cookie é armazenado, mas não será enviado em HTTP, apenas em HTTPS]                             | [RFC 6265bis]     |
| [Content-Security-Policy (upgrade-insecure-requests)]     | [Pode ser ignorado ou aplicado parcialmente, mas não garante segurança de transporte em HTTP]                             | [W3C CSP]     |

---

## Reflexão final (opcional, até 10 linhas)

> O que você aprendeu que não conhecia antes deste laboratório? Há algum
> cabeçalho, código de status ou comportamento que passou a olhar com
> mais atenção? Alguma dificuldade que recomendaria evitar para a próxima turma?

[reflexão]

---

## Encerramento — justificativa de segurança (Fluxo B)

**Parágrafo: por que a remoção de certificado é dispensável neste fluxo e por que seria obrigatória para o aluno administrador:**

[redigir, em até 5 linhas, com base na seção 4.6 do readme.md]

- [ ] HTTPS-First Mode / HTTPS-Only Mode reabilitado no navegador
- [ ] Fiddler / mitmproxy / HTTP Toolkit fechado (porta de proxy liberada)
- [ ] Configuração de proxy removida do navegador (se aplicável)

---

## Checklist de entrega

- [ ] Todos os campos `[...]` substituídos
- [ ] Pasta `evidencias/` com capturas nomeadas por atividade (incluindo Atv. 9)
- [ ] 11 questões de verificação respondidas
- [ ] Atividade 9 (redirecionamento HTTP→HTTPS) documentada
- [ ] Justificativa de encerramento redigida
- [ ] Arquivo compactado como `NOME_RA_LAB_HTTP_FLUXOB.zip`
- [ ] Submetido no Microsoft Teams dentro do prazo
