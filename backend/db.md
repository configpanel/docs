# Database connection

```ts
import { d1Connect } from '@configpanel/d1';

export async function connect() {
  await d1Connect('my-binding-name');
}
```
