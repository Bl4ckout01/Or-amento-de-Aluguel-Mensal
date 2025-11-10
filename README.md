# Orçamento-de-Aluguel-Mensal

import csv
from datetime import datetime, timedelta

class Imovel:
    def __init__(self, tipo, valor_base):
        self.tipo = tipo
        self.valor_base = valor_base
        self.quartos_extras = 0
        self.vagas_garagem = 0
        self.desconto_sem_filhos = False

class CalculadoraAluguel:
    def __init__(self):
        self.valor_contrato = 2000.00
        self.parcelas_contrato = 5
    
    def calcular_aluguel(self, imovel):
        # Calcular valor base com adicionais
        valor_quartos = imovel.quartos_extras * 150
        valor_garagem = imovel.vagas_garagem * 100
        valor_bruto = imovel.valor_base + valor_quartos + valor_garagem
        
        # Aplicar desconto se não tiver filhos
        if imovel.desconto_sem_filhos:
            valor_final = valor_bruto * 0.95
        else:
            valor_final = valor_bruto
        
        return valor_final
    
    def gerar_parcelas(self, imovel, valor_aluguel):
        parcelas = []
        data_atual = datetime.now()
        
        for i in range(12):
            data_parcela = data_atual + timedelta(days=30*i)
            
            # Primeiros 5 meses incluem parcela do contrato
            if i < 5:
                valor_total = valor_aluguel + (self.valor_contrato / self.parcelas_contrato)
                descricao = f"Aluguel + Contrato {i+1}/5"
            else:
                valor_total = valor_aluguel
                descricao = "Aluguel"
            
            parcelas.append({
                'mes': data_parcela.strftime("%m/%Y"),
                'descricao': descricao,
                'valor': valor_total
            })
        
        return parcelas

def main():
    print("=== CALCULADORA DE ALUGUEL ===")
    
    # Criar calculadora
    calculadora = CalculadoraAluguel()
    
    # Escolher tipo de imóvel
    print("\nTipos de imóvel disponíveis:")
    print("1 - Apartamento de 1 quarto (R$ 700,00)")
    print("2 - Casa de 1 quarto (R$ 900,00)")
    print("3 - Estúdio (R$ 1200,00)")
    
    opcao = input("\nEscolha o tipo de imóvel (1-3): ")
    
    if opcao == "1":
        imovel = Imovel("Apartamento de 1 quarto", 700.00)
    elif opcao == "2":
        imovel = Imovel("Casa de 1 quarto", 900.00)
    elif opcao == "3":
        imovel = Imovel("Estúdio", 1200.00)
    else:
        print("Opção inválida!")
        return
    
    # Adicionais
    imovel.quartos_extras = int(input("Quantos quartos extras? "))
    imovel.vagas_garagem = int(input("Quantas vagas de garagem? "))
    
    # Desconto
    sem_filhos = input("Cliente não tem filhos? (s/n): ").lower()
    imovel.desconto_sem_filhos = (sem_filhos == 's')
    
    # Calcular aluguel
    valor_aluguel = calculadora.calcular_aluguel(imovel)
    parcelas = calculadora.gerar_parcelas(imovel, valor_aluguel)
    
    # Exibir resultados
    print("\n=== RESULTADO ===")
    print(f"Tipo: {imovel.tipo}")
    print(f"Quartos extras: {imovel.quartos_extras}")
    print(f"Vagas garagem: {imovel.vagas_garagem}")
    print(f"Desconto sem filhos: {'Sim' if imovel.desconto_sem_filhos else 'Não'}")
    print(f"Valor do aluguel mensal: R$ {valor_aluguel:.2f}")
    print(f"Contrato: R$ {calculadora.valor_contrato:.2f} em 5x")
    
    # Gerar arquivo CSV
    nome_arquivo = f"orcamento_{datetime.now().strftime('%Y%m%d_%H%M%S')}.csv"
    
    with open(nome_arquivo, 'w', newline='', encoding='utf-8') as arquivo:
        writer = csv.writer(arquivo)
        
        # Cabeçalho
        writer.writerow(['Parcela', 'Mês', 'Descrição', 'Valor (R$)'])
        
        # Parcelas
        for i, parcela in enumerate(parcelas, 1):
            writer.writerow([
                i,
                parcela['mes'],
                parcela['descricao'],
                f"{parcela['valor']:.2f}"
            ])
    
    print(f"\nArquivo '{nome_arquivo}' gerado com sucesso!")
    print("\nParcelas geradas:")
    for parcela in parcelas[:3]:  # Mostrar apenas as 3 primeiras
        print(f"{parcela['mes']}: {parcela['descricao']} - R$ {parcela['valor']:.2f}")
    print("...")

if __name__ == "__main__":
    main()

Primeiro repositório de muitos. 
