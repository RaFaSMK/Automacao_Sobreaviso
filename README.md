# 📟 Automação de Cadastro de Sobreaviso

Automação desenvolvida em Python para eliminar o trabalho manual e repetitivo de cadastrar escalas de sobreaviso em um sistema interno corporativo. O script lê os dados diretamente de uma planilha Excel (.xlsx) — seja um arquivo local ou um link — e registra cada entrada automaticamente no sistema via requisições HTTP.

---

## 🚀 Problema resolvido

O processo manual exigia abrir o sistema, preencher nome, data e telefone de cada funcionário de sobreaviso, um por um. Com dezenas de entradas por mês, o processo era lento e sujeito a erros humanos.

Este script automatiza **todo o fluxo**: leitura da planilha → autenticação no sistema → envio dos dados via POST, reduzindo o tempo de cadastro de minutos para segundos.

---

## ⚙️ Como funciona

```
Planilha Excel (.xlsx)
        │
        ▼
 Leitura e parsing
 (datas em pt-BR, nomes com acentos)
        │
        ▼
  Login no sistema
  (sessão HTTP autenticada)
        │
        ▼
  Envio dos registros
  (POST para cada funcionário)
        │
        ▼
  Feedback no terminal ✅ / ❌
```

### Fluxo detalhado

1. **Carregamento da planilha** — aceita arquivo local `.xlsx` ou URL direta
2. **Parsing das datas** — interpreta datas em português (ex: `"Sábado, 10 de junho"`) usando `Babel` e converte para `YYYY-MM-DD`
3. **Normalização de nomes** — remove acentos via `unicodedata` para evitar conflitos no mapeamento funcionário → telefone
4. **Autenticação** — faz login no sistema interno via `POST` mantendo sessão ativa com `requests.Session()`
5. **Cadastro em lote** — itera sobre cada entrada da planilha e envia os dados ao endpoint de cadastro
6. **Feedback em tempo real** — exibe no terminal o status de cada cadastro (sucesso ou código de erro HTTP)

---

## 🛠️ Tecnologias utilizadas

| Biblioteca | Uso |
|---|---|
| `requests` | Requisições HTTP (login + POST de dados) |
| `openpyxl` | Leitura de planilhas `.xlsx` |
| `babel` | Parsing de nomes de meses em português |
| `unicodedata` | Normalização de strings com acentos |
| `re` | Parsing de datas com expressões regulares |
| `datetime` | Obtenção do ano atual |

---

## 📋 Pré-requisitos

- Python 3.8+
- Instalar as dependências:

```bash
pip install requests openpyxl babel python-dotenv
```

---

## ▶️ Como usar

1. **Clone o repositório**
```bash
git clone https://github.com/RaFaSMK/Automacao_Sobreaviso.git
cd Automacao_Sobreaviso
```

2. **Configure as variáveis de ambiente** — copie o `.env.example` e preencha com seus dados:
```bash
cp .env.example .env
```
```env
LOGIN_URL=https://seu-sistema-interno/index.php
POST_URL=https://seu-sistema-interno/cadastrar.php
USERNAME=seu_usuario
```

3. **Configure o dicionário de funcionários** no arquivo `main.py`:
```python
funcionarios = {
    "joao": {"nome": "João Silva", "telefone": "11999999999"},
    # adicione os demais funcionários...
}
```

4. **Execute o script**
```bash
python main.py
```

5. **Siga as instruções no terminal:**
```
Digite a senha: ****
Digite o NOME do arquivo .xlsx ou COLE o LINK: EscalaMaio.xlsx
```

---

## 📁 Estrutura do projeto

```
Automacao_Sobreaviso/
├── main.py   # Script principal
├── .env.example            # Template de variáveis de ambiente
├── .gitignore              # Exclui .env e planilhas do versionamento
└── README.md
```

---

## 🔒 Observações de segurança

- As credenciais de acesso **não estão hardcoded** — a senha é solicitada via `input()` em tempo de execução
- Dados pessoais dos funcionários (nomes e telefones) foram **removidos do código** antes da publicação
- O SSL está desabilitado (`verify=False`) pois o sistema interno usa certificado autoassinado — **não recomendado em ambientes externos**

---

## 🔮 Melhorias futuras

- [ ] Suporte a múltiplos tipos de escala (técnicos e analistas)
- [ ] Interface de linha de comando com `argparse`
- [ ] Leitura de credenciais via variáveis de ambiente (`.env`)
- [ ] Log persistente em arquivo `.txt` ou `.csv`
- [ ] Testes automatizados com `pytest` e mock de requisições

---

## 👤 Autor

Feito por **[Rafael Wagner](https://github.com/RaFaSMK)**
