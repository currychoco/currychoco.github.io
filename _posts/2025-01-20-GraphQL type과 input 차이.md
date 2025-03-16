---
layout: post
title: GraphQL type과 input 차이
categories: [일상 중 궁금했던]
comments: true
---

# GraphQL에서 `type`과 `input`의 차이 및 해결 방법

## `type`을 `input` 자리에 사용할 수 없는 이유
GraphQL에서는 `type`과 `input`을 구분해서 써야 함
- `type`은 **쿼리 응답(response)**용
- `input`은 **Mutation 입력값(input)**용
- `type`을 `input`으로 못 쓰니까 따로 `input` 정의해야 함

## 해결 방법: `input` 따로 정의하기
```graphql
type Brand {
    id: ID!
    name: String!
    description: String
    createdAt: String!
}

input AddBrandInput {
    name: String!
    description: String
}

type Mutation {
    addBrand(input: AddBrandInput!): Brand!
}
```

## Kotlin + Spring Boot에서 활용 예시
```kotlin
@GraphQLInputType
data class AddBrandInput(
    val name: String,
    val description: String?
)

@MutationMapping
fun addBrand(@Argument input: AddBrandInput): BrandDTO {
    return BrandDTO(name = input.name, description = input.description)
}
```

