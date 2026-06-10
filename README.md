# Microsserviços - Contratos gRPC (Proto)

Este repositório contém a definição dos contratos de comunicação (`.proto`) e os códigos em Go gerados automaticamente pelo compilador do Protocol Buffers (protoc) para o sistema de gerenciamento de pedidos (Order).

## 📌 Estrutura
* `order/order.proto`: Arquivo de definição do serviço e das mensagens gRPC.
* `golang/order/`: Código Go gerado automaticamente contendo as interfaces do servidor e as structs das requisições.

## 🚀 Como este repositório é utilizado
Este projeto não é executado isoladamente. Ele atua como um módulo importado pelo microsserviço principal. A lógica de negócio e o servidor gRPC real estão implementados no repositório principal da aplicação.
