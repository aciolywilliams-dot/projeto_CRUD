import sqlite3


def conectar():
    conexao = sqlite3.connect("academia.db")
    return conexao


def criar_tabela():
    conn = conectar()
    cursor = conn.cursor()

    cursor.execute("""
    CREATE TABLE IF NOT EXISTS alunos (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nome TEXT NOT NULL,
        cpf TEXT UNIQUE NOT NULL,
        data_nascimento TEXT NOT NULL,
        data_matricula TEXT NOT NULL,
        pagamento TEXT NOT NULL
    )
    """)

    conn.commit()
    conn.close()


def inserir_aluno(nome, cpf, data_nascimento, data_matricula, pagamento):
    conn = conectar()
    cursor = conn.cursor()

    cursor.execute("""
    INSERT INTO alunos (nome, cpf, data_nascimento, data_matricula, pagamento)
    VALUES (?, ?, ?, ?, ?)
    """, (nome, cpf, data_nascimento, data_matricula, pagamento))

    conn.commit()
    conn.close()


def listar_alunos():
    conn = conectar()
    cursor = conn.cursor()

    cursor.execute("SELECT * FROM alunos")
    alunos = cursor.fetchall()

    for aluno in alunos:
        print(aluno)

    conn.close()


def atualizar_pagamento(cpf, novo_pagamento):
    conn = conectar()
    cursor = conn.cursor()

    cursor.execute("""
    UPDATE alunos
    SET pagamento = ?
    WHERE cpf = ?
    """, (novo_pagamento, cpf))

    conn.commit()
    conn.close()


def deletar_aluno(cpf):
    conn = conectar()
    cursor = conn.cursor()

    cursor.execute("""
    DELETE FROM alunos
    WHERE cpf = ?
    """, (cpf,))

    conn.commit()
    conn.close()


if __name__ == "__main__":
    criar_tabela()
