# Uso do PyTest

O PyTest oferece diversas opções para executar testes no seu projeto Python. Abaixo, estão alguns dos principais comandos e funcionalidades para rodar seus testes.

## Comandos de ajuda

Obter ajuda sobre a versão, nomes de opções e variáveis de ambiente

```
pytest --version # mostra de onde o pytest foi importado
pytest --fixtures # exibe os argumentos de função embutidos disponíveis
pytest -h ou pytest --help # exibe ajuda sobre opções de linha de comando
```

## Executando Testes

Para executar todos os testes, utilize:

**Executando testes em um arquivo**

```
pytest test_arquivo.py
```

**Executando testes em um diretório**

```
pytest test_diretorio.py
```

pytest -k 'MinhaClasse e não metodo'

!!! Nota

    Isso executará testes cujos nomes correspondam à expressão de string fornecida (ignorando maiúsculas e minúsculas), que pode incluir operadores Python que utilizam nomes de arquivos, nomes de classes e nomes de funções como variáveis. O exemplo acima executará TestMinhaClasse.test_qualquer, mas não TestMinhaClasse.test_metodo_simples. Use "" em vez de '' na expressão ao executar isso no Windows.


## Executando testes por argumentos de coleta

Passe o nome do arquivo do módulo relativo ao diretório de trabalho, seguido por especificadores como o nome da classe e o nome da função separados por caracteres ::, e parâmetros de parametrização entre colchetes [].

**Para executar um teste específico dentro de um módulo:**
```
pytest tests/test_arquivo.py::test_funcao
```

**Para executar todos os testes em uma classe:**
```
pytest tests/test_arquivo.py::TestClasse
```

**Especificando um método de teste específico:**
```
pytest tests/test_arquivo.py::TestClasse::test_metodo
```

**specificando uma parametrização específica de um teste:**
```
pytest tests/test_arquivo.py::test_funcao[x1,y2]
```

**Controlando o tempo de duração da execução do teste**

Para obter uma lista dos 10 testes mais lentos com duração superior a 1,0s:
```
pytest --durations=10 --durations-min=1.0
```
!!! Nota
    Por padrão, o pytest não mostrará as durações dos testes que são muito pequenas (<0,005s), a menos que o parâmetro "-vv" seja adicionado na linha de comando.

## Chamando Pytest diretamente do código

Você pode invocar o pytest diretamente a partir de código Python:
```
retcode = pytest.main()
```
Isso age como se você estivesse chamando “pytest” a partir da linha de comando. Ele não levantará SystemExit, mas retornará o código de saída. Se você não passar nenhum argumento, o main lerá os argumentos a partir dos argumentos da linha de comando do processo ("sys.argv"), o que pode não ser desejável. Você pode passar opções e argumentos explicitamente:
```
retcode = pytest.main(["-x", "meudiretorio"])
```


## Plugins

Você também pode utilizar plugins feitos por outras pessoas nos testes, ou até mesmo fazer seu próprio plugin.

Para instalar e desistalar plugins utilize os seguintes comandos:

```bash
pip install pytest-NAME
pip uninstall pytest-NAME
```
Se um plugin é instalado, o pytest automaticamente acha e faz a integração, não há necessidade de ativá-los.

Aqui está alguns plugins populares:

* pytest-django: Escreve testes para aplicativos Django, utilizando a integração com pytest.

* pytest-cov: Relatórios de cobertura de código, compatível com testes distribuídos.

* pytest-instafail: Relata falhas enquanto o teste está sendo executado.

* pytest-timeout: Define limites de tempo para testes com base em marcações de funções ou definições globais.

* pytest-pep8: Uma opção "--pep8" para habilitar a verificação de conformidade com o PEP8.
