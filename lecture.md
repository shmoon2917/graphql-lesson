### Introduction To Apollo

#### Error 해결하자

ERROR:root:code for hash sha256 was not found.

See `man xcode-select` for more details.

ERROR:root:code for hash md5 was not found.

### 장점

캐싱 가능

하나의 싱귤러 컬렉션을 가지고 사용할 수 있다. 다시 리페칭할 필요 x

#### default way

```javascript
import { default as CollectionsOverview } from '../';
```

#### How to use

```javascript
const GET_COLLECTIONS = gql`
  {
    collections {
      id
      items {
        id
        ...
      }
    }
  }
`;

const CollectionsOverviewContainer = () => {
  return (
    <Query query={GET_COLLECTIONS}>
      {({ loading, error, data }) => {
        if (loading) return <Spinner />;
        return <CollectionsOverview collections={data.collections} />;
      }}
    </Query>
  );
};
```

##### with variables

```javascript
const GET_COLLECTIONS_BY_TITLE = gql`
  query getCollectionsByTitle($title: String!) {
    getCollectionsByTitle(title: $title) {
      id
      items {
        id
        ...
      }
    }
  }
`;

const CollectionPageContainer = ({ match }) => {
  return (
    <Query
      query={GET_COLLECTIONS_BY_TITLE}
      variables={{ title: match.params.collectionId }}
    >
      {({ loading, error, data }) => {
        if (loading) return <Spinner />;
        const { getCollectionsByTitle } = data;
        return <CollectionPage collection={getCollectionsByTitle} />;
      }}
    </Query>
  );
};
```

#### Redux vs GraphQL

Local Cache 를 리덕스의 스토어와 비슷한 개념이라고 생각.

유일한 차이점은 로컬 캐시에 대해 쿼리나 뮤테이션을 하는 유일한 방법은 쿼리나 뮤테이션을 사용하는 것뿐이라는 점.

프론트에서의 리졸버 함수는 백엔드에서도 똑같이 사용된다. 백엔드에서는 database의 데이터를 업데이트 하는 요청을 하기 위해.

[resolver](https://www.apollographql.com/docs/apollo-server/data/resolvers/)

[cache](https://www.apollographql.com/docs/react/caching/cache-configuration/)

#### Should You Use GraphQL?
