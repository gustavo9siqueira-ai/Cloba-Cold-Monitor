# ❄ Cloba ColdMonitor — Spring Boot + H2 + Spring Data JPA

Sistema de monitoramento de temperaturas de equipamentos de refrigeração.
**Versão 2.0** — Todos os dados são persistidos em banco de dados real.

---

## 🗂 Estrutura do Projeto

```
ClobaColdMonitor/
├── pom.xml
└── src/main/
    ├── java/com/cloba/coldmonitor/
    │   ├── ClobaColdMonitorApplication.java   ← Main (Spring Boot + Swing)
    │   ├── model/
    │   │   ├── Usuario.java                   ← Entidade JPA: usuários
    │   │   ├── Equipamento.java               ← Entidade JPA: equipamentos
    │   │   └── RegistroTemperatura.java       ← Entidade JPA: histórico de temperaturas
    │   ├── repository/
    │   │   ├── UsuarioRepository.java
    │   │   ├── EquipamentoRepository.java
    │   │   └── RegistroTemperaturaRepository.java
    │   ├── service/
    │   │   ├── UsuarioService.java
    │   │   ├── EquipamentoService.java
    │   │   └── RegistroTemperaturaService.java
    │   └── ui/
    │       ├── LoginFrame.java
    │       ├── CadastroUsuarioDialog.java
    │       ├── MenuFrame.java
    │       ├── CadastroFrame.java
    │       ├── RegistroFrame.java
    │       ├── HistoricoFrame.java
    │       ├── AlertasFrame.java
    │       ├── RelatoriosFrame.java
    │       └── SobreFrame.java
    └── resources/
        └── application.properties             ← Configuração H2 + JPA
```

---

## ⚙ Tecnologias

| Tecnologia         | Versão | Uso                                    |
|--------------------|--------|----------------------------------------|
| Java               | 17+    | Linguagem principal                    |
| Spring Boot        | 3.2.3  | Framework principal                    |
| Spring Data JPA    | —      | Abstração para banco de dados          |
| Hibernate          | —      | ORM (mapeamento objeto-relacional)     |
| H2 Database        | —      | Banco de dados em arquivo local        |
| Java Swing         | —      | Interface gráfica desktop              |

---

## 🗄 Banco de Dados

O banco H2 é criado automaticamente como arquivo local:
- **Arquivo:** `coldmonitor-db.mv.db` (criado na pasta onde você rodar o .jar)
- **Tabelas criadas automaticamente:** `usuarios`, `equipamentos`, `registros_temperatura`
- **Usuário inicial:** `admin` / `admin123` (criado automaticamente se não houver usuários)

---

## 🚀 Como Rodar

### Pré-requisitos
- Java 17 ou superior
- Maven 3.6+ (`mvn --version` para verificar)

### Passo a passo

```bash
# 1. Entre na pasta do projeto
cd ClobaColdMonitor

# 2. Compile e gere o JAR
mvn clean package -DskipTests

# 3. Execute
java -jar target/coldmonitor-1.0.0.jar
```

### Atalho (compile + execute em um comando)
```bash
mvn spring-boot:run
```

---

## 🔐 Login

Na primeira execução, o sistema cria automaticamente:
- **Login:** `admin`
- **Senha:** `admin123`

Você pode criar novos usuários clicando em **"Novo Usuário"** na tela de login.

---

## 📋 O Que Mudou em Relação à Versão Original

| Antes (mockado)                        | Agora (banco de dados)                          |
|----------------------------------------|-------------------------------------------------|
| Login fixo `admin` / `123` no código   | Usuários cadastrados e autenticados pelo banco  |
| Equipamentos hardcoded no ComboBox     | Equipamentos cadastrados na tela + salvos no BD |
| Histórico com dados fake na tabela     | Histórico real buscado do banco com filtros     |
| Alertas mockados na lista              | Alertas calculados a partir de registros reais  |
| Relatório com números fixos            | Relatório com MAX/MIN/MÉDIA calculados por query|
| Nada era salvo                         | Tudo é persistido em `coldmonitor-db.mv.db`     |

---

## 🏗 Fluxo de Uso Recomendado

1. **Login** → entre com `admin` / `admin123`
2. **Cadastro de Equipamentos** → adicione geladeiras, freezers etc. com limites de temperatura
3. **Registro de Temperaturas** → registre leituras para os equipamentos cadastrados
4. **Alertas** → veja automaticamente quais leituras estão fora do limite
5. **Histórico** → filtre por equipamento ou visualize todas as leituras
6. **Relatórios** → veja estatísticas (máx/mín/média) calculadas para hoje

---

## 💡 Notas Técnicas

- O banco H2 usa modo **FILE** (não in-memory), então os dados **persistem** entre execuções
- O Hibernate cria/atualiza as tabelas automaticamente (`ddl-auto=update`)
- O status de cada registro é calculado automaticamente: `OK`, `ALERTA` ou `CRÍTICO`
- `ALERTA` = dentro da margem de 10% dos limites; `CRÍTICO` = fora dos limites
