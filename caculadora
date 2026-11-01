#include <stdio.h>
#include <math.h>

#define MAX_TAXA 10.0
#define ERRO 0.000000001

// Calcula o valor futuro
double calcularFV(double pv, double pmt, double i, double n) {
    if (i == 0)
        return pv + pmt * n;

    double fator = pow(1 + i, n);

    return pv * fator + pmt * (fator - 1) / i;
}

// Calcula o valor presente
double calcularPV(double fv, double pmt, double i, double n) {
    if (i == 0)
        return fv - pmt * n;

    double fator = pow(1 + i, n);

    return (fv - pmt * (fator - 1) / i) / fator;
}

// Calcula o pagamento periódico
double calcularPMT(double pv, double fv, double i, double n) {
    if (i == 0)
        return (fv - pv) / n;

    double fator = pow(1 + i, n);

    return (fv - pv * fator) * i / (fator - 1);
}

// Calcula o número de períodos
double calcularN(double pv, double pmt, double fv, double i) {
    if (i == 0) {
        if (pmt == 0) return -1;
        return (fv - pv) / pmt;
    }

    if (pmt == 0) {
        if (pv <= 0 || fv <= 0 || i <= -1)
            return -1;

        return log(fv / pv) / log(1 + i);
    }

    double denominador = pv + pmt / i;
    double numerador = fv + pmt / i;

    if (denominador == 0 || numerador / denominador <= 0)
        return -1;

    return log(numerador / denominador) / log(1 + i);
}

// Função auxiliar para calcular juros
double funcaoJuros(double i, double pv, double pmt,
                   double fv, double n) {
    return calcularFV(pv, pmt, i, n) - fv;
}

// Calcula a taxa de juros por bisseção
double calcularI(double pv, double pmt, double fv, double n) {
    double inicio = 0.0;
    double fim = MAX_TAXA;

    double fInicio = funcaoJuros(inicio, pv, pmt, fv, n);
    double fFim = funcaoJuros(fim, pv, pmt, fv, n);

    if (fabs(fInicio) < ERRO)
        return 0;

    if (fInicio * fFim > 0)
        return -1;

    for (int k = 0; k < 200; k++) {
        double meio = (inicio + fim) / 2;
        double fMeio = funcaoJuros(meio, pv, pmt, fv, n);

        if (fabs(fMeio) < ERRO)
            return meio;

        if (fInicio * fMeio < 0) {
            fim = meio;
        } else {
            inicio = meio;
            fInicio = fMeio;
        }
    }

    return (inicio + fim) / 2;
}

int main() {
    int opcao;
    double n = 0, i = 0, pv = 0, pmt = 0, fv = 0;
    double resultado;

    printf("====================================\n");
    printf("       CALCULADORA FINANCEIRA\n");
    printf("       TIME VALUE OF MONEY\n");
    printf("====================================\n");

    printf("\nQual variavel deseja calcular?\n");
    printf("1 - Numero de periodos (n)\n");
    printf("2 - Taxa de juros (i)\n");
    printf("3 - Valor presente (PV)\n");
    printf("4 - Pagamento periodico (PMT)\n");
    printf("5 - Valor futuro (FV)\n");

    printf("\nEscolha: ");
    scanf("%d", &opcao);

    if (opcao < 1 || opcao > 5) {
        printf("Opcao invalida!\n");
        return 1;
    }

    printf("\nInforme os valores conhecidos:\n");

    if (opcao != 1) {
        printf("Numero de periodos (n): ");
        scanf("%lf", &n);
    }

    if (opcao != 2) {
        printf("Taxa de juros (i em decimal): ");
        scanf("%lf", &i);
    }

    if (opcao != 3) {
        printf("Valor presente (PV): ");
        scanf("%lf", &pv);
    }

    if (opcao != 4) {
        printf("Pagamento (PMT): ");
        scanf("%lf", &pmt);
    }

    if (opcao != 5) {
        printf("Valor futuro (FV): ");
        scanf("%lf", &fv);
    }

    if (n < 0 || i < 0) {
        printf("\nErro: n e i nao podem ser negativos.\n");
        return 1;
    }

    switch (opcao) {

        case 1:
            resultado = calcularN(pv, pmt, fv, i);
            printf("\nNumero de periodos (n): %.6lf\n", resultado);
            break;

        case 2:
            resultado = calcularI(pv, pmt, fv, n);

            if (resultado < 0)
                printf("\nNao foi possivel encontrar a taxa.\n");
            else
                printf("\nTaxa de juros (i): %.6lf%%\n",
                       resultado * 100);
            break;

        case 3:
            resultado = calcularPV(fv, pmt, i, n);
            printf("\nValor presente (PV): %.2lf\n", resultado);
            break;

        case 4:
            if (n == 0) {
                printf("\nErro: numero de periodos nao pode ser zero.\n");
                break;
            }

            resultado = calcularPMT(pv, fv, i, n);
            printf("\nPagamento (PMT): %.2lf\n", resultado);
            break;

        case 5:
            resultado = calcularFV(pv, pmt, i, n);
            printf("\nValor futuro (FV): %.2lf\n", resultado);
            break;
    }

    printf("\n====================================\n");

    return 0;
}
