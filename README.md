# microservices-proto

Definições Protocol Buffers (`.proto`) e código Go gerado para os microsserviços do projeto de Sistemas Distribuídos / Programação Distribuída (IFPB - Prof. Ruan Delgado Gomes).

## Estrutura

```
microservices-proto/
├── order/
│   └── order.proto
├── payment/
│   └── payment.proto
├── shipping/
│   └── shipping.proto
├── golang/
│   ├── order/        # código Go gerado (order.pb.go, order_grpc.pb.go)
│   ├── payment/       # código Go gerado
│   └── shipping/      # código Go gerado
└── run.sh
```

Cada serviço (`order`, `payment`, `shipping`) é publicado como um submódulo Go independente, sob o path `github.com/flaviorique10/microservices-proto/golang/<serviço>`, permitindo que cada microsserviço do repositório `microservices` importe apenas o proto de que precisa.

## Como gerar o código Go a partir dos `.proto`

Pré-requisitos: `protoc` instalado e disponível no `PATH`, além do Go.

```bash
# Edite SERVICE_NAME dentro de run.sh para o serviço que deseja regenerar
# (order, payment ou shipping), depois rode:
./run.sh
```

O script:
1. Instala/atualiza `protoc-gen-go` e `protoc-gen-go-grpc`.
2. Gera os arquivos `<serviço>.pb.go` e `<serviço>_grpc.pb.go` dentro de `golang/<serviço>/`.
3. Garante que o `go.mod` daquele submódulo está correto (`go mod init` / `go mod tidy`).

Após gerar, faça commit e push das mudanças — os outros microsserviços (no repositório `microservices`) consomem essa dependência via `go get`, baixando diretamente do GitHub.

## Serviços definidos

### Order (`order.proto`)
- `Order.Create(CreateOrderRequest) → CreateOrderResponse`
- Recebe cliente, itens do pedido (código, preço unitário, quantidade) e total.
- Retorna o ID do pedido criado e o prazo de entrega calculado (`delivery_days`).

### Payment (`payment.proto`)
- `Payment.Create(CreatePaymentRequest) → CreatePaymentResponse`
- Recebe usuário, ID do pedido e valor total a cobrar.

### Shipping (`shipping.proto`)
- `ShippingService.GetDeliveryTime(ShippingRequest) → ShippingResponse`
- Recebe o ID do pedido e a lista de itens (código + quantidade).
- Retorna o prazo de entrega em dias: **1 dia mínimo + 1 dia adicional a cada 5 unidades** do total pedido.
