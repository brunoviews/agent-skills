# Example: a product detail view

A complete view, file by file. It loads a product, shows how much stock is left, lets the user pick a quantity and add it to the cart. Small, but it has every kind of thing a view holds: loading and error states, a business threshold, a derived value, bounded actions and a semantic visual state.

It uses styled-components with a theme that has `colors`, `spacing`, `radius`, `typography` and `touch` tokens. With another styling library the split is the same; only `styles.ts` changes.

This code compiles under `strict` TypeScript.

```
src/views/productDetail/
├── types.ts
├── viewmodel.ts
├── styles.ts
└── index.tsx
```

---

## `types.ts` — the contract

Written first: the props, the data shape, and the one semantic state the other three files agree on.

```ts
export interface ProductDetailProps {
  productId: string;
}

export interface Product {
  id: string;
  name: string;
  price: number;
  stock: number;
}

/**
 * Stock state as the user should perceive it. The viewmodel decides it
 * (the "low" threshold is a business rule); styles only give each one a color.
 */
export type StockState = 'available' | 'low' | 'soldOut';
```

---

## `viewmodel.ts` — every decision

What to notice:

- **The threshold lives here** (`LOW_STOCK`), and so does the text for each stock state. Changing "low stock" from 5 to 3 never touches the styles.
- **`addLabel` is decided here**, including the "Adding…" text. `index.tsx` doesn't choose between strings.
- **The actions can't produce an invalid quantity**: `onIncrement` stops at the stock, `onDecrement` stops at 1. The view gets no `setQuantity` and so has no way to write 0 or 999.
- **`canIncrement`, `canDecrement` and `canAddToCart` are decided once** and only read by the view.

```ts
import { useEffect, useState } from 'react';
import { addToCart, fetchProduct } from '../../services/products';
import type { Product, StockState } from './types';

// Below this many units, the product shows as "low stock".
const LOW_STOCK = 5;

export const useViewModel = (productId: string) => {
  const [product, setProduct] = useState<Product | null>(null);
  const [loadError, setLoadError] = useState(false);
  const [loadAttempt, setLoadAttempt] = useState(0);
  const [quantity, setQuantity] = useState(1);
  const [adding, setAdding] = useState(false);

  useEffect(() => {
    let cancelled = false;
    fetchProduct(productId)
      .then((loaded) => !cancelled && setProduct(loaded))
      .catch(() => !cancelled && setLoadError(true));
    return () => {
      cancelled = true;
    };
  }, [productId, loadAttempt]);

  const stock = product?.stock ?? 0;
  const stockState: StockState = stock === 0 ? 'soldOut' : stock <= LOW_STOCK ? 'low' : 'available';
  const stockLabel = { available: 'In stock', low: `Only ${stock} left`, soldOut: 'Sold out' }[stockState];
  const total = (product?.price ?? 0) * quantity;

  const onAddToCart = async () => {
    if (!product || stockState === 'soldOut' || adding) return;
    setAdding(true);
    try {
      await addToCart(product.id, quantity);
    } finally {
      setAdding(false);
    }
  };

  return {
    loading: !product && !loadError,
    loadError,
    name: product?.name ?? '',
    stockState,
    stockLabel,
    quantity,
    canDecrement: quantity > 1,
    canIncrement: quantity < stock,
    canAddToCart: stockState !== 'soldOut' && !adding,
    addLabel: adding ? 'Adding…' : `Add to cart · ${total.toFixed(2)} €`,
    onDecrement: () => setQuantity((q) => (q > 1 ? q - 1 : q)),
    onIncrement: () => setQuantity((q) => (q < stock ? q + 1 : q)),
    onAddToCart,
    onRetry: () => {
      setLoadError(false);
      setLoadAttempt((n) => n + 1);
    },
  };
};
```

---

## `styles.ts` — how each thing looks

What to notice:

- **`StockBadge` receives a `state`, not a color.** The map from state to token is the only place where "low" becomes amber.
- **The disabled look** (`opacity`) reads the button's own `disabled` prop. Styles never ask *why* it's disabled.
- Every value comes from the theme, including the 44pt minimum touch size.

```ts
import styled from 'styled-components/native';
import type { StockState } from './types';

export const Container = styled.View`
  flex: 1;
  gap: ${({ theme }) => theme.spacing.lg}px;
  padding: ${({ theme }) => theme.spacing.lg}px;
  background-color: ${({ theme }) => theme.colors.surface.main};
`;

export const Skeleton = styled.View`
  height: 120px;
  border-radius: ${({ theme }) => theme.radius.lg}px;
  background-color: ${({ theme }) => theme.colors.surface.elevated};
`;

export const Name = styled.Text`
  font-size: ${({ theme }) => theme.typography.title.fontSize}px;
  line-height: ${({ theme }) => theme.typography.title.lineHeight}px;
  font-weight: ${({ theme }) => theme.typography.title.fontWeight};
  color: ${({ theme }) => theme.colors.text.primary};
`;

// The viewmodel decides the state; this file only decides how each state looks.
export const StockBadge = styled.Text<{ state: StockState }>`
  font-size: ${({ theme }) => theme.typography.label.fontSize}px;
  font-weight: ${({ theme }) => theme.typography.label.fontWeight};
  color: ${({ theme, state }) =>
    ({
      available: theme.colors.semantic.success.main,
      low: theme.colors.semantic.warning.main,
      soldOut: theme.colors.text.tertiary,
    })[state]};
`;

export const Stepper = styled.View`
  flex-direction: row;
  align-items: center;
  gap: ${({ theme }) => theme.spacing.md}px;
`;

export const StepButton = styled.Pressable`
  min-width: ${({ theme }) => theme.touch.min}px;
  min-height: ${({ theme }) => theme.touch.min}px;
  align-items: center;
  justify-content: center;
  border-radius: ${({ theme }) => theme.radius.full}px;
  background-color: ${({ theme }) => theme.colors.surface.interactive};
  opacity: ${({ disabled }) => (disabled ? 0.4 : 1)};
`;

export const StepLabel = styled.Text`
  font-size: ${({ theme }) => theme.typography.headline.fontSize}px;
  color: ${({ theme }) => theme.colors.text.primary};
`;

export const Quantity = styled.Text`
  min-width: 32px;
  text-align: center;
  font-size: ${({ theme }) => theme.typography.headline.fontSize}px;
  color: ${({ theme }) => theme.colors.text.primary};
`;

export const AddButton = styled.Pressable`
  min-height: ${({ theme }) => theme.touch.min}px;
  align-items: center;
  justify-content: center;
  border-radius: ${({ theme }) => theme.radius.full}px;
  background-color: ${({ theme }) => theme.colors.accent.container};
  opacity: ${({ disabled }) => (disabled ? 0.4 : 1)};
`;

export const AddLabel = styled.Text`
  font-size: ${({ theme }) => theme.typography.body.fontSize}px;
  font-weight: 600;
  color: ${({ theme }) => theme.colors.accent.onAccent};
`;

export const ErrorText = styled.Text`
  font-size: ${({ theme }) => theme.typography.body.fontSize}px;
  color: ${({ theme }) => theme.colors.text.secondary};
`;
```

---

## `index.tsx` — only reads and renders

What to notice: there's no calculation, no color and no condition that the viewmodel didn't already resolve. The two early returns only read `loading` and `loadError`.

```tsx
import {
  AddButton,
  AddLabel,
  Container,
  ErrorText,
  Name,
  Quantity,
  Skeleton,
  StepButton,
  StepLabel,
  Stepper,
  StockBadge,
} from './styles';
import type { ProductDetailProps } from './types';
import { useViewModel } from './viewmodel';

export default function ProductDetailView({ productId }: ProductDetailProps) {
  const {
    loading,
    loadError,
    name,
    stockState,
    stockLabel,
    quantity,
    canDecrement,
    canIncrement,
    canAddToCart,
    addLabel,
    onDecrement,
    onIncrement,
    onAddToCart,
    onRetry,
  } = useViewModel(productId);

  if (loading) {
    return (
      <Container>
        <Skeleton />
      </Container>
    );
  }

  if (loadError) {
    return (
      <Container>
        <ErrorText>We couldn't load this product.</ErrorText>
        <AddButton onPress={onRetry}>
          <AddLabel>Try again</AddLabel>
        </AddButton>
      </Container>
    );
  }

  return (
    <Container>
      <Name>{name}</Name>
      <StockBadge state={stockState}>{stockLabel}</StockBadge>

      <Stepper>
        <StepButton disabled={!canDecrement} onPress={onDecrement} accessibilityLabel="Decrease quantity">
          <StepLabel>−</StepLabel>
        </StepButton>
        <Quantity>{quantity}</Quantity>
        <StepButton disabled={!canIncrement} onPress={onIncrement} accessibilityLabel="Increase quantity">
          <StepLabel>+</StepLabel>
        </StepButton>
      </Stepper>

      <AddButton disabled={!canAddToCart} onPress={onAddToCart}>
        <AddLabel>{addLabel}</AddLabel>
      </AddButton>
    </Container>
  );
}
```
