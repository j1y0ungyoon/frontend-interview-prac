# Next.js의 getStaticProps, getStaticPaths, getServerSideProps에 대서 각각 설명해주세요

넥스트는 기본적으로 서버사이드 렌더링임(클라이언트 사이드 렌더링인 리액트와 이 부분이 가장 다름)

그래서 개발자도구의 페이지 소스를 보면 서버에서 렌더링한 일부 데이터를 미리 볼 수 있고, 검색 엔진 크롤러가 이를 읽을 수 있어 SEO에도 도움이 됨

## getStaticProps

- 페이지 컴포넌트에서 사용되며, 페이지 컴포넌트에서 데이터를 강제로 pre-fetching 해야 할 필요가 있다면 getStaticProps를 추가하면 됨
- getStaticProps를 사용하면 getStaticProps를 사용한 컴포넌트 안의 데이터를 pre-fetching할 때마다 추가함
- 만약 페이지 컴포넌트가 데이터를 필요로하지 않으면 getStaticProps를 추가할 필요 없음
- getStaticProps는 서버에서 실행되고, 빌드 타임 중에 데이터를 가져와 페이지를 사전 렌더링 함
- 때문에 브라우저에서 실행되지 않는 Node.js 코드를 실행할 수 있음
  ex: fs(파일 시스템)에 접근해 파일에서 데이터를 읽어올 수 있음
- pre-fetching된 페이지가 생성된 이후에는 pre-fetching된 데이터가 오래된 데이터일 수 있기 때문에, getStaticProps 내부에서 반환하는 객체 안에 revalidate라는 키를 추가함으로써 페이지가 구축 및 배포된 이후에도 계속 재생성 되어야한다고 Next.js에 요청할 수 있음.
- 몇 초마다 revalidate할 수 있는지 정할 수 있는데, 예를 들어 10초마다 revalidate하고 싶다면 revalidate: 10이라고 적으면 됨
- getStaticProps는 데이터가 있는 페이지를 pre-rendering하기 좋음

## getStaticPaths

- 동적 라우팅을 지원하기 위해 사용됨
- 특정 경로에 대한 동적 매개변수를 처리하고, 해당 경로에 대한 사전 렌더링을 가능하게 함.
- 만약 동적 매개변수가 있는 동적 페이지가 있다면 해당 페이지의 인스턴스(경로)가 얼마나 사전 렌더링 되어야할지도 Next.js에게 알려줘야함
  예를 들면 `[id].js` 라는 페이지가 있다면 동적 매개변수인 id에 따라 페이지가 무한까지 생길 수 있음.
- 그래서 getStaticPaths로 얼마나 pre-rendering이 되어야하는지 알려줄 수 있음
- `[id].js` 같은 동적 페이지에 getStaticProps를 사용할 때 같이 getStaticPaths를 추가함
- 그렇지 않으면 Next.js는 얼마 만큼 페이지를 미리 렌더링해야할 지 몰라 에러가 발생함
- getStaticPaths 내부에 paths라는 키에 사전 렌더링할 경로에 대한 페이지를 적으면 Next.js는 paths에 적힌 경로에 대한 모든 인스턴스를 생성함. 그리고 생성한 페이지의 인스턴스마다 getStaticProps를 호출함

## getServerSideProps

- 그런데 빌드 과정 중에 사전 구축을 원하지 않거나 revalidate를 사용해도 데이터가 적절하게 업데이트 되지 않을 수도 있음
- 대신 들어오는 모든 요청에 대해 관련 로직을 실행하려는 경우도 있음. 즉, 요청에 엑세스가 필요하거나 데이터가 늘 변경되는 경우 getServerSideProps를 사용하면 됨
- getServerSideProps는 각 요청마다 실행되므로 항상 최신 데이터를 반환함
- getStaticProps와 다르게 필드 과정이 아닌 서버에서만 실행됨
- 서버에서 즉각 사전 렌더링되고, 응답과 요청 객체로 엑세스 할 수 있음
- 즉, getServerSideProps는 미리 실행되지 않고 필요할 때만 실행됨
