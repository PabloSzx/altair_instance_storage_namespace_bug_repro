# altair_instance_storage_namespace_bug_repro

```sh
pnpm i

pnpm start
```

Then you can go to [`http://localhost:8020/altair`](http://localhost:8020/altair)

Then, in the code, uncomment the line `instanceStorageNamespace: "other"`:

```ts
import Fastify from "fastify";

import { CreateApp, gql } from "@graphql-ez/fastify";
import { ezAltairIDE } from "@graphql-ez/plugin-altair";
import { ezSchema } from "@graphql-ez/plugin-schema";

const app = Fastify({
  logger: true,
});

const { buildApp } = CreateApp({
  ez: {
    plugins: [
      ezAltairIDE({
        initialQuery: "query{hello}",
        // Uncomment this next line
        // instanceStorageNamespace: "other",
      }),
      ezSchema({
        schema: {
          typeDefs: gql`
            type Query {
              hello: String!
            }
          `,
          resolvers: {
            Query: {
              hello() {
                return "Hello World";
              },
            },
          },
        },
      }),
    ],
  },
});

app.register(buildApp().fastifyPlugin);

app.listen(8020);

```