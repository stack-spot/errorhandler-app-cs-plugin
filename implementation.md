Adicione ao seu `IApplicationBuilder`, via `app.UseErrorHandler()`, no `Startup` da aplicação ou `Program`. 

```csharp
app.UseErrorHandler();
```

#### Exceções tratadas

- Utilizando `HttpResponseException`, existe a possibilidade de personalizar a mensagem, bem como definir um `HttpStatusCode`. Além do retorno padronizado da API, existe a possibilidade de gravar o log dos erros que estão ocorrendo, através da propriedade `logActive`. Caso o valor seja igual a `false`, o log não é gravado.
- O `HttpResponseException` possui diversas sobrecargas, sendo possível atribuir valor nas propriedades abaixo:

| **Campo** | **Descrição** |
| :--- | :--- |
| `logActive` | Campo que permite ativar a gravação de log |
| `statusCode` | `HttpStatusCode` que será retornado na requisição |
| `logLevel` | Level do log |
| `message` | Mensagem que será retornada na requisição |
| `exception` | Exceção a ser gravada |

**Exemplos de uso:** 

500 - Internal Server Error

```csharp
throw new HttpResponseException("Ocorreu um erro ao tentar salvar o registro", true);
```

Resultado:
```json
{
  "error_id": "62ec9afe-64ad-46cb-8df0-abfd306cb7dc",
  "message": "Ocorreu um erro ao tentar salvar o registro"
}
```

400 - Bad Request - neste caso abaixo, o log não foi ativado.

```csharp
throw new HttpResponseException(HttpStatusCode.BadRequest, "Campo Nome é obrigatório", false);
```

Resultado:
```json
{
  "error_id": "62ec9afe-64ad-46cb-8df0-abfd306cb7dc",
  "message": "Campo Nome é obrigatório"
}
```

400 - Bad Request

```csharp
throw new HttpResponseException(HttpStatusCode.BadRequest, "Campo Nome é obrigatório", true);
```

Resultado:
```json
{
  "error_id": "62ec9afe-64ad-46cb-8df0-abfd306cb7dc",
  "message": "Campo Nome é obrigatório"
}
```

- Utilizando `HttpResponseObjectException` - este tem comportamento similar ao `HttpResponseException`, contendo as mesmas possibilidades citadas anteriomente. Mas neste caso também é possível receber um `HttpRequestMessage` e `HttpResponseMessage` como parâmetros.
- O `HttpResponseException` possui diversas sobrecargas, sendo possível atribuir valor as propriedades abaixo:

#### Exceções não tratadas

Sempre que ocorrer uma exceção não tratada, a exceção é suprimida e o log sempre será registrado. O `HttpStatusCode` será `500 - Internal Server Error` e a API retornará algo conforme o exemplo abaixo:

```json
{
  "error_id": "62ec9afe-64ad-46cb-8df0-abfd306cb7dc",
  "message": "An unexpected system error has occurred"
}
```
