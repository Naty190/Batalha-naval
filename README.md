✅ O que vou te entregar:
Código simples e funcional
Estrutura clara de pastas
Explicações no código (comentários)
Um README.md com instruções de uso 
Regras simplificadas:
Tabuleiro 5x5
3 navios por jogador
O jogador joga contra o computador
O jogo continua até todos os navios de um lado afundarem

# Batalha-naval # batalha_naval.py - Versão novato
import random

# Tamanho do tabuleiro
TAMANHO = 5
NAVIOS = 3

# Cria um tabuleiro vazio
def criar_tabuleiro():
    return [['~'] * TAMANHO for _ in range(TAMANHO)]

# Mostra o tabuleiro
def mostrar_tabuleiro(tabuleiro, mostrar_navios=False):
    print("  " + " ".join(str(i) for i in range(TAMANHO)))
    for i, linha in enumerate(tabuleiro):
        linha_visivel = []
        for celula in linha:
            if celula == 'N' and not mostrar_navios:
                linha_visivel.append('~')
            else:
                linha_visivel.append(celula)
        print(f"{i} " + " ".join(linha_visivel))

# Posiciona navios aleatoriamente no tabuleiro
def posicionar_navios(tabuleiro):
    navios_colocados = 0
    while navios_colocados < NAVIOS:
        linha = random.randint(0, TAMANHO - 1)
        coluna = random.randint(0, TAMANHO - 1)
        if tabuleiro[linha][coluna] == '~':
            tabuleiro[linha][coluna] = 'N'
            navios_colocados += 1

# Realiza um disparo
def disparar(tabuleiro, linha, coluna):
    if tabuleiro[linha][coluna] == 'N':
        tabuleiro[linha][coluna] = 'X'
        return True
    elif tabuleiro[linha][coluna] == '~':
        tabuleiro[linha][coluna] = 'O'
    return False

# Conta os navios restantes
def contar_navios(tabuleiro):
    return sum(linha.count('N') for linha in tabuleiro)

# Jogo principal
def jogar():
    jogador = criar_tabuleiro()
    computador = criar_tabuleiro()

    posicionar_navios(jogador)
    posicionar_navios(computador)

    print("\nBem-vindo ao Batalha Naval!")

    while True:
        print("\nSeu tabuleiro:")
        mostrar_tabuleiro(jogador, mostrar_navios=True)

        print("\nTabuleiro do computador:")
        mostrar_tabuleiro(computador)

        # Jogador ataca
        try:
            linha = int(input("Digite a linha (0 a 4): "))
            coluna = int(input("Digite a coluna (0 a 4): "))
            if 0 <= linha < TAMANHO and 0 <= coluna < TAMANHO:
                if disparar(computador, linha, coluna):
                    print("\nAcertou um navio!")
                else:
                    print("\nErrou!")
            else:
                print("\nCoordenadas fora do limite. Tente novamente.")
                continue
        except ValueError:
            print("\nEntrada inválida. Tente novamente.")
            continue

        # Verifica vitória do jogador
        if contar_navios(computador) == 0:
            print("\nParabéns! Você venceu!")
            break

        # Computador ataca
        while True:
            linha_pc = random.randint(0, TAMANHO - 1)
            coluna_pc = random.randint(0, TAMANHO - 1)
            if jogador[linha_pc][coluna_pc] in ('~', 'N'):
                acertou = disparar(jogador, linha_pc, coluna_pc)
                print(f"\nComputador atacou ({linha_pc}, {coluna_pc}) e {"acertou!" if acertou else "errou."}")
                break

        # Verifica vitória do computador
        if contar_navios(jogador) == 0:
            print("\nO computador venceu. Fim de jogo.")
            break

if __name__ == "__main__":
    jogar()
