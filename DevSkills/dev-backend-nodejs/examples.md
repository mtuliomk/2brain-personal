# Exemplos de código

Exemplos de referência para as convenções descritas em [Skill.md](DevSkills/dev-backend-nodejs/SKILL.md).

## Tipagem

```typescript
type CreateUserInput = {
  name: string;
  email: string;
};

interface IUserRepository {
  findById(id: string): Promise<User | null>;
}

class UserRepository implements IUserRepository {
  async findById(id: string): Promise<User | null> {
    // ...
  }
}
```

## Tratamento de erros e logs

```typescript
try {
  await paymentGateway.charge(orderId, amount);
} catch (error) {
  logger.error('Falha ao processar cobrança', { orderId, amount, error });
  throw error;
}
```

## Estilo e implementação

### Concorrência limitada

```typescript
import pLimit from 'p-limit';

const limit = pLimit(5);

const results = await Promise.all(
  items.map((item) => limit(() => processItem(item)))
);
```

### Injeção de dependências

```typescript
class OrderService {
  constructor(
    private readonly orderRepository: IOrderRepository,
    private readonly paymentGateway: IPaymentGateway,
    private readonly logger: ILogger,
  ) {}

  async completeOrder(orderId: string): Promise<void> {
    // usa this.orderRepository, this.paymentGateway e this.logger
  }
}
```
