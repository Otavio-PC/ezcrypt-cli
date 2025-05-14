# ezcript-cli

**ezcript-cli** é uma ferramenta de linha de comando escrita em C para **criptografar e descriptografar arquivos** simples, funcionando exclusivamente em sistemas baseados em Unix/Linux.

> ⚠️ **Atenção**: Este projeto possui fins **educacionais** e **experimentais**. Não deve ser utilizado como método de segurança real em ambientes críticos ou que demandem criptografia robusta.

---

## ✨ Sobre o projeto

O **ezcript-cli** utiliza uma abordagem simples para criptografia, baseada em modificações binárias previsíveis e mapeadas. Ele **armazena as informações necessárias para a descriptografia dentro do próprio arquivo criptografado**, aproveitando espaços fixos determinados logicamente a partir da estrutura do arquivo.

Essa técnica é possível graças ao uso de funções de posicionamento de stream como:

- `SEEK_END` (fim do arquivo),
- `SEEK_SET` (início do arquivo),
- e manipulação direta de bits usando `fseek`, `fread`, `fwrite`.

A lógica se baseia na alteração de cada bit do arquivo conforme um algoritmo definido (exemplo: inversão de bits, rotação, XOR simples etc.), e os metadados (como senha ou identificador do algoritmo usado) são gravados nas posições finais ou específicas do arquivo. O programa entende como localizar e interpretar essas posições com precisão, garantindo a reversibilidade da operação.

---

## 🧠 Como funciona

1. O usuário fornece o arquivo original e uma senha de um caractere.
2. O programa aplica um algoritmo de criptografia definido (personalizável).
3. A senha e dados de controle são armazenados **dentro do arquivo criptografado**.
4. O arquivo é regravado com as alterações e tem suas permissões modificadas para maior proteção.
5. Para descriptografar, basta executar novamente o `ezcript-cli` no terminal e informar a senha.
6. O programa valida as informações armazenadas no arquivo e reverte os bits conforme necessário.

---

## 🔐 Segurança e limitações

Este programa **não utiliza criptografia segura** nem algoritmos como AES, RSA ou curvas elípticas. Ele também **não armazena a senha em arquivos externos**, evitando dependência de outros recursos — o que simplifica, mas enfraquece a segurança.

Além disso, como o arquivo criptografado **depende da integridade dos seus próprios bits modificados**, qualquer edição manual ou corrupção pode torná-lo impossível de restaurar.

---

## 🛠️ Tecnologias utilizadas

- **C (ISO C99)**
- Manipulação de arquivos com `fopen`, `fread`, `fwrite`, `fseek`
- Bibliotecas do sistema Linux:
  - `<unistd.h>` (para permissões e usuário)
  - `<sys/stat.h>` (para `chmod`, `chown`, etc.)

---

## ⚙️ Requisitos e compatibilidade

- Sistemas suportados: **Linux** (qualquer distribuição moderna com libc)
- Compilador: `gcc` ou `clang`
- Acesso a permissões de leitura/escrita dos arquivos a serem criptografados
- Terminal para execução do programa via CLI

---

## 🧪 Exemplos de Casos de Uso

```bash
$ ./ezcript-cli encrypt my_file.pdf
Enter password: A
File encrypted successfully!

$ ./ezcript-cli decrypt my_file.pdf
Enter password: A
Decryption complete.