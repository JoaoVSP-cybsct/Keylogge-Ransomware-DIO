# Keylogge-Ransomware-DIO# 

# Ransomware & Keylogger em Python

Este repositório contém a implementação de simulações de malwares (Ransomware e Keylogger) desenvolvidos em Python. O projeto foi realizado como parte do desafio do Bootcamp de Cibersegurança, com o objetivo de compreender o funcionamento ofensivo para fortalecer estratégias defensivas.

> **⚠️ AVISO LEGAL:** Este projeto foi desenvolvido estritamente para fins educacionais e acadêmicos em ambiente controlado. O uso desses scripts para atacar sistemas sem consentimento prévio é ilegal e antiético.

## 🛠️ Ferramentas e Bibliotecas Utilizadas

* **Python 3**
* **Cryptography (Fernet):** Para implementação da criptografia simétrica.
* **Pynput:** Para captura de eventos do teclado.
* **OS / IO:** Para manipulação de arquivos e sistema operacional.

---

## 📂 Estrutura do Projeto

### 1. Ransomware (Criptografia de Arquivos)

O conjunto de scripts simula um ataque onde arquivos são sequestrados e só podem ser recuperados com uma chave específica.

* **`ransomware.py`**:
    * Gera uma chave de criptografia simétrica (`chave.key`).
    * Percorre o diretório atual procurando arquivos.
    * **Lógica de Proteção:** Possui uma lista de exceções (Allowlist) para não criptografar o próprio script, a chave ou a nota de resgate.
    * Criptografa os arquivos encontrados sobrescrevendo os dados originais.
    * Gera um arquivo `LEIA ISSO.txt` com o pedido de resgate.
* **`descriptografar.py`**:
    * Lê a chave gerada (`chave.key`).
    * Percorre os arquivos criptografados.
    * Reverte o processo, devolvendo o conteúdo original dos arquivos.

### 2. Keylogger (Captura de Teclas)

O script monitora a entrada do teclado do usuário e registra o conteúdo em um arquivo de log local.

* **`keylogger.py`**:
    * Utiliza a biblioteca `pynput` para escutar eventos de *press* (pressionar tecla).
    * **Tratamento de Dados:** Formata teclas especiais (Space, Enter, Tab) para manter o texto legível e ignora teclas de controle (Shift, Ctrl, Alt) para evitar poluição visual.
    * Salva tudo em tempo real no arquivo `log.txt`.
* **`keylogger_em.py`**:
    * **Exfiltração via E-mail:** Estende a funcionalidade básica integrando a biblioteca `smtplib` para envio de dados via protocolo SMTP.
    * **Ciclos de Reporte:** Configurado para acumular as teclas e enviar o log por e-mail em intervalos regulares (ex: a cada 60 segundos), simulando o roubo ativo de credenciais.
---

## 🚀 Passo a passo

### Pré-requisitos
Instale as bibliotecas necessárias:
```bash 
pip install cryptography pynput
```

## Simulando o Ransomware:

Crie uma pasta de teste e coloque alguns arquivos de texto (.txt) dentro.


<img width="350" height="283" alt="image" src="https://github.com/user-attachments/assets/01214c38-baa0-4fef-8df3-f2e12f4faab5" />




Execute o malware: python ransomware.py.


<img width="1016" height="205" alt="py" src="https://github.com/user-attachments/assets/85b1c887-ca5e-49ae-95df-4cfc8c19d82d" />




Resultado: Os arquivos ficarão ilegíveis e o arquivo chave.key será criado juntamente com o arquivo LEIA ISSO.txt.


<img width="1168" height="279" alt="image" src="https://github.com/user-attachments/assets/9bce1e38-489a-4fbf-b508-db09f324de6a" />
<img width="820" height="242" alt="image" src="https://github.com/user-attachments/assets/e8c4bb68-759f-4364-beff-7c7ec9d2f135" />


## Recuperação de arquivos

Execute o decifrador: python descriptografar.py.



<img width="933" height="178" alt="image" src="https://github.com/user-attachments/assets/e227e50b-e1a9-4f73-84ff-897d6d719528" />


Resultado: Os arquivos voltam ao texto original.



<img width="670" height="280" alt="image" src="https://github.com/user-attachments/assets/770f8490-8170-4cd0-9645-a83e95ab7463" />
<img width="601" height="271" alt="image" src="https://github.com/user-attachments/assets/24f2183c-8177-413a-86d9-b507df605035" />


---
## Simulando o Keylogger

Execute o script: python keylogger.py.

Digite qualquer texto em outra janela (bloco de notas, navegador).

![Imagem do WhatsApp de 2025-11-19 à(s) 01 16 21_f1c75dc5](https://github.com/user-attachments/assets/b8e18977-f352-487c-abb5-288d5d6cff46)



Verifique o arquivo log.txt criado na pasta. O conteúdo digitado estará lá.

<img width="1615" height="284" alt="image" src="https://github.com/user-attachments/assets/2dd66ad1-4ff2-4897-b80e-f8324983e375" />

## Keylogger com envio de dados para o e-mail

! Antes de tudo crie um e-mail para testes e ative a configuração em 2 etapas.

Acesse o site:
```bash
account.google.com/apppasswords
```
E crie uma senha para utilizar durante o teste:


<img width="1223" height="800" alt="image" src="https://github.com/user-attachments/assets/c26d5a13-e04f-4c96-a840-503dc0b6c082" />

Execute o código e digite algo no computador para teste.


<img width="1245" height="227" alt="image" src="https://github.com/user-attachments/assets/cf47a104-8176-44de-9d82-89c0b372bfec" />


Após o teste somente aguardar 1 minuto para que chegue no e-mail informado.

<img width="1514" height="421" alt="image" src="https://github.com/user-attachments/assets/0fbdd923-4548-4a5f-a224-6cb1d39636f4" />

---

# 🛡️ Reflexão sobre Defesa (Blue Team)

O objetivo principal deste estudo é entender o ataque para construir a defesa. Abaixo, listo as medidas de mitigação para as ameaças simuladas:

## Contra Ransomware:

1- Backups Offline: A defesa mais eficaz. Se os arquivos forem criptografados, basta restaurar o backup que não estava conectado à rede.

2- Análise Heurística: Antivírus modernos detectam o comportamento de "abrir e sobrescrever" múltiplos arquivos rapidamente, bloqueando o processo antes que o dano seja total.

3- Listas de Controle de Acesso (ACL): Restringir permissões de escrita em pastas críticas impede que o malware altere arquivos do sistema.

4- Exibir Extensões de Arquivos: Muitos usuários são enganados por arquivos como documento.pdf.exe. Configurar o Windows para mostrar extensões ajuda a identificar executáveis suspeitos.

## Contra Keyloggers:

1- Múltiplo Fator de Autenticação (MFA): Mesmo que o keylogger capture a senha digitada, o atacante não terá o código do token (celular/app), impedindo o acesso à conta.

2- Teclados Virtuais: Em sites bancários, o uso do mouse para clicar nas teclas evita que o pynput ou hooks de teclado capturem a senha.

3- Monitoramento de Processos: Verificar o Gerenciador de Tarefas por processos suspeitos (muitas vezes com nomes genéricos como python.exe ou svchost.exe rodando fora do padrão) ajuda na detecção.
