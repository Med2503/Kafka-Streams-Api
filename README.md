# 🏦 Kafka Streams - Bank Transactions Processor

A real-time **stream processing application** using **Kafka Streams** to manage bank balances and handle rejected transactions efficiently.

---

## 📌 Overview

This application processes **bank transactions** in real-time, maintains **customer balances** in a **stateful Kafka Streams topology**, and handles **rejected transactions** by routing them to a separate topic for error handling.

The system uses:

- **Kafka Topics** for input and output streams
- **Kafka Streams DSL** for stateful stream processing
- **Docker Compose** for setting up Kafka infrastructure

---

## 🧩 Key Concepts

### 🧾 BankTransaction

Represents a customer's transaction:

- `amount`: positive (credit) or negative (debit)
- Other transaction metadata (timestamp, ID, etc.)

### 💰 BankBalance

Represents a customer's current account state:

- Current balance
- Metadata from the most recent transaction

---

## 🔁 Kafka Streams Topology

The processing logic performs the following:

1. **Read from `bank-transactions` topic**  
   → `key = balanceId`

2. **Group by key**  
   → Using `groupByKey()` to organize transactions per user

3. **Aggregate transactions into balances**  
   → `aggregate()` transactions per user into a `BankBalance`  
   → Store results in a **State Store**

4. **Convert KTable to KStream**  
   → Emits every balance update as a stream

5. **Write updated balances to `bank-balances` topic**

6. **Extract last transaction from balance and map it to a BankTransaction**

7. **Filter REJECTED transactions**

8. **Write rejected ones to `rejected-transactions` topic**

---

## 🛠️ Technologies Used

- **Java 17**
- **Spring Boot**
- **Apache Kafka**
- **Kafka Streams**
- **Docker Compose**
- **Lombok (optional)**


## 🚀 Running the Project

### 1. Start Kafka Infrastructure

This project includes a pre-configured `docker-compose.yml` file to launch Kafka and Zookeeper, and a script to create required topics.

```bash
docker compose -f ./docker-compose.yml up


Project Structure

kafka-streams-bank-processor/
├── src/
│   ├── model/              # BankTransaction & BankBalance classes
│   ├── topology/           # Kafka Streams topology builder
│   └── config/             # Kafka configuration
├── docker-compose.yml
├── create-topics.sh
└── README.md


📊 Topics Used
Topic Name	Purpose
bank-transactions	Input stream of all bank transactions
bank-balances	Output stream of updated balances
rejected-transactions	Output stream of rejected operations




