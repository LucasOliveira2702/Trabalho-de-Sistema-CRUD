#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define ARQUIVO "pessoas.bin"

typedef struct {
    char nome[100];
    char cpf[12];  // 11 dígitos + '\0'
    int idade;
    char email[100];
} Pessoa;

/*-------------------------------------------------------------*/
/* Função para verificar se um CPF já está cadastrado */
int cpf_existe(const char *cpf) {
    FILE *f = fopen(ARQUIVO, "rb");
    if (!f) return 0;

    Pessoa p;
    while (fread(&p, sizeof(Pessoa), 1, f)) {
        if (strcmp(p.cpf, cpf) == 0) {
            fclose(f);
            return 1;
        }
    }
    fclose(f);
    return 0;
}

/*-------------------------------------------------------------*/
/* CREATE – Cadastrar nova pessoa */
void cadastrar() {
    Pessoa p;

    printf("Nome: ");
    fgets(p.nome, 100, stdin);
    p.nome[strcspn(p.nome, "\n")] = 0;

    printf("CPF (11 dígitos): ");
    fgets(p.cpf, 12, stdin);
    p.cpf[strcspn(p.cpf, "\n")] = 0;

    if (cpf_existe(p.cpf)) {
        printf("❌ CPF já cadastrado!\n");
        return;
    }

    printf("Idade: ");
    scanf("%d", &p.idade);
    getchar();

    printf("E-mail: ");
    fgets(p.email, 100, stdin);
    p.email[strcspn(p.email, "\n")] = 0;

    FILE *f = fopen(ARQUIVO, "ab");
    fwrite(&p, sizeof(Pessoa), 1, f);
    fclose(f);

    printf("✔ Cadastro realizado com sucesso!\n");
}

/*-------------------------------------------------------------*/
/* READ – Listar todas as pessoas */
void listar() {
    FILE *f = fopen(ARQUIVO, "rb");
    if (!f) {
        printf("Nenhum cadastro encontrado.\n");
        return;
    }

    Pessoa p;
    printf("\n---- LISTA DE PESSOAS ----\n");
    while (fread(&p, sizeof(Pessoa), 1, f)) {
        printf("\nNome: %s\nCPF: %s\nIdade: %d\nE-mail: %s\n",
               p.nome, p.cpf, p.idade, p.email);
    }
    fclose(f);
}

/*-------------------------------------------------------------*/
/* READ – Buscar por CPF */
void buscar() {
    char cpf[12];
    printf("CPF a buscar: ");
    fgets(cpf, 12, stdin);
    cpf[strcspn(cpf, "\n")] = 0;

    FILE *f = fopen(ARQUIVO, "rb");
    if (!f) {
        printf("Arquivo inexistente.\n");
        return;
    }

    Pessoa p;
    while (fread(&p, sizeof(Pessoa), 1, f)) {
        if (strcmp(p.cpf, cpf) == 0) {
            printf("\nEncontrado!\nNome: %s\nIdade: %d\nE-mail: %s\n",
                   p.nome, p.idade, p.email);
            fclose(f);
            return;
        }
    }

    printf("❌ Pessoa não encontrada.\n");
    fclose(f);
}

/*-------------------------------------------------------------*/
/* UPDATE – Atualizar por CPF */
void atualizar() {
    char cpf[12];
    printf("CPF a atualizar: ");
    fgets(cpf, 12, stdin);
    cpf[strcspn(cpf, "\n")] = 0;

    FILE *f = fopen(ARQUIVO, "rb");
    if (!f) {
        printf("Arquivo inexistente.\n");
        return;
    }

    FILE *temp = fopen("temp.bin", "wb");
    Pessoa p;
    int encontrado = 0;

    while (fread(&p, sizeof(Pessoa), 1, f)) {
        if (strcmp(p.cpf, cpf) == 0) {
            encontrado = 1;

            printf("Novo nome: ");
            fgets(p.nome, 100, stdin);
            p.nome[strcspn(p.nome, "\n")] = 0;

            printf("Nova idade: ");
            scanf("%d", &p.idade);
            getchar();

            printf("Novo e-mail: ");
            fgets(p.email, 100, stdin);
            p.email[strcspn(p.email, "\n")] = 0;
        }
        fwrite(&p, sizeof(Pessoa), 1, temp);
    }

    fclose(f);
    fclose(temp);

    remove(ARQUIVO);
    rename("temp.bin", ARQUIVO);

    if (encontrado)
        printf("✔ Atualização concluída!\n");
    else
        printf("❌ CPF não encontrado.\n");
}

/*-------------------------------------------------------------*/
/* DELETE – Remover por CPF */
void remover_pessoa() {
    char cpf[12];
    printf("CPF a remover: ");
    fgets(cpf, 12, stdin);
    cpf[strcspn(cpf, "\n")] = 0;

    FILE *f = fopen(ARQUIVO, "rb");
    if (!f) {
        printf("Arquivo inexistente.\n");
        return;
    }

    FILE *temp = fopen("temp.bin", "wb");
    Pessoa p;
    int encontrado = 0;

    while (fread(&p, sizeof(Pessoa), 1, f)) {
        if (strcmp(p.cpf, cpf) == 0) {
            encontrado = 1;
            continue;  // não grava o removido
        }
        fwrite(&p, sizeof(Pessoa), 1, temp);
    }

    fclose(f);
    fclose(temp);

    remove(ARQUIVO);
    rename("temp.bin", ARQUIVO);

    if (encontrado)
        printf("✔ Pessoa removida.\n");
    else
        printf("❌ CPF não encontrado.\n");
}

/*-------------------------------------------------------------*/
/* MENU PRINCIPAL */
int main() {
    int opcao;

    do {
        printf("\n===== MENU =====\n");
        printf("1. Cadastrar pessoa\n");
        printf("2. Listar pessoas\n");
        printf("3. Buscar por CPF\n");
        printf("4. Atualizar pessoa\n");
        printf("5. Remover pessoa\n");
        printf("6. Sair\n");
        printf("Escolha: ");
        scanf("%d", &opcao);
        getchar(); // limpar buffer

        switch(opcao) {
            case 1: cadastrar(); break;
            case 2: listar(); break;
            case 3: buscar(); break;
            case 4: atualizar(); break;
            case 5: remover_pessoa(); break;
            case 6: printf("Saindo...\n"); break;
            default: printf("Opcao inválida!\n");
        }
    } while (opcao != 6);

    return 0;
}
