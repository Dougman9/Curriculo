# Curriculo

from reportlab.lib.pagesizes import letter
from reportlab.pdfgen import canvas
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.platypus import Paragraph, SimpleDocTemplate, Spacer
import os
import platform

def criar_curriculo(nome, endereco, contato, formacao, cursos_extras, experiencias, filename="curriculo.pdf"):
    # Cria um documento PDF
    pdf = SimpleDocTemplate(filename, pagesize=letter)
    styles = getSampleStyleSheet()
    conteudo = []

    # Adiciona o título
    titulo = Paragraph(f"<b>Currículo de {nome}</b>", styles['Title'])
    conteudo.append(titulo)
    conteudo.append(Spacer(1, 12))

    # Adiciona as seções
    conteudo.append(Paragraph("<b>Endereço:</b>", styles['Heading2']))
    conteudo.append(Paragraph(endereco, styles['BodyText']))
    conteudo.append(Spacer(1, 12))

    conteudo.append(Paragraph("<b>Contato:</b>", styles['Heading2']))
    conteudo.append(Paragraph(contato, styles['BodyText']))
    conteudo.append(Spacer(1, 12))

    conteudo.append(Paragraph("<b>Formação:</b>", styles['Heading2']))
    conteudo.append(Paragraph(formacao, styles['BodyText']))
    conteudo.append(Spacer(1, 12))

    conteudo.append(Paragraph("<b>Experiência Profissional:</b>", styles['Heading2']))
    for experiencia in experiencias:
        conteudo.append(Paragraph(f"- {experiencia}", styles['BodyText']))
    conteudo.append(Spacer(1, 12))

    conteudo.append(Paragraph("<b>Cursos Extras:</b>", styles['Heading2']))
    for curso in cursos_extras:
        conteudo.append(Paragraph(f"- {curso}", styles['BodyText']))
    conteudo.append(Spacer(1, 12))

    # Constrói o PDF
    pdf.build(conteudo)

def abrir_pdf(filename):
    # Abre o arquivo PDF no visualizador padrão do sistema
    sistema_operacional = platform.system()
    if sistema_operacional == "Windows":
        os.startfile(filename)
    elif sistema_operacional == "Darwin":  # macOS
        os.system(f"open {filename}")
    else:  # Linux e outros
        os.system(f"xdg-open {filename}")

def main():
    # Solicita informações do usuário
    nome = input("Digite seu nome: ")
    endereco = input("Digite seu endereço: ")
    contato = input("Digite seu contato (telefone, e-mail, LinkedIn, etc.): ")
    formacao = input("Digite sua formação: ")

    # Coleta cursos extras
    cursos_extras = []
    while True:
        curso = input("Digite um curso extra (ou deixe em branco para terminar): ")
        if curso == "":
            break
        cursos_extras.append(curso)

    # Coleta experiências profissionais
    experiencias = []
    while True:
        experiencia = input("Digite uma experiência profissional (ou deixe em branco para terminar): ")
        if experiencia == "":
            break
        experiencias.append(experiencia)

    # Gera o currículo em PDF
    filename = "curriculo.pdf"
    criar_curriculo(nome, endereco, contato, formacao, cursos_extras, experiencias, filename)
    print("Currículo gerado com sucesso!")

    # Abre o PDF automaticamente
    abrir_pdf(filename)

if __name__ == "__main__":
    main()
