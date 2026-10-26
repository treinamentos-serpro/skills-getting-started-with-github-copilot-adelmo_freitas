## Plan: Testes da API FastAPI

Adicionar cobertura automatizada dos endpoints atuais em uma pasta `tests/`, usando `pytest`, `TestClient` do FastAPI e o padrão Arrange-Act-Assert. A API mantém estado em uma variável global em memória; cada teste deve restaurar esse estado para evitar dependência de ordem.

**Passos**
1. Incluir `pytest` como dependência de desenvolvimento em `requirements.txt`, preservando as dependências existentes; usar `pytest -q` para executar a suíte.
2. Criar `tests/test_app.py`, importar `app` e `activities` de `src.app`, configurar `TestClient` e fixture que restaure os participantes antes/depois de cada caso.
3. Cobrir `GET /activities` e os fluxos de cadastro: sucesso, atividade inexistente (404) e cadastro duplicado (400).
4. Cobrir remoção: sucesso (participante deixa de constar na atividade), atividade inexistente (404) e participante não inscrito (404). Em cada teste, separar claramente Arrange (preparar dados/estado), Act (fazer a requisição) e Assert (validar status e conteúdo JSON), mantendo os casos independentes.
5. Executar `pytest -q` e corrigir falhas sem alterar o comportamento da API fora do necessário para os testes.

**Arquivos relevantes**
- `requirements.txt` — adicionar `pytest`; `httpx` já está declarado para o cliente de teste.
- `pytest.ini` — manter a configuração atual `pythonpath = .`.
- `src/app.py` — endpoints e dicionário global `activities` que os testes exercitam; nenhuma alteração prevista.
- `tests/test_app.py` — novo conjunto de testes de integração da API.

**Verificação**
1. Instalar as dependências declaradas, se necessário.
2. Executar `pytest -q` a partir da raiz e confirmar que todos os casos passam.

**Decisões**
- Usar `fastapi.testclient.TestClient` com `pytest`, sem adicionar outra biblioteca de testes HTTP.
- Isolar os testes por meio de snapshot/restauração do estado global, em vez de alterar a aplicação para injeção de dependências ou persistência.
- Não incluir testes de frontend nem mudar contratos HTTP existentes.
- A atividade local recomenda explicitamente `pytest`, AAA, `requirements.txt` e uma pasta `tests/`; não prescreve nome de arquivo nem lista de casos.
