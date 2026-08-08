Why use "Dipendecy Injection" instead of just import stuff using the import function of your programming language ? 
Both are very similar in runtime, but very different architecturally !

The main differnce ? Coupling : 

Let's look at this code : 

```typescript
import logger from '@/logger';

logger.info(...)
```

Now your function is *hard-coupled* to a specific implementation and a specific logging strategy