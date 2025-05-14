# ezcript-cli

A simple, experimental file encryption CLI tool written in C.

🌐 Available in other languages: [🇧🇷 Leia em Português](i18n/README_pt.md)

**Designed for educational and portfolio purposes only**.

---

## 📦 About the Project

`ezcript-cli` is a terminal-based program that performs basic file encryption and decryption by manipulating binary data directly in a Linux environment.

Its key feature is that **all the necessary information for decryption is stored within the encrypted file itself**, at specific, known bit positions. These reference points are managed by the program using standard file seeking constants such as `SEEK_END`, `SEEK_SET`, etc.

This makes the program self-contained: the same binary is used to both encrypt and decrypt, with no external key storage needed.

> ⚠️ **Warning:** This project is not suitable for real-world encryption purposes. It is intentionally simple and should only be used for learning and experimentation.

---

## 💡 How It Works (Logic Overview)

1. The user selects a file and provides a password (a single character in this implementation).
2. The encryption algorithm modifies the file’s binary content using a reversible bitwise logic (e.g., inversion of bits).
3. Metadata and decryption markers — including the "password" — are saved inside the file, in bits located at predefined offsets.
4. The original file is transformed and made readable only to the current user (using Linux file permission control with `chmod`/`chown`).
5. To decrypt, the program reads the embedded metadata and reverses the transformation — as long as the correct password is given.

---

## ⚙️ System Requirements

- Linux-based operating system  
- GCC or any standard C compiler  
- Terminal access  
- Root or user permission to change file modes and owners (for `chmod`, `chown`)

---

## 🔐 Important Notes

- This is **not a secure encryption algorithm**. It is vulnerable to inspection, reverse engineering, and should **never** be used to store sensitive data.
- Its purpose is to demonstrate:
  - Bitwise file manipulation
  - Embedded metadata techniques
  - Basic file permission handling in C
  - CLI application development

---

## 🧪 Example Use Cases

```bash
$ ./ezcript-cli encrypt my_file.pdf
Enter password: A
File encrypted successfully!

$ ./ezcript-cli decrypt my_file.pdf
Enter password: A
Decryption complete.
