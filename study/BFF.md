
## BFF란 무엇인가?
BFF (Backend For Frontend)는 프론트엔드의 요구사항에 맞게 최적화된 백엔드 계층을 추가하는 아키텍쳐 패턴이다. **클라이언트의 요구사항에 맞춰 데이터를 변환하고 최적화**하는  역할을 뜻한다.

| 왜 BFF 가 필요할까? 
| 각 클라이언트(모바일, 데스크톱)에 특화된 백엔드를 위해서! 

BFF 는 단순한 API 프록시가 아니라, 프론트엔드 개발자가 원하는 방식으로 데이터를 제공하는 맞춤형 백엔드이다. 

### 기존의 문제점 
✅ 클라이언트별 최적화 부족 
- 모바일과 웹이 동일한 API 를 사용할 경우 모바일에서 불필요한 데이터까지 받아야 하거나, 웹에서 데이터가 부족해 추가 요청을 할 수 있다.

✅ 데이터 가공하는 추가 작업이 필수 




## BFF의 기본 개념
기존에는 모든 클라이언트가 같은 API 를 사용했지만, BFF 를 도입하면 각 클라이언트에 특화된 백엔드를 둘 수 있다. 

BFF는 API Gateway와 같은 **전역적인 역할**이 아니라
**특정 클라이언트**를 위한 맞춤형 데이터를 제공하는 역할이다.

### BFF 가 등장한 배경 
✅ 다양한 클라이언트 지원 
- 모바일, 웹, 테블릿 등 다양한 기기를 고려해야 한다.
  
✅ 성능 최적화 
- 기존에는 클라이언트가 여러개의 API 요청을 보내야 원하는 데이터를 받았으나 BFF 를 활용하면 한번의 요청으로 필요한 데이터를 조합해 받을 수 있게 된다. 

- 특히 모바일에서는 최소한의 요청이 되게끔 제공하는 것이 좋다.

### 장점 
BFF 가 클라이언트와 백엔드 사이에 위치하여 데이터를 클라이언트에 최적화된 형태로 가공 


✅  클라이언트별 맞춤형 API 제공
- 필요없는 데이터를 제외하거나, 필요한 데이터를 추가해서 제공 가능
- 모바일에서는 사진을 빼고 받을 수 있다. 

✅ 불필요한 API 요청 감소
- 여러개의 요청을 BFF 한번으로 조합해서 제공이 가능하다.
- `/dashbord` 요청 한번으로 `/user`, `/orders` , `/baskets` 세개의 API 를 가공하여 한 번의 응답으로 전달이 가능하다. 

✅  프론트엔드 개발 속도 향상
- 원하는 데이터 변경시에 백엔드에 요청하는 것이 아닌 BFF 를 조작하여 쉽게 수정이 가능하다

✅ 보안 및 인증 로직 처리 
- 민감한 데이터를 필터링하여 클라이언트에 전달 가능 
- 토큰 및 세션 관리를 BFF에서 집중적으로 처리 가능 


##  BFF의 아키텍처 및 설계 패턴
BFF 는 클라이언트와 백엔드 사이의 중간 계층으로 동작한다. 

##### 다시 보는 BFF의 핵심 역할
1. 클라이언트 맞춤형 응답 제공
2. API 조합 및 데이터 집계 
3. 캐싱 및 최적화
4. 인증 및 보안 로직 처리

### 단일 BFF 패턴
- 하나의 BFF 가 모든 클라이언트를 처리하는 방식 
- 클라이언트가 적고, 데이터 처리 방식이 크게 다르지 않을때 사용 

### 다중 BFF 패턴
- 클라이언트별로 BFF 를 개별적으로 운영 
- 웹, 모바일 등 클라이언트마다 최적화된 데이터 제공 가능 
- 클라이언트별 맞춤 데이터를 최적화하여 제공 가능.

### API 집계(Aggregation) 패턴
- API 응답을 한번에 처리하여 성능 최적화 
- 여러개의 API 호출을 수행하고, 하나의 응답으로 묶어 반황 
- 네트워크 요청 수를 줄여 성능 개선 가능

### 캐싱 패턴
- 반복적인 요청을 줄여 성능 최적화 
- 동일 요청이 많을 시, BFF 에서 캐싱하여 백엔드 부담 줄인다.
- Redis 를 활용하여 캐싱한다.
- 백엔드 API 부하 감소 및 응답 속도 향상 


## BFF 구현 방법 

🔥 빠르고 가벼운 프레임워크
BFF 를 구현할 때는 클라이언트와 백엔드 사이에서 동작하기 때문에, 
빠르고 가벼운 프레임워크를 사용하는 것이 좋다. 
✔️ Express (NestJS, Node)

🔥 GraphQL vs REST API 
응답을 어떻게 조합하는 것이 좋을까 ? 
GraphQL 는 클라이언트 최적화 API 로 좋은 선택지이다.
- 필요한 데이터만 선택 가능 
- 여러개의 REST API 호출없이 쿼리 하나로 필요한 데이터 요청 가능 

🔥 성능 최적화 
✅  API 요청 최적화 
- 대량의 API 요청을 효율적으로 처리하기 위해 **비동기**로 구현하는 것이 중요
- `Promise.all()` 을 사용하여 병렬 요청 처리 
- GraphQL 을 사용해 여러 데이터 요청을 한번에 처리 

✅ 캐싱 (Redis)
- 반복적 API 캐싱
- 사용자 프로필, 상품 목록 등 자주 변하지 않은 데이터 캐싱 

✅ 로깅 및 모니터링 가능

✅ 인증 
- JWT,Oauth 를 활용해 BFF에서 인증 로직 중앙화
- API Gateway 와 연동해 보안 강화 


#### 예시 코드 (Express 기반 )

```js
const express = require("express"); 
const axios = require("axios"); 
const app = express(); 

app.get("/dashboard", async (req, res) => { 
  try { 
    const [user, orders, recommendations] = await Promise.all([ axios.get("https://api.example.com/user"), axios.get("orders"), axios.get("recommendations")]); 
    res.json({ 
      user: user.data, 
      orders: orders.data, 
      recommendations: recommendations.data
    }); 
  } 
  catch (error) { 
    res.status(500).json({ error: "Failed to fetch data" }); 
  } 
});
app.listen(3000, () => console.log("BFF is running on port 3000"));
```

#### 예시 (graphQL + Apollo Express )

```js
const { ApolloServer, gql } = require("apollo-server-express");
const express = require("express");
const axios = require("axios");
const app = express();

// GraphQL 스키마 정의 (Dashboard 타입 없이 개별 필드 사용)
const typeDefs = gql`
  type User {
    id: ID!
    name: String!
    email: String!
  }

  type Order {
    id: ID!
    total: Float!
  }

  type Recommendation {
    id: ID!
    product: String!
  }

  type Query {
    user: User
    orders: [Order]
    recommendations: [Recommendation]
  }
`;

// GraphQL Resolvers
const resolvers = {
  Query: {
    user: async () => {
      const response = await axios.get("https://api.example.com/user");
      return response.data;
    },
    orders: async () => {
      const response = await axios.get("https://api.example.com/orders");
      return response.data;
    },
    recommendations: async () => {
      const response = await axios.get("example.com/recommendations");
      return response.data;
    }
  }
};

// Apollo 서버 설정
const server = new ApolloServer({ typeDefs, resolvers });
server.start().then(() => {
  server.applyMiddleware({ app });
  app.listen(4000, () => console.log("GraphQL BFF on port 4000"));
});

```

## 결론 

#### 단점 
✅ 추가적인 서버 관리 필요 
- BFF 서버를 추가로 배포하고 유지보수 해야 해서, 추가적인 인프라 관리 비용으로 이어진다.

✅ 단일 장애 지점
BFF 서버가 다운되면 전체 서비스에 영향을 준다. 그래서 BFF 서버를 복수로 배포하거나 로드 밸런싱을 적용하는 것이 좋다. 

✅ 백엔드 복잡성 증가 
BFF 에서 다뤄야 할 로직이 많아지면 단순한 데이터 전송 이상의 작업을 해서 관리 & 테스트가 복잡해진다. 특히 비즈니스 로직이 BFF 에 있으면 유지보수가 어렵다. 

✅ 오버헤드 발생
BFF 가 여러 API 를 호출하고, 데이터를 가공하는 시점에서 응답 시간이 길어질 수 있다. 
BFF 서버의 성능 최적화와 캐싱 전략을 잘 적용해야 한다. 


#### 적용해야 할 경우 
그럼에도 불구하고 BFF 를 사용하는 이유를 알아보자 

✅ 복잡한 데이터 변환 및 조합이 필요한 경우 

✅ 다양한 클라이언트를 지원하는 경우 

✅  보안이 중요한 서비스에서 API 를 추상화 하는 경우 

✅  불필요한 데이터 호출을 줄이고 필요한 데이터만 응답받고 싶을 경우 


BFF는 프론트엔드와 백엔드 간의  효율적인 데이터 전달과 보안 최적화를 해주는 유용한 패턴이지만,
모든 상황에 적합한 건 아니기 때문에 적절한 상황에서 잘 활용하는 게 중요하다.

