# Sistema-bancario-otimizado
bootcam da DIO para otimizar o sistema bancario adicionando funçoes no nosso codigo py.

# ==================== Dados ====================
usuarios = []
contas = []

# ==================== Funções ====================

def criar_usuario():
    cpf = input("Informe o CPF (somente números): ").strip()
    usuario = [u for u in usuarios if u["cpf"] == cpf]
    
    if usuario:
        print("Usuário já existente!")
        return

    nome = input("Informe o nome completo: ").strip()
    data_nascimento = input("Informe a data de nascimento (dd/mm/aaaa): ").strip()
    endereco = input("Informe o endereço (logradouro, nº - bairro - cidade/UF): ").strip()

    usuarios.append({
        "nome": nome,
        "data_nascimento": data_nascimento,
        "cpf": cpf,
        "endereco": endereco
    })

    print("Usuário criado com sucesso!")

def listar_usuarios():
    if not usuarios:
        print("Nenhum usuário cadastrado.")
        return

    for u in usuarios:
        print(f"\nNome: {u['nome']}\nCPF: {u['cpf']}\nNascimento: {u['data_nascimento']}\nEndereço: {u['endereco']}\n")

def criar_conta():
    cpf = input("Informe o CPF do usuário: ").strip()
    usuario = [u for u in usuarios if u["cpf"] == cpf]

    if not usuario:
        print("Usuário não encontrado. Crie o usuário primeiro.")
        return

    numero_conta = len(contas) + 1
    contas.append({
        "agencia": "0001",
        "numero": numero_conta,
        "usuario": usuario[0],
        "saldo": 0,
        "extrato": "",
        "saques": 0
    })

    print(f"Conta criada com sucesso! Número da conta: {numero_conta}")

def listar_contas():
    if not contas:
        print("Nenhuma conta cadastrada.")
        return

    for c in contas:
        print(f"\nAgência: {c['agencia']}\nConta: {c['numero']}\nTitular: {c['usuario']['nome']}\nSaldo: R$ {c['saldo']:.2f}\n")

def depositar(conta):
    valor = float(input("Informe o valor do depósito: "))
    if valor > 0:
        conta["saldo"] += valor
        conta["extrato"] += f"Depósito: R$ {valor:.2f}\n"
        print("Depósito realizado com sucesso!")
    else:
        print("Valor inválido.")

def sacar(conta):
    valor = float(input("Informe o valor do saque: "))
    LIMITE = 500
    LIMITE_SAQUES = 3

    if valor <= 0:
        print("Valor inválido.")
    elif valor > conta["saldo"]:
        print("Saldo insuficiente.")
    elif valor > LIMITE:
        print("Valor excede o limite por saque.")
    elif conta["saques"] >= LIMITE_SAQUES:
        print("Número máximo de saques atingido.")
    else:
        conta["saldo"] -= valor
        conta["extrato"] += f"Saque: R$ {valor:.2f}\n"
        conta["saques"] += 1
        print("Saque realizado com sucesso!")

def exibir_extrato(conta):
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not conta["extrato"] else conta["extrato"])
    print(f"\nSaldo: R$ {conta['saldo']:.2f}")
    print("==========================================")

def selecionar_conta():
    if not contas:
        print("Nenhuma conta disponível.")
        return None

    numero = int(input("Informe o número da conta: "))
    conta = next((c for c in contas if c["numero"] == numero), None)

    if not conta:
        print("Conta não encontrada.")
    return conta

# ==================== Menu Principal ====================

menu = """
[u] Criar usuário
[lu] Listar usuários
[cc] Criar conta
[lc] Listar contas
[d] Depositar
[s] Sacar
[e] Extrato
[q] Sair
=> """

while True:
    opcao = input(menu).strip()

    if opcao == "u":
        criar_usuario()

    elif opcao == "lu":
        listar_usuarios()

    elif opcao == "cc":
        criar_conta()

    elif opcao == "lc":
        listar_contas()

    elif opcao == "d":
        conta = selecionar_conta()
        if conta:
            depositar(conta)

    elif opcao == "s":
        conta = selecionar_conta()
        if conta:
            sacar(conta)

    elif opcao == "e":
        conta = selecionar_conta()
        if conta:
            exibir_extrato(conta)

    elif opcao == "q":
        print("Saindo do sistema. Até logo!")
        break

    else:
        print("Opção inválida. Tente novamente.")
