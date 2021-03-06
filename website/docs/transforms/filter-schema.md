---
id: filter-schema
title: Filter Schema Transform
sidebar_label: Filter Schema
---

The `filterSchema` transform allows you to filter fields in specific types.

```
yarn add @graphql-mesh/transform-filter-schema
```

## How to use?

Add the following configuration to your Mesh config file:

```yaml
transforms:
  - filterSchema:
      mode: bare | wrap
      filters:
        - "!User" # <-- This will remove `User` type

        - Query.!admins # <-- This will remove field `admins` from `Query` type
        - Mutation.!{addUser, removeUser} # <-- This will remove fields `addUser` and `removeUser` from `Mutation` type
        - User.{id, username, name, age} # <-- This will remove all fields, from User type, except `id`, `username`, `name` and `age`

        - Query.user.id # <-- This will remove all args from field `user`, in Query type, except `id` only
        - Query.user.!name # <-- This will remove argument `id` from field `user`, in Query type
        - Query.user.{id, name} # <-- This will remove all args for field `user`, in Query type, except `id` and `name`
        - Query.user.!{id, name} # <-- This will remove args `id` and `name` from field `user`, in Query type
```

Let's assume you have the following schema,

```graphql
type Query {
  me: User
  users: [User]
  user(id: ID, name: String): User
  admins: [User]
}

type Mutation {
  updateMyProfile(name: String, age: Int): User
  addUser(username: String, name: String, age: Int): User
  removeUser(id: ID): ID
}

type User {
  id: ID
  username: String
  password: String
  name: String
  age: Int
  ipAddress: String
}
```

With the following Filter Schema config,
```yaml
transforms:
  - filterSchema:
      mode: bare | wrap
      filters:
        - Query.!admins
        - Mutation.!{addUser, removeUser}
        - User.{username, name, age} # <-- This will remove all fields, from User type, except `id`, `username`, `name` and `age`

        - Query.user.!name # <-- This will remove argument `id` from field `user`, in Query type
```

It would become the following schema:

```graphql
type Query {
  me: User
  users: [User]
  user(id: ID): User
}

type Mutation {
  updateMyProfile(name: String, age: Int): User
}

type User {
  username: String
  name: String
  age: Int
}
```

> For information about "bare" and "wrap" modes, please read the [dedicated section](https://graphql-mesh.com/docs/getting-started/mesh-transforms#two-different-modes).

## Config API Reference

{@import ../generated-markdown/FilterSchemaTransform.generated.md}
