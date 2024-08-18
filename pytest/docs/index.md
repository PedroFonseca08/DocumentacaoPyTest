# Introdução

O Pytest é uma ferramenta de testes para Python que se destaca pela sua simplicidade e flexibilidade. Ele facilita a criação e execução de testes automatizados em diversos tipos de projetos, desde pequenos scripts até grandes aplicações.

## Benefícios dos Testes

Os testes são fundamentais para garantir a qualidade e a confiabilidade de um software. Eles permitem identificar erros e bugs de forma antecipada, reduzindo os custos de manutenção e evitando problemas em produção.

- **Qualidade e Estabilidade**: Testes garantem que o código funciona conforme o esperado, mesmo após modificações.
- **Facilidade de Manutenção**: Com testes automatizados, é possível refatorar o código com segurança.
- **Eficiência no Desenvolvimento**: Detectar problemas cedo acelera o ciclo de desenvolvimento.

Investir em testes aumenta a confiança no software e melhora a experiência do usuário final.

## Instalação

Digite a seguinte linha de comando para realizar a instalação do Pytest:

```
pip install pytest
```

Verifique se a instalação do Pytest foi bem sucedida:

```
$ pytest --version
pytest 8.3.2
```

## Criando um teste

Crie um arquivo test_simples.py com o seguinte código:

=== "python"
```python
def function(x):
    return x + 1

def test_answer():
    assert function(3) == 5
```

!!! Nota

    O Pytest permite que você use o assert padrão do Python para verificar expectativas e valores em testes.

Em seguida, execute o comando:

```
$ pytest
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-8.x.y, pluggy-1.x.y
rootdir: /home/sweet/project
collected 1 item

test_simples.py F                                                     [100%]

================================= FAILURES =================================
_______________________________ test_answer ________________________________

    def test_answer():
>       assert function(3) == 5
E       assert 4 == 5
E        +  where 4 = function(3)

test_simples.py:6: AssertionError
========================= short test summary info ==========================
FAILED test_simples.py::test_answer - assert 4 == 5
============================ 1 failed in 0.12s =============================
```

## Verificando exceção

Use o auxiliar raises para verificar se um código gera uma exceção:

``` python
# test_sysexit.py
import pytest


def function():
    raise SystemExit(1)


def test_meuTeste():
    with pytest.raises(SystemExit):
        function()
```

Executando o teste da função no modo quieto ("-q"):
```
$ pytest -q test_sysexit.py
.                                                                    [100%]
1 passed in 0.12s
```


## Agrupando vários testes em uma classe


Depois de desenvolver vários testes, você pode querer agrupá-los em uma classe. O Pytest possiblita a criação de uma classe contendo mais de um teste:

=== "python"
``` python
# test_classe.py
class TestClasse:
    def test_um(self):
        x = "this"
        assert "h" in x

    def test_dois(self):
        x = "hello"
        assert hasattr(x, "check")
```

!!! Nota
    O PyTest segue convenções para descobrir testes em projetos Python:

    1. Se nenhum argumento for fornecido, a busca começa no diretório atual ou em testpaths.
    2. Ele procura recursivamente por arquivos com padrões test_*.py ou *_test.py.
    3. Dentro desses arquivos, coleta funções e métodos de teste prefixados com test_, seja fora de classes ou dentro de classes prefixadas com Test (sem métodos __init__).
    4. Testes também podem ser descobertos por meio da herança de unittest.TestCase.

Saída:
```
$ pytest -q test_class.py
.F                                                                   [100%]
================================= FAILURES =================================
____________________________ TestClass.test_two ____________________________

self = <test_class.TestClass object at 0xdeadbeef0001>

    def test_two(self):
        x = "hello"
>       assert hasattr(x, "check")
E       Assertiumrror: assert False
E        +  where False = hasattr('hello', 'check')

test_class.py:8: AssertionError
===============dois======= short test summary info ==========================
FAILED test_class.py::TestClass::test_two - AssertionError: assert False
1 failed, 1 passed in 0.12s
```

## Suporte às versões do Python

Essa tabela exibe as versões do PyTest e as versões mínimas correspondentes do Python necessárias para cada uma delas.

| Versão do PyTest | Versão Mínima do Python |
| ---------------- | ----------------------- |
| 8.0+             | 3.8+                    |
| 7.1+             | 3.7+                    |
| 6.2 - 7.0        | 3.6+                    |
| 5.0 - 6.1        | 3.5+                    |
| 3.3 - 4.6        | 2.7, 3.4+               |
