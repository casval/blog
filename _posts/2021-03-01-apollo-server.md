---
layout: post
title: GraphQL - Server #1
categories:
  - study
tags: 
  - javascript
---
## Apollo server 구축하기

얄코의 GraphQL 강의를 보고 서버 구현에 대한 내용을 정리

### 필요 모듈

* apollo-server

### 코드상의 실행 법

```javascript
const { ApolloServer} = require('apollo-server')

const typeDefs = ...
const resolvers = ...

const server = new ApolloServer({ typeDefs, resolvers })
server.listen().then(data => {
  console.log('Server ready at ${data.url}')
})
```

* apollo-server에서 ApolloServer를 가져와 서버를 실행한다.
* 서버 실행시 인자는 graphql을 이용하여 정의한 typeDefs와 resolvers를 인자로 받는다.
  * typeDef: GraphQL 명세에서 사용될 데이터, 요청의 타입 정의, gql (template literal tag)로 생성됨
  * resolver: 서비스의 액션들을 함수로 지정, 요청에 따라 데이터를 CRUD

### GraphQL Playground

ApolloServer에서 제공하는 GraphQL type, resolver 명세를 확인 가능한 fe로 데이터 요청 및 전송 테스트를 제공한다.

### typeDefs 작성

typeDefs는 apollo-server에서 제공하는 gql을 이용하여 작성한다.

#### Query 및 type 작성

CRUD 가운데 Read의 경우는 Query를 사용한다.

```javascript
const { gql } = require('apollo-server')

const typeDefs = gql`
	type Query {
		teams: [Team]
	}
  type Team {
    id: Int
    manager: String
    office: String
    extension_number: String
    mascot: String
    cleaning_duty: String
    project: String
  }
`
```

* query에 대한 스펙을 명시하는 곳이며 클라이언트는 이곳에 정의된 스펙 내에있는 데이터에 대해서만 쿼리 요청이 가능하다.
* teams라는 query를 작성했으며 반환되는 값은 Team type의 배열
* Team type의 객체는 Query 아래에 정의되어 있다.

### resolvers 작성

```javascript
const resolvers = {
  Query: {
    teams: () => database.teams
  }
}
```

* query가 발생했을때 해당 쿼리에 대한 로직을 처리하는 함수들이 있다.
* teams query가 오면 db에서 teams에 대한 데이터를 반환해 준다.



### Query에 조건주기

* Query에 조건을 주기 위해서는 쿼리 이름 뒤에 괄호를 넣고 조건의 변수와 타입을 명시해 준다.

* resolvers 에는 team 쿼리에 대해 해당 id에 해당하는 team을 반환해 주는 로직을 구현해 준다.

* 다음은 id를 이용해 하나의 team을 가져오는 쿼리이다.

  ```javascript
  const { gql } = require('apollo-server')
  const typeDefs = gql`
    type Query {
      team(id: Int): Team
    }
  	type Team {
      id: Int
      manager: String
      office: String
      extension_number: String
      mascot: String
      cleaning_duty: String
      project: String
    }
  `
  
  const resolvers = {
    team: (parent, args, context, info) => database.teams.find(team => team.id === args.id)
  }
  ```

* Playground에서 쿼리를 호출하는 방식은 다음과 같다.

  ```javascript
  query{
    team(id: 1) {
      id
      mamager
      office
      extension_number
      mascot
      cleaning_duty
      project
    }
  }
  ```

### 서로다른 타입을 연결하여 가져오기

* Supply라는 타입이 Team 타입의 id를 참조하고 있는 경우 Team 타입을 가져오는 경우 관련된 Supply들을 Team 내부에 연결하여 가져올 수 있다.

  * Team type에 Supplies를 추가, resolver에서 Supplies를 함께 반환해 주도록 구현

  * ```javascript
    const { gql } = require('apollo-server')
    const typeDefs = gql`
      type Query {
        team(id: Int): Team
      }
    	type Team {
        id: Int
        manager: String
        office: String
        extension_number: String
        mascot: String
        cleaning_duty: String
        project: String
    		supplies: [Supply]
      }
    	type Supply {
    		id: String
    		team: Int
    	}
    `
    
    const resolvers = {
      team: (parent, args, context, info) => {
        const t = database.teams.find(team => team.id === args.id)
        t.supplies = database.supplies.filter(supply => {
          return supply.team === team.id
        })
        return t
      }
    }
    ```

  * query는 다음과 같이 사용한다.

  * ```javascript
    query {
      team(id: 1) {
        id
        manager
        office
        extension_number
        mascot
        cleaning_duty
        project
        supplies:{
          id
          team
        }
      }
    }
    ```

### Mutation 구현하기

CRUD 중 Create, Update, Delete는 Mutation을 이용하여 구현한다.

다음은 하나의 Team을 삭제하는 Mutation을 정의한다.

* parameter는 id이고 return은 삭제되는 Team을 반환한다.

```javascript
const { gql } = require('apollo-server')

const typeDefs = gql`
	type Mutation {
		deleteTeam(id: String): Team
	}
	type Team {
    id: Int
    manager: String
    office: String
    extension_number: String
    mascot: String
    cleaning_duty: String
    project: String
  }
`
```

resolvers 내부에는 실제 처리를 하는 메소드를 구현한다.

```javascript
const resolvers = {
  deleteTeam: (parent, args, context, info) => {
    const deleted = database.teams.find(t => t.id === args.id)
    const database.teams = database.teams.filter(t => t.id !== args.id)
    return deleted
  }
}
```

* 실제 database를 사용하는 환경에서는 SQL의 query를 이용하여 구현한다.



### 모듈화

많은 양의 query와 mutation, resolver 들이 사용되면 하나의 파일로 관리하기 힘들다.

이럴때는 각각의 기능에 따라 모듈로 나누어 구현을 해준다.

보통 Query, Mutation, typeDefs + resolvers 의 세가지로 나누어 모듈화를 한다.

* Query와 Mutation은 하나의 파일에 모든 타입을 모아 넣는다.
* 각각의 다른 타입은 타입 이름을 파일명으로 타입정의와 resolver를 함께 적어 준다.

#### 예제

Team 타입과 Equipment 타입이 있는 경우 다음과 같이 구성한다.

##### _queries.js

Client 에서 요청을 하기 위한 query를 정의

```javascript
const { gql } = require('apollo-server')

const typeDefs = gql`
	type Query {
		teams: [Team]
		equipments: [Equipments]
  }
`
```

##### _mutations.js

Client에서 요청을 하기 위한 mutation을 정의

```javascript
const { gql } = require('apollo-server')

const typeDefs = gql`
	type Mutation {
		deleteTeam(id: String): Team
		editEquipment(id: String): Equipment
	}
`
```

##### team.js

Team의 타입과 resolver를 정의

```javascript
const { gql } = require('apollo-server')

const typeDefs = gql`
	type Team {
		id: String
		name: String
  }
`

const resolvers = {
  Query: {
    teams: (parent, args) => database.teams
  },
  Mutation: {
    deleteTeam: (parent, args) => {
      const team = database.teams.find(t => t.id === args.id)
      database.teams = database.teams.filter(t => t.id !== args.id)
      return team
    }
  }
}
```

##### equipment.js

Equipment의 타입과 resolver를 정의

```javascript
const { gql } = require('apollo-server')

const typeDefs = gql`
	type Equipment {
		id: String
		used_by: String
		count: Int
		new_or_used: String
	}
`

const resolvers = {
  Query: {
    equipments: (parent, args) => database.equipments
  },
  Mutation: {
    editEquipment: (parent, args) => {
      const e = database.equipments.find(e => e.id === args.id)
      if (e) {
        Object.assign(e, args)
      }
      return e
    }
  }
}
```

##### index.js

new ApolloServer() 실행시 args로 사용되는 typeDefs와 resolvers는 다음과 같이 사용 가능

* typeDefs: 단일 변수 또는 배열로 지정 가능
* resolvers: 단일 Object 또는 Merge 된 배열로 가능

위의 특징을 이용하여 다름과 같이 작성 한다.

```javascript
const { ApolloServer} = require('apollo-server')

const queries = require('./_queries')
const mutations = require('./mutations')
const equipments = require('./equipments')
const teams = require('./teams')

// typeDefs를 배열로 묶어 준다.
const typeDefs = [
  queries,
  mutations,
  equipments.typeDefs,
  teams.typeDefs,
]

// resolvers를 배열로 묶어 준다.
const resolvers = [
  equipments.resolvers,
  teams.resolvers,
]

// 서버 실행
const server = new ApolloServer({ typeDefs, resolvers })
server.listen().then(data => {
  console.log('Server ready at ${data.url}')
})
```

