# Exemplos

Agora que vimos a importância da utilização de testes automatizados nos nossos programas e como prepará-los a partir do framework PyTest. Vamos mergulhar em alguns exemplos:

## Exemplo da Conta Bancária

O primeiro exemplo remonta um programa que simula uma conta bancária e é baseado nos testes de algumas funcionalidades da conta.

**>** Vamos dar uma olhada nas classes ContaBancaria e Cliente

=== "ContaBancaria.py"

    ```python
    class Conta:

        quantidade_contas = 0
        
        @classmethod
        def novo_id(cls):
            cls.quantidade_contas += 1
            return cls.quantidade_contas

        @classmethod
        def getQuantidadeContas(cls):
            return cls.quantidade_contas

        def __init__(self, cliente, saldoInicial):
            self.cliente = cliente
            self.id = self.novo_id()
            self.saldo = saldoInicial

        def getSaldo(self):
            return self.saldo

        def sacar(self, valor):
            # Se houver saldo suficiente para o saque:
            if ( self.saldo - valor >= 0 ):
                self.saldo -= valor
                return True
            else:
                return False
        
        def depositar(self, valor):
            if ( valor > 0 ):
                self.saldo += valor
                return True
            else:
                return False

        def transferir(self, contaDestino, valor):
            if ( valor > 0 ):
                self.sacar(valor)
                contaDestino.depositar(valor)
                return True
            else:
                return False
    ```

=== "Cliente.py"

    ```python
    class Cliente:

        total_clientes = 0

        @classmethod
        def novo_id(cls):
            cls.total_clientes += 1
            return cls.total_clientes

        @classmethod
        def getTotalClientes(cls):
            return cls.total_clientes

        def __init__(self, nome):
            self.nome = nome
            self.id = self.novo_id()

        def getNome(self):
            return self.nome

        def getId(self):
            return self.id
    ```

**>** Um cliente possui um nome e um id e métodos de acesso aos seus atributos

**>** Uma conta bancária possui um número identificador, um cliente associado e um saldo atual.

Nosso objetivo é criar testes automatizados para cuidar dos métodos da nossa conta bancária.

1. sacar()
1. depositar()
1. transferir()

Dessa forma garantimos a integridade da classe ContaBancaria.

Para isso, criamos uma classe de teste somente para as funcionalidades da conta bancária
Segue o trecho de código:

=== "test_contaBancaria.py"

    ```python
    from banco.cliente import Cliente
    from banco.contaBancaria import Conta

    # o saque deve falhar!
    def test_sacar_sem_saldo():
        conta = Conta(Cliente("Jesimar"), 0) # Criando uma conta com saldo inicial igual a 0
        resultado = conta.sacar(1000) # Tentando sacar 1000 do saldo atual
        assert resultado == False

    # o saque deve ocorrer com sucesso!
    def test_sacar_com_saldo():
        conta = Conta(Cliente("Jesimar"), 1000) # Criando uma conta com saldo inicial igual a 1000
        resultado = conta.sacar(500) # Sacando 500 do saldo atual
        assert resultado == True

    # o depósito deve falhar!
    def test_depositar_valor_negativo():
        conta = Conta(Cliente("Jesimar"), 0) # Criando uma conta com saldo inicial igual a 0
        resultado = conta.depositar(-500) # Tentando depositar -500 no saldo atual
        assert resultado == False

    # o depósito deve ocorrer com sucesso!
    def test_depositar_valor_positivo():
        conta = Conta(Cliente("Jesimar"), 0) # Criando uma conta com saldo inicial igual a 0
        resultado = conta.depositar(500) # Depositando um valor válido igual a 500
        assert resultado == True

    # Não deve conseguir sacar e não deve depositar na outra conta
    def test_transferir_sem_saldo():
        conta1 = Conta(Cliente("Raphael"), 0) # Cliente 1 sem saldo inicial
        conta2 = Conta(Cliente("Jesimar"), 1000) # Cliente 2 com 1000 reais como saldo inicial

        saldoConta1Antes = conta1.getSaldo() # Salvando o saldo da conta 1 antes da operação
        saldoConta2Antes = conta2.getSaldo() # Salvando o saldo da conta 2 antes da operação

        resultado = conta1.transferir(conta2, 2000) # Cliente 1 tenta transferir 2000 reais

        saldoConta1Depois = conta1.getSaldo() # Salvando o saldo da conta 1 após da operação
        saldoConta2Depois = conta2.getSaldo() # Salvando o saldo da conta 2 após da operação

        assert (resultado == False and saldoConta1Antes == saldoConta1Depois and saldoConta2Antes == saldoConta2Depois)

    ```

Podemos observar acima que foram criados dois métodos para testar as funcionalidades de saque e dois para depósito:

!!! Falhas failure
    **Sacar sem saldo e Depositar um valor negativo!**

!!! success
    **Sacar com saldo e Depositar um valor positivo!**

Já o teste de transferência é mais interessante.
Ele envolve utilizar os outros métodos que já possuem seus próprios testes, sacar e depositar e a sua interação.
Ao analisar o código do teste de transferência, é possível perceber que há uma lógica por traz dessa operação que não
pode ser ignorada. Essa lógica consiste no seguinte fato:

**>** Caso a conta de origem da trasferência não possua saldo suficiente para realizar a operação, o valor não deve ser sacado. Isso já nos foi garantido se nosso teste de saque foi bem sucedido. Ao final da operação, é evidente que o valor de transferência deve ser decrementado do saldo da conta de origem e incrementado no saldo da conta de destino, e é justamente isso que nosso teste verifica.

Dessa forma, vamos realizar os testes e verificar o que o PyTest nos informa:


