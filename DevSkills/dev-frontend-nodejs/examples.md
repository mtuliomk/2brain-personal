# Exemplos de código

Exemplos de referência para as convenções descritas em [SKILL.md](DevSkills/dev-frontend-nodejs/SKILL.md).

## Tipagem e componentes

```tsx
type UserCardProps = {
  user: {
    id: string;
    name: string;
    avatarUrl?: string;
  };
  onSelect: (userId: string) => void;
};

export function UserCard({ user, onSelect }: UserCardProps): JSX.Element {
  return (
    <article>
      {user.avatarUrl ? <img src={user.avatarUrl} alt="" /> : null}
      <h2>{user.name}</h2>
      <button type="button" onClick={() => onSelect(user.id)}>
        Selecionar
      </button>
    </article>
  );
}
```

## Formulário acessível

```tsx
<form onSubmit={handleSubmit} noValidate>
  <label htmlFor="email">E-mail</label>
  <input
    id="email"
    name="email"
    type="email"
    value={email}
    onChange={handleEmailChange}
    aria-invalid={Boolean(emailError)}
    aria-describedby={emailError ? 'email-error' : undefined}
  />
  {emailError ? <p id="email-error" role="alert">{emailError}</p> : null}
  <button type="submit" disabled={isSubmitting}>
    {isSubmitting ? 'Enviando…' : 'Enviar'}
  </button>
</form>
```

## Efeito assíncrono com cancelamento

```tsx
useEffect(() => {
  const controller = new AbortController();

  async function loadUser(): Promise<void> {
    setState({ status: 'loading' });

    try {
      const response = await fetch(`/api/users/${userId}`, {
        signal: controller.signal,
      });
      if (!response.ok) throw new Error('Falha ao carregar usuário');
      const user = await response.json() as unknown;
      setState({ status: 'success', user: parseUser(user) });
    } catch (error) {
      if (error instanceof DOMException && error.name === 'AbortError') return;
      setState({ status: 'error' });
    }
  }

  void loadUser();
  return () => controller.abort();
}, [userId]);
```

## Teste orientado ao comportamento

```tsx
it('permite selecionar um usuário pelo nome acessível', async () => {
  const onSelect = vi.fn();
  render(<UserCard user={{ id: 'u-1', name: 'Ana' }} onSelect={onSelect} />);

  await userEvent.click(screen.getByRole('button', { name: 'Selecionar' }));

  expect(onSelect).toHaveBeenCalledWith('u-1');
});
```
